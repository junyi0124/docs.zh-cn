---
title: 值的比较
ms.date: 07/20/2015
helpviewer_keywords:
- variables [Visual Basic], comparing values
- Visual Basic code, operators
- Visual Basic code, expressions
- comparison operators [Visual Basic], comparing expressions
- numeric expressions
- operators [Visual Basic], comparison
- expressions [Visual Basic], comparing
ms.assetid: 60da0c76-9458-4afc-97e9-44a7939c064c
ms.openlocfilehash: e424dd58cada8cda250554a4a8870e1900d0fa7d
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91085966"
---
# <a name="value-comparisons-visual-basic"></a>值的比较 (Visual Basic)

比较运算符可用于构造比较数值变量的值的表达式。 这些表达式 `Boolean` 根据比较结果为 true 或 false 返回值。 此类表达式的示例如下所示。  
  
 `45 > 26`  
  
 `26 > 45`  
  
 第一个表达式的计算结果为 `True` ，因为45大于26。 第二个示例的计算结果为 `False` ，因为26不大于45。  
  
 您还可以采用这种方式比较数值表达式。 比较的表达式本身就是复杂表达式，如下面的示例中所示。  
  
 `x / 45 * (y +17) >= System.Math.Sqrt(z) / (p - (x * 16))`  
  
 前面的复杂表达式包括文本、变量和函数调用。 将计算比较运算符两侧的表达式，然后使用比较运算符比较结果值 `>=` 。 如果左侧表达式的值大于或等于右侧表达式的值，则整个表达式的计算结果为 `True` ; 否则，其计算结果为 `False` 。  
  
 用于比较值的表达式最常用于 `If...Then` 构造中，如下面的示例中所示。  
  
 [!code-vb[VbVbalrOperators#84](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#84)]  
  
 `=`符号是比较运算符以及赋值运算符。 用作比较运算符时，它计算左侧的值是否等于右侧的值，如下面的示例中所示。  
  
 [!code-vb[VbVbalrOperators#85](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#85)]  
  
 你还可以在需要值的任何位置使用比较表达式 `Boolean` ，如在 `If` 、 `While` 、 `Loop` 或 `ElseIf` 语句中，或者在赋值或向变量传递值时使用 `Boolean` 。 在下面的示例中，比较表达式返回的值被分配给一个 `Boolean` 变量。  
  
 [!code-vb[VbVbalrOperators#86](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#86)]  
  
## <a name="see-also"></a>请参阅

- [布尔表达式](boolean-expressions.md)
- [运算符和表达式](index.md)
- [Comparison Operators in Visual Basic](comparison-operators.md)
- [如何：计算数值](how-to-calculate-numeric-values.md)
- [Visual Basic 中的运算符优先级](../../../language-reference/operators/operator-precedence.md)
