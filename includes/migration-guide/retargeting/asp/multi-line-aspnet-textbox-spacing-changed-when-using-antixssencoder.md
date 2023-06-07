---
ms.openlocfilehash: 53860bb867522503c5cb9bd35e25fadd00a116a2
ms.sourcegitcommit: 97405ed212f69b0a32faa66a5d5fae7e76628b68
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/01/2020
ms.locfileid: "91609151"
---
### <a name="multi-line-aspnet-textbox-spacing-changed-when-using-antixssencoder"></a>使用 AntiXSSEncoder 时，多行 ASP.NET 文本框间距已更改

#### <a name="details"></a>详细信息

在 .NET Framework 4.0 中，如果使用 <xref:System.Web.Security.AntiXss.AntiXssEncoder?displayProperty=fullName>，则多余行将插入回发的多行文本框中的各行之间。 在 .NET Framework 4.5 中，不包括这些多余的换行符，仅在 Web 应用面向 .NET Framework 4.5 时才可包含

#### <a name="suggestion"></a>建议

请注意，重定向到 .NET Framework 4.5 的 4.0 Web 应用可能改进了多行文本框，从而不再插入多余的换行符。 如果不需要这样做，当应用在 .NET Framework 4.5 上运行时，通过面向 .NET Framework 4.0 可使用旧行为。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.5         |
| 类型    | 重定目标 |
