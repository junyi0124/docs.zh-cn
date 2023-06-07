---
title: 使用 XPathNavigator 移除 XML 数据
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 9f436bca-1b96-494b-a6d2-e102c7551752
ms.openlocfilehash: 265275e3b200ec7bf38df997ace622f2056ad66f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95697618"
---
# <a name="remove-xml-data-using-xpathnavigator"></a>使用 XPathNavigator 移除 XML 数据

<xref:System.Xml.XPath.XPathNavigator> 类提供一组方法用于移除 XML 文档中的节点和值。 要使用这些方法，<xref:System.Xml.XPath.XPathNavigator> 对象必须可编辑，即其 <xref:System.Xml.XPath.XPathNavigator.CanEdit%2A> 属性必须为 `true`。  
  
 可以编辑 XML 文档的 <xref:System.Xml.XPath.XPathNavigator> 对象由 <xref:System.Xml.XmlDocument.CreateNavigator%2A> 类的 <xref:System.Xml.XmlDocument> 方法创建。 由 <xref:System.Xml.XPath.XPathNavigator> 类创建的 <xref:System.Xml.XPath.XPathDocument> 对象是只读的，如果尝试使用由 <xref:System.Xml.XPath.XPathNavigator> 对象创建的 <xref:System.Xml.XPath.XPathDocument> 对象的编辑方法，将引发 <xref:System.NotSupportedException>。  
  
 若要详细了解如何创建可编辑 <xref:System.Xml.XPath.XPathNavigator> 对象，请参阅[使用 XPathDocument 和 XmlDocument 读取 XML 数据](reading-xml-data-using-xpathdocument-and-xmldocument.md)。  
  
## <a name="removing-nodes"></a>移除节点  

 <xref:System.Xml.XPath.XPathNavigator> 类提供 <xref:System.Xml.XPath.XPathNavigator.DeleteSelf%2A> 方法来移除 XML 文档中的节点。  
  
### <a name="removing-a-node"></a>移除节点  

 <xref:System.Xml.XPath.XPathNavigator> 类提供 <xref:System.Xml.XPath.XPathNavigator.DeleteSelf%2A> 方法来删除 XML 文档中 <xref:System.Xml.XPath.XPathNavigator> 对象当前所处的节点。  
  
 使用 <xref:System.Xml.XPath.XPathNavigator.DeleteSelf%2A> 方法删除了某个节点之后，该节点无法再从 <xref:System.Xml.XmlDocument> 对象的根节点访问。 节点删除之后，<xref:System.Xml.XPath.XPathNavigator> 将位于所删除节点的父节点。  
  
 删除操作不会影响位于所删除节点上的任何 <xref:System.Xml.XPath.XPathNavigator> 对象的位置。 这些 <xref:System.Xml.XPath.XPathNavigator> 对象可以在已删除的子树内移动，在这方面看是有效的，但是不能使用 <xref:System.Xml.XPath.XPathNavigator> 类常规的节点集浏览方法移至主节点树。  
  
> [!NOTE]
> <xref:System.Xml.XPath.XPathNavigator.MoveTo%2A> 类的 <xref:System.Xml.XPath.XPathNavigator> 方法可以用于将这些 <xref:System.Xml.XPath.XPathNavigator> 对象移回主节点树，或从主节点树移至已删除的子树。  
  
 在以下示例中，`price` 文件的第一个 `book` 元素的 `contosoBooks.xml` 元素使用 <xref:System.Xml.XPath.XPathNavigator.DeleteSelf%2A> 方法删除。 在 <xref:System.Xml.XPath.XPathNavigator> 元素删除之后，`price` 对象的位置位于父级的 `book` 元素上。  
  
```vb  
Dim document As XmlDocument = New XmlDocument()  
document.Load("contosoBooks.xml")  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
navigator.MoveToChild("bookstore", "http://www.contoso.com/books")  
navigator.MoveToChild("book", "http://www.contoso.com/books")  
navigator.MoveToChild("price", "http://www.contoso.com/books")  
  
navigator.DeleteSelf()  
  
Console.WriteLine("Position after delete: {0}", navigator.Name)  
Console.WriteLine(navigator.OuterXml)  
```  
  
