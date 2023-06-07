---
title: 逻辑运算符和位运算符
ms.date: 07/20/2015
helpviewer_keywords:
- short-circuiting
- Boolean expressions
- logical operators [Visual Basic], Boolean expressions
- operators [Visual Basic], logical
- AndAlso operator [Visual Basic]
- Not operator [Visual Basic], Boolean expressions
- Xor operator [Visual Basic], Boolean expressions
- And operator [Visual Basic], logical operators
- logical operators [Visual Basic]
- expressions [Visual Basic], Boolean
- Or operator [Visual Basic], logical operators
- Visual Basic code, operators
- short-circuiting [Visual Basic], logical operators
- logical operators [Visual Basic], short-circuiting
- Visual Basic code, expressions
- logical operators [Visual Basic], binary
- OrElse operator [Visual Basic]
- logical operators [Visual Basic], unary
ms.assetid: ca474e13-567d-4b1d-a18b-301433705e57
ms.openlocfilehash: c15b9337f262563941699c0ff8fe5219ca6a5c93
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91085992"
---
# <a name="logical-and-bitwise-operators-in-visual-basic"></a>Visual Basic 中的逻辑运算符和位运算符

逻辑运算符比较 `Boolean` 表达式并返回 `Boolean` 结果。 `And`、、 `Or` `AndAlso` 、 `OrElse` 和 `Xor` 运算符都是*二进制*，因为运算符采用两个操作数，而 `Not` 运算符是*一元*，因为它采用单个操作数。 其中一些运算符还可以对整数值执行按位逻辑运算。  
  
