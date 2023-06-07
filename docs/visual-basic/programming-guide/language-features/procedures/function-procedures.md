---
title: Function 过程
ms.date: 07/20/2015
helpviewer_keywords:
- Function procedures
- return values [Visual Basic], function procedures
- Visual Basic code, procedures
- procedures [Visual Basic], calling
- procedures [Visual Basic], Function procedures
- syntax [Visual Basic], function procedures
ms.assetid: 1b9f632c-553b-4cb6-920a-ded117ead8c0
ms.openlocfilehash: b0ba96a875fd8785e45eee565beefe4b961ffc9d
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84388746"
---
# <a name="function-procedures-visual-basic"></a>Function 过程（Visual Basic）

`Function`过程是由和语句括起来的一系列 Visual Basic `Function` 语句 `End Function` 。 该 `Function` 过程执行任务，然后将控制权返回给调用代码。 当它返回 control 时，它还会将值返回到调用代码。

每次调用该过程时，其语句都会运行，从语句后面的第一个可执行语句开始， `Function` 到遇到的第一个 `End Function` 、 `Exit Function` 或 `Return` 语句结束。

您可以 `Function` 在模块、类或结构中定义过程。 `Public`默认情况下，这意味着您可以从应用程序中可以访问您定义它的模块、类或结构的任何位置调用它。

`Function`过程可以采用由调用代码传递给它的参数，如常量、变量或表达式。

## <a name="declaration-syntax"></a>声明语法

声明过程的语法如下所示 `Function` ：

```vb
[Modifiers] Function FunctionName [(ParameterList)] As ReturnType
    [Statements]
End Function
```

*修饰符*可以指定有关重载、重写、共享和隐藏的访问级别和信息。 有关详细信息，请参阅[函数语句](../../../language-reference/statements/function-statement.md)。

声明每个参数的方式与处理[Sub 过程](./sub-procedures.md)的方式相同。

### <a name="data-type"></a>数据类型

每个 `Function` 过程都有一种数据类型，就像每个变量一样。 此数据类型由 `As` 语句中的子句指定 `Function` ，它确定函数返回到调用代码的值的数据类型。 下面的示例声明阐释了这一点。

```vb
Function Yesterday() As Date
End Function

Function FindSqrt(radicand As Single) As Single
End Function
```

有关详细信息，请参阅[Function 语句](../../../language-reference/statements/function-statement.md)中的 "part"。

### <a name="returning-values"></a>返回值

`Function`过程返回到调用代码的值称为其返回值。 此过程通过以下两种方式之一返回此值：

- 它使用 `Return` 语句来指定返回值，并立即将控制权返回给调用程序。 下面的示例对此进行了演示。

  ```vb
  Function FunctionName [(ParameterList)] As ReturnType
      ' The following statement immediately transfers control back
      ' to the calling code and returns the value of Expression.
      Return Expression
  End Function
  ```

- 它在过程的一个或多个语句中为其自己的函数名称赋值。 直到 `Exit Function` 执行或语句后，控件才会返回到调用程序 `End Function` 。 下面的示例对此进行了演示。

  ```vb
  Function FunctionName [(ParameterList)] As ReturnType
      ' The following statement does not transfer control back to the calling code.
      FunctionName = Expression
      ' When control returns to the calling code, Expression is the return value.
  End Function
  ```

将返回值分配给函数名称的优点是，在遇到或语句之前，控件不会从过程返回 `Exit Function` `End Function` 。 这使您可以分配一个初步的值并在以后必要时进行调整。

有关返回值的详细信息，请参阅[函数语句](../../../language-reference/statements/function-statement.md)。 有关返回数组的信息，请参阅[数组](../arrays/index.md)。

## <a name="calling-syntax"></a>调用语法

`Function`通过在赋值语句或表达式的右侧包含其名称和参数来调用过程。 必须为所有非可选参数提供值，并且必须将参数列表括在括号中。 如果未提供任何参数，则可以选择省略括号。

对过程的调用语法如下所示 `Function` 。

*lvalue* `=`*functionname* `[(`*argumentlist*    `)]`

`If ((`*functionname* `[(`*argumentlist* `)] / 3) <=`*表达式*  `) Then`

调用 `Function` 过程时，无需使用其返回值。 否则，将执行该函数的所有操作，但将忽略返回值。 <xref:Microsoft.VisualBasic.Interaction.MsgBox%2A>通常以这种方式调用。

### <a name="illustration-of-declaration-and-call"></a>声明和调用的插图

下面的 `Function` 过程计算直角三角形的最长边（或斜边），并给出另一方的值。

[!code-vb[VbVbcnProcedures#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#1)]

下面的示例演示对的典型调用 `hypotenuse` 。

[!code-vb[VbVbcnProcedures#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#6)]

## <a name="see-also"></a>另请参阅

- [过程](./index.md)
- [Sub 过程](./sub-procedures.md)
- [Property 过程](./property-procedures.md)
- [运算符过程](./operator-procedures.md)
- [过程形参和实参](./procedure-parameters-and-arguments.md)
- [Function 语句](../../../language-reference/statements/function-statement.md)
- [如何：创建返回值的过程](./how-to-create-a-procedure-that-returns-a-value.md)
- [如何：从过程返回值](./how-to-return-a-value-from-a-procedure.md)
- [如何：调用返回值的过程](./how-to-call-a-procedure-that-returns-a-value.md)
