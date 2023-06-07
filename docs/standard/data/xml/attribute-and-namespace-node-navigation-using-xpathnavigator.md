---
title: 使用 XPathNavigator 的属性和命名空间节点定位
ms.date: 03/30/2017
ms.assetid: 23975f88-e0af-4b88-93de-9e20e11880ad
ms.openlocfilehash: d561afdc477119a6d9d04eacfcd6a6bdb9d0e2d6
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95725386"
---
# <a name="attribute-and-namespace-node-navigation-using-xpathnavigator"></a>使用 XPathNavigator 的属性和命名空间节点定位

<xref:System.Xml.XPath.XPathNavigator> 类提供两组导航方法，第一组如[使用 XPathNavigator 的节点集定位](node-set-navigation-using-xpathnavigator.md)主题中所述，用于浏览 <xref:System.Xml.XPath.XPathDocument> 或 <xref:System.Xml.XmlDocument> 对象中的节点集  。 第二组如本主题中所述，用于浏览 <xref:System.Xml.XPath.XPathDocument> 或 <xref:System.Xml.XmlDocument> 对象中的属性和命名空间节点  。  
  
## <a name="attribute-node-navigation"></a>浏览属性节点  

 属性是元素的属性，不是元素的子级。 这一区别很重要，因为用来浏览同级节点、父节点和子节点的 <xref:System.Xml.XPath.XPathNavigator> 类的方法不同。  
  
 例如，<xref:System.Xml.XPath.XPathNavigator.MoveToPrevious%2A> 和 <xref:System.Xml.XPath.XPathNavigator.MoveToNext%2A> 方法不用来从元素浏览到属性或在属性间浏览。 属性采用不同的浏览方法。  
  
 以下是 <xref:System.Xml.XPath.XPathNavigator> 类的属性浏览方法。  
  
- <xref:System.Xml.XPath.XPathNavigator.MoveToAttribute%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.MoveToFirstAttribute%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.MoveToNextAttribute%2A>  
  
 在当前节点是元素时，可以使用 <xref:System.Xml.XPath.XPathNavigator.HasAttributes%2A> 方法查看是否存在任何与此元素关联的属性。 如果已知元素具有属性，有多种方法可以访问这些属性。 要从元素中检索单个属性，请使用 <xref:System.Xml.XPath.XPathNavigator.GetAttribute%2A> 方法。 若要将 <xref:System.Xml.XPath.XPathNavigator> 移动到特定属性，请使用 <xref:System.Xml.XPath.XPathNavigator.MoveToAttribute%2A> 方法。 还可以循环访问元素的每个属性，方法是先使用 <xref:System.Xml.XPath.XPathNavigator.MoveToFirstAttribute%2A> 方法，然后多次调用 <xref:System.Xml.XPath.XPathNavigator.MoveToNextAttribute%2A> 方法。  
  
> [!NOTE]
> 当 <xref:System.Xml.XPath.XPathNavigator> 对象位于某个属性或命名空间节点上时，<xref:System.Xml.XPath.XPathNavigator.MoveToChild%2A>、<xref:System.Xml.XPath.XPathNavigator.MoveToFirst%2A>、<xref:System.Xml.XPath.XPathNavigator.MoveToFirstChild%2A>、<xref:System.Xml.XPath.XPathNavigator.MoveToFollowing%2A>、<xref:System.Xml.XPath.XPathNavigator.MoveToId%2A>、<xref:System.Xml.XPath.XPathNavigator.MoveToNext%2A> 和 <xref:System.Xml.XPath.XPathNavigator.MoveToPrevious%2A> 方法始终返回 `false`，并对 <xref:System.Xml.XPath.XPathNavigator> 的位置没有影响。 <xref:System.Xml.XPath.XPathNavigator.MoveTo%2A>、<xref:System.Xml.XPath.XPathNavigator.MoveToParent%2A> 和 <xref:System.Xml.XPath.XPathNavigator.MoveToRoot%2A> 方法例外。  
  
