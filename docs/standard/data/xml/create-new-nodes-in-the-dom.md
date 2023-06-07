---
title: 在 DOM 中创建新节点
ms.date: 03/30/2017
ms.assetid: 6c2b9789-b61a-49f9-b33f-db01a945edf2
ms.openlocfilehash: dea7add100fbdbb9e761fe39d0d824d27975757f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95704742"
---
# <a name="create-new-nodes-in-the-dom"></a>在 DOM 中创建新节点

<xref:System.Xml.XmlDocument> 为所有节点类型提供了 create 方法。 为该方法提供名称（需要时）以及那些具有内容的节点（如文本节点）的内容或其他参数，这样便可创建节点。 下面的方法需要填充名称和几个其他参数以创建相应的节点。  
  
- <xref:System.Xml.XmlDocument.CreateCDataSection%2A>  
  
- <xref:System.Xml.XmlDocument.CreateComment%2A>  
  
- <xref:System.Xml.XmlDocument.CreateDocumentFragment%2A>  
  
- <xref:System.Xml.XmlDocument.CreateDocumentType%2A>  
  
- <xref:System.Xml.XmlDocument.CreateElement%2A>  
  
- <xref:System.Xml.XmlDocument.CreateNode%2A>  
  
- <xref:System.Xml.XmlDocument.CreateProcessingInstruction%2A>  
  
- <xref:System.Xml.XmlDocument.CreateSignificantWhitespace%2A>  
  
- <xref:System.Xml.XmlDocument.CreateTextNode%2A>  
  
- <xref:System.Xml.XmlDocument.CreateWhitespace%2A>  
  
- <xref:System.Xml.XmlDocument.CreateXmlDeclaration%2A>  
  
 其他节点类型不仅仅只要求向参数提供数据。  
  
 若要了解属性，请参阅[新建 DOM 中元素的属性](creating-new-attributes-for-elements-in-the-dom.md)。 若要了解元素和属性名验证，请参阅[新建节点时的 XML 元素和属性名验证](xml-element-and-attribute-name-verification-when-creating-new-nodes.md)。 若要了解如何创建实体引用，请参阅[新建实体引用](creating-new-entity-references.md)。 若要了解命名空间对实体引用扩展产生的影响，请参阅[命名空间影响包含元素和属性的新节点的实体引用扩展](namespace-affect-on-entity-ref-expansion-for-new-nodes.md)。  
  
 创建新节点后，有几个方法可用于将其插入到树中。 下表列出了这些方法，并描述了新节点在 XML 文档对象模型 (DOM) 中的位置。  
  
|方法|节点位置|  
|------------|--------------------|  
|<xref:System.Xml.XmlNode.InsertBefore%2A>|插入到引用节点之前。 例如，在位置 5 插入新节点：<br /><br /> `Dim refChild As XmlNode = node.ChildNodes(4) 'The reference is zero-based.node.InsertBefore(newChild, refChild);`<br /><br /> `XmlNode refChild = node.ChildNodes[4]; //The reference is zero-based. node.InsertBefore(newChild, refChild);`<br /><br /> 有关更多信息，请参见 <xref:System.Xml.XmlNode.InsertBefore%2A> 方法。|  
|<xref:System.Xml.XmlNode.InsertAfter%2A>|插入到引用节点之后。 例如：<br /><br /> `node.InsertAfter(newChild, refChild)`<br /><br /> `node.InsertAfter(newChild, refChild);`<br /><br /> 有关更多信息，请参见 <xref:System.Xml.XmlNode.InsertAfter%2A> 方法。|  
|<xref:System.Xml.XmlNode.AppendChild%2A>|将节点添加到给定节点的子节点列表的末尾。 如果所添加的节点是 <xref:System.Xml.XmlDocumentFragment>，则会将文档片段的全部内容移至该节点的子列表中。 有关更多信息，请参见 <xref:System.Xml.XmlNode.AppendChild%2A> 方法。|  
|<xref:System.Xml.XmlNode.PrependChild%2A>|将节点添加到给定节点的子节点列表的开头。 如果所添加的节点是 <xref:System.Xml.XmlDocumentFragment>，则会将文档片段的全部内容移至该节点的子列表中。 有关更多信息，请参见 <xref:System.Xml.XmlNode.PrependChild%2A> 方法。|  
|<xref:System.Xml.XmlAttributeCollection.Append%2A>|将 <xref:System.Xml.XmlAttribute> 节点追加到与元素关联的属性集合的末尾。 有关更多信息，请参见 <xref:System.Xml.XmlAttributeCollection.Append%2A> 方法。|  
  
## <a name="see-also"></a>请参阅

- [XML 文档对象模型 (DOM)](xml-document-object-model-dom.md)
