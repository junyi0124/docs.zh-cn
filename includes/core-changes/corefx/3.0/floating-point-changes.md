---
ms.openlocfilehash: 719f336e1b38597674d6ee8f0c5429dd965054b1
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96031974"
---
### <a name="floating-point-formatting-and-parsing-behavior-changed"></a>浮点格式设置和分析行为已更改

浮点分析和格式设置行为（由 <xref:System.Double> 和 <xref:System.Single> 类型实现）现符合 IEEE 标准。

#### <a name="change-description"></a>更改描述

在 .NET Core 2.2 及更低版本中，通过 <xref:System.Double.ToString%2A?displayProperty=nameWithType> 和 <xref:System.Single.ToString%2A?displayProperty=nameWithType> 进行格式设置的行为，以及使用 <xref:System.Double.Parse%2A?displayProperty=nameWithType>、<xref:System.Double.TryParse%2A?displayProperty=nameWithType>、<xref:System.Single.Parse%2A?displayProperty=nameWithType> 和 <xref:System.Single.TryParse%2A?displayProperty=nameWithType> 进行分析的行为不符合 IEEE 标准。 因此，没法保证值在没有任何受支持的标准或自定义格式字符串的情况下双向传输。 对于某些输入，尝试分析已设置格式的值可能会失败；而对于其他输入，分析后的值与原始值不相等。

自 .NET Core 3.0 起，分析和格式设置操作均符合 IEEE 754 标准。 这可确保 .NET 中浮点类型的行为与符合 IEEE 标准的语言（例如 C#）一致。 有关浮点改进的详细信息，请参阅 [Floating-point parsing and formatting improvements in .NET Core 3.0](https://devblogs.microsoft.com/dotnet/floating-point-parsing-and-formatting-improvements-in-net-core-3-0/)（.NET Core 3.0 中浮点分析和格式设置改进）博客文章。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="recommended-action"></a>建议操作

在 [Floating-point parsing and formatting improvements in .NET Core 3.0](https://devblogs.microsoft.com/dotnet/floating-point-parsing-and-formatting-improvements-in-net-core-3-0/)（.NET Core 3.0 中浮点分析和格式设置改进）博客文章的“对现有代码的潜在影响”部分中，如果你在与 .NET Core 2.2 应用程序比较时观察到行为有变，则通常建议更改代码，这涉及到使用不同的标准或自定义格式字符串来强制执行所需的行为。 如果一些结果之前都不正确，则它们可能没法修复。

#### <a name="category"></a>类别

Core .NET 库

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Double.ToString%2A?displayProperty=nameWithType>
- <xref:System.Single.ToString%2A?displayProperty=nameWithType>
- <xref:System.Double.Parse%2A?displayProperty=nameWithType>
- <xref:System.Double.TryParse%2A?displayProperty=nameWithType>
- <xref:System.Single.Parse%2A?displayProperty=nameWithType>
- <xref:System.Single.TryParse%2A?displayProperty=nameWithType>

<!-- 

#### Affected APIs

- `Overload:System.Double.ToString`
- `Overload:System.Single.ToString`
- `Overload:System.Double.Parse`
- `Overload:System.Double.TryParse`
- `Overload:System.Single.Parse`
- `Overload:System.Single.TryParse`

-->
