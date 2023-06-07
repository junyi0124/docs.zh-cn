---
title: BinaryFormatter 事件源
description: 了解如何使用 BinaryFormatter 事件源记录进行序列化或反序列化的时间。
ms.date: 12/03/2020
ms.author: levib
author: GrabYourPitchforks
ms.openlocfilehash: 9ae2af77b6cfc270207fb0f2969ada8c2eca1153
ms.sourcegitcommit: ecd9e9bb2225eb76f819722ea8b24988fe46f34c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2020
ms.locfileid: "96740387"
---
# <a name="binaryformatter-event-source"></a>BinaryFormatter 事件源

从 .NET 5.0 开始，<xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> 包含内置的 <xref:System.Diagnostics.Tracing.EventSource>，你可通过此功能了解进行对象序列化或反序列化的时间。 应用可以使用 <xref:System.Diagnostics.Tracing.EventListener> 派生的类型侦听这些通知，并进行记录。

此功能不能代替 <xref:System.Runtime.Serialization.SerializationBinder> 或 <xref:System.Runtime.Serialization.ISerializationSurrogate>，不能用于修改要序列化或反序列化的数据。 此事件系统用于深入了解要序列化或反序列化的类型。 它还可用于检测对 `BinaryFormatter` 基础结构的意外调用，如来自第三方库代码的调用。

## <a name="description-of-events"></a>事件说明

`BinaryFormatter` 事件源的已知名称为 `System.Runtime.Serialization.Formatters.Binary.BinaryFormatterEventSource`。 侦听器可以订阅 6 个事件。

### <a name="serializationstart-event-id--10"></a>SerializationStart 事件（ID 为 `10`）

在 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Serialize%2A?displayProperty=nameWithType> 已调用并且序列化进程已启动时引发。 此事件与 `SerializationEnd` 事件成对出现。 对象在其自己的序列化例程中调用 `SerializationStart` 时，`BinaryFormatter.Serialize` 事件可以递归方式调用。

此事件不包含有效负载。

### <a name="serializationend-event-id--11"></a>SerializationEnd 事件（ID 为 `11`）

在 `BinaryFormatter.Serialize` 已完成其工作时引发。 `SerializationEnd` 每次出现时，都表示最后一个未成对的 `SerializationStart` 事件完成。

此事件不包含有效负载。

### <a name="serializingobject-event-id--12"></a>SerializingObject 事件（ID 为 `12`）

在 `BinaryFormatter.Serialize` 正在序列化非基元类型时引发。 `BinaryFormatter` 基础结构会对某些类型（如 `string` 和 `int`）进行特殊处理，当它遇到这些类型时不会引发此事件。 此事件将针对用户定义的类型和 `BinaryFormatter` 本机无法理解的其他类型引发。

此事件可在 `SerializationStart` 和 `SerializationEnd` 事件之间引发零次或多次。

此事件包含有效负载，其中包括一个参数：

* `typeName` (`string`)：要序列化的类型的程序集限定名称（请参阅 <xref:System.Type.AssemblyQualifiedName?displayProperty=nameWithType>）。

### <a name="deserializationstart-event-id--20"></a>DeserializationStart 事件（ID 为 `20`）

在 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize%2A?displayProperty=nameWithType> 已调用并且反序列化进程已启动时引发。 此事件与 `DeserializationEnd` 事件成对出现。 对象在其自己的反序列化例程中调用 `DeserializationStart` 时，`BinaryFormatter.Deserialize` 事件可以递归方式调用。

此事件不包含有效负载。

### <a name="deserializationend-event-id--21"></a>DeserializationEnd 事件（ID 为 `21`）

在 `BinaryFormatter.Deserialize` 已完成其工作时引发。 `DeserializationEnd` 每次出现时，都表示最后一个未成对的 `DeserializationStart` 事件完成。

此事件不包含有效负载。

### <a name="deserializingobject-event-id--22"></a>DeserializingObject 事件（ID 为 `22`）

在 `BinaryFormatter.Deserialize` 正在反序列化非基元类型时引发。 `BinaryFormatter` 基础结构会对某些类型（如 `string` 和 `int`）进行特殊处理，当它遇到这些类型时不会引发此事件。 此事件将针对用户定义的类型和 `BinaryFormatter` 本机无法理解的其他类型引发。

此事件可在 `DeserializationStart` 和 `DeserializationEnd` 事件之间引发零次或多次。

此事件包含有效负载，其中包括一个参数。

* `typeName` (`string`)：要反序列化的类型的程序集限定名称（请参阅 <xref:System.Type.AssemblyQualifiedName?displayProperty=nameWithType>）。

### <a name="advanced-subscribing-to-a-subset-of-notifications"></a>\[高级\] 订阅部分通知

仅需要订阅部分通知的侦听器可以选择要启用的关键字。

* `Serialization` = `(EventKeywords)1`：引发 `SerializationStart`、`SerializationEnd` 和 `SerializingObject` 事件。
* `Deserialization` = `(EventKeywords)2`：引发 `DeserializationStart`、`DeserializationEnd` 和 `DeserializingObject` 事件。

如果在 `EventListener` 注册过程中未提供关键字筛选器，则系统将引发所有事件。

有关详细信息，请参阅 <xref:System.Diagnostics.Tracing.EventKeywords?displayProperty=nameWithType>。

## <a name="sample-code"></a>示例代码

下面的代码：

- 创建将写入 `System.Console` 的 `EventListener` 派生的类型，
- 为该侦听器订阅 `BinaryFormatter` 生成的通知，
- 使用 `BinaryFormatter` 序列化和反序列化简单的对象图，并
- 分析已引发的事件。

:::code language="csharp" source="snippets/binaryformatter-event-source/csharp/Program.cs":::

上面的代码生成的输出与下面的示例相似：

```output
Event SerializationStart (id=10) received.
Event SerializingObject (id=12) received.
typeName = BinaryFormatterEventSample.Person, BinaryFormatterEventSample, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
Event SerializingObject (id=12) received.
typeName = BinaryFormatterEventSample.Book, BinaryFormatterEventSample, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
Event SerializationStop (id=11) received.
Event DeserializationStart (id=20) received.
Event DeserializingObject (id=22) received.
typeName = BinaryFormatterEventSample.Person, BinaryFormatterEventSample, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
Event DeserializingObject (id=22) received.
typeName = BinaryFormatterEventSample.Book, BinaryFormatterEventSample, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
Event DeserializationStop (id=21) received.
Rehydrated person Logan Edwards
Favorite book: A Tale of Two Cities by Charles Dickens, list price 10.25
```

在此示例中，基于控制台的 `EventListener` 将对序列化启动、`Person` 和 `Book` 的实例序列化以及序列化完成进行记录。 同样，反序列化启动后，`Person` 和 `Book` 的实例将进行反序列化，然后反序列化完成。

然后，应用将打印已反序列化的 `Person` 中包含的值，以证明对象确实已正确地进行序列化和反序列化。

## <a name="see-also"></a>请参阅

有关使用 `EventListener` 接收基于 `EventSource` 的通知的详细信息，请参阅 [`EventListener` 类](xref:System.Diagnostics.Tracing.EventListener)。
