---
title: 使用 XPathNavigator 插入 XML 数据
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
ms.assetid: 2ed8c28b-b88d-4be7-9c87-92df01f0821f
ms.openlocfilehash: 50e5b363a35eb3f11d7eb26bb34c53910a59201b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95733407"
---
# <a name="insert-xml-data-using-xpathnavigator"></a>使用 XPathNavigator 插入 XML 数据

<xref:System.Xml.XPath.XPathNavigator> 类提供一组方法用于在 XML 文档中插入同级节点、子节点和属性节点。 要使用这些方法，<xref:System.Xml.XPath.XPathNavigator> 对象必须可编辑，即其 <xref:System.Xml.XPath.XPathNavigator.CanEdit%2A> 属性必须为 `true`。  
  
 可以编辑 XML 文档的 <xref:System.Xml.XPath.XPathNavigator> 对象由 <xref:System.Xml.XmlDocument.CreateNavigator%2A> 类的 <xref:System.Xml.XmlDocument> 方法创建。 由 <xref:System.Xml.XPath.XPathNavigator> 类创建的 <xref:System.Xml.XPath.XPathDocument> 对象是只读的，如果尝试使用由 <xref:System.Xml.XPath.XPathNavigator> 对象创建的 <xref:System.Xml.XPath.XPathDocument> 对象的编辑方法，将引发 <xref:System.NotSupportedException>。  
  
 若要详细了解如何创建可编辑 <xref:System.Xml.XPath.XPathNavigator> 对象，请参阅[使用 XPathDocument 和 XmlDocument 读取 XML 数据](reading-xml-data-using-xpathdocument-and-xmldocument.md)。  
  
## <a name="inserting-nodes"></a>插入节点  

 <xref:System.Xml.XPath.XPathNavigator> 类提供在 XML 文档中插入同级节点、子节点和属性节点的方法。 通过这些方法可以在与 <xref:System.Xml.XPath.XPathNavigator> 对象的当前位置有关的不同位置插入节点和属性，如下面各节所述。  
  
### <a name="inserting-sibling-nodes"></a>插入同级节点  

 <xref:System.Xml.XPath.XPathNavigator> 类提供下列方法来插入同辈节点。  
  
