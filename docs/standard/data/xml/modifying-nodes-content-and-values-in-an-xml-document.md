---
title: 修改 XML 文档中的节点、内容和值
ms.date: 03/30/2017
ms.assetid: 761773e0-db72-4986-b9f5-a522213d8397
ms.openlocfilehash: c3f3f4a279f3bcb967e018abafbd2e1f7a0af473
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95714466"
---
# <a name="modifying-nodes-content-and-values-in-an-xml-document"></a>修改 XML 文档中的节点、内容和值

有多种方法可以修改文档中的节点和内容。 你可以：  
  
- 使用 <xref:System.Xml.XmlNode.Value%2A> 属性更改节点的值。  
  
- 通过用新节点替换节点来修改全部节点集。 此操作使用 <xref:System.Xml.XmlNode.InnerXml%2A> 属性完成。  
  
- 使用 <xref:System.Xml.XmlNode.RemoveChild%2A> 方法用新节点替换现有节点。  
  
- 使用 <xref:System.Xml.XmlCharacterData>、<xref:System.Xml.XmlCharacterData.AppendData%2A> 或 <xref:System.Xml.XmlCharacterData.InsertData%2A> 方法向从 <xref:System.Xml.XmlCharacterData.ReplaceData%2A> 类继承的节点添加附加字符。  
  
- 通过在从 <xref:System.Xml.XmlCharacterData.DeleteData%2A> 继承的节点类型上使用 <xref:System.Xml.XmlCharacterData> 方法移除某个范围的字符，以修改内容。  
  
 更改节点值的一个简单方法是使用 `node.Value = "new value";`。 下表列出了此单个代码行作用于的节点类型，以及对于该节点类型将更改的确切数据。  
  
|节点类型|更改的数据|  
|---------------|------------------|  
|特性|属性的值。|  
|CDATASection|CDATA 节的内容。|  
|注释|注释的内容。|  
|ProcessingInstruction|内容（不包括目标）。|  
|Text|文本的内容。|  
|XmlDeclaration|声明的内容，不包括 `<?xml` 和 `?>` 标记。|  
|Whitespace|空白的值。 可以将该值设置为四个可识别的 XML 空白字符之一：空格、制表符、CR 或 LF。|  
|SignificantWhitespace|有效空白的值。 可以将该值设置为四个可识别的 XML 空白字符之一：空格、制表符、CR 或 LF。|  
  
 该表中未列出的任何节点类型都不是设置了值的有效节点类型。 设置任何其他节点类型的值都将引发 <xref:System.InvalidOperationException>。  
  
 <xref:System.Xml.XmlNode.InnerXml%2A> 属性更改当前节点的子节点标记。 设置此属性将用给定字符串的分析内容替换子节点。 分析在当前命名空间上下文中完成。 此外，<xref:System.Xml.XmlNode.InnerXml%2A> 移除多余的命名空间声明。 因此，大量的剪切和粘贴操作并不会使文档的大小因多余的命名空间声明而增加。 有关显示命名空间对 <xref:System.Xml.XmlNode.InnerXml%2A> 操作的影响的代码示例，请参见 <xref:System.Xml.XmlDocument.InnerXml%2A> 属性。  
  
 当使用 <xref:System.Xml.XmlCharacterData.ReplaceData%2A> 和 <xref:System.Xml.XmlNode.RemoveChild%2A> 方法时，这两个方法返回已替换或移除的节点。 此节点可以重新插入 XML 文档对象模型 (DOM) 中的任何其他位置。 <xref:System.Xml.XmlCharacterData.ReplaceData%2A> 方法对插入到文档中的节点执行两个验证检查。 第一个检查确保该节点成为某个节点的子级，这个节点可具有其类型的子节点。 第二个检查确保插入的节点不是它成为其子级的节点的上级。 违犯这两个条件中的任何一个都将引发 <xref:System.InvalidOperationException>。  
  
 向可编辑的节点中添加或从中移除只读子级是有效的。 然而，试图修改只读节点本身将引发 <xref:System.InvalidOperationException> 异常。 修改 <xref:System.Xml.XmlEntityReference> 节点的子级便属于这种情况。 该子级是只读的，因此无法修改。 任何修改它们的尝试都将引发 <xref:System.InvalidOperationException>。  
  
## <a name="see-also"></a>请参阅

- [XML 文档对象模型 (DOM)](xml-document-object-model-dom.md)
