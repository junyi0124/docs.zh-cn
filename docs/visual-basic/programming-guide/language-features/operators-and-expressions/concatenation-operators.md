---
title: 串联运算符
ms.date: 07/20/2015
helpviewer_keywords:
- '& operator [Visual Basic], concatenation'
- concatenation operators [Visual Basic]
- operators [Visual Basic], concatenation
- Visual Basic code, operators
- + operator [Visual Basic], concatenation
- concatenation operators [Visual Basic]
ms.assetid: e59908c3-89e0-41ae-933d-3e8826c16a04
ms.openlocfilehash: c123438a86a2c3293a99770107d970535fcdbdf8
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84388785"
---
# <a name="concatenation-operators-in-visual-basic"></a>串联运算符 (Visual Basic)

串联运算符将多个字符串联接为一个字符串。 有两种串联运算符：`+` 和 `&`。 这两种串联运算符都执行基本的串联运算，如下面的示例所示。

```vb
Dim x As String = "Mic" & "ro" & "soft"
Dim y As String = "Mic" + "ro" + "soft"
' The preceding statements set both x and y to "Microsoft".
```

这两种运算符还可以串联 `String` 变量，如下面的示例所示。

[!code-vb[VbVbalrOperators#76](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#76)]

## <a name="differences-between-the-two-concatenation-operators"></a>两种串联运算符之间的区别

[+ 运算符](../../../language-reference/operators/addition-operator.md)的主要目的是添加两个数字。 然而，它还可以将数值操作数与字符串操作数串联起来。 `+` 操作数具有一套复杂的规则，用来确定是相加、串联、指示编译器错误还是引发运行时的 <xref:System.InvalidCastException> 异常。

仅为操作数定义[& 运算符](../../../language-reference/operators/concatenation-operator.md) `String` ，并且无论的设置如何，它始终将其操作数扩大到 `String` `Option Strict` 。 对于字符串串联操作，建议使用 `&` 运算符，原因是它以独占方式为字符串定义，并降低产生意外转换的可能性。

## <a name="performance-string-and-stringbuilder"></a>性能：字符串和 StringBuilder

如果你对字符串执行大量操作（如串联、删除或替换），则通过 <xref:System.Text.StringBuilder> 命名空间中的 <xref:System.Text> 类可能会提高性能。 该类采用额外指令来创建和初始化 <xref:System.Text.StringBuilder> 对象，并且使用其他指令将其最终值转换为 `String`，但此时你可能会恢复使用 <xref:System.Text.StringBuilder>，因为它的执行速度更快。

## <a name="see-also"></a>另请参阅

- [Option Strict 语句](../../../language-reference/statements/option-strict-statement.md)
- [字符串操作方法的类型 (Visual Basic)](../strings/types-of-string-manipulation-methods.md)
- [算术运算符 (Visual Basic)](arithmetic-operators.md)
- [Comparison Operators in Visual Basic](comparison-operators.md)
- [Visual Basic 中的逻辑运算符和位运算符](logical-and-bitwise-operators.md)
