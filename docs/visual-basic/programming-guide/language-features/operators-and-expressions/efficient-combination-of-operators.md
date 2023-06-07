---
title: 运算符的有效组合
ms.date: 07/20/2015
helpviewer_keywords:
- expressions [Visual Basic], parentheses
- operators [Visual Basic], associativity
- expressions [Visual Basic], operators
- operators [Visual Basic], precedence
- Visual Basic code, operators
- Visual Basic code, expressions
- operators [Visual Basic], complex expressions
- expressions [Visual Basic], complex
- parentheses [Visual Basic], complex expressions
- numeric expressions
ms.assetid: bd22340e-b5be-458b-8772-3916c02309a4
ms.openlocfilehash: 9ba6be8e1dd03c0589f712b0e9b39258953cd223
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91077080"
---
# <a name="efficient-combination-of-operators-visual-basic"></a>运算符的有效组合 (Visual Basic)

复杂表达式可以包含多个不同的运算符。 下面的示例对此进行了演示。  
  
 `x = (45 * (y + z)) ^ (2 / 85) * 5 + z`  
  
 如前面的示例所示，创建复杂表达式需要透彻地理解运算符优先级的规则。 有关详细信息，请参阅 [Visual Basic 中的运算符优先级](../../../language-reference/operators/operator-precedence.md)。  
  
## <a name="parenthetical-expressions"></a>括号中的表达式  

 通常，你希望以不同于运算符优先级确定的顺序执行操作。 请看下面的示例。  
  
 `x = z * y + 4`  
  
 前面的示例 `z` 将乘以 `y` ，然后将结果添加到 `4` 。 但是，如果想要在 `y` 将 `4` 结果与相乘之前添加 and `z` ，则可以通过使用括号覆盖普通运算符优先级。 通过将表达式括在括号中，可以强制首先计算表达式，而不考虑运算符优先级。 若要强制前面的示例先执行加法操作，可以重写它，如以下示例中所示。  
  
 `x = z * (y + 4)`  
  
 前面的示例添加 `y` and `4` ，然后将 sum 相乘 `z` 。  
  
### <a name="nested-parenthetical-expressions"></a>嵌套的括号表达式  

 可以将表达式嵌套在多个级别的括号中，以便进一步覆盖优先级。 首先计算最深层嵌套在括号中的表达式，然后计算下一个最深层嵌套的表达式，依此类推到最深层嵌套的表达式，最后将表达式括在括号内。 下面的示例对此进行了演示。  
  
 `x = (z * 4) ^ (y * (z + 2))`  
  
 在前面的示例中， `z + 2` 首先计算，然后是其他括号表达式。 在此示例中，求幂的优先级通常高于加法或乘法，因为其他表达式括在括号中。  
  
## <a name="see-also"></a>请参阅

- [算术运算符 (Visual Basic)](arithmetic-operators.md)
- [Comparison Operators in Visual Basic](comparison-operators.md)
- [Visual Basic 中的逻辑运算符和位运算符](logical-and-bitwise-operators.md)
- [逻辑/按位运算符 (Visual Basic)](../../../language-reference/operators/logical-bitwise-operators.md)
- [布尔表达式](boolean-expressions.md)
- [值的比较](value-comparisons.md)
- [如何：计算数值](how-to-calculate-numeric-values.md)
- [Visual Basic 中的运算符优先级](../../../language-reference/operators/operator-precedence.md)
