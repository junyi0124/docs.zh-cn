---
title: Sub 过程
ms.date: 07/20/2015
helpviewer_keywords:
- Sub procedures [Visual Basic], about Sub procedures
- statement blocks
- Sub procedures [Visual Basic], calling
- procedures [Visual Basic], calling
- Sub procedures [Visual Basic], syntax
- Sub procedures
- procedures [Visual Basic], Sub
- syntax [Visual Basic], Sub procedures
ms.assetid: 6a0a4958-ed0a-4d3d-8d31-0772c82bda58
ms.openlocfilehash: 9ca1d302a0bc8e989e0b2dddf8cce68e89211d57
ms.sourcegitcommit: 5d769956a04b6d68484dd717077fabc191c21da5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/17/2020
ms.locfileid: "76163808"
---
# <a name="sub-procedures-visual-basic"></a>Sub 过程（Visual Basic）

`Sub` 过程是一系列由 `Sub` 和 `End Sub` 语句括起来的 Visual Basic 语句。 `Sub` 过程执行任务，然后将控制权返回给调用代码，但它不会将值返回给调用代码。

每次调用该过程时，都会执行其语句，从 `Sub` 语句后面的第一个可执行语句开始，到遇到的第一个 `End Sub`、`Exit Sub`或 `Return` 语句结束。

您可以在模块、类和结构中定义 `Sub` 过程。 默认情况下，它是 `Public`的，这意味着您可以从应用程序中可以访问您定义它的模块、类或结构的任何位置调用它。 术语*方法*描述了从其定义模块、类或结构外部访问的 `Sub` 或 `Function` 过程。 有关详细信息，请参阅[过程](./index.md)。

`Sub` 过程可以采用由调用代码传递给它的参数，如常量、变量或表达式。

## <a name="declaration-syntax"></a>声明语法

声明 `Sub` 过程的语法如下所示：

```vb
[modifiers] Sub SubName[(parameterList)]
    ' Statements of the Sub procedure.
End Sub
```

`modifiers` 可以指定有关重载、重写、共享和隐藏的访问级别和信息。 有关详细信息，请参阅[Sub 语句](../../../language-reference/statements/sub-statement.md)。

## <a name="parameter-declaration"></a>参数声明

声明每个过程参数的方式与声明变量的方式类似，即指定参数名称和数据类型。 还可以指定传递机制，并指定参数是可选的还是参数数组。

参数列表中每个参数的语法如下所示：

```vb
[Optional] [ByVal | ByRef] [ParamArray] parameterName As DataType
```

如果该参数是可选的，则还必须提供默认值作为其声明的一部分。 指定默认值的语法如下所示：

```vb
Optional [ByVal | ByRef]  parameterName As DataType = defaultValue
```

### <a name="parameters-as-local-variables"></a>参数作为局部变量

当控件传递给过程时，每个参数都被视为局部变量。 这意味着其生存期与过程的生存期相同，其作用域为整个过程。

## <a name="calling-syntax"></a>调用语法

使用独立调用语句显式调用 `Sub` 过程。 不能通过在表达式中使用其名称来调用它。 必须为所有非可选参数提供值，并且必须将参数列表括在括号中。 如果未提供任何参数，则可以选择省略括号。 `Call` 关键字是可选的，但不建议使用。

调用 `Sub` 过程的语法如下所示：

```vb
[Call] SubName[(argumentlist)]
```

可以从定义它的类的外部调用 `Sub` 方法。 首先，必须使用 `New` 关键字创建类的实例，或调用返回类的实例的方法。 有关详细信息，请参阅[New 运算符](../../../language-reference/operators/new-operator.md)。 然后，可以使用以下语法对实例对象调用 `Sub` 方法：

```vb
object.MethodName[(argumentList)]
```

### <a name="illustration-of-declaration-and-call"></a>声明和调用的插图

以下 `Sub` 过程告知计算机操作员应用程序将要执行的任务，还会显示时间戳。 应用程序只会从不同位置调用 `tellOperator`，而不是在每个任务开始时重复此代码。 每个调用都在 `task` 参数中传递一个字符串，该字符串标识要启动的任务。

[!code-vb[VbVbcnProcedures#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#2)]

下面的示例演示对 `tellOperator`的典型调用。

[!code-vb[VbVbcnProcedures#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#3)]

## <a name="see-also"></a>另请参阅

- [过程](./index.md)
- [Function 过程](./function-procedures.md)
- [属性过程](./property-procedures.md)
- [运算符过程](./operator-procedures.md)
- [过程参数和自变量](./procedure-parameters-and-arguments.md)
- [Sub 语句](../../../language-reference/statements/sub-statement.md)
- [如何：调用不返回值的过程](./how-to-call-a-procedure-that-does-not-return-a-value.md)
- [如何：在 Visual Basic 中调用事件处理程序](./how-to-call-an-event-handler.md)
