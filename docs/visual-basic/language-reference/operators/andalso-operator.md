---
title: AndAlso 运算符
ms.date: 07/20/2015
f1_keywords:
- vb.AndAlso
- AndAlso
helpviewer_keywords:
- short-circuiting
- AndAlso operator [Visual Basic]
- operators [Visual Basic], short-circuiting
- operators [Visual Basic], conjunction
- short-circuit evaluation
ms.assetid: bbc15191-b374-495b-9b8f-7b8c2f4388eb
ms.openlocfilehash: aff4621b8f415b9441ad1edf537b9b0736892bb8
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90874856"
---
# <a name="andalso-operator-visual-basic"></a>AndAlso 运算符 (Visual Basic)

对两个表达式执行短路逻辑与运算。  
  
## <a name="syntax"></a>语法  
  
```vb
result = expression1 AndAlso expression2  
```  
  
## <a name="parts"></a>组成部分  
  
|术语|定义|  
|---|---|  
|`result`|必需。 任何 `Boolean` 表达式。 结果是 `Boolean` 这两个表达式的比较结果。|  
|`expression1`|必需。 任何 `Boolean` 表达式。|  
|`expression2`|必需。 任何 `Boolean` 表达式。|  
  
## <a name="remarks"></a>备注  

 如果编译后的代码可以根据另一个表达式的结果跳过对一个表达式的计算，则将逻辑运算称为 *短路* 。 如果第一个表达式的计算结果确定了运算的最终结果，则不需要计算第二个表达式，因为它不能更改最终结果。 如果绕过的表达式较复杂，或者它涉及过程调用，则短路可以提高性能。  
  
 如果两个表达式的计算结果均为 `True` ， `result` 则为 `True` 。 下表说明了如何 `result` 确定。  
  
|如果 `expression1` 为 |并且 `expression2` 为|的值 `result` 为|  
|---|---|---|  
|`True`|`True`|`True`|  
|`True`|`False`|`False`|  
|`False`|不计算 () |`False`|  
  
## <a name="data-types"></a>数据类型  

 `AndAlso`仅为[布尔数据类型](../data-types/boolean-data-type.md)定义运算符。 Visual Basic 在计算表达式之前，根据需要将每个操作数转换为 `Boolean` 。 如果将结果赋给数值类型，Visual Basic 会将其从转换 `Boolean` 为该类型，使其 `False` 成为 `0` 并 `True` 变成 `-1` 。
有关详细信息，请参阅 [布尔类型转换](../data-types/boolean-data-type.md#type-conversions)。
  
## <a name="overloading"></a>重载  

 [And 运算符](and-operator.md)和[IsFalse 运算符](isfalse-operator.md)可以*重载*，这意味着当操作数具有该类或结构的类型时，该类或结构可以重新定义它们的行为。 重载 `And` 和 `IsFalse` 运算符会影响运算符的行为 `AndAlso` 。 如果你的代码 `AndAlso` 在重载和的类或结构上使用 `And` `IsFalse` ，请确保你了解其重新定义的行为。 有关详细信息，请参阅 [Operator Procedures](../../programming-guide/language-features/procedures/operator-procedures.md)。  
  
## <a name="example"></a>示例  

 下面的示例使用 `AndAlso` 运算符对两个表达式执行逻辑与运算。 结果是一个 `Boolean` 值，该值表示整个联合表达式是否为 true。 如果第一个表达式为 `False` ，则不计算第二个表达式。  
  
 [!code-vb[VbVbalrOperators#24](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#24)]  
  
 前面的示例 `True` 分别生成、 `False` 和的结果 `False` 。 在的计算中 `secondCheck` ，不计算第二个表达式，因为第一个表达式已经存在 `False` 。 但是，在的计算中计算第二个表达式 `thirdCheck` 。  
  
## <a name="example"></a>示例  

 下面的示例演示了在 `Function` 数组的元素中搜索给定值的过程。 如果数组为空，或超出了数组长度，则该 `While` 语句不会对照搜索值测试数组元素。  
  
 [!code-vb[VbVbalrOperators#25](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#25)]  
  
## <a name="see-also"></a>另请参阅

- [逻辑/按位运算符 (Visual Basic)](logical-bitwise-operators.md)
- [Visual Basic 中的运算符优先级](operator-precedence.md)
- [按功能列出的运算符](operators-listed-by-functionality.md)
- [And 运算符](and-operator.md)
- [IsFalse 运算符](isfalse-operator.md)
- [Visual Basic 中的逻辑运算符和位运算符](../../programming-guide/language-features/operators-and-expressions/logical-and-bitwise-operators.md)
