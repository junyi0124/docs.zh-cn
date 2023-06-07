---
title: 移除 DOM 中的节点内容
ms.date: 03/30/2017
ms.assetid: 615d81a7-f44f-416c-a9ab-bfe03f85e6e4
ms.openlocfilehash: d624e3eff3d61c2b8d312f370a4a11e3aede37e5
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95721486"
---
# <a name="removing-node-content-in-the-dom"></a>移除 DOM 中的节点内容

对于从 <xref:System.Xml.XmlCharacterData> 继承的节点类型，包括 <xref:System.Xml.XmlComment>、<xref:System.Xml.XmlText>、<xref:System.Xml.XmlCDataSection>、<xref:System.Xml.XmlWhitespace> 和 <xref:System.Xml.XmlSignificantWhitespace> 节点类型，可以使用 <xref:System.Xml.XmlCharacterData.DeleteData%2A> 方法移除字符，该方法从节点中删除某个范围的字符。 如果要完全移除内容，则移除包含此内容的节点。 如果要保留节点，但节点内容不正确，则修改内容。 若要了解如何修改节点内容，请参阅[修改 XML 文档中的节点、内容和值](modifying-nodes-content-and-values-in-an-xml-document.md)。  
  
## <a name="see-also"></a>请参阅

- [XML 文档对象模型 (DOM)](xml-document-object-model-dom.md)
