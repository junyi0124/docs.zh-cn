---
title: 将不会从此事件处理程序中移除 Lambda 表达式
ms.date: 07/20/2015
f1_keywords:
- bc42326
- vbc42326
helpviewer_keywords:
- BC42326
ms.assetid: 63214dc6-0112-4245-8ebf-7c9e8f5a5782
ms.openlocfilehash: 5a30e63044b51f8176dfeebdcc9eb8fd517739ae
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92163111"
---
# <a name="bc42326-lambda-expression-will-not-be-removed-from-this-event-handler"></a>BC42326：不会从此事件处理程序中移除 Lambda 表达式

Lambda 表达式将不会从此事件处理程序中移除。 将 lambda 表达式分配给一个变量，并使用该变量添加和移除该事件。

当 lambda 表达式与事件处理程序一起使用时，可能看不到预期的行为。 编译器会为每个 lambda 表达式定义生成新的方法，即使它们相同也是如此。 因此，会显示以下代码 `False` 。

```vb
Module Module1

    Sub Main()
        Dim fun1 As ChangeInteger = Function(p As Integer) p + 1
        Dim fun2 As ChangeInteger = Function(p As Integer) p + 1
        Console.WriteLine(fun1 = fun2)
    End Sub

    Delegate Function ChangeInteger(ByVal x As Integer) As Integer

End Module
```

当 lambda 表达式与事件处理程序一起使用时，这可能会导致意外的结果。 在下面的示例中，语句不删除由添加的 lambda 表达式 `AddHandler` `RemoveHandler` 。

```vb
Module Module1

    Event ProcessInteger(ByVal x As Integer)

    Sub Main()

        ' The following line adds one listener to the event.
        AddHandler ProcessInteger, Function(m As Integer) m

        ' The following statement searches the current listeners
        ' for a match to remove. However, this lambda is not the same
        ' as the previous one, so nothing is removed.
        RemoveHandler ProcessInteger, Function(m As Integer) m

    End Sub
End Module
```

默认情况下，此消息是一个警告。 有关如何隐藏警告或将警告视为错误的详细信息，请参阅 [Configuring Warnings in Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic)。

**错误 ID：** BC42326

## <a name="to-correct-this-error"></a>更正此错误

若要避免此警告并删除 lambda 表达式，请将 lambda 表达式分配给一个变量，并在和语句中使用该变量 `AddHandler` `RemoveHandler` ，如下面的示例中所示。

```vb
Module Module1

    Event ProcessInteger(ByVal x As Integer)

    Dim PrintHandler As ProcessIntegerEventHandler

    Sub Main()

        ' Assign the lambda expression to a variable.
        PrintHandler = Function(m As Integer) m

        ' Use the variable to add the listener.
        AddHandler ProcessInteger, PrintHandler

        ' Use the variable again when you want to remove the listener.
        RemoveHandler ProcessInteger, PrintHandler

    End Sub
End Module
```

## <a name="see-also"></a>请参阅

- [Lambda 表达式](../../programming-guide/language-features/procedures/lambda-expressions.md)
- [宽松委托转换](../../programming-guide/language-features/delegates/relaxed-delegate-conversion.md)
- [事件](../../programming-guide/language-features/events/index.md)
