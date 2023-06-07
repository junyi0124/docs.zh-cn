---
title: XslCompiledTransform 类的输入
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 834049f1-ab41-449e-9f10-4a1d0701bc48
ms.openlocfilehash: af529f1c6ccfe3abe761c7707772d6f9697c179d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95733420"
---
# <a name="inputs-to-the-xslcompiledtransform-class"></a>XslCompiledTransform 类的输入

<xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> 方法接受三种输入类型的源文档：实现 <xref:System.Xml.XPath.IXPathNavigable> 接口的对象、读取源文档的 <xref:System.Xml.XmlReader> 对象或字符串 URI。  
  
> [!NOTE]
> 默认情况下，<xref:System.Xml.Xsl.XslCompiledTransform> 类保留空白。 这符合 [W3C XSLT 1.0 第 3.4 节的建议](https://www.w3.org/TR/xslt.html#strip)。  
  
## <a name="ixpathnavigable-interface"></a>IXPathNavigable 接口  

 <xref:System.Xml.XPath.IXPathNavigable> 接口在 <xref:System.Xml.XmlNode> 和 <xref:System.Xml.XPath.XPathDocument> 类中实现。 这两个类表示 XML 数据的内存中缓存。  
  
- <xref:System.Xml.XmlNode> 类基于 W3C 文档对象模型 (DOM) 并具有编辑功能。  
  
- <xref:System.Xml.XPath.XPathDocument> 类是基于 XPath 数据模型的只读数据存储。 <xref:System.Xml.XPath.XPathDocument> 是 XSLT 处理建议使用的类。 与 <xref:System.Xml.XmlNode> 类相比，此类的性能更强。  
  
> [!NOTE]
> 转换将应用于整个文档。 换句话说，如果你传入文档根节点以外的一个节点，并不能防止转换进程访问已加载文档的所有节点。 若要转换节点片段，必须创建一个仅包含节点片段的对象，并将该对象传递给 <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> 方法。 有关详细信息，请参阅[如何：转换节点片段](how-to-transform-a-node-fragment.md)。  
  
 下面的示例使用 <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A?displayProperty=nameWithType> 方法，借助 transform.xsl 样式表将 books.xml 文件转换为 books.html 文件。 books.xml 和 transform.xsl 文件可在以下主题中找到：[如何：通过使用程序集执行 XSLT 转换](how-to-perform-an-xslt-transformation-by-using-an-assembly.md)。  
  
 [!code-csharp[XslCompiledTransform.Transform2#1](../../../../samples/snippets/csharp/VS_Snippets_Data/XslCompiledTransform.Transform2/CS/Program.cs#1)]
 [!code-vb[XslCompiledTransform.Transform2#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XslCompiledTransform.Transform2/VB/Module1.vb#1)]  
  
## <a name="xmlreader-object"></a>XmlReader 对象  

 <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> 方法从 <xref:System.Xml.XmlReader> 的当前节点及其所有子节点加载。 这样，可以使用文档的一部分作为上下文文档使用。 <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> 方法返回后，<xref:System.Xml.XmlReader> 将位于上下文文档结尾之后的下一个节点上。 如果已到达文档结尾，<xref:System.Xml.XmlReader> 将位于文件结尾 (EOF)。  
  
 下面的示例使用 <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A?displayProperty=nameWithType> 方法，借助 transform.xsl 样式表将 books.xml 文件转换为 books.html 文件。 books.xml 和 transform.xsl 文件可在以下主题中找到：[如何：通过使用程序集执行 XSLT 转换](how-to-perform-an-xslt-transformation-by-using-an-assembly.md)。  
  
 [!code-csharp[XslCompiledTransform.Transform2#2](../../../../samples/snippets/csharp/VS_Snippets_Data/XslCompiledTransform.Transform2/CS/Program.cs#2)]
 [!code-vb[XslCompiledTransform.Transform2#2](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XslCompiledTransform.Transform2/VB/Module1.vb#2)]  
  
## <a name="string-uri"></a>字符串 URI  

 还可以将源文档 URI 指定为 XSLT 输入。 <xref:System.Xml.XmlResolver> 用于解析 URI。 可以指定要使用的 <xref:System.Xml.XmlResolver>，方法是将其传递给 <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> 方法。 如果未指定 <xref:System.Xml.XmlResolver>，<xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> 方法将使用没有凭据的默认 <xref:System.Xml.XmlUrlResolver>。  
  
 下面的示例使用 <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A?displayProperty=nameWithType> 方法，借助 transform.xsl 样式表将 books.xml 文件转换为 books.html 文件。 books.xml 和 transform.xsl 文件可在以下主题中找到：[如何：通过使用程序集执行 XSLT 转换](how-to-perform-an-xslt-transformation-by-using-an-assembly.md)。  
  
 [!code-csharp[XslCompiledTransform.Transform2#3](../../../../samples/snippets/csharp/VS_Snippets_Data/XslCompiledTransform.Transform2/CS/Program.cs#3)]
 [!code-vb[XslCompiledTransform.Transform2#3](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XslCompiledTransform.Transform2/VB/Module1.vb#3)]  
  
 有关详细信息，请参阅[在 XSLT 处理期间解析外部资源](resolving-external-resources-during-xslt-processing.md)。  
  
## <a name="see-also"></a>请参阅

- [XSLT 转换](xslt-transformations.md)
