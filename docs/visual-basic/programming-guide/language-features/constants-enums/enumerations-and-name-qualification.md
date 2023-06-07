---
title: 枚举和名称限定
ms.date: 07/20/2015
helpviewer_keywords:
- declarations [Visual Basic], enumerations
- Imports statement [Visual Basic], namespace declarations
- declaring namespaces [Visual Basic], enumerations
- name collisions
- ambiguous names [Visual Basic], enumerations
- enumerations [Visual Basic], name qualification
- names [Visual Basic], avoiding conflicts
- namespaces [Visual Basic], declaring
- naming conflicts, enumerations
- naming conflicts, qualifying names
- declaring enumerations
- references [Visual Basic], enumeration members
- naming conventions [Visual Basic], naming conflicts
- declarations [Visual Basic], namespaces
ms.assetid: 08ba2738-df52-4140-bc55-f57c871c9b73
ms.openlocfilehash: 6e067d72e557b97f8626b148e173e3d1583f92b8
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91086265"
---
# <a name="enumerations-and-name-qualification-visual-basic"></a>枚举和名称限定 (Visual Basic)

通常，在引用枚举成员时，必须使用枚举名称限定成员名称。 例如，若要引用枚举的 `Sunday` 成员 `Days` ，请使用以下语法：  
  
 [!code-vb[VbEnumsTask#18](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#18)]  
  
## <a name="using-the-imports-statement"></a>使用 Imports 语句  

 可以通过将 `Imports` 语句添加到代码的命名空间声明部分来避免使用完全限定的名称，如以下示例中所示：  
  
 [!code-vb[VbEnumsTask#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class1.vb#22)]  
  
 `Imports`语句从被引用项目和程序集中导入命名空间名称，并从与该语句出现的模块相同的项目中导入命名空间名称。 添加此语句后，无需限定即可引用枚举成员，如以下示例中所示：  
  
 [!code-vb[VbEnumsTask#24](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class1.vb#24)]  
  
 通过组织枚举中的相关常量集，可以在不同的上下文中使用相同的常量名称。 例如，可以对和枚举中的 weekday 常量使用相同的名称 `Days` `WorkDays` 。 如果对枚举使用 `Imports` 语句，则必须小心，以避免不明确的引用。 请考虑以下示例：  
  
 [!code-vb[VbEnumsTask#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class1.vb#22)]  
  
 [!code-vb[VbEnumsTask#25](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class1.vb#25)]  
  
 假定这 `Monday` 是 `Days` 枚举和枚举的成员 `Workdays` ，则此代码会生成编译器错误。 若要避免引用单个常量时出现不明确的引用，请使用其枚举来限定常量名称。 下面的代码引用 `Saturday` 和枚举中的常量 `Days` `WorkDays` 。  
  
 [!code-vb[VbEnumsTask#32](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#32)]  
  
## <a name="see-also"></a>请参阅

- [常量和枚举](../../../language-reference/constants-and-enumerations.md)
- [如何：声明枚举](how-to-declare-enumerations.md)
- [如何：引用枚举成员](how-to-refer-to-an-enumeration-member.md)
- [如何：在 Visual Basic 中循环访问枚举](how-to-iterate-through-an-enumeration.md)
- [如何：确定与枚举值关联的字符串](how-to-determine-the-string-associated-with-an-enumeration-value.md)
- [何时使用枚举](when-to-use-an-enumeration.md)
- [常数和文本数据类型](constant-and-literal-data-types.md)
- [Enum 语句](../../../language-reference/statements/enum-statement.md)
- [Imports 语句（.NET 命名空间和类型）](../../../language-reference/statements/imports-statement-net-namespace-and-type.md)
- [数据类型](../../../language-reference/data-types/index.md)
