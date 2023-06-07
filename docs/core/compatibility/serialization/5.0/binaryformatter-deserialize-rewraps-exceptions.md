---
title: 中断性变更：BinaryFormatter.Deserialize 重新包装一些异常
description: 了解 .NET 5.0 中的以下中断性变更：BinaryFormatter.Deserialize 重新包装 SerializationException 中的一些异常对象。
ms.date: 08/18/2020
ms.openlocfilehash: 90dc4cce6785fdb38644cca2a2e9aff65eb7a313
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95759118"
---
# <a name="binaryformatterdeserialize-rewraps-some-exceptions-in-serializationexception"></a>BinaryFormatter.Deserialize 重新包装 SerializationException 中的一些异常

现在，<xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize%2A?displayProperty=nameWithType> 方法在将异常传播回调用方之前，会重新包装 <xref:System.Runtime.Serialization.SerializationException> 中的某些异常对象。

## <a name="change-description"></a>更改描述

以前，<xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize%2A?displayProperty=nameWithType> 方法允许一些任意异常（如 <xref:System.ArgumentNullException>）将堆栈向上传播到其调用方。

在 .NET 5.0 及更高版本中，<xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize%2A?displayProperty=nameWithType> 方法更主动地捕获由于反序列化操作无效而发生的异常，并将它们包装在 <xref:System.Runtime.Serialization.SerializationException> 中。

## <a name="version-introduced"></a>引入的版本

5.0

## <a name="recommended-action"></a>建议操作

在大多数情况下，你不必执行任何操作。 但是，如果你的调用站点依赖于引发的特定异常，则可以从外部 <xref:System.Runtime.Serialization.SerializationException> 中解包异常，如下面的示例中所示。

```csharp
Stream inputStream = GetInputStream();
var formatter = new BinaryFormatter();

try
{
    object deserialized = formatter.Deserialize(inputStream);
}
catch (MyException myEx)
{
    // Handle 'myEx' here in case it was thrown directly.
}
catch (SerializationException serEx) when (serEx.InnerException is MyException myEx)
{
    // Handle 'myEx' here in case it was wrapped in SerializationException.
}
```

## <a name="affected-apis"></a>受影响的 API

- <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize%2A?displayProperty=fullName>

<!--

### Affected APIs

- `Overload:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize`

### Category

Serialization

-->