## <a name="namespace-node-navigation"></a>浏览命名空间节点  

 每个元素都有一组关联的命名空间节点，一个命名空间节点用于元素范围内绑定到某个命名空间 URI 的每个不同的命名空间前缀（包括绑定到 `http://www.w3.org/XML/1998/namespace` 命名空间的 XML 前缀，该前缀在每个 XML 文档中隐式声明），一个命名空间节点用于默认命名空间（如果处于元素范围内）。 元素是每个命名空间节点的父级；但是，命名空间节点不是其父元素的子级。  
  
 与属性相同，<xref:System.Xml.XPath.XPathNavigator.MoveToPrevious%2A> 和 <xref:System.Xml.XPath.XPathNavigator.MoveToNext%2A> 方法不用来从元素浏览到命名空间节点或在命名空间节点间浏览。 命名空间节点采用不同的浏览方法。  
  
 以下是 <xref:System.Xml.XPath.XPathNavigator> 类的命名空间浏览方法。  
  
- <xref:System.Xml.XPath.XPathNavigator.MoveToNamespace%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.MoveToFirstNamespace%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.MoveToNextNamespace%2A>  
  
 在 XML 文档中任何元素的范围内，始终至少存在一个命名空间节点。 此命名空间节点的前缀为 `xml`，命名空间 URI 为 `http://www.w3.org/XML/1998/namespace`。 要在给定特定前缀的情况下在范围内检索命名空间 URI，请使用 <xref:System.Xml.XPath.XPathNavigator.GetNamespace%2A> 方法。 要将 <xref:System.Xml.XPath.XPathNavigator> 对象移至特定命名空间节点，请使用 <xref:System.Xml.XPath.XPathNavigator.MoveToNamespace%2A> 方法。 还可以循环访问元素范围中的每个命名空间节点，方法是先使用 <xref:System.Xml.XPath.XPathNavigator.MoveToFirstNamespace%2A> 方法，然后多次调用 <xref:System.Xml.XPath.XPathNavigator.MoveToNextNamespace%2A> 方法。  
  
> [!NOTE]
> 当 <xref:System.Xml.XPath.XPathNavigator> 对象位于某个属性或命名空间节点上时，<xref:System.Xml.XPath.XPathNavigator.MoveToChild%2A>、<xref:System.Xml.XPath.XPathNavigator.MoveToFirst%2A>、<xref:System.Xml.XPath.XPathNavigator.MoveToFirstChild%2A>、<xref:System.Xml.XPath.XPathNavigator.MoveToFollowing%2A>、<xref:System.Xml.XPath.XPathNavigator.MoveToId%2A>、<xref:System.Xml.XPath.XPathNavigator.MoveToNext%2A> 和 <xref:System.Xml.XPath.XPathNavigator.MoveToPrevious%2A> 方法始终返回 `false`，并对 <xref:System.Xml.XPath.XPathNavigator> 的位置没有影响。 <xref:System.Xml.XPath.XPathNavigator.MoveTo%2A>、<xref:System.Xml.XPath.XPathNavigator.MoveToParent%2A> 和 <xref:System.Xml.XPath.XPathNavigator.MoveToRoot%2A> 方法例外。  
  
### <a name="the-xpathnamespacescope-enumeration"></a>XPathNamespaceScope 枚举  

 在浏览命名空间节点时，<xref:System.Xml.XPath.XPathNavigator.MoveToFirstNamespace%2A> 和 <xref:System.Xml.XPath.XPathNavigator.MoveToNextNamespace%2A> 方法可以使用 <xref:System.Xml.XPath.XPathNamespaceScope> 参数调用。 这些方法的行为与未使用任何参数调用的对应方法不同。 <xref:System.Xml.XPath.XPathNamespaceScope> 枚举包含值 <xref:System.Xml.XPath.XPathNamespaceScope.All>、<xref:System.Xml.XPath.XPathNamespaceScope.ExcludeXml> 或 <xref:System.Xml.XPath.XPathNamespaceScope.Local>。  
  
 下列示例显示 <xref:System.Xml.XPath.XPathNavigator.MoveToFirstNamespace%2A> 和 <xref:System.Xml.XPath.XPathNavigator.MoveToNextNamespace%2A> 方法在 XML 文档中的不同范围内返回的命名空间。  
  
