---
title: 如何：循环访问枚举
ms.date: 07/20/2015
helpviewer_keywords:
- arrays [Visual Basic], iterating
- enumerations [Visual Basic], iterating
- ListBox control [Windows Forms], populating from an enumeration
ms.assetid: e5aa10eb-cfcd-4a3b-8e76-f06b8f2002be
ms.openlocfilehash: 21c170d4708b90987a3f1e9c18969b8803fcdbe0
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91058711"
---
# <a name="how-to-iterate-through-an-enumeration-in-visual-basic"></a>如何：在 Visual Basic 中循环访问枚举

枚举提供了使用相关常量集以及将常量值与名称相关联的一个便捷方法。 若要循环访问枚举，可以使用方法将它移动到数组中 <xref:System.Enum.GetValues%2A> 。 你还可以使用语句来循环访问枚举 `For...Each` ，方法是使用 <xref:System.Enum.GetNames%2A> 或 <xref:System.Enum.GetValues%2A> 方法来提取字符串或数字值。  
  
### <a name="to-iterate-through-an-enumeration"></a>循环访问枚举  
  
- 声明一个数组，然后使用方法将枚举转换为它，就 <xref:System.Enum.GetValues%2A> 像传递任何其他变量一样。 下面的示例在 <xref:Microsoft.VisualBasic.FirstDayOfWeek> 循环访问枚举时显示枚举的每个成员。  
  
     [!code-vb[VbEnumsTask#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#7)]  
  
## <a name="see-also"></a>请参阅

- [枚举概述](enumerations-overview.md)
- [如何：声明枚举](how-to-declare-enumerations.md)
- [何时使用枚举](when-to-use-an-enumeration.md)
- [如何：确定与枚举值关联的字符串](how-to-determine-the-string-associated-with-an-enumeration-value.md)
- [如何：引用枚举成员](how-to-refer-to-an-enumeration-member.md)
- [枚举和名称限定](enumerations-and-name-qualification.md)
- [数组](../arrays/index.md)
