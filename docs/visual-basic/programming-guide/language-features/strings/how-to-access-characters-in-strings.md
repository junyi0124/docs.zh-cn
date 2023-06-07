---
title: 如何：访问字符串中的字符
ms.date: 07/20/2015
helpviewer_keywords:
- strings [Visual Basic], accessing characters
- characters [Visual Basic], accessing in strings
ms.assetid: 02c5206c-ffab-494d-b648-3b2ea358dc34
ms.openlocfilehash: 4ea37c4393a8ece5513327b22c6c0ba4b027c781
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91059179"
---
# <a name="how-to-access-characters-in-strings-in-visual-basic"></a>如何：在 Visual Basic 中访问字符串中的字符

此示例演示如何使用 <xref:System.String.Chars%2A> 属性访问字符串中指定位置处的字符。  
  
## <a name="example"></a>示例  

 有时，请在字符串中包含有关字符串中的字符和这些字符的位置的数据。 你可以将字符串视为)  (实例的字符数组 `Char` ; 你可以通过属性引用该字符的索引来检索特定字符 <xref:System.String.Chars%2A> 。  
  
 [!code-vb[VbVbalrStrings#49](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#49)]  
  
 `index`属性的参数 <xref:System.String.Chars%2A> 是从零开始的。  
  
## <a name="robust-programming"></a>可靠编程  

 <xref:System.String.Chars%2A>属性返回指定位置处的字符。 但是，某些 Unicode 字符可以由多个字符表示。 有关如何使用 Unicode 字符的详细信息，请参阅 [如何：将字符串转换为字符数组](how-to-convert-a-string-to-an-array-of-characters.md)。  
  
 <xref:System.String.Chars%2A> <xref:System.IndexOutOfRangeException> 如果 `index` 参数大于或等于字符串的长度，则属性引发异常; 如果小于零，则引发异常。  
  
## <a name="see-also"></a>请参阅

- <xref:System.String.Chars%2A>
- [如何：将字符串转换为字符数组](how-to-convert-a-string-to-an-array-of-characters.md)
- [Visual Basic 中字符串和其他数据类型间的转换](converting-between-strings-and-other-data-types.md)
- [字符串](index.md)
