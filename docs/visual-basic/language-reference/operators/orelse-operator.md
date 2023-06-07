---
title: OrElse 运算符
ms.date: 07/20/2015
f1_keywords:
- OrElse
- vb.OrElse
helpviewer_keywords:
- short-circuiting
- operators [Visual Basic], short-circuiting
- operators [Visual Basic], disjunction
- short-circuit evaluation
- OrElse operator [Visual Basic]
ms.assetid: 253803d8-05b0-47d7-b213-abd222847779
ms.openlocfilehash: edac3eeaef5d0127f10a0d570ca27c8158806ff3
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90866722"
---
# <a name="orelse-operator-visual-basic"></a>OrElse 运算符 (Visual Basic)

对两个表达式执行短路包含逻辑析取。  
  
## <a name="syntax"></a>语法  
  
```vb
result = expression1 OrElse expression2  
```  
  
## <a name="parts"></a>组成部分  

 `result`  
 必需。 任何 `Boolean` 表达式。  
  
 `expression1`  
 必需。 任何 `Boolean` 表达式。  
  
 `expression2`  
 必需。 任何 `Boolean` 表达式。  
  
## <a name="remarks"></a>备注  

 如果编译后的代码可以根据另一个表达式的结果跳过对一个表达式的计算，则将逻辑运算称为 *短路* 。 如果第一个表达式的计算结果确定了运算的最终结果，则不需要计算第二个表达式，因为它不能更改最终结果。 如果绕过的表达式较复杂，或者它涉及过程调用，则短路可以提高性能。  
  
 如果任意一个或两个表达式的计算结果为 `True` ， `result` 则为 `True` 。 下表说明了如何 `result` 确定。  
  
|如果 `expression1` 为 |并且 `expression2` 为|的值 `result` 为|  
|-------------------------|--------------------------|------------------------------|  
|`True`|不计算 () |`True`|  
|`False`|`True`|`True`|  
|`False`|`False`|`False`|  
  
## <a name="data-types"></a>数据类型  

 `OrElse`仅为[布尔数据类型](../data-types/boolean-data-type.md)定义运算符。 Visual Basic 在计算表达式之前，根据需要将每个操作数转换为 `Boolean` 。 如果将结果赋给数值类型，Visual Basic 会将其从转换 `Boolean` 为该类型，使其 `False` 成为 `0` 并 `True` 变成 `-1` 。
有关详细信息，请参阅 [布尔类型转换](../data-types/boolean-data-type.md#type-conversions)。
  
## <a name="overloading"></a>重载  

 可以*重载* [Or 运算符](or-operator.md)和[IsTrue 运算符](istrue-operator.md)，这意味着当操作数具有该类或结构的类型时，该类或结构可以重新定义其行为。 重载 `Or` 和 `IsTrue` 运算符会影响运算符的行为 `OrElse` 。 如果你的代码 `OrElse` 在重载和的类或结构上使用 `Or` `IsTrue` ，请确保你了解其重新定义的行为。 有关详细信息，请参阅 [Operator Procedures](../../programming-guide/language-features/procedures/operator-procedures.md)。  
  
## <a name="example"></a>示例  

 下面的示例使用 `OrElse` 运算符对两个表达式执行逻辑析取。 结果是一个 `Boolean` 值，该值表示两个表达式之一是否为 true。 如果第一个表达式为 `True` ，则不计算第二个表达式。  
  
 [!code-vb[VbVbalrOperators#37](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#37)]  
  
 前面的示例 `True` 分别生成、 `True` 和的结果 `False` 。 在的计算中 `firstCheck` ，不计算第二个表达式，因为第一个表达式已经存在 `True` 。 但是，在的计算中计算第二个表达式 `secondCheck` 。  
  
## <a name="example"></a>示例  

 下面的示例显示了 `If` `Then` 包含两个过程调用的 ... 语句。 如果第一次调用返回 `True` ，则不会调用第二个过程。 如果第二个过程执行的重要任务应始终在此部分代码运行时执行，这可能会产生意外的结果。  
  
 [!code-vb[VbVbalrOperators#38](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#38)]  
  
## <a name="see-also"></a>另请参阅

- [逻辑/按位运算符 (Visual Basic)](logical-bitwise-operators.md)
- [Visual Basic 中的运算符优先级](operator-precedence.md)
- [按功能列出的运算符](operators-listed-by-functionality.md)
- [Or 运算符](or-operator.md)
- [IsTrue 运算符](istrue-operator.md)
- [Visual Basic 中的逻辑运算符和位运算符](../../programming-guide/language-features/operators-and-expressions/logical-and-bitwise-operators.md)
