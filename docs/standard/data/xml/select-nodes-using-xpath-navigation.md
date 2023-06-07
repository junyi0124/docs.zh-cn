---
title: 使用 XPath 导航选择节点
description: 了解如何在 .NET 中选择 XML 节点。 如果使用文档对象模型 (DOM) 方法，则可使用 XML 路径语言 (XPath) 浏览功能查询 DOM 信息。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 8e4450dc-56b3-472b-b467-32f5694f83ad
ms.openlocfilehash: b6b68d9351431acc6d9ef20276f51ac4d667d325
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734681"
---
# <a name="select-nodes-using-xpath-navigation"></a>使用 XPath 导航选择节点

XML 文档对象模型 (DOM) 包含的方法使您可以使用 XML 路径语言 (XPath) 浏览功能查询 DOM 中的信息。 可以使用 XPath 查找单个特定节点，或查找与某个条件匹配的所有节点。  
  
## <a name="xpath-select-methods"></a>XPath 选择方法  

 DOM 类提供两种 XPath 选择方法：<xref:System.Xml.XmlNode.SelectSingleNode%2A> 方法和 <xref:System.Xml.XmlNode.SelectNodes%2A> 方法。 <xref:System.Xml.XmlNode.SelectSingleNode%2A> 方法返回符合选择条件的第一个节点。 <xref:System.Xml.XmlNode.SelectNodes%2A> 方法返回包含匹配节点的 <xref:System.Xml.XmlNodeList>。  
  
 下面的示例使用 <xref:System.Xml.XmlNode.SelectSingleNode%2A> 方法来选择其作者姓氏符合指定条件的第一个 `book` 节点。 bookstore.xml 文件（在本主题末尾提供）用作输入文件。  
  
```vb  
Dim doc As New XmlDocument()  
doc.Load("bookstore.xml")  
Dim root As XmlNode = doc.DocumentElement  
  
' Add the namespace.  
Dim nsmgr As New XmlNamespaceManager(doc.NameTable)  
nsmgr.AddNamespace("bk", "urn:newbooks-schema")  
  
' Select and display the first node in which the author's
' last name is Kingsolver.  
Dim node As XmlNode = root.SelectSingleNode( _  
     "descendant::bk:book[bk:author/bk:last-name='Kingsolver']", nsmgr)  
Console.WriteLine(node.InnerXml)  
```  
  
```csharp  
// Load the document and set the root element.  
XmlDocument doc = new XmlDocument();  
doc.Load("bookstore.xml");  
XmlNode root = doc.DocumentElement;  
  
// Add the namespace.  
XmlNamespaceManager nsmgr = new XmlNamespaceManager(doc.NameTable);  
nsmgr.AddNamespace("bk", "urn:newbooks-schema");  
  
// Select and display the first node in which the author's
// last name is Kingsolver.  
XmlNode node = root.SelectSingleNode(  
    "descendant::bk:book[bk:author/bk:last-name='Kingsolver']", nsmgr);  
Console.WriteLine(node.InnerXml);  
```  
  
 下一个示例使用 <xref:System.Xml.XmlNode.SelectNodes%2A> 方法来选择其价格大于指定金额的所有书节点。 然后，以编程方式将选定列表中的每本书的价格减去 10%。 最后，将更新的文件写入控制台。 bookstore.xml 文件（在本主题末尾提供）用作输入文件。  
  
```vb  
' Load the document and set the root element.  
Dim doc As New XmlDocument()  
doc.Load("bookstore.xml")  
Dim root As XmlNode = doc.DocumentElement  
  
' Add the namespace.  
Dim nsmgr As New XmlNamespaceManager(doc.NameTable)  
nsmgr.AddNamespace("bk", "urn:newbooks-schema")  
  
' Select all nodes where the book price is greater than 10.00.  
Dim nodeList As XmlNodeList = root.SelectNodes( _  
     "descendant::bk:book[bk:price>10.00]", nsmgr)  
For Each book As XmlNode In nodeList  
     Dim price As Double  
     price = Math.Round(Convert.ToSingle( _  
          book.LastChild.InnerText) * 0.9, 2)  
     book.LastChild.InnerText = price.ToString()  
Next  
  
' Display the updated document.  
doc.Save(Console.Out)  
```  
  
