---
title: 运算符和表达式
ms.date: 07/20/2015
helpviewer_keywords:
- operators [Visual Basic], operands
- operators [Visual Basic]
- operands [Visual Basic], definition
- Visual Basic code, operators
- Visual Basic code, expressions
- operands
- expressions [Visual Basic]
ms.assetid: b86f3131-94ee-448f-96cd-79611e028b26
ms.openlocfilehash: 01e3a53e998774caee8863675b9151a140606852
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91071672"
---
# <a name="operators-and-expressions-in-visual-basic"></a>Visual Basic 中的运算符和表达式

运算符** 是对包含值的一个或多个代码元素执行运算的代码元素。 值元素包括变量、常量、文本、属性、`Function` 和 `Operator` 过程的返回结果以及表达式。  
  
 表达式** 是一系列与运算符结合使用的值元素，将生成新值。 运算符通过执行计算、比较或其他运算来处理值元素。  
  
## <a name="types-of-operators"></a>运算符类型  

 Visual Basic 提供以下类型的运算符：  
  
- [算术运算符](arithmetic-operators.md)：对数字值执行常见计算，包括更改位模式。  
  
- [比较运算符](comparison-operators.md)：比较两个表达式，并返回 `Boolean` 值来表示比较结果。  
  
- [串联运算符](concatenation-operators.md) 将多个字符串联接为一个字符串。  
  
- [Visual Basic 中的逻辑和位运算运算符](logical-and-bitwise-operators.md)：合并 `Boolean` 或数字值，并以值的形式返回数据类型相同的结果。  
  
 与运算符合并的值元素称为相应运算符的操作数**。 运算符与值元素共同构成了表达式，构成语句** 的赋值运算符除外。 有关详细信息，请参阅[语句](../statements.md)。  
  
## <a name="evaluation-of-expressions"></a>表达式计算  

 表达式的最终结果表示采用常见数据类型（如 `Boolean`、`String` 或数字类型）的值。  
  
 下面展示了表达式示例。  
  
 `5 + 4`  
  
 `' The preceding expression evaluates to 9.`  
  
 `15 * System.Math.Sqrt(9) + x`  
  
 `' The preceding expression evaluates to 45 plus the value of x.`  
  
 `"Concat" & "ena" & "tion"`  
  
 `' The preceding expression evaluates to "Concatenation".`  
  
 `763 < 23`  
  
 `' The preceding expression evaluates to False.`  
  
 多个运算符可以在一个表达式或语句中执行运算，如以下示例所示。  
  
 [!code-vb[VbVbalrOperators#56](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#56)]  
  
 在前面的示例中，Visual Basic 在赋值运算符右侧的表达式中执行运算 (`=`) ，然后将结果值分配给左侧的变量 `x` 。 对于可以合并到表达式中的运算符数量没有实际限制，但需要了解 [Visual Basic 中的运算符优先级](../../../language-reference/operators/operator-precedence.md)，以确保结果符合预期。  

## <a name="see-also"></a>请参阅

- [运算符](../../../language-reference/operators/index.md)
- [运算符的有效组合](efficient-combination-of-operators.md)
- [语句](../../../language-reference/statements/index.md)
