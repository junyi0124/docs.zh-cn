---
title: XslTransform 的 XPathDocument 输入
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 7d1bbe8b-ed43-4e62-a5ba-d602d244f4ae
ms.openlocfilehash: 29bb345c2654baec8fbd2adce3788a4b4f2d582d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720862"
---
# <a name="xpathdocument-input-to-xsltransform"></a>XslTransform 的 XPathDocument 输入

<xref:System.Xml.XPath.XPathDocument> 是只读缓存，配合 <xref:System.Xml.Xsl.XslTransform> 处理文档。 它在结构上与 XML 文档对象模型 (DOM) 类似，但是已使用 <xref:System.Xml.XPath.XPathNavigator> 上的 XPath 优化功能，针对可扩展样式表语言转换 (XSLT) 处理和 XML 路径语言 (XPath) 数据模型进行了高度优化。  
  
> [!NOTE]
> <xref:System.Xml.Xsl.XslTransform> 类在 .NET Framework 2.0 中已过时。 可以使用 <xref:System.Xml.Xsl.XslCompiledTransform> 类执行可扩展样式表语言转换 (XSLT) 转换。 请参阅[使用 XslCompiledTransform 类](using-the-xslcompiledtransform-class.md)和[从 XslTransform 类迁移](migrating-from-the-xsltransform-class.md)，以获取详细信息。  
  
 下面的代码示例创建 <xref:System.Xml.XPath.XPathDocument> 作为转换的输入。  
  
```vb  
Dim xslt as XslTransform = new XslTransform()  
Xslt.Load(someStylesheet)  
Dim doc as XPathDocument = New XPathDocument("books.xml")  
Dim fs as StringWriter = new StringWriter()  
Xslt.Transform(doc, Nothing, fs, Nothing);  
```  
  
```csharp  
XslTransform xslt = new XslTransform();  
Xslt.Load(someStylesheet);  
XPathDocument doc = XPathDocument("books.xml");  
StringWriter fs = new StringWriter();  
Xslt.Transform(doc, null, fs, null);  
```  
  
## <a name="see-also"></a>请参阅

- [XslTransform 类实现 XSLT 处理器](xsltransform-class-implements-the-xslt-processor.md)
