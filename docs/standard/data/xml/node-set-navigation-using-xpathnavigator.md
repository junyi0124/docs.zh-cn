---
title: 使用 XPathNavigator 的节点集定位
ms.date: 03/30/2017
ms.assetid: 1a954b41-7173-40bc-8544-d430f209b1e5
ms.openlocfilehash: 592acfb5e4065d707f4dda09f349e8b783656148
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95714293"
---
# <a name="node-set-navigation-using-xpathnavigator"></a>使用 XPathNavigator 的节点集定位

可以使用 <xref:System.Xml.XPath.XPathDocument> 类的节点集浏览方法在 <xref:System.Xml.XmlDocument> 或 <xref:System.Xml.XPath.XPathNavigator> 对象中浏览节点。 可以浏览所有节点，也可以浏览 <xref:System.Xml.XPath.XPathNavigator> 类的一种选择方法返回的所选节点集。  
  
## <a name="element-node-set-navigation"></a>浏览元素节点集  

 <xref:System.Xml.XPath.XPathNavigator> 类提供多种方法用于浏览元素节点。 下表显示可用的浏览方法以及各种方法如何移动的说明；其中不包括用于浏览属性和命名空间节点的方法。  
  
 若要详细了解如何在 <xref:System.Xml.XPath.XPathNavigator> 对象中选择节点，请参阅[使用 XPathNavigator 选择、计算和匹配 XML 数据](selecting-evaluating-and-matching-xml-data-using-xpathnavigator.md)。 若要详细了解如何浏览属性和命名空间节点，请参阅[使用 XPathNavigator 的属性和命名空间节点定位](attribute-and-namespace-node-navigation-using-xpathnavigator.md)。  
  
|方法|描述|  
|------------|-----------------|  
|<xref:System.Xml.XPath.XPathNavigator.MoveTo%2A>|将 <xref:System.Xml.XPath.XPathNavigator> 移至所指定的 <xref:System.Xml.XPath.XPathNavigator> 的相同位置。|  
|<xref:System.Xml.XPath.XPathNavigator.MoveToChild%2A>|将 <xref:System.Xml.XPath.XPathNavigator> 移至当前节点的子节点。|  
|<xref:System.Xml.XPath.XPathNavigator.MoveToFirst%2A>|将 <xref:System.Xml.XPath.XPathNavigator> 移至当前节点的第一个同级节点。|  
|<xref:System.Xml.XPath.XPathNavigator.MoveToFirstChild%2A>|将 <xref:System.Xml.XPath.XPathNavigator> 移至当前节点的第一个子节点。|  
|<xref:System.Xml.XPath.XPathNavigator.MoveToFollowing%2A>|将 <xref:System.Xml.XPath.XPathNavigator> 按文档顺序移至指定的元素。|  
|<xref:System.Xml.XPath.XPathNavigator.MoveToId%2A>|将 <xref:System.Xml.XPath.XPathNavigator> 移至 `ID` 类型属性的值与给定 <xref:System.String> 匹配的节点。|  
|<xref:System.Xml.XPath.XPathNavigator.MoveToNext%2A>|将 <xref:System.Xml.XPath.XPathNavigator> 移至当前节点的下一个同级节点。|  
|<xref:System.Xml.XPath.XPathNavigator.MoveToParent%2A>|将 <xref:System.Xml.XPath.XPathNavigator> 移至当前节点的父节点。|  
|<xref:System.Xml.XPath.XPathNavigator.MoveToPrevious%2A>|将 <xref:System.Xml.XPath.XPathNavigator> 移至当前节点的上一个同级节点。|  
|<xref:System.Xml.XPath.XPathNavigator.MoveToRoot%2A>|将 <xref:System.Xml.XPath.XPathNavigator> 移至 XML 文档的根节点。|  
  
## <a name="comments-and-processing-instruction-node-navigation"></a>浏览注释和处理指令节点  

 下列 <xref:System.Xml.XPath.XPathNavigator> 类方法可以从 XML 文档中的其他节点移至注释或处理指令节点。  
  
- <xref:System.Xml.XPath.XPathNavigator.MoveTo%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.MoveToNext%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.MoveToPrevious%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.MoveToFirst%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.MoveToFirstChild%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.MoveToChild%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.MoveToParent%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.MoveToId%2A>  
  
## <a name="see-also"></a>请参阅

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [使用 XPath 数据模型处理 XML 数据](process-xml-data-using-the-xpath-data-model.md)
- [使用 XPathNavigator 的属性和命名空间节点定位](attribute-and-namespace-node-navigation-using-xpathnavigator.md)
- [使用 XPathNavigator 提取 XML 数据](extract-xml-data-using-xpathnavigator.md)
- [使用 XPathNavigator 访问强类型 XML 数据](accessing-strongly-typed-xml-data-using-xpathnavigator.md)
