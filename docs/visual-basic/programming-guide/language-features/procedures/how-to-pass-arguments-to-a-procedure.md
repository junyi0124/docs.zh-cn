---
title: 如何：将参数传递给过程
ms.date: 07/20/2015
helpviewer_keywords:
- arguments [Visual Basic], passing to procedures
- procedures [Visual Basic], arguments
- procedures [Visual Basic], parameters
- procedure arguments
- Visual Basic code, procedures
- procedure parameters
- procedures [Visual Basic], calling
- argument passing [Visual Basic], procedures
ms.assetid: 08723588-3890-4ddc-8249-79e049e0f241
ms.openlocfilehash: 816d6388a0dbb7ae346074d258ff651c793c5e0e
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91071503"
---
# <a name="how-to-pass-arguments-to-a-procedure-visual-basic"></a>如何：将参数传递给过程 (Visual Basic)

调用过程时，请在括号中使用参数列表的过程名称。 提供与过程定义的每个必需参数相对应的参数，可选择为参数提供参数 `Optional` 。 如果未 `Optional` 在调用中提供参数，则必须包含一个逗号，以便在提供任何后续参数时将其位置标记到参数列表中。  
  
 如果要传递的参数的数据类型不同于其对应参数的参数（例如 `Byte` `String` ），则可以将类型检查开关 ([Option Strict 语句](../../../language-reference/statements/option-strict-statement.md)) 设置为 `Off` 。 如果 `Option Strict` 为 `On` ，则必须使用扩大转换或显式转换关键字。 有关详细信息，请参阅 [扩大和收缩转换](../data-types/widening-and-narrowing-conversions.md) 和 [类型转换函数](../../../language-reference/functions/type-conversion-functions.md)。  
  
 有关详细信息，请参阅 [过程参数和参数](./procedure-parameters-and-arguments.md)。  
  
### <a name="to-pass-one-or-more-arguments-to-a-procedure"></a>将一个或多个自变量传递给过程  
  
1. 在调用语句中，在过程名称后面加上括号。  
  
2. 在括号内，放入参数列表。 为过程定义的每个必需参数包含一个参数，并使用逗号分隔参数。  
  
3. 请确保每个参数都是一个有效的表达式，该表达式的计算结果为可转换为相应参数的过程定义的类型的数据类型。  
  
4. 如果将某个参数定义为 [可选](../../../language-reference/modifiers/optional.md)参数，则可以将其包含在自变量列表中，或将其省略。 如果省略该参数，则该过程将使用为该参数定义的默认值。  
  
5. 如果省略参数的自变量， `Optional` 并且参数列表中后面有另一个参数，则可以在参数列表中用额外的逗号标记省略参数的位置。  
  
     下面的示例调用 Visual Basic <xref:Microsoft.VisualBasic.Interaction.MsgBox%2A> 函数。  
  
     [!code-vb[VbVbcnProcedures#34](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#34)]  
  
     前面的示例提供了必需的第一个参数，即要显示的消息字符串。 它省略了可选的第二个参数的参数，该参数指定要在消息框中显示的按钮。 由于调用不提供值，因此 `MsgBox` 将使用默认值，该默认值 `MsgBoxStyle.OKOnly` 仅显示 **"确定"** 按钮。  
  
     自变量列表中的第二个逗号标记省略的第二个参数的位置，最后一个字符串传递给的可选第三个参数 `MsgBox` ，即要在标题栏中显示的文本。  
  
## <a name="see-also"></a>请参阅

- [Sub 过程](./sub-procedures.md)
- [Function 过程](./function-procedures.md)
- [Property 过程](./property-procedures.md)
- [运算符过程](./operator-procedures.md)
- [如何：为过程定义参数](./how-to-define-a-parameter-for-a-procedure.md)
- [按值和按引用传递参数](./passing-arguments-by-value-and-by-reference.md)
- [递归过程](./recursive-procedures.md)
- [过程重载](./procedure-overloading.md)
- [对象和类](../objects-and-classes/index.md)
- [面向对象的编程 (Visual Basic)](../../concepts/object-oriented-programming.md)
