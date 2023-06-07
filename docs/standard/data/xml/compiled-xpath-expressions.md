---
title: 已编译的 XPath 表达式
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: e25dd95f-b64c-4d8b-a3a4-379e1aa0ad55
ms.openlocfilehash: 99f06db3c6f1e634e9c4a677c01d1b0849afe43f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95701505"
---
# <a name="compiled-xpath-expressions"></a>已编译的 XPath 表达式

<xref:System.Xml.XPath.XPathExpression> 对象表示从 <xref:System.Xml.XPath.XPathExpression.Compile%2A> 类的静态 <xref:System.Xml.XPath.XPathExpression> 方法或 <xref:System.Xml.XPath.XPathNavigator.Compile%2A> 类的 <xref:System.Xml.XPath.XPathNavigator> 方法返回的已编译 XPath 查询。  
  
## <a name="the-xpathexpression-class"></a>XPathExpression 类  

 如果多次使用相同的 XPath 查询，通过 <xref:System.Xml.XPath.XPathExpression> 对象表示的已编译 XPath 查询非常有用。  
  
 例如，如果多次调用 <xref:System.Xml.XPath.XPathNavigator.Select%2A> 方法，而不是每次使用表示 XPath 查询的字符串，请使用 <xref:System.Xml.XPath.XPathExpression.Compile%2A> 类的 <xref:System.Xml.XPath.XPathExpression> 方法或 <xref:System.Xml.XPath.XPathNavigator.Compile%2A> 类的 <xref:System.Xml.XPath.XPathNavigator> 方法来编译并缓存 <xref:System.Xml.XPath.XPathExpression> 对象中的 XPath 查询，以便重复使用并提高性能。  
  
 编译后，根据从 XPath 查询返回的类型，<xref:System.Xml.XPath.XPathExpression> 对象也许可以作为下列 <xref:System.Xml.XPath.XPathNavigator> 类方法的输入。  
  
- <xref:System.Xml.XPath.XPathNavigator.Evaluate%2A?displayProperty=nameWithType>  
  
- <xref:System.Xml.XPath.XPathNavigator.Evaluate%2A?displayProperty=nameWithType>  
  
- <xref:System.Xml.XPath.XPathNavigator.Matches%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.Select%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.SelectSingleNode%2A>  
  
 下表介绍每种 W3C XPath 返回类型、其等效的 Microsoft .NET Framework 类型以及 <xref:System.Xml.XPath.XPathExpression> 对象基于其返回类型也许可以用于的方法。  
  
|W3C XPath 返回类型|等效的 .NET Framework 类型|描述|方法|  
|---------------------------|------------------------------------|-----------------|-------------|  
|`Node set`|<xref:System.Xml.XPath.XPathNodeIterator>|未排序的节点集合，按照文档顺序创建，没有重复。|<xref:System.Xml.XPath.XPathNavigator.Select%2A> 或 <xref:System.Xml.XPath.XPathNavigator.Evaluate%2A>|  
|`Boolean`|<xref:System.Boolean>|`true` 或 `false` 值。|<xref:System.Xml.XPath.XPathNavigator.Evaluate%2A> 或<br /><br /> <xref:System.Xml.XPath.XPathNavigator.Matches%2A>|  
|`Number`|<xref:System.Double>|一个浮点数字。|<xref:System.Xml.XPath.XPathNavigator.Evaluate%2A>|  
|`String`|<xref:System.String>|UCS 字符序列。|<xref:System.Xml.XPath.XPathNavigator.Evaluate%2A>|  
  
> [!NOTE]
> <xref:System.Xml.XPath.XPathNavigator.Matches%2A> 方法允许将 XPath 表达式作为其参数。 <xref:System.Xml.XPath.XPathNavigator.SelectSingleNode%2A> 方法返回 <xref:System.Xml.XPath.XPathNavigator> 对象，而不是一种 W3C XPath 返回类型。  
  
### <a name="the-returntype-property"></a>ReturnType 属性  

 在 XPath 查询编译为 <xref:System.Xml.XPath.XPathExpression> 对象之后，可以使用 <xref:System.Xml.XPath.XPathExpression.ReturnType%2A> 对象的 <xref:System.Xml.XPath.XPathExpression> 属性来确定 XPath 查询返回的内容。  
  
 <xref:System.Xml.XPath.XPathExpression.ReturnType%2A> 属性返回表示 W3C XPath 返回类型的下列 <xref:System.Xml.XPath.XPathResultType> 枚举值之一。  
  
