---
title: 保存和写出文档
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 097b0cb1-5743-4c3a-86ef-caf5cbe6750d
ms.openlocfilehash: 14497bb5b027209c4707eab9bcf1b60f85740dfd
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95697553"
---
# <a name="saving-and-writing-a-document"></a>保存和写出文档

加载并保存 <xref:System.Xml.XmlDocument> 后，保存的文档在下列方面可能不同于原始文档：  
  
- 如果在调用 <xref:System.Xml.XmlDocument.PreserveWhitespace%2A> 方法之间将 `true` 属性设置为 <xref:System.Xml.XmlDocument.Save%2A>，文档中的空白在输出中将保留；如果此属性为 `false`，<xref:System.Xml.XmlDocument> 将使输出自动缩进。  
  
- 各个属性之间的所有空白都缩减为一个空白字符。  
  
- 更改元素间的空白。 保留有效空白，但不保留无效空白。 不过，在文档保存后，它默认使用 <xref:System.Xml.XmlTextWriter> 缩进  模式简洁打印输出，以提升文档的可读性。  
  
- 属性值两边所用的引号字符在默认情况下更改为双引号。 可以使用 <xref:System.Xml.XmlTextReader.QuoteChar%2A> 的 <xref:System.Xml.XmlTextWriter> 属性将引号字符设置为双引号或单引号。  
  
- 默认情况下，扩展像 `{` 这样的数字字符实体。  
  
- 不保留输入文档中的字节顺序标记。 除非显式创建指定不同编码的 XML 声明，否则 UCS-2 保存为 UTF-8。  
  
- 如果要将 <xref:System.Xml.XmlDocument> 写出到文件或流中，则写出的输出与文档内容相同。 也就是说，仅当文档中包含 <xref:System.Xml.XmlDeclaration> 时才写出 ，并且写出文档时所使用的编码与声明节点中给定的编码相同。  
  
## <a name="writing-an-xmldeclaration"></a>写出 XmlDeclaration  

 <xref:System.Xml.XmlDocument>、<xref:System.Xml.XmlDeclaration> 和 <xref:System.Xml.XmlNode.OuterXml%2A> 的 <xref:System.Xml.XmlNode.InnerXml%2A> 和 <xref:System.Xml.XmlNode.WriteTo%2A> 成员与 <xref:System.Xml.XmlDocument> 和 <xref:System.Xml.XmlDocument.Save%2A> 的 <xref:System.Xml.XmlDocument.WriteContentTo%2A> 方法共同创建 XML 声明。  
  
 对于 <xref:System.Xml.XmlDocument> 的 <xref:System.Xml.XmlNode.OuterXml%2A> 属性、<xref:System.Xml.XmlDocument.InnerXml%2A> 以及 <xref:System.Xml.XmlDocument.Save%2A>、<xref:System.Xml.XmlDocument.WriteTo%2A> 和 <xref:System.Xml.XmlDocument.WriteContentTo%2A> 方法，在 XML 声明中写出的编码从 <xref:System.Xml.XmlDeclaration> 节点获取。 如果没有 <xref:System.Xml.XmlDeclaration> 节点，则不会写出 <xref:System.Xml.XmlDeclaration>。如果 <xref:System.Xml.XmlDeclaration> 节点中没有编码，则不会在 XML 声明中写出编码。  
  
 <xref:System.Xml.XmlDocument.Save%2A?displayProperty=nameWithType> 和 <xref:System.Xml.XmlDocument.Save%2A?displayProperty=nameWithType> 方法始终写出 <xref:System.Xml.XmlDeclaration>。 这些方法从其写入的编写器中获取编码。 也就是说，编写器的编码值重写文档和 <xref:System.Xml.XmlDeclaration> 对象的编码。 例如，下列代码不在输出文件 `out.xml` 中的 XML 声明中写出编码。  
  
```vb  
Dim doc As New XmlDocument()  
Dim tw As XmlTextWriter = New XmlTextWriter("out.xml", Nothing)  
doc.Load("text.xml")  
doc.Save(tw)  
```  
  
```csharp  
XmlDocument doc = new XmlDocument();  
XmlTextWriter tw = new XmlTextWriter("out.xml", null);  
doc.Load("text.xml");  
doc.Save(tw);  
```  
  
 对于 <xref:System.Xml.XmlDocument.Save%2A> 方法，XML 声明使用 <xref:System.Xml.XmlWriter.WriteStartDocument%2A> 类中的 <xref:System.Xml.XmlWriter> 方法写出。 因此，重写 <xref:System.Xml.XmlWriter.WriteStartDocument%2A> 方法将更改如何编写文档的开头。  
  
 对于 <xref:System.Xml.XmlNode.OuterXml%2A>、<xref:System.Xml.XmlDeclaration.WriteTo%2A> 和 <xref:System.Xml.XmlNode.InnerXml%2A> 的 <xref:System.Xml.XmlDeclaration> 成员，如果未设置 <xref:System.Xml.XmlDeclaration.Encoding%2A> 属性，则不会写出任何编码。否则，在 XML 声明中写出的编码与 <xref:System.Xml.XmlDeclaration.Encoding%2A> 属性中的编码相同。  
  
## <a name="writing-document-content-using-the-outerxml-property"></a>用 OuterXml 属性写出文档内容  

 <xref:System.Xml.XmlNode.OuterXml%2A> 属性是 Microsoft 对万维网联合会 (W3C) XML 文档对象模型 (DOM) 标准的扩展。 <xref:System.Xml.XmlNode.OuterXml%2A> 属性用于获取整个 XML 文档的标记，或者只获取单个节点及其子节点的标记。 <xref:System.Xml.XmlNode.OuterXml%2A> 返回表示给定节点及其所有子节点的标记。  
  
 下面的代码示例显示了如何将整个文档保存为一个字符串。  
  
```vb  
Dim mydoc As New XmlDocument()  
' Perform application needs here, like mydoc.Load("myfile");  
' Now save the entire document to a string variable called "xml".  
Dim xml As String = mydoc.OuterXml  
```  
  
```csharp  
XmlDocument mydoc = new XmlDocument();  
// Perform application needs here, like mydoc.Load("myfile");  
// Now save the entire document to a string variable called "xml".  
string xml = mydoc.OuterXml;  
```  
  
 下面的代码示例显示了如何只保存文档元素。  
  
```vb  
' For the content of the Document Element only.  
Dim xml As String = mydoc.DocumentElement.OuterXml  
```  
  
```csharp  
// For the content of the Document Element only.  
string xml = mydoc.DocumentElement.OuterXml;  
```  
  
 与此相反，如果您需要子节点的内容，可以使用 <xref:System.Xml.XmlNode.InnerText%2A> 属性。  
  
## <a name="see-also"></a>请参阅

- [XML 文档对象模型 (DOM)](xml-document-object-model-dom.md)
