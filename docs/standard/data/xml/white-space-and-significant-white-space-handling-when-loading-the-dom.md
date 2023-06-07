---
title: 加载 DOM 时的空白和有效空白处理
ms.date: 03/30/2017
ms.assetid: 1b141a0a-50d8-4ebd-83cd-a84449bb22b2
ms.openlocfilehash: 6282b47e8a63143e32df39db02e79e7b44d38275
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95675550"
---
# <a name="white-space-and-significant-white-space-handling-when-loading-the-dom"></a>加载 DOM 时的空白和有效空白处理

加载文档时，可以将选项设置为暂留空格，并在文档树中创建 XmlWhitespace  节点。 若要创建空格节点，请将 PreserveWhitespace  属性设置为 true。 如果将此属性设置为默认值 false  ，不会创建空格节点。 无论 PreserveWhitespace  标志的设置如何，始终暂留重要的空格节点，并且始终在内存中创建 XmlSignificantWhitespace  节点来表示此数据。  
  
 如果文档是从读取器加载，仅当 XmlTextReader  上的 WhitespaceHandling  属性未设置为 WhitespaceHandling.None  时，XmlDocument  类的 PreserveWhitespace  标志属性设置才会影响 XmlWhitespace  的创建。 读取器上的 WhitespaceHandling  属性值优先于 XmlDocument  上的此标志设置。 若要详细了解 XmlSignificantWhitespace  ，请参阅 <xref:System.Xml.XmlSignificantWhitespace>。  
  
## <a name="see-also"></a>请参阅

- [XML 文档对象模型 (DOM)](xml-document-object-model-dom.md)
