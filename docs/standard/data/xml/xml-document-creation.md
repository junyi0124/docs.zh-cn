---
title: 创建 XML 文档
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 877e9c62-b082-4bfb-bc5b-f47297eb30ef
ms.openlocfilehash: 18c391e33e0c43f2407ccbc87c12b6c25a12509d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95686522"
---
# <a name="xml-document-creation"></a>创建 XML 文档

有两种创建 XML 文档的方法。 一种方法是，创建不含参数的 XmlDocument  。 另一种方法是，创建 XmlDocument  ，并向它传递 XmlNameTable 参数。 下面的示例展示了如何不使用任何参数新建空 XmlDocument  。  
  
```vb  
Dim doc As New XmlDocument()  
```  
  
```csharp  
XmlDocument doc = new XmlDocument();  
```  
  
 创建文档后，可以使用 Load  方法在文档中加载字符串、流、URL、文本读取器或 XmlReader  派生类中的数据。 还有另一种加载方法 LoadXML  方法，可用于从字符串中读取 XML。 若要详细了解各种 Load  方法，请参阅[将 XML 文档读取到 DOM 中](reading-an-xml-document-into-the-dom.md)。  
  
 有一个类名为 XmlNameTable  。 此类是原子化字符串对象的表。 该表使 XML 分析器可以高效地对 XML 文档中所有重复的元素和属性的名称使用相同的字符串对象。 XmlNameTable  在文档创建（如上所示）时自动创建，并在文档加载时加载属性名和元素名称。 如果已有包含名称表的文档，且这些名称在另一个文档中很有用，可以使用需要使用 XmlNameTable  参数的 Load  方法新建文档。 使用此方法创建文档时，它使用现有 XmlNameTable  ，其中包含已从其他文档加载到其中的所有属性和元素。 它可用于有效地比较元素和属性的名称。 若要详细了解 XmlNameTable  ，请参阅[使用 XmlNameTable 比较对象](object-comparison-using-xmlnametable.md)。 有关参考，请参阅 <xref:System.Xml.XmlNameTable>。  
  
## <a name="see-also"></a>请参阅

- [XML 文档对象模型 (DOM)](xml-document-object-model-dom.md)
