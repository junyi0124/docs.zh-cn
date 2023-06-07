---
title: Protobuf 和一个字段的变量类型-gRPC for WCF 开发人员
description: 了解如何使用 Any 类型和一个关键字在消息中表示变体对象类型。
ms.date: 09/09/2019
ms.openlocfilehash: 6fe7acbd1ec35289f7ad6f3acee8509ab934619d
ms.sourcegitcommit: 771c554c84ba38cbd4ac0578324ec4cfc979cf2e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/21/2020
ms.locfileid: "77543191"
---
# <a name="protobuf-any-and-oneof-fields-for-variant-types"></a>Protobuf 和一个字段的变量类型

在 Windows Communication Foundation （WCF）中处理动态属性类型（即，类型 `object`的属性）非常复杂。 例如，必须指定序列化程序并提供[KnownType](xref:System.Runtime.Serialization.KnownTypeAttribute)属性。

协议缓冲区（Protobuf）提供了两个用于处理可能属于多个类型的值的更简单选项。 `Any` 类型可以表示任何已知的 Protobuf 消息类型。 您可以使用 `oneof` 关键字来指定在任何消息中只能设置一个字段范围中的一个字段。

## <a name="any"></a>Any

`Any` 是 Protobuf 的 "已知类型"：一系列有用的可重复使用的消息类型，具有所有支持语言的实现。 若要使用 `Any` 类型，必须导入 `google/protobuf/any.proto` 定义。

```protobuf
syntax "proto3"

import "google/protobuf/any.proto"

message Stock {
    // Stock-specific data
}

message Currency {
    // Currency-specific data
}

message ChangeNotification {
    int32 id = 1;
    google.protobuf.Any instrument = 2;
}
```

在C#代码中，`Any` 类提供用于设置字段、提取消息和检查类型的方法。

```csharp
public void FormatChangeNotification(ChangeNotification change)
{
    if (change.Instrument.Is(Stock.Descriptor))
    {
        FormatStock(change.Instrument.Unpack<Stock>());
    }
    else if (change.Instrument.Is(Currency.Descriptor))
    {
        FormatCurrency(change.Instrument.Unpack<Currency>());
    }
    else
    {
        throw new ArgumentException("Unknown instrument type");
    }
}
```

Protobuf 的内部反射代码使用每个生成的类型上的 `Descriptor` 静态字段来解析 `Any` 字段类型。 还有一个 `TryUnpack<T>` 方法，但即使在失败时也会创建 `T` 的未初始化实例。 如前文所述，最好使用 `Is` 方法。

## <a name="oneof"></a>一个

一个字段是一项语言功能：编译器在生成 message 类时处理 `oneof` 关键字。 使用 `oneof` 指定 `ChangeNotification` 消息可能如下所示：

```protobuf
message Stock {
    // Stock-specific data
}

message Currency {
    // Currency-specific data
}

message ChangeNotification {
  int32 id = 1;
  oneof instrument {
    Stock stock = 2;
    Currency currency = 3;
  }
}
```

在整个消息声明中，`oneof` 集内的字段必须具有唯一的字段编号。

使用 `oneof`时，生成C#的代码包括一个枚举，用于指定已设置了哪些字段。 可以测试枚举，查找设置的字段。 未设置的字段将返回 `null` 或默认值，而不是引发异常。

```csharp
public void FormatChangeNotification(ChangeNotification change)
{
    switch (change.InstrumentCase)
    {
        case ChangeNotification.InstrumentOneofCase.None:
            return;
        case ChangeNotification.InstrumentOneofCase.Stock:
            FormatStock(change.Stock);
            break;
        case ChangeNotification.InstrumentOneofCase.Currency:
            FormatCurrency(change.Currency);
            break;
        default:
            throw new ArgumentException("Unknown instrument type");
    }
}
```

设置属于 `oneof` 集一部分的任何字段都将自动清除该集中的任何其他字段。 不能将 `repeated` 与 `oneof`一起使用。 相反，您可以创建包含重复字段或 `oneof` 集的嵌套消息，以解决此限制。

>[!div class="step-by-step"]
>[上一页](protobuf-reserved.md)
>[下一页](protobuf-enums.md)
