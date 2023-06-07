---
ms.openlocfilehash: 935d9b2368ea4a0eeca7943dcbd584d24d2a6633
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96031975"
---
### <a name="floating-point-parsing-operations-no-longer-fail-or-throw-an-overflowexception"></a>浮点分析操作不再失败或引发 OverflowException

浮点分析方法在分析数值超出 <xref:System.Single> 或 <xref:System.Double> 浮点类型范围的字符串时，不再引发 <xref:System.OverflowException> 或返回 `false`。

#### <a name="change-description"></a>更改描述

在 .NET Core2.2 和早期版本中，<xref:System.Double.Parse%2A?displayProperty=nameWithType> 和 <xref:System.Single.Parse%2A?displayProperty=nameWithType> 方法对超出其各自类型范围的值会引发 <xref:System.OverflowException>。 对于超出范围的数值的字符串表示形式，<xref:System.Double.TryParse%2A?displayProperty=nameWithType> 和 <xref:System.Single.TryParse%2A?displayProperty=nameWithType> 方法返回 `false`。

从 .NET Core 3.0 开始，分析超出范围的数值字符串时，<xref:System.Double.Parse%2A?displayProperty=nameWithType>、<xref:System.Double.TryParse%2A?displayProperty=nameWithType>、<xref:System.Single.Parse%2A?displayProperty=nameWithType> 和 <xref:System.Single.TryParse%2A?displayProperty=nameWithType> 方法不再失败。 相反，<xref:System.Double> 分析方法对于超过 <xref:System.Double.MaxValue?displayProperty=nameWithType> 的值返回 <xref:System.Double.PositiveInfinity?displayProperty=nameWithType>，对于小于 <xref:System.Double.MinValue?displayProperty=nameWithType> 的值返回 <xref:System.Double.NegativeInfinity?displayProperty=nameWithType>。 同样，<xref:System.Single> 分析方法对于超过 <xref:System.Single.MaxValue?displayProperty=nameWithType> 的值返回 <xref:System.Single.PositiveInfinity?displayProperty=nameWithType>，对于小于 <xref:System.Single.MinValue?displayProperty=nameWithType> 的值返回 <xref:System.Single.NegativeInfinity?displayProperty=nameWithType>。

此更改是为了改进 IEEE 754:2008 的符合性。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="recommended-action"></a>建议操作

此更改会以两种方式之一影响你的代码：

- 你的代码依赖于 <xref:System.OverflowException> 在发生溢出时执行的处理程序。 在这种情况下，应删除 `catch` 语句，并在 `If` 语句中放置必要的代码，以测试 <xref:System.Double.IsInfinity%2A?displayProperty=nameWithType> 或 <xref:System.Single.IsInfinity%2A?displayProperty=nameWithType> 是否为 `true`。

- 代码假设浮点值不是 `Infinity`。 在这种情况下，应添加必要的代码来检查 `PositiveInfinity` 和 `NegativeInfinity` 的浮点值。

#### <a name="category"></a>类别

Core .NET 库

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Double.Parse%2A?displayProperty=nameWithType>
- <xref:System.Double.TryParse%2A?displayProperty=nameWithType>
- <xref:System.Single.Parse%2A?displayProperty=nameWithType>
- <xref:System.Single.TryParse%2A?displayProperty=nameWithType>

<!--

#### Affected APIs

- `Overload:System.Double.Parse`
- `Overload:System.Double.TryParse`
- `Overload:System.Single.Parse`
- `Overload:System.Single.TryParse`

-->
