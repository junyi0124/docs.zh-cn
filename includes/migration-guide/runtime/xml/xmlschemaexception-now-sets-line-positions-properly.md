---
ms.openlocfilehash: 04d1c1162dc79bbcb0578c6661466f4d58a57acc
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497911"
---
### <a name="xmlschemaexception-now-sets-line-positions-properly"></a>XmlSchemaException 现在正确设置行位置

#### <a name="details"></a>详细信息

如果将 <xref:System.Xml.Linq.LoadOptions.SetLineInfo> 值传递给 Load 方法并发生验证错误，则 <xref:System.Xml.Schema.XmlSchemaException.LineNumber> 和 <xref:System.Xml.Schema.XmlSchemaException.LinePosition> 属性现在包含行信息。

#### <a name="suggestion"></a>建议

应该更新假设不设置 <xref:System.Xml.Schema.XmlSchemaException.LineNumber> 和 <xref:System.Xml.Schema.XmlSchemaException.LinePosition> 的异常处理代码，因为这些属性将会在加载 XML 且使用 SetLineInfo 的时候正确设置。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |边缘|
|Version|4.5|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Xml.Linq.LoadOptions.SetLineInfo?displayProperty=nameWithType>

<!--

#### Affected APIs

- `F:System.Xml.Linq.LoadOptions.SetLineInfo`

-->
