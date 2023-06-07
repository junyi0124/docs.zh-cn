---
title: If 运算符
ms.date: 07/20/2015
f1_keywords:
- vb.IfOperator
- IfOperator
helpviewer_keywords:
- ternary operators [Visual Basic]
- conditional execution
- If expressions [Visual Basic]
- conditional operator [Visual Basic]
- If Operator [Visual Basic]
ms.assetid: dd56c9df-7cd4-442c-9ba6-20c70ee44c8f
ms.openlocfilehash: 28fb2afb2c4cf78ffbbb028145de647a8dc512ed
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84371098"
---
# <a name="if-operator-visual-basic"></a>If 运算符 (Visual Basic)

使用短路计算有条件地返回两个值中的一个值。 `If`可以用三个参数或两个参数调用运算符。

## <a name="syntax"></a>语法

```vb
If( [argument1,] argument2, argument3 )
```

## <a name="if-operator-called-with-three-arguments"></a>如果调用运算符时有三个参数

`If`使用三个参数调用时，第一个参数的计算结果必须为可强制转换为的值 `Boolean` 。 `Boolean`该值将确定计算并返回其他两个参数中的哪一个。 以下列表仅适用于 `If` 使用三个参数调用运算符的情况。

### <a name="parts"></a>组成部分

|术语|定义|
|---|---|
|`argument1`|必需。 `Boolean`. 确定要计算和返回的其他参数的值。|
|`argument2`|必需。 `Object`. 如果计算结果为，则计算并返回 `argument1` `True` 。|
|`argument3`|必需。 `Object`. 如果计算结果为，则计算并返回， `argument1` `False` 如果 `argument1` 为[可为空](../../programming-guide/language-features/data-types/nullable-value-types.md)的 `Boolean` 变量， [Nothing](../nothing.md)则为。|

`If`使用三个参数调用的运算符的工作方式类似于 `IIf` 函数，只不过它使用短路计算。 `IIf`函数始终计算其所有三个参数，而 `If` 具有三个参数的运算符只计算两个参数中的两个。 计算第一个 `If` 参数，并将结果强制转换为 `Boolean` 值， `True` 或 `False` 。 如果值为 `True` ， `argument2` 则计算，并返回其值，但不会对其值进行 `argument3` 求值。 如果表达式的值 `Boolean` 为 `False` ， `argument3` 则计算，并返回其值，但不会对其值进行 `argument2` 求值。 下面的示例演示使用 `If` 三个参数时的用法：

[!code-vb[VbVbalrOperators#100](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class4.vb#100)]

下面的示例说明了短路计算的值。 该示例显示两次 `number` `divisor` 除为零时，尝试将变量除以变量 `divisor` 。 在这种情况下，应返回0，并且不会尝试执行除法，因为将导致运行时错误。 由于 `If` 表达式使用短路计算，因此它将计算第二个或第三个参数，具体取决于第一个参数的值。 如果第一个参数为 true，则除数不为零，并且可以安全地计算第二个参数并执行除法运算。 如果第一个参数为 false，则只计算第三个参数，并返回0。 因此，当除数为0时，将不会尝试执行除法，也不会产生错误结果。 但是，由于不 `IIf` 使用短路计算，因此即使第一个参数为 false，也会计算第二个参数。 这将导致运行时被零除错误。

[!code-vb[VbVbalrOperators#101](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class4.vb#101)]

## <a name="if-operator-called-with-two-arguments"></a>如果运算符调用了两个自变量

可以省略的第一个参数 `If` 。 这使得仅使用两个参数即可调用运算符。 以下列表仅适用于 `If` 通过两个参数调用运算符的情况。

### <a name="parts"></a>组成部分

|术语|定义|
|---|---|
|`argument2`|必需。 `Object`. 必须是引用或可以为 null 的值类型。 当计算结果为之外的任何值时，计算并返回 `Nothing` 。|
|`argument3`|必需。 `Object`. 如果计算结果为，则计算并返回 `argument2` `Nothing` 。|

如果 `Boolean` 省略该参数，则第一个参数必须是引用或可以为 null 的值类型。 如果第一个参数的计算结果为 `Nothing` ，则返回第二个参数的值。 在所有其他情况下，返回第一个参数的值。 下面的示例演示了此计算的工作原理：

[!code-vb[VbVbalrOperators#102](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class4.vb#102)]

## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.Interaction.IIf%2A>
- [可以为 null 的值类型](../../programming-guide/language-features/data-types/nullable-value-types.md)
- [Nothing](../nothing.md)
