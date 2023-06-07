---
title: Or 运算符
ms.date: 07/20/2015
f1_keywords:
- vb.Or
helpviewer_keywords:
- Or operator [Visual Basic]
- operators [Visual Basic], bitwise
- inclusive Or operator [Visual Basic]
- bitwise operators [Visual Basic], OR operator
- Or keyword [Visual Basic]
- operators [Visual Basic], inclusive or
- operators [Visual Basic], disjunction
- bitwise comparison [Visual Basic]
- logical disjunction
- disjunction operator [Visual Basic]
ms.assetid: 41ed6905-bf3d-468a-9e3b-03c10d461891
ms.openlocfilehash: f6cfd1073ada42aa2db8be9b14c81319bc0db294
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90874757"
---
# <a name="or-operator-visual-basic"></a>Or 运算符 (Visual Basic)

对两个表达式执行逻辑 `Boolean` 或运算，或对两个数值表达式执行按位析取。  
  
## <a name="syntax"></a>语法  
  
```vb  
result = expression1 Or expression2  
```  
  
## <a name="parts"></a>组成部分  

 `result`  
 必需。 任何 `Boolean` 或数值表达式。 为了进行 `Boolean` 比较， `result` 是两个值的包含逻辑析取 `Boolean` 。 对于按位运算， `result` 是表示两个数值位模式的包含按位析取的数值。  
  
 `expression1`  
 必需。 任何 `Boolean` 或数值表达式。  
  
 `expression2`  
 必需。 任何 `Boolean` 或数值表达式。  
  
## <a name="remarks"></a>备注  

 为了进行 `Boolean` 比较 `result` ， `False` 如果和的 `expression1` `expression2` 计算结果都为，则为 `False` 。 下表说明了如何 `result` 确定。  
  
|如果 `expression1` 为 |并且 `expression2` 为|的值 `result` 为|  
|-------------------------|--------------------------|------------------------------|  
|`True`|`True`|`True`|  
|`True`|`False`|`True`|  
|`False`|`True`|`True`|  
|`False`|`False`|`False`|  
  
> [!NOTE]
> 在 `Boolean` 比较中， `Or` 运算符始终计算两个表达式，这可能包括进行过程调用。 [OrElse 运算符](orelse-operator.md)执行*短路*，这意味着，如果 `expression1` 为，则 `True` `expression2` 不计算。  
  
 对于按位运算， `Or` 运算符对两个数值表达式中的相同位置执行按位比较，并根据下表设置中的相应位 `result` 。  
  
|如果中的位 `expression1` 是|和中的位 `expression2` 是|中的位 `result` 是|  
|--------------------------------|---------------------------------|----------------------------|  
|1|1|1|  
|1|0|1|  
|0|1|1|  
|0|0|0|  
  
> [!NOTE]
> 由于逻辑 and 位运算符的优先级低于其他算术运算符和关系运算符，因此应将任何按位运算括在括号中，以确保准确执行。  
  
## <a name="data-types"></a>数据类型  

 如果操作数由一个 `Boolean` 表达式和一个数值表达式组成，则 Visual Basic 会将 `Boolean` 表达式转换为数值， ( 为– 1; 对于) ，则将 `True` `False` 执行按位运算。  
  
 对于 `Boolean` 比较，结果的数据类型为 `Boolean` 。 对于按位比较，结果数据类型是适用于和的数据类型的数值类型 `expression1` `expression2` 。 请参阅 [运算符结果的数据类型](data-types-of-operator-results.md)中的 "关系和按位比较" 表。  
  
## <a name="overloading"></a>重载  

 `Or`运算符可以*重载*，这意味着当操作数具有该类或结构的类型时，该类或结构可以重新定义其行为。 如果你的代码在该类或结构上使用此运算符，请确保了解其重新定义的行为。 有关详细信息，请参阅 [Operator Procedures](../../programming-guide/language-features/procedures/operator-procedures.md)。  
  
## <a name="example"></a>示例  

 下面的示例使用 `Or` 运算符对两个表达式执行包含逻辑析取。 结果是一个 `Boolean` 值，该值表示两个表达式中的一个是否为 `True` 。  
  
 [!code-vb[VbVbalrOperators#35](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#35)]  
  
 前面的示例 `True` 分别生成、 `True` 和的结果 `False` 。  
  
## <a name="example"></a>示例  

 下面的示例使用 `Or` 运算符对两个数值表达式的各个位执行包含逻辑析取。 如果操作数中的相应位均设置为1，则结果模式中的位将设置为1。  
  
 [!code-vb[VbVbalrOperators#36](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#36)]  
  
 前面的示例分别生成10、14和14的结果。  
  
## <a name="see-also"></a>另请参阅

- [逻辑/按位运算符 (Visual Basic)](logical-bitwise-operators.md)
- [Visual Basic 中的运算符优先级](operator-precedence.md)
- [按功能列出的运算符](operators-listed-by-functionality.md)
- [OrElse 运算符](orelse-operator.md)
- [Visual Basic 中的逻辑运算符和位运算符](../../programming-guide/language-features/operators-and-expressions/logical-and-bitwise-operators.md)