```csharp  
XmlDocument document = new XmlDocument();  
document.Load("contosoBooks.xml");  
XPathNavigator navigator = document.CreateNavigator();  
  
navigator.MoveToChild("bookstore", "http://www.contoso.com/books");  
navigator.MoveToChild("book", "http://www.contoso.com/books");  
navigator.MoveToChild("price", "http://www.contoso.com/books");  
  
navigator.DeleteSelf();  
  
Console.WriteLine("Position after delete: {0}", navigator.Name);  
Console.WriteLine(navigator.OuterXml);  
```  
  
 该示例使用 `contosoBooks.xml` 文件作为输入。  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
### <a name="removing-an-attribute-node"></a>移除属性节点  

 属性节点使用 <xref:System.Xml.XPath.XPathNavigator.DeleteSelf%2A> 方法从 XML 文档中移除。  
  
 属性节点删除之后，无法再从 <xref:System.Xml.XmlDocument> 对象的根节点中访问，<xref:System.Xml.XPath.XPathNavigator> 对象将位于父元素上。  
  
#### <a name="default-attributes"></a>默认属性  

 无论用于移除属性的方法是什么，当移除在 XML 文档的 DTD 或 XML 架构中定义为默认属性的属性时有特殊限制。 除非同时移除了默认属性所属的元素，否则不能移除默认属性。 对于声明了默认属性的元素，默认属性始终存在，因此，删除默认属性将使替换属性插入元素并初始化为所声明的默认值。  
  
## <a name="removing-values"></a>移除值  

 <xref:System.Xml.XPath.XPathNavigator> 类提供 <xref:System.Xml.XPath.XPathNavigator.SetValue%2A> 和 <xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A> 方法来移除 XML 文档中非类型化的和类型化的值。  
  
### <a name="removing-untyped-values"></a>移除非类型化的值  

 <xref:System.Xml.XPath.XPathNavigator.SetValue%2A> 方法只需将作为参数传递的非类型化的 `string` 值作为 <xref:System.Xml.XPath.XPathNavigator> 对象当前所处的节点的值插入。 如果将空字符串传递给 <xref:System.Xml.XPath.XPathNavigator.SetValue%2A> 方法，将移除当前节点的值。  
  
 以下示例使用 `price` 方法移除 `book` 文件中第一个 `contosoBooks.xml` 元素的 <xref:System.Xml.XPath.XPathNavigator.SetValue%2A> 元素的值。  
  
```vb  
Dim document As XmlDocument = New XmlDocument()  
document.Load("contosoBooks.xml")  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
navigator.MoveToChild("bookstore", "http://www.contoso.com/books")  
navigator.MoveToChild("book", "http://www.contoso.com/books")  
navigator.MoveToChild("price", "http://www.contoso.com/books")  
  
navigator.SetValue("")  
  
navigator.MoveToRoot()  
Console.WriteLine(navigator.OuterXml)  
```  
  
```csharp  
XmlDocument document = new XmlDocument();  
document.Load("contosoBooks.xml");  
XPathNavigator navigator = document.CreateNavigator();  
  
navigator.MoveToChild("bookstore", "http://www.contoso.com/books");  
navigator.MoveToChild("book", "http://www.contoso.com/books");  
navigator.MoveToChild("price", "http://www.contoso.com/books");  
  
navigator.SetValue("");  
  
navigator.MoveToRoot();  
Console.WriteLine(navigator.OuterXml);  
```  
  
 该示例使用 `contosoBooks.xml` 文件作为输入。  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