- <xref:System.Xml.XPath.XPathResultType.Any>  
  
- <xref:System.Xml.XPath.XPathResultType.Boolean>  
  
- <xref:System.Xml.XPath.XPathResultType.Error>  
  
- <xref:System.Xml.XPath.XPathResultType.Navigator>  
  
- <xref:System.Xml.XPath.XPathResultType.NodeSet>  
  
- <xref:System.Xml.XPath.XPathResultType.Number>  
  
- <xref:System.Xml.XPath.XPathResultType.String>  
  
 以下示例使用 <xref:System.Xml.XPath.XPathExpression> 对象从 `books.xml` 文件中返回一个数字和一个节点集。 每个 <xref:System.Xml.XPath.XPathExpression.ReturnType%2A> 对象的 <xref:System.Xml.XPath.XPathExpression> 属性以及 <xref:System.Xml.XPath.XPathNavigator.Evaluate%2A> 和 <xref:System.Xml.XPath.XPathNavigator.Select%2A> 方法的返回结果将写入控制台。  
  
```vb  
Dim document As XPathDocument = New XPathDocument("books.xml")  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
' Returns a number.  
Dim query1 As XPathExpression = navigator.Compile("bookstore/book/price/text()*10")  
Console.WriteLine(query1.ReturnType)  
  
Dim number As Double = CType(navigator.Evaluate(query1), Double)  
Console.WriteLine(number)  
  
' Returns a node set.  
Dim query2 As XPathExpression = navigator.Compile("bookstore/book/price")  
Console.WriteLine(query2.ReturnType)  
  
Dim nodes As XPathNodeIterator = navigator.Select(query2)  
nodes.MoveNext()  
Console.WriteLine(nodes.Current.Value)  
```  
  
```csharp  
XPathDocument document = new XPathDocument("books.xml");  
XPathNavigator navigator = document.CreateNavigator();  
  
// Returns a number.  
XPathExpression query1 = navigator.Compile("bookstore/book/price/text()*10");  
Console.WriteLine(query1.ReturnType);  
  
Double number = (Double)navigator.Evaluate(query1);  
Console.WriteLine(number);  
  
// Returns a node set.  
XPathExpression query2 = navigator.Compile("bookstore/book/price");  
Console.WriteLine(query2.ReturnType);  
  
XPathNodeIterator nodes = navigator.Select(query2);  
nodes.MoveNext();  
Console.WriteLine(nodes.Current.Value);  
```  
  
 该示例使用 `books.xml` 文件作为输入。  
  
 [!code-xml[XPathXMLExamples#1](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/books.xml#1)]  
  
### <a name="higher-performance-xpath-expressions"></a>性能更强的 XPath 表达式  

 为了获得更好的性能，请尽可能在查询中使用最具体的 XPath 表达式。 例如，如果 `book` 节点是 `bookstore` 节点的子节点，并且 `bookstore` 节点是 XML 文档的顶级元素，使用 XPath 表达式 `/bookstore/book` 比使用 `//book` 速度更快。 `//book` XPath 表达式将扫描 XML 树中的每个节点来标识匹配的节点。  
  
 此外，如果选择条件很简单，使用 <xref:System.Xml.XPath.XPathNavigator> 类提供的节点集浏览方法的性能可能会强于 <xref:System.Xml.XPath.XPathNavigator> 类提供的选择方法。 例如，如果需要选择当前节点的第一个子级，使用 <xref:System.Xml.XPath.XPathNavigator.MoveToFirst%2A> 方法比使用 `child::*[1]` XPath 表达式和 <xref:System.Xml.XPath.XPathNavigator.Select%2A> 方法速度更快。  
  
 若要详细了解 <xref:System.Xml.XPath.XPathNavigator> 类的节点集导航方法，请参阅[使用 XPathNavigator 的节点集定位](node-set-navigation-using-xpathnavigator.md)。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [使用 XPath 数据模型处理 XML 数据](process-xml-data-using-the-xpath-data-model.md)
- [使用 XPathNavigator 选择 XML 数据](select-xml-data-using-xpathnavigator.md)
- [使用 XPathNavigator 计算 XPath 表达式](evaluate-xpath-expressions-using-xpathnavigator.md)
- [使用 XPathNavigator 匹配节点](matching-nodes-using-xpathnavigator.md)
- [XPath 查询识别的节点类型](node-types-recognized-with-xpath-queries.md)
- [XPath 查询和命名空间](xpath-queries-and-namespaces.md)