## <a name="unary-logical-operator"></a>一元逻辑运算符  

 [Not 运算符](../../../language-reference/operators/not-operator.md)对表达式执行*negation*逻辑非运算 `Boolean` 。 它生成其操作数的逻辑相反。 如果表达式的计算结果为 `True` ，则 `Not` 返回 `False` ; 如果表达式的计算结果为， `False` `Not` `True` 则返回。 下面的示例对此进行了演示。  
  
 [!code-vb[VbVbalrOperators#77](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#77)]  
  
## <a name="binary-logical-operators"></a>二元逻辑运算符  

 [And 运算符](../../../language-reference/operators/and-operator.md)对两个表达式执行逻辑与*运算* `Boolean` 。 如果两个表达式的计算结果都为 `True` ，则 `And` 返回 `True` 。 如果至少有一个表达式的计算结果为 `False` ，则 `And` 返回 `False` 。  
  
 [Or 运算符](../../../language-reference/operators/or-operator.md)对两个表达式执行*逻辑或**运算* `Boolean` 。 如果两个表达式的计算结果为 `True` ，或均为计算结果 `True` ，则 `Or` 返回 `True` 。 如果两个表达式的计算结果均为 `True` ，则 `Or` 返回 `False` 。  
  
 [Xor 运算符](../../../language-reference/operators/xor-operator.md)对两个表达式执行逻辑*异*运算 `Boolean` 。 如果恰好一个表达式的计算结果为 `True` ，而不是同时返回，则 `Xor` 返回 `True` 。 如果两个表达式的计算结果都为 `True` ，或者都为 `False` ，则 `Xor` 返回 `False` 。  
  
 下面的示例演示了 `And` 、 `Or` 和 `Xor` 运算符。  
  
 [!code-vb[VbVbalrOperators#78](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#78)]  
  
## <a name="short-circuiting-logical-operations"></a>短路逻辑运算  

 [AndAlso 运算符](../../../language-reference/operators/andalso-operator.md)与运算符非常类似 `And` ，因为它还对两个表达式执行逻辑与运算 `Boolean` 。 这两者之间的主要区别在于 `AndAlso` 表现 *短路* 行为。 如果表达式中的第一个表达式的 `AndAlso` 计算结果为 `False` ，则不会计算第二个表达式，因为它不能更改最终结果，并 `AndAlso` 返回 `False` 。  
  
 同样， [OrElse 运算符](../../../language-reference/operators/orelse-operator.md) 对两个表达式执行短路逻辑析取 `Boolean` 。 如果表达式中的第一个表达式的 `OrElse` 计算结果为 `True` ，则不会计算第二个表达式，因为它不能更改最终结果，并 `OrElse` 返回 `True` 。  
  
### <a name="short-circuiting-trade-offs"></a>短路权衡  

 短路可以通过不计算无法更改逻辑操作结果的表达式来提高性能。 但是，如果该表达式执行其他操作，则短路将跳过这些操作。 例如，如果表达式包含对过程的调用，则在 `Function` 表达式为短路时不会调用该过程，并且中包含的任何其他代码都不 `Function` 会运行。 因此，该函数可能会偶尔运行，并且可能无法正确测试。 或程序逻辑可能依赖于中的代码 `Function` 。  
  
 下面的示例演示了 `And` 、 `Or` 和其短路对应项之间的差异。  
  
 [!code-vb[VbVbalrOperators#81](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#81)]  
  
 [!code-vb[VbVbalrOperators#80](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#80)]  
  
 [!code-vb[VbVbalrOperators#79](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#79)]  
  
 在前面的示例中，请注意，当调用短路时，中的一些重要代码 `checkIfValid()` 不会运行。 `If`即使返回，第一条语句 `checkIfValid()` 也 `12 > 45` 会调用 `False` ，因为 `And` 不会进行短路。 第二个 `If` 语句不调用 `checkIfValid()` ，因为当 `12 > 45` 返回时 `False` ，会 `AndAlso` 将第二个表达式短路。 `If`即使返回，第三条语句 `checkIfValid()` 也 `12 < 45` 会调用 `True` ，因为 `Or` 不会进行短路。 第四个 `If` 语句不调用 `checkIfValid()` ，因为当 `12 < 45` 返回时 `True` ，会 `OrElse` 将第二个表达式短路。  
  
## <a name="bitwise-operations"></a>按位运算  

 按位运算以 binary (第 2) 形式计算两个整数值。 它们比较相应位置上的位，然后基于比较分配值。 下面的示例阐释 `And` 运算符。  
  
 [!code-vb[VbVbalrConcepts#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrConcepts/VB/Class1.vb#2)]  
  
 前面的示例将的值设置 `x` 为1。 出现这种情况的原因如下：  
  
- 值被视为 binary：  
  
     二进制格式的 3 = 011  
  
     二进制格式的 5 = 101  
  
- `And`运算符比较二进制表示形式，一次 (位) 一个二进制位置。 如果给定位置处的两个位均为1，则将1置于结果中的该位置。 如果任一位为0，则将0放入结果中的该位置。 在前面的示例中，此过程如下所示：  
  
     011 (3 二进制格式)   
  
     101 (二进制格式的 5)   
  
     001以二进制格式 (结果)   
  
- 结果被视为 decimal。 值001是1的二进制表示形式，因此 `x` = 1。  
  
 按位 `Or` 运算类似，不同之处在于，如果两个比较位中有一个或两个均为1，则将1分配给结果位。 `Xor` 如果两个比较位 (都不) 为1，则将1指定给结果位。 `Not` 采用单个操作数并反转所有位（包括符号位），并将该值分配给结果。 这意味着，对于有符号正数， `Not` 始终返回负值，对于负数， `Not` 始终返回正值或零值。  
  
 `AndAlso`和 `OrElse` 运算符不支持按位运算。  
  
> [!NOTE]
> 只能对整数类型执行按位运算。 必须先将浮点值转换为整数类型，然后才能继续执行位运算。  
  
## <a name="see-also"></a>请参阅

- [逻辑/按位运算符 (Visual Basic)](../../../language-reference/operators/logical-bitwise-operators.md)
- [布尔表达式](boolean-expressions.md)
- [算术运算符 (Visual Basic)](arithmetic-operators.md)
- [Comparison Operators in Visual Basic](comparison-operators.md)
- [串联运算符 (Visual Basic)](concatenation-operators.md)
- [运算符的有效组合](efficient-combination-of-operators.md)