### <a name="removing-typed-values"></a>移除类型化的值  

 如果节点的类型为 W3C XML 架构的简单类型，在设置值之前，将针对简单类型的各个方面检查通过 <xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A> 方法插入的新值。 如果新值不符合节点的类型（例如在类型为 `-1` 的元素上设置值 `xs:positiveInteger`），将引发异常。 <xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A> 方法也不能作为参数传递 `null`。 因此，移除类型化节点的值必须符合节点的架构类型。  
  
 以下示例使用 `price` 方法移除 `book` 文件中第一个 `contosoBooks.xml` 元素的 <xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A> 元素的值，方法是将值设置为 `0`。 节点的值未移除，但是图书的价格已根据 `xs:decimal` 的数据类型移除。  
  
```vb  
Dim settings As XmlReaderSettings = New XmlReaderSettings()  
settings.Schemas.Add("http://www.contoso.com/books", "contosoBooks.xsd")  
settings.ValidationType = ValidationType.Schema  
  
Dim reader As XmlReader = XmlReader.Create("contosoBooks.xml", settings)  
  
Dim document As XmlDocument = New XmlDocument()  
document.Load(reader)  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
navigator.MoveToChild("bookstore", "http://www.contoso.com/books")  
navigator.MoveToChild("book", "http://www.contoso.com/books")  
navigator.MoveToChild("price", "http://www.contoso.com/books")  
  
navigator.SetTypedValue(0)  
  
navigator.MoveToRoot()  
Console.WriteLine(navigator.OuterXml)  
```  
  
```csharp  
XmlReaderSettings settings = new XmlReaderSettings();  
settings.Schemas.Add("http://www.contoso.com/books", "contosoBooks.xsd");  
settings.ValidationType = ValidationType.Schema;  
  
XmlReader reader = XmlReader.Create("contosoBooks.xml", settings);  
  
XmlDocument document = new XmlDocument();  
document.Load(reader);  
XPathNavigator navigator = document.CreateNavigator();  
  
navigator.MoveToChild("bookstore", "http://www.contoso.com/books");  
navigator.MoveToChild("book", "http://www.contoso.com/books");  
navigator.MoveToChild("price", "http://www.contoso.com/books");  
  
navigator.SetTypedValue(0);  
  
navigator.MoveToRoot();  
Console.WriteLine(navigator.OuterXml);  
```  
  
## <a name="namespace-nodes"></a>命名空间节点  

 命名空间节点不能从 <xref:System.Xml.XmlDocument> 对象中删除。 如果尝试使用 <xref:System.Xml.XPath.XPathNavigator.DeleteSelf%2A> 方法删除命名空间节点，将引发异常。  
  
## <a name="the-innerxml-and-outerxml-properties"></a>InnerXml 和 OuterXml 属性  

 <xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> 类的 <xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> 和 <xref:System.Xml.XPath.XPathNavigator> 属性更改 <xref:System.Xml.XPath.XPathNavigator> 对象当前所处的节点的 XML 标记。  
  
 <xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> 属性更改 <xref:System.Xml.XPath.XPathNavigator> 对象当前所处的子节点以及给定 XML `string` 的已分析内容的 XML 标记。 同样，<xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> 属性更改 <xref:System.Xml.XPath.XPathNavigator> 对象当前所处的子节点以及当前节点本身的 XML 标记。  
  
 除了本主题中所述的方法之外，<xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> 和 <xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> 属性也可以用于移除 XML 文档中的节点和值。 若要详细了解如何使用 <xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> 和 <xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> 属性修改节点，请参阅[使用 XPathNavigator 修改 XML 数据](modify-xml-data-using-xpathnavigator.md)主题。  
  
## <a name="saving-an-xml-document"></a>保存 XML 文档  

 使用 <xref:System.Xml.XmlDocument> 类的方法保存本主题中所述的方法对 <xref:System.Xml.XmlDocument> 对象的更改。 若要详细了解如何保存对 <xref:System.Xml.XmlDocument> 对象所做的更改，请参阅[保存和编写文档](saving-and-writing-a-document.md)。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [使用 XPath 数据模型处理 XML 数据](process-xml-data-using-the-xpath-data-model.md)
- [使用 XPathNavigator 插入 XML 数据](insert-xml-data-using-xpathnavigator.md)
- [使用 XPathNavigator 修改 XML 数据](modify-xml-data-using-xpathnavigator.md)
