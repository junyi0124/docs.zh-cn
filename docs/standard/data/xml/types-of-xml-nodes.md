---
title: XML 节点类型
ms.date: 03/30/2017
ms.assetid: 71d03b78-6898-4ce7-b0fc-1282573f31f7
ms.openlocfilehash: edf47246782e86cf134ea88d41381bed9ff16f69
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95675628"
---
# <a name="types-of-xml-nodes"></a>XML 节点类型

当将 XML 文档作为节点树读入内存时，这些节点的节点类型在创建节点时确定。 XML 文档对象模型 (DOM) 具有多种节点类型，这些类型由万维网联合会 (W3C) 确定并在 1.1.1 节“The DOM Structure Model”中列出。 下表列出了节点类型、分配给该节点类型的对象以及每种节点类型的简短说明。  
  
|DOM 节点类型|对象|描述|  
|-------------------|------------|-----------------|  
|Document|<xref:System.Xml.XmlDocument>|树中所有节点的容器。 它也称作文档根，文档根并非总是与根元素相同。|  
|DocumentFragment|<xref:System.Xml.XmlDocumentFragment>|包含一个或多个不带任何树结构的节点的临时袋。|  
|DocumentType|<xref:System.Xml.XmlDocumentType>|表示 `<!DOCTYPE…>` 节点。|  
|EntityReference|<xref:System.Xml.XmlEntityReference>|表示非扩展的实体引用文本。|  
|元素|<xref:System.Xml.XmlElement>|表示元素节点。|  
|Attr|<xref:System.Xml.XmlAttribute>|为元素的属性。|  
|ProcessingInstruction|<xref:System.Xml.XmlProcessingInstruction>|为处理指令节点。|  
|注释|<xref:System.Xml.XmlComment>|注释节点。|  
|Text|<xref:System.Xml.XmlText>|属于某个元素或属性的文本。|  
|CDATASection|<xref:System.Xml.XmlCDataSection>|表示 CDATA。|  
|实体|<xref:System.Xml.XmlEntity>|表示 XML 文档（来自内部文档类型定义 (DTD) 子集或来自外部 DTD 和参数实体）中的 `<!ENTITY…>` 声明。|  
|Notation|<xref:System.Xml.XmlNotation>|表示 DTD 中声明的表示法。|  
  
 尽管属性 (attr) 在 W3C DOM 级别 1 的第 1.2 节“基本接口”中作为节点列出，但不能将属性视为任何元素节点的子级。  
  
 下表列出了 W3C 未定义的其他节点类型，但这些类型可作为 XmlNodeType 枚举在 Microsoft .NET Framework 对象模型中使用。 因此，这些节点类型不存在匹配的 DOM 节点类型列。  
  
|节点类型|描述|  
|---------------|-----------------|  
|<xref:System.Xml.XmlDeclaration>|表示声明节点 `<?xml version="1.0"…>`。|  
|<xref:System.Xml.XmlSignificantWhitespace>|表示有效空白（混合内容中的空白）。|  
|<xref:System.Xml.XmlWhitespace>|表示元素内容中的空白。|  
|EndElement|当 XmlReader 到达元素末尾时返回。<br /><br /> 示例 XML： **\</item>**<br /><br /> 有关详细信息，请参阅 <xref:System.Xml.XmlNodeType>。|  
|EndEntity|当 XmlReader 由于 <xref:System.Xml.XmlReader.ResolveEntity%2A> 调用而到达实体替换的末尾时返回。 有关详细信息，请参阅 <xref:System.Xml.XmlNodeType>。|  
  
 若要查看读取 XML 并对节点类型使用 case 构造以打印节点及其内容相关信息的代码示例，请参阅 <xref:System.Xml.XmlSignificantWhitespace.NodeType%2A>。  
  
 若要详细了解节点类型及对应的相当对象名称的对象层次结构，请参阅 [XML 文档对象模型 (DOM) 层次结构](xml-document-object-model-dom-hierarchy.md)。 若要详细了解在节点树中创建的对象，请参阅[将对象层次结构映射到 XML 数据](mapping-the-object-hierarchy-to-xml-data.md)。  
  
## <a name="see-also"></a>请参阅

- [XML 文档对象模型 (DOM)](xml-document-object-model-dom.md)
