---
title: 如何：确定与枚举值关联的字符串
ms.date: 07/20/2015
helpviewer_keywords:
- enumerations [Visual Basic]
- strings [Visual Basic], enumeration values
- values [Visual Basic], enumeration members
ms.assetid: 9253e7c8-579c-49a2-8f26-392b20ea99eb
ms.openlocfilehash: 4138759bfbb049b77406fc536219b40d3ed9e2a5
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91058764"
---
# <a name="how-to-determine-the-string-associated-with-an-enumeration-value-visual-basic"></a>如何：确定与枚举值关联的字符串 (Visual Basic)

<xref:System.Enum.GetValues%2A>和 <xref:System.Enum.GetNames%2A> 方法允许您确定与枚举成员关联的字符串和值。  
  
### <a name="to-determine-the-string-associated-with-an-enumeration"></a>确定与枚举关联的字符串  
  
- 使用 <xref:System.Enum.GetNames%2A> 方法检索与枚举成员关联的字符串。 此示例声明一个枚举， `flavorEnum` 然后使用 <xref:System.Enum.GetNames%2A> 方法显示与每个成员关联的字符串。  
  
     [!code-vb[VbEnumsTask#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#2)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.Enum.GetValues%2A>
- <xref:System.Enum.GetNames%2A>
- <xref:System.Enum>
- [如何：声明枚举](how-to-declare-enumerations.md)
- [如何：引用枚举成员](how-to-refer-to-an-enumeration-member.md)
- [枚举和名称限定](enumerations-and-name-qualification.md)
- [如何：在 Visual Basic 中循环访问枚举](how-to-iterate-through-an-enumeration.md)
- [何时使用枚举](when-to-use-an-enumeration.md)
- [Enum 语句](../../../language-reference/statements/enum-statement.md)
