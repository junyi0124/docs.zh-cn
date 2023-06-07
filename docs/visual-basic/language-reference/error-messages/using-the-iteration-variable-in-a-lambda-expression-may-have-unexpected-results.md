---
title: 在 lambda 表达式中使用迭代变量可能会产生意外的结果
ms.date: 07/20/2015
f1_keywords:
- vbc42324
- bc42324
helpviewer_keywords:
- BC42324
ms.assetid: b5c2c4bd-3b2a-4a73-aaeb-55728eb03b68
ms.openlocfilehash: d0deea94084565ea91debe2b6db30def4cd9e00e
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92161486"
---
# <a name="bc42324-using-the-iteration-variable-in-a-lambda-expression-may-have-unexpected-results"></a>BC42324：在 lambda 表达式中使用迭代变量可能会产生意外的结果

在 lambda 表达式中使用迭代变量可能会产生意外的结果。 改为在循环内创建一个局部变量，并为其分配迭代变量的值。

 如果在循环中声明的 lambda 表达式中使用循环迭代变量，则会出现此警告。 例如，下面的示例将导致出现警告。

```vb
For i As Integer = 1 To 10
    ' The warning is given for the use of i.
    Dim exampleFunc As Func(Of Integer) = Function() i
Next
```

 下面的示例显示了可能出现的意外结果。

```vb
Module Module1
    Sub Main()
        Dim array1 As Func(Of Integer)() = New Func(Of Integer)(4) {}

        For i As Integer = 0 To 4
            array1(i) = Function() i
        Next

        For Each funcElement In array1
            System.Console.WriteLine(funcElement())
        Next

    End Sub
End Module
```

 `For`循环创建 lambda 表达式的数组，其中每个表达式都返回循环迭代变量的值 `i` 。 当在循环中计算 lambda 表达式时 `For Each` ，可能会发现在循环中显示0、1、2、3和4的后续值 `i` `For` 。 相反，您将看到显示的五次的最终值 `i` ：

 `5`

 `5`

 `5`

 `5`

 `5`

 默认情况下，此消息是一个警告。 有关隐藏警告或将警告视为错误的详细信息，请参阅 [Configuring Warnings in Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic)。

 **错误 ID：** BC42324

## <a name="to-correct-this-error"></a>更正此错误

- 将迭代变量的值分配给局部变量，并在 lambda 表达式中使用该局部变量。

```vb
Module Module1
    Sub Main()
        Dim array1 As Func(Of Integer)() = New Func(Of Integer)(4) {}

        For i As Integer = 0 To 4
            Dim j = i
            array1(i) = Function() j
        Next

        For Each funcElement In array1
            System.Console.WriteLine(funcElement())
        Next

    End Sub
End Module
```

## <a name="see-also"></a>请参阅

- [Lambda 表达式](../../programming-guide/language-features/procedures/lambda-expressions.md)
