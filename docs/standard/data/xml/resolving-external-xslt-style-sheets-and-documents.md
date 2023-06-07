---
title: 解析外部 XSLT 样式表和文档
ms.date: 03/30/2017
ms.assetid: 920cfe3b-d525-4bb2-abf6-9431651f9cf9
ms.openlocfilehash: f1fbf950f7d67d46937652987516e5faf9857b85
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95686561"
---
# <a name="resolving-external-xslt-style-sheets-and-documents"></a>解析外部 XSLT 样式表和文档

转换过程中会有几个场合需要解析外部资源。  
  
> [!NOTE]
> <xref:System.Xml.Xsl.XslTransform> 类在 .NET Framework 2.0 中已过时。 可以使用 <xref:System.Xml.Xsl.XslCompiledTransform> 类执行可扩展样式表语言转换 (XSLT) 转换。  
  
 转换过程中会有几个场合需要解析外部资源：  
  
- 在 <xref:System.Xml.Xsl.XslTransform.Load%2A> 期间定位外部样式表时。  
  
- 在 <xref:System.Xml.Xsl.XslTransform.Load%2A> 期间解析样式表中的任何 `<xsl:include>` 或 `<xsl:import>` 元素时。  
  
- 在 <xref:System.Xml.Xsl.XslTransform.Transform%2A> 期间解析任何 `document()` 函数时。  
  
## <a name="using-the-xmlresolver-class"></a>使用 XmlResolver 类  

 如果访问网络资源需要身份验证，则使用有 <xref:System.Xml.Xsl.XslTransform.Load%2A> 参数的 <xref:System.Xml.XmlResolver> 方法，以便传递包含必要的凭据属性集的 <xref:System.Xml.XmlResolver> 对象。  
  
 如果要使用一个自定义 <xref:System.Xml.XmlResolver>，或者如果需要指定其他凭据，下表根据需要解析外部资源的不同情况列出了需要执行的任务。  
  
|需要解析的过程|所需任务|  
|--------------------------------------|-------------------|  
|在 <xref:System.Xml.Xsl.XslTransform.Load%2A> 期间定位样式表时。|如果样式表在要求凭据的资源上，则指定接受 <xref:System.Xml.Xsl.XslTransform.Load%2A> 参数的重载 <xref:System.Xml.XmlResolver> 方法。|  
|在 <xref:System.Xml.Xsl.XslTransform.Load%2A> 期间解析 `<xsl:include>` 或 `<xsl:import>` 时。|指定接受 <xref:System.Xml.Xsl.XslTransform.Load%2A> 参数的重载 <xref:System.Xml.XmlResolver> 方法。 <xref:System.Xml.XmlResolver> 用来加载由 `import` 或 `include` 语句引用的样式表。 如果您传入 `null`，则不解析外部资源。|  
|在转换期间解析任何 `document()` 函数时。|在转换期间，使用需要使用 <xref:System.Xml.XmlResolver> 参数的 <xref:System.Xml.Xsl.XslTransform.Transform%2A> 方法指定 <xref:System.Xml.XmlResolver>。|  
  
 除了输入流提供的初始 XML 数据外，`document()` 函数还从样式表中检索其他 XML 资源。 因为此函数允许包括可能位于其他位置的 XML 数据，所以，向 <xref:System.Xml.XmlResolver> 方法提供一个具有 `null` 值的 <xref:System.Xml.Xsl.XslTransform.Transform%2A> 将使 `document()` 函数无法执行。 若要使用 `document()` 函数，除具有适当的权限集外，还应使用接受 <xref:System.Xml.Xsl.XslTransform.Transform%2A> 参数的 <xref:System.Xml.XmlResolver> 方法。  
  
 有关 <xref:System.Xml.Xsl.XslTransform.Load%2A> 方法及其 <xref:System.Xml.XmlResolver> 的使用的更多信息，请参见 <xref:System.Xml.Xsl.XslTransform.Load%28System.String%2CSystem.Xml.XmlResolver%29?displayProperty=nameWithType>。  
  
 调用 <xref:System.Xml.Xsl.XslTransform.Transform%2A> 方法时，将针对在加载时提供的证据来计算权限，并将权限集分配给整个转换过程。 如果 `document()` 函数尝试启动一个操作而该操作需要权限集中没有的权限，就会引发异常。  
  
## <a name="see-also"></a>请参阅

- [XslTransform 类的 XSLT 转换](xslt-transformations-with-the-xsltransform-class.md)
- [XslTransform 类实现 XSLT 处理器](xsltransform-class-implements-the-xslt-processor.md)
- [XslTransform 的输出](outputs-from-an-xsltransform.md)
- [不同存储区的 XSLT 转换](xslt-transformations-over-different-stores.md)
- [样式表参数和扩展对象的 XsltArgumentList](xsltargumentlist-for-style-sheet-parameters-and-extension-objects.md)
- [使用 \<msxsl:script> 编写 XSLT 样式表脚本](xslt-stylesheet-scripting-using-msxsl-script.md)
- [对 msxsl:node-set() 函数的支持](support-for-the-msxsl-node-set-function.md)
- [转换中的 XPathNavigator](xpathnavigator-in-transformations.md)
- [转换中的 XPathNodeIterator](xpathnodeiterator-in-transformations.md)
- [XslTransform 的 XPathDocument 输入](xpathdocument-input-to-xsltransform.md)
- [XslTransform 的 XmlDataDocument 输入](xmldatadocument-input-to-xsltransform.md)
- [XslTransform 的 XmlDocument 输入](xmldocument-input-to-xsltransform.md)
