---
title: XML 文本中的空白
ms.date: 07/20/2015
helpviewer_keywords:
- white space [XML in Visual Basic]
- XML literals [Visual Basic], white space
ms.assetid: dfe3a9ff-d69a-418e-a6b5-476f4ed84219
ms.openlocfilehash: 5db8f92117e77d96eab34f28758546393e2afca0
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91099094"
---
# <a name="white-space-in-xml-literals-visual-basic"></a>XML 文本中的空白 (Visual Basic)

Visual Basic 编译器在创建对象时仅合并 XML 文本中的有效空白字符 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] 。 不包含无意义的空白字符。  
  
## <a name="significant-and-insignificant-white-space"></a>重要且无意义的空白  

 XML 文本中的空格字符仅在三个区域中很重要：  
  
- 在属性值中。  
  
- 当它们是元素的文本内容的一部分并且文本还包含其他字符时。  
  
- 当它们位于元素的文本内容的嵌入式表达式中时。  
  
 否则，编译器会将空白字符视为不重要的，并且不会在文本的对象中包括 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] 。  
  
 若要在 XML 文本中包含无意义的空白，请使用包含带有空格的字符串文字的嵌入式表达式。  
  
> [!NOTE]
> 如果 `xml:space` 属性出现在 XML 元素文本中，则 Visual Basic 编译器在对象中包括属性 <xref:System.Xml.Linq.XElement> ，但添加此属性不会更改编译器处理空白的方式。  
  
## <a name="examples"></a>示例  

 下面的示例包含两个 XML 元素：外部和内部。 这两个元素在其文本内容中都包含空格。 外部元素中的空白是无意义的，因为它仅包含空格和 XML 元素。 内部元素中的空格非常重要，因为它包含空格和文本。  
  
 [!code-vb[VbXMLSamples#29](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples13.vb#29)]  
  
 运行时，此代码显示以下文本。  
  
```xml  
<outer>  
  <inner>  
                                          Inner text  
                                      </inner>  
</outer>  
```  
  
## <a name="see-also"></a>请参阅

- [在 Visual Basic 中创建 XML](creating-xml.md)
