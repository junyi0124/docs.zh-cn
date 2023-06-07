---
title: ^ 运算符
ms.date: 07/20/2015
f1_keywords:
- vb.^
helpviewer_keywords:
- raising numbers to powers
- ^ operator [Visual Basic], exponentiation
- square operator [Visual Basic]
- ^ operator [Visual Basic]
- exponentiation operator [Visual Basic]
- exponent
- numbers [Visual Basic], rasing
- powers
- arithmetic operators [Visual Basic], exponentiation
ms.assetid: d89a1ca8-83da-4784-a87b-a9d7dceb3f62
ms.openlocfilehash: e139becf74ae367266a23513e18d0bdbdd24cdea
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84371383"
---
# <a name="-operator-visual-basic"></a>^ 运算符 (Visual Basic)

以一个数字为底、另一数字为幂求值。

## <a name="syntax"></a>语法

```vb
number ^ exponent
```

## <a name="parts"></a>组成部分

`number`\
必需。 任何数值表达式。

`exponent`\
必需。 任何数值表达式。

## <a name="result"></a>结果

结果会 `number` 引发的次幂 `exponent` ，总是作为 `Double` 值。

## <a name="supported-types"></a>支持的类型

`Double`. 任何不同类型的操作数都转换为 `Double` 。

## <a name="remarks"></a>备注

Visual Basic 总是对[Double 数据类型](../data-types/double-data-type.md)执行幂运算。

的值 `exponent` 可以是小数、负数或同时为两者。

如果在单个表达式中执行了多个幂运算，则会 `^` 按从左至右的顺序计算运算符。

> [!NOTE]
> `^`运算符可以*重载*，这意味着当操作数具有该类或结构的类型时，该类或结构可以重新定义其行为。 如果你的代码在该类或结构上使用此运算符，请确保了解其重新定义的行为。 有关详细信息，请参阅 [Operator Procedures](../../programming-guide/language-features/procedures/operator-procedures.md)。

## <a name="example"></a>示例

下面的示例使用运算符来计算 `^` 指数的幂。 结果是第一个操作数的次幂。

[!code-vb[VbVbalrOperators#20](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#20)]

前面的示例生成了以下结果：

`exp1`设置为4（2 squared）。

`exp2`设置为19683（3个立方，然后是该值的立方）。

`exp3`设置为-125 （-5 的立方）。

`exp4`设置为625（-5 到第四次幂）。

`exp5`设置为2（多维数据集的第8个）。

`exp6`设置为0.5 （1.0 除以8的 cube 根）。

请注意前面示例的表达式中的括号的重要性。 由于*运算符优先级*，Visual Basic 通常在 `^` 任何其他运算符（甚至一元运算符）之前执行运算符 `–` 。 如果 `exp4` 和的 `exp6` 计算结果没有括号，则会生成以下结果：

`exp4 = -5 ^ 4`将计算为–（5到第四次幂），这会导致-625。

`exp6 = 8 ^ -1.0 / 3.0`将计算为（8到-1 个幂，即0.125）除以3.0，这将导致0.041666666666666666666666666666667。

## <a name="see-also"></a>另请参阅

- [^ = 运算符](exponentiation-assignment-operator.md)
- [算术运算符](arithmetic-operators.md)
- [Visual Basic 中的运算符优先级](operator-precedence.md)
- [按功能列出的运算符](operators-listed-by-functionality.md)
- [算术运算符 (Visual Basic)](../../programming-guide/language-features/operators-and-expressions/arithmetic-operators.md)
