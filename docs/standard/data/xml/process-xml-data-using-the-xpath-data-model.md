---
title: 使用 XPath 数据模型处理 XML 数据
ms.date: 03/30/2017
ms.assetid: 536c6fce-1453-4654-9c72-bca54d47e081
ms.openlocfilehash: 840fb40cc650d8f65af533d4102f18132bce3f53
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95686951"
---
# <a name="process-xml-data-using-the-xpath-data-model"></a>使用 XPath 数据模型处理 XML 数据

<xref:System.Xml?displayProperty=nameWithType> 命名空间使用 <xref:System.Xml.XmlDocument> 或 <xref:System.Xml.XPath.XPathDocument> 类提供内存中 XML 文档、片断、节点或节点集的编程表示形式。  
  
 <xref:System.Xml.XPath.XPathDocument> 类使用 XPath 数据模型提供 XML 文档在内存中的快速只读表示形式。 <xref:System.Xml.XmlDocument> 类提供实现 W3C 文档对象模型 (DOM) 级别 1 核心和核心 DOM 级别 2 的 XML 文档在内存中的可编辑表示形式。 这两个类均实现 <xref:System.Xml.XPath.IXPathNavigable> 接口，并返回 <xref:System.Xml.XPath.XPathNavigator> 对象，用于选择、计算、浏览和（在某些情况下）编辑基础 XML 数据。  
  
 下面各节介绍 <xref:System.Xml.XPath.XPathNavigator> 类的功能（基于返回该类的类）。  
  
## <a name="in-this-section"></a>本节内容  

 [使用 XPathDocument 和 XmlDocument 读取 XML 数据](reading-xml-data-using-xpathdocument-and-xmldocument.md)  
 描述如何创建只读 <xref:System.Xml.XPath.XPathDocument> 类对象来读取 XML 文档以及如何创建可编辑的 <xref:System.Xml.XmlDocument> 类对象来读取和编辑 XML 文档。 本主题还描述如何从每个类返回 <xref:System.Xml.XPath.XPathNavigator> 对象，以浏览和编辑 XML 文档。  
  
 [使用 XPathNavigator 选择、计算和匹配 XML 数据](selecting-evaluating-and-matching-xml-data-using-xpathnavigator.md)  
 介绍 <xref:System.Xml.XPath.XPathNavigator> 类中的方法，这些方法用于使用 XPath 查询在 <xref:System.Xml.XPath.XPathDocument> 或 <xref:System.Xml.XmlDocument> 对象中选择节点，计算和检查 XPath 表达式的结果，并确定 XML 文档中的节点是否与给定的 XPath 表达式匹配。  
  
 [使用 XPathNavigator 访问 XML 数据](accessing-xml-data-using-xpathnavigator.md)  
 介绍 <xref:System.Xml.XPath.XPathNavigator> 类中的方法，这些方法用于在 <xref:System.Xml.XPath.XPathDocument> 或 <xref:System.Xml.XmlDocument> 对象中浏览节点，提取 XML 数据，以及访问强类型 XML 数据。  
  
 [使用 XPathNavigator 编辑 XML 数据](editing-xml-data-using-xpathnavigator.md)  
 介绍 <xref:System.Xml.XPath.XPathNavigator> 类中的方法，这些方法用于在 <xref:System.Xml.XmlDocument> 对象包含的 XML 文档中插入、修改和移除节点和值。  
  
 [使用 XPathNavigator 验证架构](schema-validation-using-xpathnavigator.md)  
 描述如何验证 <xref:System.Xml.XPath.XPathDocument> 或 <xref:System.Xml.XmlDocument> 对象中包含的 XML 内容。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [使用 DOM 模型处理 XML 数据](process-xml-data-using-the-dom-model.md)
