---
ms.openlocfilehash: b648aee35ff44730f545f0fa06f4e0a86615dece
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90606844"
---
### <a name="httputilityjavascriptstringencode-escapes-ampersand"></a>HttpUtility.JavaScriptStringEncode 转义 & 号

#### <a name="details"></a>详细信息

自 .NET Framework 4.5 起，<xref:System.Web.HttpUtility.JavaScriptStringEncode(System.String)?displayProperty=fullName> 转义 &amp; 符号。

#### <a name="suggestion"></a>建议

如果应用依赖于此方法以前的行为，可将 aspnet:JavaScriptDoNotEncodeAmpersand 设置添加到配置文件中的 [ASP.NET appSettings 元素](/previous-versions/aspnet/hh975440(v=vs.120))。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |次要|
|Version|4.5|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Web.HttpUtility.JavaScriptStringEncode(System.String)?displayProperty=nameWithType>
- <xref:System.Web.HttpUtility.JavaScriptStringEncode(System.String,System.Boolean)?displayProperty=nameWithType>

<!--

#### Affected APIs

- `M:System.Web.HttpUtility.JavaScriptStringEncode(System.String)`
- `M:System.Web.HttpUtility.JavaScriptStringEncode(System.String,System.Boolean)`

-->
