---
ms.openlocfilehash: 904a6abee2b4b2cf2f5727fb70e286c8a1a592c4
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496565"
---
### <a name="aspnet-validationcontextmembername-is-not-null-when-using-custom-dataannotationsvalidationattribute"></a>使用自定义 DataAnnotations.ValidationAttribute 时，ASP.NET ValidationContext.MemberName 不为 NULL

#### <a name="details"></a>详细信息

在 .NET Framework 4.7.2 及更低版本中，使用自定义 <xref:System.ComponentModel.DataAnnotations.ValidationAttribute?displayProperty=nameWithType> 时，<xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName?displayProperty=nameWithType> 属性返回 `null`。 在 2019 年 10 月更新之前的 .NET Framework 4.8 版本中，它返回成员名称。 从 .NET Framework 4.8 的 [.NET Framework 2019 年 10 月质量汇总预览](https://devblogs.microsoft.com/dotnet/net-framework-october-2019-preview-of-quality-rollup/)开始，它默认将返回 `null`，但是可以选择改为返回成员名称。

#### <a name="suggestion"></a>建议

将以下设置添加到属性的 web.config 文件中，以返回 .NET Framework 4.8 和更高版本的 [.NET Framework 2019 年 10 月质量汇总预览](https://devblogs.microsoft.com/dotnet/net-framework-october-2019-preview-of-quality-rollup/)中的成员名称：<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;appSettings&gt;&#13;&#10;...&#13;&#10;&lt;add key=&quot;aspnet:GetValidationMemberName&quot;  value=&quot;true&quot;/&gt;&#13;&#10;...&#13;&#10;&lt;/appSettings&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>在 2019 年 10 月更新之前的 .NET Framework 4.8 版本中，将此内容添加到 web.config 文件中将还原以前的行为，并且属性返回 `null`。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |未知|
|Version|4.8|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName?displayProperty=nameWithType>

<!--

#### Affected APIs

- `P:System.ComponentModel.DataAnnotations.ValidationContext.MemberName`

-->
