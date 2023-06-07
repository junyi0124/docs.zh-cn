---
ms.openlocfilehash: b3cc04d5675ea63a0a6b967e293da8a1bd79830d
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96031971"
---
### <a name="uribuilder-properties-no-longer-prepend-leading-characters"></a>UriBuilder 属性不再预置前导字符

如果已存在一个字符，则 <xref:System.UriBuilder.Fragment?displayProperty=nameWithType> 不再预置前导 `#` 字符，且 <xref:System.UriBuilder.Query?displayProperty=nameWithType> 不再预置前导 `?` 字符。

#### <a name="change-description"></a>更改描述

在 .NET Framework 中，<xref:System.UriBuilder.Fragment?displayProperty=nameWithType> 和 <xref:System.UriBuilder.Query?displayProperty=nameWithType> 属性始终将 `#` 或 `?` 字符分别预置到所存储的值。 如果字符串已经包含其中一个前导字符，则此行为可能会导致存储值中包含多个 `#` 或 `?` 字符。 例如，<xref:System.UriBuilder.Fragment?displayProperty=nameWithType> 的值可能会变为 `##main`。

从 .NET Core 1.0 开始，如果在字符串的开头已经存在一个字符，则这些属性将不再将 `#` 或 `?` 字符预置到存储值之前。

#### <a name="version-introduced"></a>引入的版本

1.0

#### <a name="recommended-action"></a>建议操作

设置属性值时，不再需要显式删除这些前导字符。 这在追加值时特别有用，因为不再需要在每次追加时删除前导 `#` 或 `?`。

例如，下面的代码片段显示 .NET Framework 和 .NET Core 之间的行为差异。

```csharp
var builder = new UriBuilder();
builder.Query = "one=1";
builder.Query += "&two=2";
builder.Query += "&three=3";
builder.Query += "&four=4";

Console.WriteLine(builder.Query);
```

- 在 .NET Framework 中，输出为 `????one=1&two=2&three=3&four=4`。
- 在 .NET Core 中，输出为 `?one=1&two=2&three=3&four=4`。

#### <a name="category"></a>类别

Core .NET 库

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.UriBuilder.Fragment?displayProperty=fullName>
- <xref:System.UriBuilder.Query?displayProperty=fullName>

<!--

#### Affected APIs

- `T:System.UriBuilder.Fragment`
- `T:System.UriBuilder.Query`

-->
