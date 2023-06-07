---
title: 字符串操作方法的类型
ms.date: 07/20/2015
helpviewer_keywords:
- strings [Visual Basic], manipulating [Visual Basic]
- string manipulation
ms.assetid: 905055cd-7f50-48fb-9eed-b0995af1dc1f
ms.openlocfilehash: c44f02880858b8a9fc1f0e70c3226623d05baa3a
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91072465"
---
# <a name="types-of-string-manipulation-methods-in-visual-basic"></a>字符串操作方法的类型 (Visual Basic)

可以通过多种不同的方法来分析和操作字符串。 某些方法是 Visual Basic 语言的一部分，其他方法是类中的固有方法 `String` 。  
  
## <a name="visual-basic-language-and-the-net-framework"></a>Visual Basic 语言和 .NET Framework  

 Visual Basic 方法用作语言的固有功能。 在代码中，可以使用它们而无需进行限定。 下面的示例演示了 Visual Basic 字符串操作命令的典型用法：  
  
 [!code-vb[VbVbalrStrings#44](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#44)]  
  
 在此示例中， `Mid` 函数对执行直接操作 `aString` ，并将值分配给 `bString` 。  
  
 有关 Visual Basic 字符串操作方法的列表，请参阅 [字符串操作摘要](../../../language-reference/keywords/string-manipulation-summary.md)。  
  
### <a name="shared-methods-and-instance-methods"></a>共享方法和实例方法  

 还可以通过类的方法操作字符串 `String` 。 中有两种类型的方法 `String` ： *共享* 方法和 *实例* 方法。  
  
#### <a name="shared-methods"></a>共享方法  

 共享方法是源自 `String` 类本身，不需要该类的实例即可工作的方法。 可以使用类的名称限定这些方法 (`String`) ，而不是使用类的实例 `String` 。 例如：  
  
 [!code-vb[VbVbalrStrings#45](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#45)]  
  
 在前面的示例中， <xref:System.String.Copy%2A?displayProperty=nameWithType> 方法是一个静态方法，它在给定的表达式上操作，并将生成的值分配给 `bString` 。  
  
#### <a name="instance-methods"></a>实例方法  

 相反，实例方法是特定实例的主干， `String` 必须使用实例名称进行限定。 例如：  
  
 [!code-vb[VbVbalrStrings#46](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#46)]  
  
 在此示例中， <xref:System.String.Substring%2A?displayProperty=nameWithType> 方法是 (实例的方法 `String` ， `aString`) 。 它在上执行操作 `aString` ，并将该值分配给 `bString` 。  
  
 有关详细信息，请参阅类的文档 <xref:System.String> 。  
  
## <a name="see-also"></a>请参阅

- [字符串介绍 (Visual Basic)](introduction-to-strings.md)
