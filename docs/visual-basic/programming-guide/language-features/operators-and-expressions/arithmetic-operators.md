---
title: 算术运算符
ms.date: 07/20/2015
helpviewer_keywords:
- type safety
- operators [Visual Basic], bitwise
- operators [Visual Basic], bit-shift
- bitwise operators [Visual Basic]
- bit-shift operators [Visual Basic]
- zero, division by zero
- operators [Visual Basic], arithmetic
- division [Visual Basic], by zero
- Visual Basic code, operators
- arithmetic operators [Visual Basic], about arithmetic operators
ms.assetid: 325dac7a-ea4f-41d5-8b48-f6e904211569
ms.openlocfilehash: 023e479736285aa2d04509e05f49fe930cb4721d
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91090074"
---
# <a name="arithmetic-operators-in-visual-basic"></a>算术运算符 (Visual Basic)

算术运算符用于执行许多常见的算术运算，这些运算涉及到用文本、变量、其他表达式、函数和属性调用和常量表示的数值的计算。 同时，采用算术运算符进行分类的是位移位运算符，它在操作数的各个位级别上起作用，并将其位模式向左或向右移动。  
  
## <a name="arithmetic-operations"></a>算术运算  

 您可以使用 [+ 运算符](../../../language-reference/operators/addition-operator.md)将两个值一起添加到表达式中，或用 [-Operator (Visual Basic) ](../../../language-reference/operators/subtraction-operator.md)进行减法运算，如下面的示例所示。  
  
 [!code-vb[VbVbalrOperators#57](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#57)]  
  
 求反还使用 [-Operator (Visual Basic) ](../../../language-reference/operators/subtraction-operator.md)，但只有一个操作数，如下面的示例所示。  
  
 [!code-vb[VbVbalrOperators#58](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#58)]  
  
 乘法和除法分别使用 [* 运算符](../../../language-reference/operators/multiplication-operator.md) 和 [/运算符 (Visual Basic) ](../../../language-reference/operators/floating-point-division-operator.md)，如下例所示。  
  
 [!code-vb[VbVbalrOperators#59](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#59)]  
  
 幂运算使用 [^ 运算符](../../../language-reference/operators/exponentiation-operator.md)，如下面的示例所示。  
  
 [!code-vb[VbVbalrOperators#60](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#60)]  
  
 使用 [\ 运算符 (Visual Basic) ](../../../language-reference/operators/integer-division-operator.md)执行整数除法运算。 整数除法返回商，即表示除数可以分为被除数的次数的整数，而不考虑任何余数。 除数和被除数都必须是整型 (、、、、、、 `SByte` `Byte` `Short` `UShort` `Integer` `UInteger` `Long` 和 `ULong`) 。 所有其他类型必须首先转换为整型类型。 下面的示例演示了整数除法。  
  
 [!code-vb[VbVbalrOperators#61](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#61)]  
  
 使用 [Mod 运算符](../../../language-reference/operators/mod-operator.md)执行取模运算。 此运算符返回将除数除以整数次后所得的余数。 如果除数和被除数均为整型类型，则返回的值是整型。 如果除数和被除数为浮点类型，则返回的值也是浮点型。 下例演示此行为。  
  
 [!code-vb[VbVbalrOperators#62](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#62)]  
  
 [!code-vb[VbVbalrOperators#63](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#63)]  
  
### <a name="attempted-division-by-zero"></a>尝试被零除  

 被零除具有不同的结果，具体取决于所涉及的数据类型。 在整数除法 (`SByte` 、 `Byte` 、 `Short` 、 `UShort` 、 `Integer` 、 `UInteger` 、 `Long` 、 `ULong`) ，.NET Framework 将引发 <xref:System.DivideByZeroException> 异常。 在或数据类型的除法运算中 `Decimal` `Single` ，.NET Framework 也会引发 <xref:System.DivideByZeroException> 异常。  
  
 在涉及数据类型的浮点除法中 `Double` ，不会引发异常，并且结果是表示、或的类成员 <xref:System.Double.NaN> <xref:System.Double.PositiveInfinity> <xref:System.Double.NegativeInfinity> ，具体取决于被除数。 下表总结了试图将值除以零的各种结果 `Double` 。  
  
|被除数数据类型|除数数据类型|股利值|结果|  
|---|---|---|---|  
|`Double`|`Double`|0|<xref:System.Double.NaN> (不是数学定义的数字) |  
|`Double`|`Double`|> 0|<xref:System.Double.PositiveInfinity>|  
|`Double`|`Double`|\< 0|<xref:System.Double.NegativeInfinity>|  
  
 捕获 <xref:System.DivideByZeroException> 异常时，可以使用其成员帮助处理异常。 例如， <xref:System.Exception.Message%2A> 属性包含异常的消息文本。 有关详细信息，请参阅 [Try...Catch...Finally 语句](../../../language-reference/statements/try-catch-finally-statement.md)。  
  
## <a name="bit-shift-operations"></a>移位运算  

 移位运算对位模式执行算术移位运算。 模式包含在左侧的操作数中，而右侧的操作数指定了要将模式移位到的位置数。 您可以用 [>> 运算符](../../../language-reference/operators/right-shift-operator.md) 将模式向右移动，或将 [<< 运算符](../../../language-reference/operators/left-shift-operator.md)左移。  
  
 模式操作数的数据类型必须是、、 `SByte` `Byte` `Short` 、 `UShort` 、、、 `Integer` `UInteger` `Long` 或 `ULong` 。 移位量操作数的数据类型必须为 `Integer` 或必须扩大到 `Integer` 。  
  
 算术移位不是循环的，这意味着，不会在另一端重新引入结果的末尾以外的位。 移位后空出的位位置设置如下：  
  
- 对于算术左移，为0  
  
- 如果为负数，则为0  
  
- 0表示无符号数据类型的算术右移位 (`Byte` 、 `UShort` 、 `UInteger` 、 `ULong`)   
  
- 1表示负数 (`SByte` 、 `Short` 、 `Integer` 或 `Long`) 的算术右移位  
  
 下面的示例将 `Integer` 值向左和向右移位。  
  
 [!code-vb[VbVbalrOperators#64](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#64)]  
  
 算术移位从不产生溢出异常。  
  
## <a name="bitwise-operations"></a>按位运算  

 除了作为逻辑运算符外，、 `Not` 、 `Or` `And` 和还会在 `Xor` 对数值使用时执行按位运算。 有关详细信息，请参阅 [Visual Basic 中逻辑与按位运算符](logical-and-bitwise-operators.md)中的 "位运算"。  
  
## <a name="type-safety"></a>类型安全  

 操作数通常应为同一类型。 例如，如果您正在使用变量执行加法操作 `Integer` ，则应将其添加到另一个 `Integer` 变量，并且应将结果分配给类型的变量 `Integer` 。  
  
 确保良好的类型安全编码做法的一种方法是使用 [Option Strict 语句](../../../language-reference/statements/option-strict-statement.md)。 如果设置 `Option Strict On` ，则 Visual Basic 会自动执行 *类型安全* 转换。 例如，如果尝试将 `Integer` 变量添加到 `Double` 变量并将值分配给 `Double` 变量，则操作会正常进行，因为 `Integer` 值可以转换为， `Double` 而不会丢失数据。 另一方面，类型不安全的转换将导致编译器错误 `Option Strict On` 。 例如，如果尝试将 `Integer` 变量添加到 `Double` 变量并将值分配给 `Integer` 变量，则编译器错误将会出现，因为 `Double` 变量不能隐式转换为类型 `Integer` 。  
  
 但是，如果您设置 `Option Strict Off` ，则 Visual Basic 允许进行隐式收缩转换，但它们可能会导致数据或精度意外丢失。 出于此原因，我们建议你在 `Option Strict On` 编写生产代码时使用。 有关详细信息，请参阅 [Widening and Narrowing Conversions](../data-types/widening-and-narrowing-conversions.md)。  
  
## <a name="see-also"></a>请参阅

- [算术运算符](../../../language-reference/operators/arithmetic-operators.md)
- [移位运算符](../../../language-reference/operators/bit-shift-operators.md)
- [Comparison Operators in Visual Basic](comparison-operators.md)
- [串联运算符 (Visual Basic)](concatenation-operators.md)
- [Visual Basic 中的逻辑运算符和位运算符](logical-and-bitwise-operators.md)
- [运算符的有效组合](efficient-combination-of-operators.md)
