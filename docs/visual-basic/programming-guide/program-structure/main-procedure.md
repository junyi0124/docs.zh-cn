---
title: 主要过程
ms.date: 07/20/2015
f1_keywords:
- vb.Main
helpviewer_keywords:
- Main procedure
- Main method [Visual Basic]
- main function
ms.assetid: f0db283e-f283-4464-b521-b90858cc1b44
ms.openlocfilehash: d6708ee13963aaae43a73b159032f64f0fffac10
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91072205"
---
# <a name="main-procedure-in-visual-basic"></a>Visual Basic 中的 Main 过程

每个 Visual Basic 应用程序都必须包含一个名为的过程 `Main` 。 此过程可用作应用程序的起点和总体控制。 当你的 `Main` 应用程序已加载并准备好将控制传递给你的应用程序时，.NET Framework 将调用你的过程。 除非要创建 Windows 窗体应用程序，否则必须 `Main` 为自己运行的应用程序编写过程。

 `Main` 包含首先运行的代码。 在中 `Main` ，您可以在程序启动时确定要加载的窗体，查明您的应用程序的副本是否已在系统上运行，为您的应用程序建立一组变量，或者打开应用程序所需的数据库。

## <a name="requirements-for-the-main-procedure"></a>Main 过程的要求

 在其自身 (上运行的文件通常具有扩展名 .exe) 必须包含 `Main` 过程。 库 (例如，扩展名为 .dll) 不会自行运行，也不需要 `Main` 过程。 可以创建的不同类型的项目的要求如下：

- 控制台应用程序会自行运行，你必须至少提供一个 `Main` 过程。

- Windows 窗体应用程序独立运行。 不过，Visual Basic 编译器会自动 `Main` 在此类应用程序中生成过程，而不需要编写一个过程。

- 类库不需要 `Main` 过程。 其中包括 Windows 控件库和 Web 控件库。 Web 应用程序部署为类库。

## <a name="declaring-the-main-procedure"></a>声明 Main 过程

 有四种方法可以声明 `Main` 过程。 它可以接受参数，也可以返回值。

> [!NOTE]
> 如果 `Main` 在类中声明，则必须使用 `Shared` 关键字。 在模块中， `Main` 无需为 `Shared` 。

- 最简单的方法是声明 `Sub` 不采用参数或返回值的过程。

    ```vb
    Module mainModule
        Sub Main()
            MsgBox("The Main procedure is starting the application.")
            ' Insert call to appropriate starting place in your code.
            MsgBox("The application is terminating.")
        End Sub
    End Module
    ```

- `Main` 还可以返回一个 `Integer` 值，操作系统使用该值作为程序的退出代码。 其他程序可以通过检查 Windows ERRORLEVEL 值来测试此代码。 若要返回退出代码，必须将声明 `Main` 为 `Function` 过程而不是 `Sub` 过程。

    ```vb
    Module mainModule
        Function Main() As Integer
            MsgBox("The Main procedure is starting the application.")
            Dim returnValue As Integer = 0
            ' Insert call to appropriate starting place in your code.
            ' On return, assign appropriate value to returnValue.
            ' 0 usually means successful completion.
            MsgBox("The application is terminating with error level " &
                 CStr(returnValue) & ".")
            Return returnValue
        End Function
    End Module
    ```

- `Main` 还可以采用 `String` 数组作为参数。 数组中的每个字符串都包含一个用于调用程序的命令行参数。 您可以根据它们的值执行不同的操作。

    ```vb
    Module mainModule
        Function Main(ByVal cmdArgs() As String) As Integer
            MsgBox("The Main procedure is starting the application.")
            Dim returnValue As Integer = 0
            ' See if there are any arguments.
            If cmdArgs.Length > 0 Then
                For argNum As Integer = 0 To UBound(cmdArgs, 1)
                    ' Insert code to examine cmdArgs(argNum) and take
                    ' appropriate action based on its value.
                Next
            End If
            ' Insert call to appropriate starting place in your code.
            ' On return, assign appropriate value to returnValue.
            ' 0 usually means successful completion.
            MsgBox("The application is terminating with error level " &
                 CStr(returnValue) & ".")
            Return returnValue
        End Function
    End Module
    ```

- 可以声明 `Main` 检查命令行参数，但不会返回退出代码，如下所示。

    ```vb
    Module mainModule
        Sub Main(ByVal cmdArgs() As String)
            MsgBox("The Main procedure is starting the application.")
            Dim returnValue As Integer = 0
            ' See if there are any arguments.
            If cmdArgs.Length > 0 Then
                For argNum As Integer = 0 To UBound(cmdArgs, 1)
                    ' Insert code to examine cmdArgs(argNum) and take
                    ' appropriate action based on its value.
                Next
            End If
            ' Insert call to appropriate starting place in your code.
            MsgBox("The application is terminating.")
        End Sub
    End Module
    ```
  
## <a name="see-also"></a>请参阅

- <xref:Microsoft.VisualBasic.Interaction.MsgBox%2A>
- <xref:System.Array.Length%2A>
- <xref:Microsoft.VisualBasic.Information.UBound%2A>
- [Visual Basic 程序的结构](structure-of-a-visual-basic-program.md)
- [-main](../../reference/command-line-compiler/main.md)
- [共享](../../language-reference/modifiers/shared.md)
- [Sub 语句](../../language-reference/statements/sub-statement.md)
- [Function 语句](../../language-reference/statements/function-statement.md)
- [Integer 数据类型](../../language-reference/data-types/integer-data-type.md)
- [String 数据类型](../../language-reference/data-types/string-data-type.md)
