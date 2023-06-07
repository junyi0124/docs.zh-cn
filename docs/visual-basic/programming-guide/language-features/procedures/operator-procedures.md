---
title: 运算符过程
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic code, procedures
- procedures [Visual Basic], operator
- Visual Basic code, operators
- syntax [Visual Basic], Operator procedures
- operators [Visual Basic], overloading
- overloaded operators [Visual Basic]
- operator overloading
- operator procedures
ms.assetid: 8c513d38-246b-4fb7-8b75-29e1364e555b
ms.openlocfilehash: a1dd183570c8aa50efff85bdaebef90bd3b0120f
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84364313"
---
# <a name="operator-procedures-visual-basic"></a>运算符过程 (Visual Basic)

运算符过程是一系列 Visual Basic 语句，用于定义标准运算符（如 `*` 、 `<>` 或 `And` ）在已定义的类或结构中的行为。 这也称为*运算符重载*。

## <a name="when-to-define-operator-procedures"></a>何时定义操作员过程

定义了类或结构后，可以将变量声明为该类或结构的类型。 有时，此类变量需要作为表达式的一部分参与操作。 为此，它必须是运算符的操作数。

Visual Basic 仅定义其基本数据类型的运算符。 如果两个操作数或其中一个操作数为类或结构的类型，则可以定义运算符的行为。

有关详细信息，请参阅[Operator 语句](../../../language-reference/statements/operator-statement.md)。

## <a name="types-of-operator-procedure"></a>运算符过程的类型

操作员过程可以是以下类型之一：

- 一元运算符的定义，其中的参数是你的类或结构的类型。

- 二元运算符的定义，其中至少有一个参数是你的类或结构的类型。

- 转换运算符的定义，其中的参数属于你的类或结构的类型。

- 返回类或结构的类型的转换运算符的定义。

 转换运算符始终为一元运算符，并且你始终使用 `CType` 作为正在定义的运算符。

## <a name="declaration-syntax"></a>声明语法

声明运算符过程的语法如下所示：

```vb
Public Shared [Widening | Narrowing] Operator operatorsymbol ( operand1 [,  operand2 ]) As datatype

' Statements of the operator procedure.

End Operator
```

`Widening` `Narrowing` 仅对类型转换运算符使用或关键字。 对于类型转换运算符，运算符符号始终是[CType 函数](../../../language-reference/functions/ctype-function.md)。

您可以声明两个操作数来定义一个二元运算符，并声明一个操作数来定义一元运算符，包括类型转换运算符。 必须声明所有操作数 `ByVal` 。

声明每个操作数的方式与声明[Sub 过程](./sub-procedures.md)的参数的方式相同。

### <a name="data-type"></a>数据类型

由于你在定义的类或结构上定义运算符，因此至少一个操作数必须是该类或结构的数据类型。 对于类型转换运算符，操作数或返回类型必须是类或结构的数据类型。

有关更多详细信息，请参阅[Operator 语句](../../../language-reference/statements/operator-statement.md)。

## <a name="calling-syntax"></a>调用语法

通过在表达式中使用运算符符号，可以隐式调用运算符过程。 提供操作数的方式与预定义运算符相同。

对运算符过程的隐式调用的语法如下所示：

`Dim testStruct As`  *structurename*

`Dim testNewStruct As`  *structurename* `= testStruct`*operatorsymbol*      `10`

### <a name="illustration-of-declaration-and-call"></a>声明和调用的插图

下面的结构将已签名的128位整数值存储为构成的高序位和低序位部分。 它定义 `+` 运算符以添加两个 `veryLong` 值并生成一个结果 `veryLong` 值。

[!code-vb[VbVbcnProcedures#23](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#23)]

下面的示例演示对 `+` 上定义的运算符的典型调用 `veryLong` 。

[!code-vb[VbVbcnProcedures#24](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#24)]

## <a name="see-also"></a>另请参阅

- [过程](./index.md)
- [Sub 过程](./sub-procedures.md)
- [Function 过程](./function-procedures.md)
- [Property 过程](./property-procedures.md)
- [过程形参和实参](./procedure-parameters-and-arguments.md)
- [Operator Statement](../../../language-reference/statements/operator-statement.md)
- [如何：定义运算符](./how-to-define-an-operator.md)
- [如何：定义转换运算符](./how-to-define-a-conversion-operator.md)
- [如何：调用运算符过程](./how-to-call-an-operator-procedure.md)
- [如何：使用定义运算符的类](./how-to-use-a-class-that-defines-operators.md)
