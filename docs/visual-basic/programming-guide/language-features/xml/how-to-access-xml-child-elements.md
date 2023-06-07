---
title: 如何：访问 XML 子元素
ms.date: 07/20/2015
helpviewer_keywords:
- XML axis [Visual Basic], child
- child axis property [Visual Basic]
- XML child axis property [Visual Basic]
- XML [Visual Basic], accessing
ms.assetid: 6689eb36-c471-469f-a82d-099ab8197b25
ms.openlocfilehash: 9c33bec9b9ea865d570bab08f27227129867cce9
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91058295"
---
# <a name="how-to-access-xml-child-elements-visual-basic"></a>如何：访问 XML 子元素 (Visual Basic)

此示例演示如何使用子轴属性访问 XML 元素中具有指定名称的所有 XML 子元素。 具体而言，它使用 <xref:System.Xml.Linq.XElement.Value%2A> 属性获取 `name` 子轴属性返回的集合中第一个元素的值。 `name`子轴属性获取对象中名为的所有子元素 `phone` `contact` 。 此示例还使用 `phone` 子轴属性来访问 `phone` 对象中包含的所有名为的子元素 `contact` 。  
  
## <a name="example"></a>示例  

 [!code-vb[VbXMLSamples#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples4.vb#10)]  
  
## <a name="compile-the-code"></a>编译代码  

 此示例需要：  
  
- 对 <xref:System.Xml.Linq> 命名空间的引用。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Xml.Linq.XContainer.Elements%2A?displayProperty=nameWithType>
- [XML Child Axis Property](../../../language-reference/xml-axis/xml-child-axis-property.md)
- [XML 值属性](../../../language-reference/xml-axis/xml-value-property.md)
- [在 Visual Basic 中访问 XML](accessing-xml.md)
- [XML](index.md)
