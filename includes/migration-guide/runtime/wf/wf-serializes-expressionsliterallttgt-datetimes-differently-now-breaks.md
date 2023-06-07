---
ms.openlocfilehash: 06424c4fa40343a881356c20003300f65e93efbb
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496680"
---
### <a name="wf-serializes-expressionsliterallttgt-datetimes-differently-now-breaks-custom-xaml-parsers"></a>WF 现在采用不同方式序列化 Expressions.Literal&lt;T&gt; DateTimes（中断自定义 XAML 分析器）

#### <a name="details"></a>详细信息

关联的 <xref:System.Windows.Markup.ValueSerializer> 对象将一个 <xref:System.DateTime?displayProperty=fullName> 或 <xref:System.DateTimeOffset?displayProperty=fullName> 对象（其 Second 和 <xref:System.DateTime.Millisecond?displayProperty=fullName> 组件非零且（针对 <xref:System.DateTime?displayProperty=fullName> 值）且其 <xref:System.DateTime.Kind> 属性不是 Unspecified）转换为属性元素语法而不是字符串。 此更改允许 <xref:System.DateTime?displayProperty=fullName> 和 <xref:System.DateTimeOffset?displayProperty=fullName> 值往返。 假定输入 XAML 是采用特性语法的自定义 XAML 分析器将无法正常运行。

#### <a name="suggestion"></a>建议

此更改允许 <xref:System.DateTime?displayProperty=fullName> 和 <xref:System.DateTimeOffset?displayProperty=fullName> 值往返。 假定输入 XAML 是采用特性语法的自定义 XAML 分析器将无法正常运行。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |边缘|
|Version|4.5|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

无法通过 API 分析检测到。

<!--

#### Affected APIs

Not detectable via API analysis.

-->
