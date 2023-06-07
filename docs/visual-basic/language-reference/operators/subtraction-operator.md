---
title: '- 运算符'
ms.date: 07/20/2015
f1_keywords:
- vb.Negate
- vb.-
helpviewer_keywords:
- operator [Visual Basic]
- operators [Visual Basic], subtraction
- operators [Visual Basic], difference
- negation operator [Visual Basic]
- operators [Visual Basic], arithmetic
- subtraction operator [Visual Basic], syntax
- arithmetic operators [Visual Basic], subtraction
- difference operator [Visual Basic]
- math operators [Visual Basic]
- operators [Visual Basic], negation
- minus operator [Visual Basic]
ms.assetid: bff2c368-662d-4c92-ac87-1d9bdfd3426a
ms.openlocfilehash: b5129c2dbb361940fa6da2cb424ee23736ba72c5
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90875325"
---
# <a name="--operator-visual-basic"></a>- 运算符 (Visual Basic)

返回数值表达式的两个数值表达式或负值之间的差。  
  
## <a name="syntax"></a>语法  
  
```vb  
expression1 – expression2
```
  
or

```vb  
–expression1  
```  
  
## <a name="parts"></a>组成部分  

 `expression1`  
 必需。 任何数值表达式。  
  
 `expression2`  
 必需，除非 `–` 操作员计算负值。 任何数值表达式。  
  
## <a name="result"></a>结果  

 结果是与的差 `expression1` 或的 `expression2` 求反值 `expression1` 。  
  
 Result 数据类型是适用于和的数据类型的数值类型 `expression1` `expression2` 。 请参阅 [运算符结果的数据类型](data-types-of-operator-results.md)中的 "整数算法" 表。  
  
## <a name="supported-types"></a>支持的类型  

 所有数值类型。 这包括无符号和浮点类型和 `Decimal` 。  
  
## <a name="remarks"></a>备注  

 在前面所示的语法中所示的第一个用法中， `–` 运算符是两个数值表达式之差的 *二进制* 算术减法运算符。  
  
 在前面所示的语法中所示的第二个用法中， `–` 运算符是表达式的负值的 *一元* 求反运算符。 在这种意义上，否定包含反转的符号， `expression1` 因此如果为负，则结果为正 `expression1` 。  
  
 如果其中一个表达式的计算结果为 [Nothing](../nothing.md)，则 `–` 运算符将其视为零。  
  
> [!NOTE]
> `–`运算符可以*重载*，这意味着当操作数具有该类或结构的类型时，该类或结构可以重新定义其行为。 如果代码对这样的类或结构使用此运算符，请确保了解其重新定义的行为。 有关详细信息，请参阅 [Operator Procedures](../../programming-guide/language-features/procedures/operator-procedures.md)。  
  
## <a name="example"></a>示例  

 下面的示例使用 `–` 运算符计算并返回两个数字之间的差，然后对数字求反。  
  
 [!code-vb[VbVbalrOperators#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#10)]  
  
 执行这些语句后， `binaryResult` 包含124.45， `unaryResult` 包含–334.90。  
  
## <a name="see-also"></a>另请参阅

- [-= 运算符 (Visual Basic) ](subtraction-assignment-operator.md)
- [算术运算符](arithmetic-operators.md)
- [Visual Basic 中的运算符优先级](operator-precedence.md)
- [按功能列出的运算符](operators-listed-by-functionality.md)
- [算术运算符 (Visual Basic)](../../programming-guide/language-features/operators-and-expressions/arithmetic-operators.md)