```csharp  
// Load the document and set the root element.  
XmlDocument doc = new XmlDocument();  
doc.Load("bookstore.xml");  
XmlNode root = doc.DocumentElement;  
  
// Add the namespace.  
XmlNamespaceManager nsmgr = new XmlNamespaceManager(doc.NameTable);  
nsmgr.AddNamespace("bk", "urn:newbooks-schema");  
  
// Select all nodes where the book price is greater than 10.00.  
XmlNodeList nodeList = root.SelectNodes(  
     "descendant::bk:book[bk:price>10.00]", nsmgr);  
foreach (XmlNode book in nodeList)  
{  
     // Discount prices by 10%.  
     double price;  
     price = Math.Round(Convert.ToSingle(  
          book.LastChild.InnerText) * 0.9, 2);  
     book.LastChild.InnerText = price.ToString();  
}  
  
// Display the updated document.  
doc.Save(Console.Out);  
```  
  
 上面的示例从文档元素开始执行 XPath 查询。 设置 XPath 查询的起始点即设置了上下文节点，该节点是 XPath 查询的起始点。 如果不希望在文档元素处开始，而是希望从文档元素的第一个子级开始，可以编写 Select 语句的代码，如下所示：  
  
```vb  
doc.DocumentElement.FirstChild.SelectNodes(. . . )  
```  
  
```csharp  
this doc.DocumentElement.FirstChild.SelectNodes(. . .);  
```  
  
 所有 <xref:System.Xml.XmlNodeList> 对象都与基础文档同步。 因此，如果循环访问节点列表并修改某个节点的值，则该节点在包含它的文档中也被更新。 请注意，在上一个示例中，在选定的 <xref:System.Xml.XmlNodeList> 中修改节点时，也会修改基础文档。  
  
> [!NOTE]
> 修改基础文档时，最好重新运行此 Select 语句。 如果修改的节点可能导致该节点被添加到节点列表（当先前没有添加它时），或者现在会导致它从节点列表中被移除，则无法保证节点列表现在是精确的。  
  
## <a name="namespaces-in-xpath-expressions"></a>XPath 表达式中的命名空间  

 XPath 表达式可以包含命名空间。 使用 <xref:System.Xml.XmlNamespaceManager> 支持命名空间解析。 如果 XPath 表达式包含前缀，前缀和命名空间 URI 对必须添加到 <xref:System.Xml.XmlNamespaceManager>，并且 <xref:System.Xml.XmlNamespaceManager> 传递给 <xref:System.Xml.XmlNode.SelectNodes%28System.String%2CSystem.Xml.XmlNamespaceManager%29> 或 <xref:System.Xml.XmlNode.SelectSingleNode%28System.String%2CSystem.Xml.XmlNamespaceManager%29> 方法。 请注意，上面的代码示例使用 <xref:System.Xml.XmlNamespaceManager> 来解析 bookstore.xml 文档的命名空间。  
  
> [!NOTE]
> 如果 XPath 表达式不包含前缀，则假定命名空间统一资源标识符 (URI) 是空的命名空间。 如果 XML 包含默认命名空间，仍必须将前缀和命名空间 URI 添加到 <xref:System.Xml.XmlNamespaceManager>；否则，不会选择任何节点。  
  
#### <a name="input-file"></a>输入文件  

 下面的 bookstore.xml 文件在本主题的示例中用作输入文件。  
  
```xml  
<?xml version='1.0'?>  
<bookstore xmlns="urn:newbooks-schema">  
  <book genre="novel" style="hardcover">  
    <title>The Handmaid's Tale</title>  
    <author>  
      <first-name>Margaret</first-name>  
      <last-name>Atwood</last-name>  
    </author>  
    <price>19.95</price>  
  </book>  
  <book genre="novel" style="other">  
    <title>The Poisonwood Bible</title>  
    <author>  
      <first-name>Barbara</first-name>  
      <last-name>Kingsolver</last-name>  
    </author>  
    <price>11.99</price>  
  </book>  
  <book genre="novel" style="paperback">  
    <title>The Bean Trees</title>  
    <author>  
      <first-name>Barbara</first-name>  
      <last-name>Kingsolver</last-name>  
    </author>  
    <price>5.99</price>  
  </book>  
</bookstore>  
```  
  
## <a name="see-also"></a>请参阅

- [XML 文档对象模型 (DOM)](xml-document-object-model-dom.md)
