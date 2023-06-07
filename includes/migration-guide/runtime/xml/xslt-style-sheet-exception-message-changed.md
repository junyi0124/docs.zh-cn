---
ms.openlocfilehash: d33d75b4c2d9bc18844f66f1fcca1e2efc6f1eee
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497869"
---
### <a name="xslt-style-sheet-exception-message-changed"></a>XSLT 样式表异常消息已更改

#### <a name="details"></a>详细信息

在 .NET Framework 4.5 中，XSLT 文件过于复杂时显示的错误消息的文本为“样式表太复杂”。在先前版本中，错误消息为“XSLT 编译错误”。取决于错误消息的文本的应用程序代码将不再有效。 但是，异常类型保持不变，因此，此更改应该不会造成实际影响。

#### <a name="suggestion"></a>建议

根据此错误条件发出的异常消息更新任何应用代码，以收到新消息，或者（最好是）更新代码以仅依赖于未更改的异常类型 (<xref:System.Xml.Xsl.XsltException?displayProperty=fullName>)。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |边缘|
|Version|4.5|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Xml.Xsl.XslCompiledTransform.Load(System.String)?displayProperty=nameWithType>
- <xref:System.Xml.Xsl.XslCompiledTransform.Load(System.Type)?displayProperty=nameWithType>
- <xref:System.Xml.Xsl.XslCompiledTransform.Load(System.Xml.XmlReader)?displayProperty=nameWithType>
- <xref:System.Xml.Xsl.XslCompiledTransform.Load(System.Xml.XPath.IXPathNavigable)?displayProperty=nameWithType>
- <xref:System.Xml.Xsl.XslCompiledTransform.Load(System.Reflection.MethodInfo,System.Byte[],System.Type[])?displayProperty=nameWithType>
- <xref:System.Xml.Xsl.XslCompiledTransform.Load(System.String,System.Xml.Xsl.XsltSettings,System.Xml.XmlResolver)?displayProperty=nameWithType>
- <xref:System.Xml.Xsl.XslCompiledTransform.Load(System.Xml.XmlReader,System.Xml.Xsl.XsltSettings,System.Xml.XmlResolver)?displayProperty=nameWithType>
- <xref:System.Xml.Xsl.XslCompiledTransform.Load(System.Xml.XPath.IXPathNavigable,System.Xml.Xsl.XsltSettings,System.Xml.XmlResolver)?displayProperty=nameWithType>

<!--

#### Affected APIs

- `M:System.Xml.Xsl.XslCompiledTransform.Load(System.String)`
- `M:System.Xml.Xsl.XslCompiledTransform.Load(System.Type)`
- `M:System.Xml.Xsl.XslCompiledTransform.Load(System.Xml.XmlReader)`
- `M:System.Xml.Xsl.XslCompiledTransform.Load(System.Xml.XPath.IXPathNavigable)`
- `M:System.Xml.Xsl.XslCompiledTransform.Load(System.Reflection.MethodInfo,System.Byte[],System.Type[])`
- `M:System.Xml.Xsl.XslCompiledTransform.Load(System.String,System.Xml.Xsl.XsltSettings,System.Xml.XmlResolver)`
- `M:System.Xml.Xsl.XslCompiledTransform.Load(System.Xml.XmlReader,System.Xml.Xsl.XsltSettings,System.Xml.XmlResolver)`
- `M:System.Xml.Xsl.XslCompiledTransform.Load(System.Xml.XPath.IXPathNavigable,System.Xml.Xsl.XsltSettings,System.Xml.XmlResolver)`

-->