- <xref:System.Xml.XPath.XPathNavigator.InsertAfter%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.InsertBefore%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.InsertElementAfter%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.InsertElementBefore%2A>  
  
 这些方法在 <xref:System.Xml.XPath.XPathNavigator> 对象当前所处的节点之前和之后插入同辈节点。  
  
 <xref:System.Xml.XPath.XPathNavigator.InsertAfter%2A> 和 <xref:System.Xml.XPath.XPathNavigator.InsertBefore%2A> 方法是重载方法，接受 `string`、<xref:System.Xml.XmlReader> 对象或包含要作为参数添加的同级节点的 <xref:System.Xml.XPath.XPathNavigator> 对象。 这两种方法还会返回用于插入同级节点的 <xref:System.Xml.XmlWriter> 对象。  
  
 <xref:System.Xml.XPath.XPathNavigator.InsertElementAfter%2A> 和 <xref:System.Xml.XPath.XPathNavigator.InsertElementBefore%2A> 方法使用命名空间前缀、本地名称、命名空间 URI 以及作为参数指定的值在 <xref:System.Xml.XPath.XPathNavigator> 对象当前所处的节点之前和之后插入单个同级节点。  
  
 在以下示例中，在 `pages` 文件中第一个 `price` 元素的 `book` 子元素之前插入新的 `contosoBooks.xml` 元素。  
  
 [!code-cpp[XPathNavigatorMethods#19](../../../../samples/snippets/cpp/VS_Snippets_Data/XPathNavigatorMethods/CPP/xpathnavigatormethods.cpp#19)]
 [!code-csharp[XPathNavigatorMethods#19](../../../../samples/snippets/csharp/VS_Snippets_Data/XPathNavigatorMethods/CS/xpathnavigatormethods.cs#19)]
 [!code-vb[XPathNavigatorMethods#19](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XPathNavigatorMethods/VB/xpathnavigatormethods.vb#19)]  
  
 该示例使用 `contosoBooks.xml` 文件作为输入。  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
 有关 <xref:System.Xml.XPath.XPathNavigator.InsertAfter%2A>、<xref:System.Xml.XPath.XPathNavigator.InsertBefore%2A>、<xref:System.Xml.XPath.XPathNavigator.InsertElementAfter%2A> 和 <xref:System.Xml.XPath.XPathNavigator.InsertElementBefore%2A> 方法的更多信息，请参见 <xref:System.Xml.XPath.XPathNavigator> 类参考文档。  
  
### <a name="inserting-child-nodes"></a>插入子节点  

 <xref:System.Xml.XPath.XPathNavigator> 类提供下列方法来插入子节点。  
  
- <xref:System.Xml.XPath.XPathNavigator.AppendChild%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.PrependChild%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.AppendChildElement%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.PrependChildElement%2A>  
  
 这些方法在 <xref:System.Xml.XPath.XPathNavigator> 对象当前所处的节点的子节点列表的结尾和开头添加子节点。  
  
 与“插入同级节点”一节中的方法类似，<xref:System.Xml.XPath.XPathNavigator.AppendChild%2A> 和 <xref:System.Xml.XPath.XPathNavigator.PrependChild%2A> 方法接受 `string`、<xref:System.Xml.XmlReader> 对象或包含要作为参数添加的子节点的 <xref:System.Xml.XPath.XPathNavigator> 对象。 这两种方法还会返回用于插入子节点的 <xref:System.Xml.XmlWriter> 对象。  
  
 <xref:System.Xml.XPath.XPathNavigator.AppendChildElement%2A> 和 <xref:System.Xml.XPath.XPathNavigator.PrependChildElement%2A> 方法也与“插入同级节点”一节中的方法类似，使用命名空间前缀、本地名称、命名空间 URI 以及作为参数指定的值在 <xref:System.Xml.XPath.XPathNavigator> 对象当前所处的节点的子节点列表的结尾和开头插入单个子节点。  
  
 在以下示例中，在 `pages` 文件中第一个 `book` 元素的子元素列表上追加新的 `contosoBooks.xml` 子元素。  
  
 [!code-cpp[XPathNavigatorMethods#2](../../../../samples/snippets/cpp/VS_Snippets_Data/XPathNavigatorMethods/CPP/xpathnavigatormethods.cpp#2)]
 [!code-csharp[XPathNavigatorMethods#2](../../../../samples/snippets/csharp/VS_Snippets_Data/XPathNavigatorMethods/CS/xpathnavigatormethods.cs#2)]
 [!code-vb[XPathNavigatorMethods#2](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XPathNavigatorMethods/VB/xpathnavigatormethods.vb#2)]  
  
 该示例使用 `contosoBooks.xml` 文件作为输入。  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
 有关 <xref:System.Xml.XPath.XPathNavigator.AppendChild%2A>、<xref:System.Xml.XPath.XPathNavigator.PrependChild%2A>、<xref:System.Xml.XPath.XPathNavigator.AppendChildElement%2A> 和 <xref:System.Xml.XPath.XPathNavigator.PrependChildElement%2A> 方法的更多信息，请参见 <xref:System.Xml.XPath.XPathNavigator> 类参考文档。  
  
### <a name="inserting-attribute-nodes"></a>插入属性节点  

 <xref:System.Xml.XPath.XPathNavigator> 类提供下列方法来插入属性节点。  
  
- <xref:System.Xml.XPath.XPathNavigator.CreateAttribute%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.CreateAttributes%2A>  
  
 这些方法在 <xref:System.Xml.XPath.XPathNavigator> 对象当前所处的元素节点上插入属性节点。 <xref:System.Xml.XPath.XPathNavigator.CreateAttribute%2A> 方法使用命名空间前缀、本地名称、命名空间 URI 以及作为参数指定的值在 <xref:System.Xml.XPath.XPathNavigator> 对象当前所处的元素节点上创建属性节点。 <xref:System.Xml.XPath.XPathNavigator.CreateAttributes%2A> 方法返回用于插入属性节点的 <xref:System.Xml.XmlWriter> 对象。  
  
 在以下示例中，使用从 `discount` 方法返回的 `currency` 对象在 `price` 文件中第一个 `book` 元素的 `contosoBooks.xml` 子元素上创建新的 <xref:System.Xml.XmlWriter> 和 <xref:System.Xml.XPath.XPathNavigator.CreateAttributes%2A> 属性。  
  
 [!code-cpp[XPathNavigatorMethods#8](../../../../samples/snippets/cpp/VS_Snippets_Data/XPathNavigatorMethods/CPP/xpathnavigatormethods.cpp#8)]
 [!code-csharp[XPathNavigatorMethods#8](../../../../samples/snippets/csharp/VS_Snippets_Data/XPathNavigatorMethods/CS/xpathnavigatormethods.cs#8)]
 [!code-vb[XPathNavigatorMethods#8](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XPathNavigatorMethods/VB/xpathnavigatormethods.vb#8)]  
  
 该示例使用 `contosoBooks.xml` 文件作为输入。  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
 有关 <xref:System.Xml.XPath.XPathNavigator.CreateAttribute%2A> 和 <xref:System.Xml.XPath.XPathNavigator.CreateAttributes%2A> 方法的更多信息，请参见 <xref:System.Xml.XPath.XPathNavigator> 类参考文档。  
  
## <a name="copying-nodes"></a>复制节点  

 在某些情况下，可能需要使用另一个 XML 文档中的内容填充 XML 文档。 <xref:System.Xml.XPath.XPathNavigator> 类和 <xref:System.Xml.XmlWriter> 类均可以将节点从现有的 <xref:System.Xml.XmlDocument> 对象或 <xref:System.Xml.XmlReader> 对象复制到 <xref:System.Xml.XPath.XPathNavigator> 对象。  
  
 <xref:System.Xml.XPath.XPathNavigator.AppendChild%2A> 类的 <xref:System.Xml.XPath.XPathNavigator.PrependChild%2A>、<xref:System.Xml.XPath.XPathNavigator.InsertBefore%2A>、<xref:System.Xml.XPath.XPathNavigator.InsertAfter%2A> 和 <xref:System.Xml.XPath.XPathNavigator> 方法全部具有重载，可以接受 <xref:System.Xml.XPath.XPathNavigator> 对象或 <xref:System.Xml.XmlReader> 对象作为参数。  
  
 <xref:System.Xml.XmlWriter.WriteNode%2A> 类的 <xref:System.Xml.XmlWriter> 方法具有重载，可以接受 <xref:System.Xml.XmlNode>、<xref:System.Xml.XmlReader> 或 <xref:System.Xml.XPath.XPathNavigator> 对象。  
  
 以下示例将所有 `book` 元素从一个文档复制到另一个文档。  
  
```vb  
Dim document As XmlDocument = New XmlDocument()  
document.Load("books.xml")  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
navigator.MoveToChild("bookstore", String.Empty)  
  
Dim newBooks As XPathDocument = New XPathDocument("newBooks.xml")  
Dim newBooksNavigator As XPathNavigator = newBooks.CreateNavigator()  
  
Dim nav As XPathNavigator  
For Each nav in newBooksNavigator.SelectDescendants("book", "", false)  
    navigator.AppendChild(nav)  
Next  
  
document.Save("newBooks.xml");  
```  
  
```csharp  
XmlDocument document = new XmlDocument();  
document.Load("books.xml");  
XPathNavigator navigator = document.CreateNavigator();  
  
navigator.MoveToChild("bookstore", String.Empty);  
  
XPathDocument newBooks = new XPathDocument("newBooks.xml");  
XPathNavigator newBooksNavigator = newBooks.CreateNavigator();  
  
foreach (XPathNavigator nav in newBooksNavigator.SelectDescendants("book", "", false))  
{  
    navigator.AppendChild(nav);  
}  
  
document.Save("newBooks.xml");  
```  
  
## <a name="inserting-values"></a>插入值  

 <xref:System.Xml.XPath.XPathNavigator> 类提供 <xref:System.Xml.XPath.XPathNavigator.SetValue%2A> 和 <xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A> 方法来将节点的值插入 <xref:System.Xml.XmlDocument> 对象。  
  
### <a name="inserting-untyped-values"></a>插入非类型化的值  

 <xref:System.Xml.XPath.XPathNavigator.SetValue%2A> 方法只需将作为参数传递的非类型化的 `string` 值作为 <xref:System.Xml.XPath.XPathNavigator> 对象当前所处的节点的值插入。 插入的值没有任何类型或不验证新值是否符合节点的类型（如果架构信息可用）。  
  
 在以下示例中，<xref:System.Xml.XPath.XPathNavigator.SetValue%2A> 方法用于更新 `price` 文件中的所有 `contosoBooks.xml` 元素。  
  
 [!code-cpp[XPathNavigatorMethods#47](../../../../samples/snippets/cpp/VS_Snippets_Data/XPathNavigatorMethods/CPP/xpathnavigatormethods.cpp#47)]
 [!code-csharp[XPathNavigatorMethods#47](../../../../samples/snippets/csharp/VS_Snippets_Data/XPathNavigatorMethods/CS/xpathnavigatormethods.cs#47)]
 [!code-vb[XPathNavigatorMethods#47](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XPathNavigatorMethods/VB/xpathnavigatormethods.vb#47)]  
  
 该示例使用 `contosoBooks.xml` 文件作为输入。  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
### <a name="inserting-typed-values"></a>插入类型化的值  

 如果节点的类型为 W3C XML 架构的简单类型，在设置值之前，将针对简单类型的各个方面检查通过 <xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A> 方法插入的新值。 如果新值不符合节点的类型（例如在类型为 `-1` 的元素上设置值 `xs:positiveInteger`），将引发异常。  
  
 以下示例尝试将 `price` 文件中第一个 `book` 元素的 `contosoBooks.xml` 元素的值更改为 <xref:System.DateTime> 值。 因为在 `price` 文件中将 `xs:decimal` 元素的 XML 架构类型定义为 `contosoBooks.xsd`，所以，这样做将引发异常。  
  
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
  
navigator.SetTypedValue(DateTime.Now)  
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
  
navigator.SetTypedValue(DateTime.Now);  
```  
  
 该示例使用 `contosoBooks.xml` 文件作为输入。  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
 该示例还使用 `contosoBooks.xsd` 作为输入。  
  
 [!code-xml[XPathXMLExamples#3](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xsd#3)]  
  
## <a name="the-innerxml-and-outerxml-properties"></a>InnerXml 和 OuterXml 属性  

 <xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> 类的 <xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> 和 <xref:System.Xml.XPath.XPathNavigator> 属性更改 <xref:System.Xml.XPath.XPathNavigator> 对象当前所处的节点的 XML 标记。  
  
 <xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> 属性更改 <xref:System.Xml.XPath.XPathNavigator> 对象当前所处的子节点以及给定 XML `string` 的已分析内容的 XML 标记。 同样，<xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> 属性更改 <xref:System.Xml.XPath.XPathNavigator> 对象当前所处的子节点以及当前节点本身的 XML 标记。  
  
 除了本主题中所述的方法之外，<xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> 和 <xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> 属性也可以用于在 XML 文档中插入节点和值。 若要详细了解如何使用 <xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> 和 <xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> 属性插入节点和值，请参阅[使用 XPathNavigator 修改 XML 数据](modify-xml-data-using-xpathnavigator.md)主题。  
  
## <a name="namespace-and-xmllang-conflicts"></a>命名空间和 xml:lang 冲突  

 在使用 `xml:lang` 类的 <xref:System.Xml.XPath.XPathNavigator.InsertBefore%2A>、<xref:System.Xml.XPath.XPathNavigator.InsertAfter%2A>、<xref:System.Xml.XPath.XPathNavigator.AppendChild%2A> 和 <xref:System.Xml.XPath.XPathNavigator.PrependChild%2A> 方法（这些方法使用 <xref:System.Xml.XPath.XPathNavigator> 对象作为参数）插入 XML 数据时，可能会发生某些与命名空间和 <xref:System.Xml.XmlReader> 声明的范围有关的冲突，。  
  
 以下是可能发生的命名空间冲突。  
  
- 如果在 <xref:System.Xml.XmlReader> 对象的上下文范围内存在命名空间，其中命名空间 URI 映射的前缀不在 <xref:System.Xml.XPath.XPathNavigator> 对象的上下文中，新的命名空间声明将添加到新插入的节点。  
  
- 如果 <xref:System.Xml.XmlReader> 对象的上下文和 <xref:System.Xml.XPath.XPathNavigator> 对象的上下文范围内存在相同的命名空间 URI，但是两个上下文中映射到该命名空间 URI 的前缀不同，新的命名空间声明将添加到新插入的节点，前缀和命名空间 URI 从 <xref:System.Xml.XmlReader> 对象获取。  
  
- 如果 <xref:System.Xml.XmlReader> 对象的上下文和 <xref:System.Xml.XPath.XPathNavigator> 对象的上下文范围内存在相同的命名空间前缀，但是两个上下文中映射到该命名空间前缀的命名空间 URI 不同，新的命名空间声明将添加到新插入的节点，该声明使用从 <xref:System.Xml.XmlReader> 对象获取的命名空间 URI 重新声明该前缀。  
  
- 如果 <xref:System.Xml.XmlReader> 对象的上下文和 <xref:System.Xml.XPath.XPathNavigator> 对象的上下文中的前缀以及命名空间 URI 相同，则不会向新插入的节点添加新的命名空间声明。  
  
> [!NOTE]
> 上面的说明同样适用于使用空 `string` 作为前缀的命名空间声明（例如默认的命名空间声明）。  
  
 以下是可能发生的 `xml:lang` 冲突。  
  
- 如果 `xml:lang` 对象的上下文范围内存在 <xref:System.Xml.XmlReader> 属性，但是 <xref:System.Xml.XPath.XPathNavigator> 对象的上下文范围内没有，从 `xml:lang` 对象获取值的 <xref:System.Xml.XmlReader> 属性将添加到新插入的节点。  
  
- 如果 `xml:lang` 对象的上下文和 <xref:System.Xml.XmlReader> 对象的上下文范围内均存在 <xref:System.Xml.XPath.XPathNavigator> 属性，但是每个属性的值不同，从 `xml:lang` 对象获取值的 <xref:System.Xml.XmlReader> 属性将添加到新插入的节点。  
  
- 如果 `xml:lang` 对象的上下文和 <xref:System.Xml.XmlReader> 对象的上下文范围内均存在 <xref:System.Xml.XPath.XPathNavigator> 属性，但是每个属性的值相同，则不会向新插入的节点添加新的 `xml:lang` 属性。  
  
- 如果 `xml:lang` 对象的上下文范围内存在 <xref:System.Xml.XPath.XPathNavigator> 属性，但是 <xref:System.Xml.XmlReader> 对象的上下文范围内不存在，则不会向新插入的节点添加 `xml:lang` 属性。  
  
## <a name="inserting-nodes-with-xmlwriter"></a>使用 XmlWriter 插入节点  

 “插入节点和值”一节中所述的用于插入同级节点、子节点和属性节点的方法均是重载方法。 <xref:System.Xml.XPath.XPathNavigator.InsertAfter%2A> 类的 <xref:System.Xml.XPath.XPathNavigator.InsertBefore%2A>、<xref:System.Xml.XPath.XPathNavigator.AppendChild%2A>、<xref:System.Xml.XPath.XPathNavigator.PrependChild%2A>、<xref:System.Xml.XPath.XPathNavigator.CreateAttributes%2A> 和 <xref:System.Xml.XPath.XPathNavigator> 方法返回用于插入节点的 <xref:System.Xml.XmlWriter> 对象。  
  
### <a name="unsupported-xmlwriter-methods"></a>不支持的 XmlWriter 方法  

 因为 XPath 数据模型与文档对象模型 (DOM) 之间存在差异，所以，<xref:System.Xml.XmlWriter> 类并非支持所有用于使用 <xref:System.Xml.XPath.XPathNavigator> 类向 XML 文档写入信息的方法。  
  
 下表说明 <xref:System.Xml.XmlWriter> 类不支持的 <xref:System.Xml.XPath.XPathNavigator> 类方法。  
  
|方法|描述|  
|------------|-----------------|  
|<xref:System.Xml.XmlWriter.WriteEntityRef%2A>|引发 <xref:System.NotSupportedException> 异常。|  
|<xref:System.Xml.XmlWriter.WriteDocType%2A>|在根级别忽略，如果在 XML 文档中的任何其他级别调用，将引发 <xref:System.NotSupportedException> 异常。|  
|<xref:System.Xml.XmlWriter.WriteCData%2A>|看作对等效字符调用 <xref:System.Xml.XmlWriter.WriteString%2A> 方法。|  
|<xref:System.Xml.XmlWriter.WriteCharEntity%2A>|看作对等效字符调用 <xref:System.Xml.XmlWriter.WriteString%2A> 方法。|  
|<xref:System.Xml.XmlWriter.WriteSurrogateCharEntity%2A>|看作对等效字符调用 <xref:System.Xml.XmlWriter.WriteString%2A> 方法。|  
  
 有关 <xref:System.Xml.XmlWriter> 类的更多信息，请参见 <xref:System.Xml.XmlWriter> 类参考文档。  
  
### <a name="multiple-xmlwriter-objects"></a>多个 XmlWriter 对象  

 可能存在多个 <xref:System.Xml.XPath.XPathNavigator> 对象，指向包含一个或多个打开的 <xref:System.Xml.XmlWriter> 对象的 XML 文档的不同部分。 在单线程方案中允许并支持多个 <xref:System.Xml.XmlWriter> 对象。  
  
 以下是在使用多个 <xref:System.Xml.XmlWriter> 对象时要考虑的重要事项。  
  
- 在调用每个 <xref:System.Xml.XmlWriter> 对象的 <xref:System.Xml.XmlWriter.Close%2A> 方法时，通过 <xref:System.Xml.XmlWriter> 对象编写的 XML 片断将添加到 XML 文档中。 直到此时，<xref:System.Xml.XmlWriter> 对象一直在编写断开的片断。 如果对 XML 文档执行某项操作，在调用 <xref:System.Xml.XmlWriter> 之前，任何通过 <xref:System.Xml.XmlWriter.Close%2A> 对象编写的片断不会受影响。  
  
- 如果特定 XML 子树上存在打开的 <xref:System.Xml.XmlWriter> 对象并且该子树已删除，<xref:System.Xml.XmlWriter> 对象仍可以添加到子树上。 只是子树成为已删除的片断。  
  
- 如果在 XML 文档的相同位置打开多个 <xref:System.Xml.XmlWriter> 对象，这些对象将按照 <xref:System.Xml.XmlWriter> 对象关闭的顺序添加到 XML 文档，而不是按照对象打开的顺序。  
  
 以下示例创建一个 <xref:System.Xml.XmlDocument> 对象，创建一个 <xref:System.Xml.XPath.XPathNavigator> 对象，然后使用 <xref:System.Xml.XmlWriter> 方法返回的 <xref:System.Xml.XPath.XPathNavigator.PrependChild%2A> 对象在 `books.xml` 文件中创建第一本图书的结构。 然后，示例将其保存为 `book.xml` 文件。  
  
```vb  
Dim document As XmlDocument = New XmlDocument()  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
Using writer As XmlWriter = navigator.PrependChild()  
  
    writer.WriteStartElement("bookstore")  
    writer.WriteStartElement("book")  
    writer.WriteAttributeString("genre", "autobiography")  
    writer.WriteAttributeString("publicationdate", "1981-03-22")  
    writer.WriteAttributeString("ISBN", "1-861003-11-0")  
    writer.WriteElementString("title", "The Autobiography of Benjamin Franklin")  
    writer.WriteStartElement("author")  
    writer.WriteElementString("first-name", "Benjamin")  
    writer.WriteElementString("last-name", "Franklin")  
    writer.WriteElementString("price", "8.99")  
    writer.WriteEndElement()  
    writer.WriteEndElement()  
    writer.WriteEndElement()  
  
End Using  
  
document.Save("book.xml")  
```  
  
```csharp  
XmlDocument document = new XmlDocument();  
XPathNavigator navigator = document.CreateNavigator();  
  
using (XmlWriter writer = navigator.PrependChild())  
{  
    writer.WriteStartElement("bookstore");  
    writer.WriteStartElement("book");  
    writer.WriteAttributeString("genre", "autobiography");  
    writer.WriteAttributeString("publicationdate", "1981-03-22");  
    writer.WriteAttributeString("ISBN", "1-861003-11-0");  
    writer.WriteElementString("title", "The Autobiography of Benjamin Franklin");  
    writer.WriteStartElement("author");  
    writer.WriteElementString("first-name", "Benjamin");  
    writer.WriteElementString("last-name", "Franklin");  
    writer.WriteElementString("price", "8.99");  
    writer.WriteEndElement();  
    writer.WriteEndElement();  
    writer.WriteEndElement();  
}  
document.Save("book.xml");  
```  
  
## <a name="saving-an-xml-document"></a>保存 XML 文档  

 使用 <xref:System.Xml.XmlDocument> 类的方法保存本主题中所述的方法对 <xref:System.Xml.XmlDocument> 对象的更改。 若要详细了解如何保存对 <xref:System.Xml.XmlDocument> 对象所做的更改，请参阅[保存和编写文档](saving-and-writing-a-document.md)。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [使用 XPath 数据模型处理 XML 数据](process-xml-data-using-the-xpath-data-model.md)
- [使用 XPathNavigator 修改 XML 数据](modify-xml-data-using-xpathnavigator.md)
- [使用 XPathNavigator 删除 XML 数据](remove-xml-data-using-xpathnavigator.md)
