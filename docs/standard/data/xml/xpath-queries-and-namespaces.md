---
title: XPath 查询和命名空间
description: 了解 XPath 查询和命名空间。 XPath 查询支持 XML 文档中的命名空间，可以使用命名空间前缀来限定元素和属性的名称。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: ef6402be-2f8e-4be2-8d3e-a80891cdef8b
ms.openlocfilehash: 74dbe6b84c8d9400790f763f811da5542c732892
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720875"
---
# <a name="xpath-queries-and-namespaces"></a>XPath 查询和命名空间

XPath 查询支持 XML 文档中的命名空间，可以使用命名空间前缀来限定元素和属性的名称。 使用命名空间前缀来限定元素和属性的名称可以限制 XPath 查询只返回属于特定命名空间的节点。  
  
 例如，如果前缀 `books` 映射到命名空间 `http://www.contoso.com/books`，以下 XPath 查询 `/books:books/books:book` 将只选择命名空间 `book` 中的 `http://www.contoso.com/books` 元素。  
  
## <a name="the-xmlnamespacemanager"></a>XmlNamespaceManager  

 要在 XPath 查询中使用命名空间，构造一个从 <xref:System.Xml.IXmlNamespaceResolver> 接口派生的对象（例如 <xref:System.Xml.XmlNamespaceManager> 类），包含要加入 XPath 查询的命名空间 URI 和前缀。  
  
 <xref:System.Xml.XmlNamespaceManager> 对象可以通过下列任意方式在查询中使用。  
  
- 使用 <xref:System.Xml.XmlNamespaceManager> 对象的 <xref:System.Xml.XPath.XPathExpression> 方法将 <xref:System.Xml.XPath.XPathExpression.SetContext%2A> 对象与现有的 <xref:System.Xml.XPath.XPathExpression> 对象关联。 还可以使用静态 <xref:System.Xml.XPath.XPathExpression> 方法编译新的 <xref:System.Xml.XPath.XPathExpression.Compile%2A> 对象，该方法使用表示 XPath 表达式的字符串和 <xref:System.Xml.XmlNamespaceManager> 对象作为参数，并返回一个新的 <xref:System.Xml.XPath.XPathExpression> 对象。  
  
- <xref:System.Xml.XmlNamespaceManager> 对象本身与表示 XPath 表达式的字符串一起作为参数传递给接受方的 <xref:System.Xml.XPath.XPathNavigator> 类方法。  
  
 以下是 <xref:System.Xml.XPath.XPathNavigator> 类中接受从 <xref:System.Xml.IXmlNamespaceResolver> 接口派生的对象作为参数的方法。  
  
- <xref:System.Xml.XPath.XPathNavigator.Evaluate%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.Select%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.SelectSingleNode%2A>  
  
### <a name="the-default-namespace"></a>默认命名空间  

 在下面的 XML 文档中，具有空前缀的默认命名空间用于声明 `http://www.contoso.com/books` 命名空间。  
  
```xml  
<books xmlns="http://www.contoso.com/books">  
    <book>  
        <title>Title</title>  
        <author>Author Name</author>  
        <price>5.50</price>  
    </book>  
</books>  
```  
  
 XPath 将空前缀作为 `null` 命名空间对待。 也就是说，XPath 查询中只能使用映射到命名空间上的前缀。 这意味着如果要针对 XML 文档中的某个命名空间进行查询，即使是默认的命名空间，也需要为其定义前缀。  
  
 例如，在没有为上面的 XML 文档定义前缀的情况下，XPath 查询 `/books/book` 不会返回任何结果。  
  
 必须绑定前缀，这样，如果查询的文档中有些节点在某个命名空间，有些节点在默认命名空间，可以避免混淆情况。  
  
 以下代码为默认命名空间定义前缀，并选择 `book` 命名空间中的所有 `http://www.contoso.com/books` 元素。  
  
```vb  
Dim document As XPathDocument = New XPathDocument("books.xml")  
Dim navigator As XPathNavigator = document.CreateNavigator()  
Dim query As XPathExpression = navigator.Compile("/books:books/books:book")  
Dim manager As XmlNamespaceManager = New XmlNamespaceManager(navigator.NameTable)  
manager.AddNamespace("books", "http://www.contoso.com/books")  
query.SetContext(manager)  
Dim nodes As XPathNodeIterator = navigator.Select(query)  
```  
  
```csharp  
XPathDocument document = new XPathDocument("books.xml");  
XPathNavigator navigator = document.CreateNavigator();  
XPathExpression query = navigator.Compile("/books:books/books:book");  
XmlNamespaceManager manager = new XmlNamespaceManager(navigator.NameTable);  
manager.AddNamespace("books", "http://www.contoso.com/books");  
query.SetContext(manager);  
XPathNodeIterator nodes = navigator.Select(query);  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [使用 XPath 数据模型处理 XML 数据](process-xml-data-using-the-xpath-data-model.md)
- [使用 XPathNavigator 选择 XML 数据](select-xml-data-using-xpathnavigator.md)
- [使用 XPathNavigator 计算 XPath 表达式](evaluate-xpath-expressions-using-xpathnavigator.md)
- [使用 XPathNavigator 匹配节点](matching-nodes-using-xpathnavigator.md)
- [XPath 查询识别的节点类型](node-types-recognized-with-xpath-queries.md)
- [已编译的 XPath 表达式](compiled-xpath-expressions.md)
