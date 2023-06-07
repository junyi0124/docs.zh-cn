---
title: 如何：调用需要使用无符号类型的 Windows 函数
ms.date: 07/20/2015
helpviewer_keywords:
- Windows functions [Visual Basic], calling
- unsigned data types [Visual Basic]
- UShort data type [Visual Basic], using
- functions [Visual Basic], calling Windows functions
- ULong data type [Visual Basic], using
- UInteger data type [Visual Basic], using
- data types [Visual Basic], using
- unsigned types [Visual Basic]
- data types [Visual Basic], unsigned
- data types [Visual Basic], numeric
- unsigned types [Visual Basic], using
ms.assetid: c2c0e712-8dc2-43b9-b4c6-345fbb02e7ce
ms.openlocfilehash: 5b78c808de4a16060d37844ad0f17e89fa6f6d84
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90548073"
---
# <a name="how-to-call-a-windows-function-that-takes-unsigned-types-visual-basic"></a>如何：调用采用无符号类型的 Windows 函数 (Visual Basic)

如果你使用的类、模块或结构具有无符号整数类型的成员，则可以使用 Visual Basic 访问这些成员。

## <a name="to-call-a-windows-function-that-takes-an-unsigned-type"></a>调用采用无符号类型的 Windows 函数

1. 使用 [Declare 语句](../../language-reference/statements/declare-statement.md) 来告知 Visual Basic 哪个库包含函数、其在库中的名称、调用顺序，以及如何在调用字符串时转换字符串。

2. 在 `Declare` 语句中，将 `UInteger` 、 `ULong` 、 `UShort` 或 `Byte` 适当地用于具有无符号类型的每个参数。

3. 请参阅你要调用的 Windows 函数的文档，以查找它所使用的常量的名称和值。 其中许多是在 Winuser.h 文件中定义的。

4. 在代码中声明必要的常量。 许多 Windows 常量为32位无符号值，你应将其声明为 `As UInteger` 。

5. 以正常方式调用函数。 下面的示例调用 Windows 函数 `MessageBox` ，该函数采用无符号整数参数。

    ```vb
    Public Class windowsMessage
        Private Declare Auto Function mb Lib "user32.dll" Alias "MessageBox" (
            ByVal hWnd As Integer,
            ByVal lpText As String,
            ByVal lpCaption As String,
            ByVal uType As UInteger) As Integer
        Private Const MB_OK As UInteger = 0
        Private Const MB_ICONEXCLAMATION As UInteger = &H30
        Private Const IDOK As UInteger = 1
        Private Const IDCLOSE As UInteger = 8
        Private Const c As UInteger = MB_OK Or MB_ICONEXCLAMATION
        Public Function messageThroughWindows() As String
            Dim r As Integer = mb(0, "Click OK if you see this!",
                "Windows API call", c)
            Dim s As String = "Windows API MessageBox returned " &
                 CStr(r)& vbCrLf & "(IDOK = " & CStr(IDOK) &
                 ", IDCLOSE = " & CStr(IDCLOSE) & ")"
            Return s
        End Function
    End Class
    ```

     可以 `messageThroughWindows` 通过以下代码测试该函数。

    ```vb
    Public Sub consumeWindowsMessage()
        Dim w As New windowsMessage
        w.messageThroughWindows()
    End Sub
    ```

    > [!CAUTION]
    > `UInteger`、 `ULong` 、 `UShort` 和 `SByte` 数据类型不是[语言独立性和与语言无关的组件](../../../standard/language-independence-and-language-independent-components.md) (CLS) 的一部分，因此符合 cls 的代码无法使用使用它们的组件。

    > [!IMPORTANT]
    > 调用非托管代码（如 (API) 的 Windows 应用程序编程接口）会使代码面临潜在的安全风险。

    > [!IMPORTANT]
    > 调用 Windows API 需要非托管代码权限，这可能会影响在部分信任情况下的执行。 有关详细信息，请参阅 <xref:System.Security.Permissions.SecurityPermission> 和 [代码访问权限](/previous-versions/dotnet/netframework-4.0/h846e9b3(v=vs.100))。

## <a name="see-also"></a>请参阅

- [数据类型](../../language-reference/data-types/index.md)
- [Integer 数据类型](../../language-reference/data-types/integer-data-type.md)
- [UInteger 数据类型](../../language-reference/data-types/uinteger-data-type.md)
- [Declare Statement](../../language-reference/statements/declare-statement.md)
- [演练：调用 Windows API](walkthrough-calling-windows-apis.md)
