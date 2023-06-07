---
title: 如何：调用扩展方法
ms.date: 07/20/2015
helpviewer_keywords:
- calling extension methods [Visual Basic]
- extension methods [Visual Basic]
ms.assetid: df07750f-40f4-4c07-a79e-1113a27cfbea
ms.openlocfilehash: 38d6e8534283f475be2409f4b7c74ef48f1f248b
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91074987"
---
# <a name="how-to-call-an-extension-method-visual-basic"></a>如何：调用扩展方法 (Visual Basic)

扩展方法使你能够向现有类添加方法。 在扩展方法被声明并引入作用域后，可以像它所扩展的类型的实例方法一样调用它。 有关如何编写扩展方法的详细信息，请参阅 [如何：编写扩展方法](./how-to-write-an-extension-method.md)。

 下面的说明引用 extension 方法 `PrintAndPunctuate` ，该方法将显示调用它的字符串实例，然后在中为第二个参数发送任何值 `punc` 。

```vb
Imports System.Runtime.CompilerServices

Module StringExtensions
    <Extension()>
    Public Sub PrintAndPunctuate(ByVal aString As String,
                                 ByVal punc As String)
        Console.WriteLine(aString & punc)
    End Sub

End Module
```

调用方法时，该方法必须在范围内。

### <a name="to-call-an-extension-method"></a>调用扩展方法

1. 声明一个变量，该变量的数据类型为扩展方法的第一个参数。 对于 `PrintAndPunctuate` ，需要一个 <xref:System.String> 变量：

    ```vb
    Dim example = "Ready"
    ```

2. 该变量将调用扩展方法，并且其值绑定到第一个参数 `aString` 。 将显示下面的调用语句 `Ready?` 。

    ```vb
    example.PrintAndPunctuate("?")
    ```

     请注意，对此扩展方法的调用看起来就像调用 <xref:System.String> 需要一个参数的任何一个实例方法：

    ```vb
    example.EndsWith("dy")
    example.IndexOf("R")
    ```

3. 声明另一个字符串变量并再次调用方法，以查看它是否适用于任何字符串。

    ```vb
    Dim example2 = " or not"
    example2.PrintAndPunctuate("!!!")
    ```

     此时间的结果为： `or not!!!` 。

## <a name="example"></a>示例

 下面的代码是创建和使用简单扩展方法的完整示例。

```vb
Imports System.Runtime.CompilerServices
Imports ConsoleApplication1.StringExtensions

Module Module1

    Sub Main()

        Dim example = "Hello"
        example.PrintAndPunctuate(".")
        example.PrintAndPunctuate("!!!!")

        Dim example2 = "Goodbye"
        example2.PrintAndPunctuate("?")
    End Sub

    <Extension()>
    Public Sub PrintAndPunctuate(ByVal aString As String,
                                 ByVal punc As String)
        Console.WriteLine(aString & punc)
    End Sub
End Module

' Output:
' Hello.
' Hello!!!!
' Goodbye?
```

## <a name="see-also"></a>请参阅

- [如何：编写扩展方法](./how-to-write-an-extension-method.md)
- [扩展方法](./extension-methods.md)
- [Visual Basic 中的范围](../declared-elements/scope.md)