```xml  
<root>  
    <element1 xmlns="http://www.contoso.com" xmlns:books="http://www.contoso.com/books">  
        <element2 />  
    </element1>  
</root>  
```  
  
 命名空间序列（先调用 <xref:System.Xml.XPath.XPathNavigator> 方法，然后再多次调用 <xref:System.Xml.XPath.XPathNavigator.MoveToFirstNamespace%2A> 方法后，<xref:System.Xml.XPath.XPathNavigator.MoveToNextNamespace%2A> 所处的命名空间）如下所示。  
  
- 位于 `element2` 上时：`xmlns:books="http://www.contoso.com/books"`、`xmlns="http://www.contoso.com"` 和 `xmlns:xml="http://www.w3.org/XML/1998/namespace"`。  
  
- 位于 `element1` 上时：`xmlns:books="http://www.contoso.com/books"`、`xmlns="http://www.contoso.com"` 和 `xmlns:xml="http://www.w3.org/XML/1998/namespace"`。  
  
- 位于 `root` 上时：`xmlns:xml="http://www.w3.org/XML/1998/namespace".`  
  
> [!NOTE]
> <xref:System.Xml.XPath.XPathNavigator> 类以相反的文档顺序返回命名空间节点。 因此，<xref:System.Xml.XPath.XPathNavigator.MoveToFirstNamespace%2A> 实质上移到当前在范围内的最后一个命名空间节点。  
  
 下列示例显示 <xref:System.Xml.XPath.XPathNavigator.MoveToFirstNamespace%2A> 和 <xref:System.Xml.XPath.XPathNavigator.MoveToNextNamespace%2A> 方法使用在 XML 文档中的不同范围内指定的 <xref:System.Xml.XPath.XPathNamespaceScope> 枚举返回的命名空间。  
  
```xml  
<root xmlns="http://www.contoso.com" xmlns:a="http://www.contoso.com/a" xmlns:b="http://www.contoso.com/b">  
    <child1 xmlns="" xmlns:a="urn:a">  
        <child2 xmlns:c="urn:c" />  
    </child1>  
</root>  
```  
  
 位于 `child2` 上时，命名空间序列（先调用 <xref:System.Xml.XPath.XPathNavigator> 方法，然后再多次调用 <xref:System.Xml.XPath.XPathNavigator.MoveToFirstNamespace%2A> 方法后，<xref:System.Xml.XPath.XPathNavigator.MoveToNextNamespace%2A> 所处的命名空间）如下所示。  
  
- <xref:System.Xml.XPath.XPathNamespaceScope.All>：`xmlns:c="urn:c"`、`xmlns:a="urn:a"`、`xmlns=""`、`xmlns:b="http://www.contoso.com/b"`、`xmlns:a="http://www.contoso.com/a"`、`xmlns="http://www.contoso.com"` 和 `xmlns:xml="http://www.w3.org/XML/1998/namespace"`。  
  
- <xref:System.Xml.XPath.XPathNamespaceScope.ExcludeXml>：`xmlns:c="urn:c"`、`xmlns:a="urn:a"`、`xmlns=""`、`xmlns:b="http://www.contoso.com/b"`、`xmlns:a="http://www.contoso.com/a"` 和 `xmlns="http://www.contoso.com"`。  
  
- <xref:System.Xml.XPath.XPathNamespaceScope.Local>: `xmlns:c="urn:c"`。  
  
> [!NOTE]
> <xref:System.Xml.XPath.XPathNavigator> 类以相反的文档顺序返回命名空间节点。 因此，<xref:System.Xml.XPath.XPathNavigator.MoveToFirstNamespace%2A> 实质上移到当前在范围内的最后一个命名空间节点。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [使用 XPath 数据模型处理 XML 数据](process-xml-data-using-the-xpath-data-model.md)
- [使用 XPathNavigator 的节点集定位](node-set-navigation-using-xpathnavigator.md)
- [使用 XPathNavigator 提取 XML 数据](extract-xml-data-using-xpathnavigator.md)
- [使用 XPathNavigator 访问强类型 XML 数据](accessing-strongly-typed-xml-data-using-xpathnavigator.md)
