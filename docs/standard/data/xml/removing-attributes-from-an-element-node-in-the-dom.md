---
title: 移除 DOM 中元素节点的属性
ms.date: 03/30/2017
ms.assetid: 7ede6f9e-a3ac-49a4-8488-ab8360a44aa4
ms.openlocfilehash: 53922c55295e852a1aa62d847313fbd657a42541
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95686756"
---
# <a name="removing-attributes-from-an-element-node-in-the-dom"></a>移除 DOM 中元素节点的属性

有多种方法可以移除属性。 一种方法是从属性集合中移除它们。 为此，请执行下列步骤：  
  
1. 使用 `XmlAttributeCollection attrs = elem.Attributes;` 获取元素的属性集合。  
  
2. 使用以下三种方法之一移除属性集合中的属性：  
  
    - 使用 <xref:System.Xml.XmlAttributeCollection.Remove%2A> 移除特定的属性。  
  
    - 使用 <xref:System.Xml.XmlAttributeCollection.RemoveAll%2A> 移除集合中的所有属性并保留没有属性的元素。  
  
    - 使用 <xref:System.Xml.XmlAttributeCollection.RemoveAt%2A> 通过索引号从属性集合中移除属性。  
  
 下列方法移除元素节点中的属性。  
  
- 使用 <xref:System.Xml.XmlElement.RemoveAllAttributes%2A> 移除属性集合。  
  
- 使用 <xref:System.Xml.XmlElement.RemoveAttribute%2A> 可按名称从集合中移除单个属性。  
  
- 使用 <xref:System.Xml.XmlElement.RemoveAttributeAt%2A> 按索引号从集合中移除单个属性。  
  
 另一个替换方法是获取元素，获取属性集合中的属性并直接移除属性节点。 若要获取属性集合中的属性，可使用名称 `XmlAttribute attr = attrs["attr_name"];`、索引 `XmlAttribute attr = attrs[0];` 或用命名空间 `XmlAttribute attr = attrs["attr_localName", "attr_namespace"]` 完全限定该名称。  
  
 无论用于移除属性的方法是什么，当移除在文档类型定义 (DTD) 中定义为默认属性的属性时有特殊限制。 除非移除了默认属性所属的元素，否则不能移除默认属性。 已声明了默认属性的元素总是存在默认属性。 如果从 <xref:System.Xml.XmlAttributeCollection> 或 <xref:System.Xml.XmlElement> 中移除默认属性，将使替换属性插入元素的 <xref:System.Xml.XmlAttributeCollection>，并初始化为所声明的默认值。 如果将某个元素定义为 `<book att1="1" att2="2" att3="3"></book>`，则将得到一个具有三个已声明的默认属性的 `book` 元素。 XML 文档对象模型 (DOM) 实现保证，只要此 `book` 元素存在，就会有三个默认属性（`att1`、`att2` 和 `att3`）。  
  
 在使用 <xref:System.Xml.XmlAttribute> 调用时，<xref:System.Xml.XmlAttributeCollection.RemoveAll%2A> 方法会将属性值设置为 String.Empty，因为属性不能没有值。  
  
## <a name="see-also"></a>请参阅

- [XML 文档对象模型 (DOM)](xml-document-object-model-dom.md)
