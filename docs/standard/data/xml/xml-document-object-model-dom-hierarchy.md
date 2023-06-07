---
title: XML 文档对象模型 (DOM) 层次结构
ms.date: 03/30/2017
ms.assetid: 9d187d4f-c76e-4223-a670-cc290783ce47
ms.openlocfilehash: 24ef51a18392fe3034e64bd585d879941fca4b95
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95718353"
---
# <a name="xml-document-object-model-dom-hierarchy"></a>XML 文档对象模型 (DOM) 层次结构

下图显示了 XML 文档对象模型 (DOM) 的类层次结构，其中万维网联合会 (W3C) 名称用括号括起来，另外还有相关的类名。  
  
 ![XML 文档对象模型 &#40;DOM&#41; 层次结构](media/dom-class-hierarchy.gif "Dom_class_hierarchy")  
XML 文档对象模型 (DOM) 层次结构  
  
 下列类不继承自 XmlNode  ：  
  
- **XmlImplementation**  
  
- **XmlNamedNodeMap**  
  
- **XmlNodeList**  
  
- **XmlNodeChangedEventArgs**  
  
 XmlImplementation  类用于创建 XML 文档。 有关详细信息，请参阅 [XML 文档创建](xml-document-creation.md)。  
  
 XmlNamedNodeMap  类处理无序节点集。 有关详细信息，请参阅[按名称或索引检索无序节点](unordered-node-retrieval-by-name-or-index.md)。  
  
 XmlNodeList  类处理有序节点列表。 有关详细信息，请参阅[按索引检索有序节点](ordered-node-retrieval-by-index.md)。  
  
 XmlNodeChangedEventArgs  类处理在 XmlDocument  上注册的事件处理程序。 有关详细信息，请参阅[使用 XmlNodeChangedEventArgs 在 XML 文档中处理事件](event-handling-in-an-xml-document-using-the-xmlnodechangedeventargs.md)。  
  
 XmlLinkedNode  类继承自 XmlNode  。 它用于重写 XmlNode  中的两个方法：PreviousSibling  和 NextSibling  方法。 然后，这些重写方法由包含上一个和下一个同级的类 XmlCharacterData  、XmlDeclaration  、XmlDocumentType  、XmlElement  、XmlEntityReference  和 XmlProcessingInstruction  继承和使用。  
  
## <a name="see-also"></a>请参阅

- [XML 文档对象模型 (DOM)](xml-document-object-model-dom.md)
