---
title: 如何：将字符串转换为字符数组
ms.date: 07/20/2015
helpviewer_keywords:
- character arrays [Visual Basic], converting strings
- arrays [Visual Basic], converting strings to
- examples [Visual Basic], string conversion
- strings [Visual Basic], converting to arrays
- string conversion [Visual Basic], arrays
ms.assetid: 1b54b686-ab29-413b-adce-6bd5422376eb
ms.openlocfilehash: e1f634fcdb23f16e794449f8fe7b53c451c8c5b8
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91059166"
---
# <a name="how-to-convert-a-string-to-an-array-of-characters-in-visual-basic"></a>如何：在 Visual Basic 中将字符串转换为字符数组

有时，对字符串中的字符和这些字符在字符串中的位置（例如在分析字符串时）使用数据是非常有用的。 此示例演示如何通过调用字符串的方法来获取字符串中的字符数组 <xref:System.String.ToCharArray%2A> 。  
  
## <a name="example"></a>示例  

 此示例演示如何将字符串拆分为 `Char` 数组，以及如何将字符串拆分为 `String` 其 Unicode 文本字符的数组。 此区别的原因是 Unicode 文本字符可以由两个或更多字符组成 `Char` (如代理项对或组合字符序列) 。 有关详细信息，请参阅 <xref:System.Globalization.TextElementEnumerator> 和 [Unicode 标准](https://www.unicode.org/standard/standard.html)。  
  
 [!code-vb[VbVbalrStrings#75](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class4.vb#75)]  
  
## <a name="example"></a>示例  

 更难将字符串拆分为其 Unicode 文本字符，但如果需要有关字符串的可视化表示形式的信息，则这是必需的。 此示例使用 <xref:System.Globalization.StringInfo.SubstringByTextElements%2A> 方法获取有关构成字符串的 Unicode 文本字符的信息。  
  
 [!code-vb[VbVbalrStrings#76](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class4.vb#76)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.String.Chars%2A>
- <xref:System.Globalization.StringInfo?displayProperty=nameWithType>
- [如何：访问字符串中的字符](how-to-access-characters-in-strings.md)
- [Visual Basic 中字符串和其他数据类型间的转换](converting-between-strings-and-other-data-types.md)
- [字符串](index.md)
