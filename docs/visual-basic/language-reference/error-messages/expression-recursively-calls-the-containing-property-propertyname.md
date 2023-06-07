---
title: 表达式递归调用包含属性“<propertyname>”
ms.date: 07/20/2015
f1_keywords:
- vbc42026
- BC42026
helpviewer_keywords:
- BC42026
ms.assetid: 4fde9db6-3bf3-48dc-8e05-981bf08969da
ms.openlocfilehash: f5b8b226d46afa0555559de0930f20025ba27f2b
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92162760"
---
# <a name="bc42026-expression-recursively-calls-the-containing-property-propertyname"></a>BC42026：表达式递归调用包含属性 " \<propertyname> "

属性定义过程中的语句将 `Set` 值存储到属性的名称中。

 保存属性值的建议方法是在 `Private` 属性的容器中定义一个变量，并在和过程中使用它 `Get` `Set` 。 然后，该 `Set` 过程应将传入值存储在此 `Private` 变量中。

 此 `Get` 过程的行为类似于 `Function` 过程，因此它可以为属性名称赋值，并通过遇到语句来返回控件 `End Get` 。 不过，建议的方法是 `Private` 在 [返回语句](../statements/return-statement.md)中包含变量作为值。

 此 `Set` 过程的行为类似于 `Sub` 过程，该过程不返回值。 因此，过程或属性名称在过程中没有特殊含义 `Set` ，因此不能在其中存储值。

 下面的示例说明了可能导致此错误的方法，并遵循建议的方法。

```vb
Public Class illustrateProperties
' The code in the following property causes this error.
    Public Property badProp() As Char
        Get
            Dim charValue As Char
            ' Insert code to update charValue.
            badProp = charValue
        End Get
        Set(ByVal Value As Char)
            ' The following statement causes this error.
            badProp = Value
            ' The value stored in the local variable badProp
            ' is not used by the Get procedure in this property.
        End Set
    End Property
' The following code uses the recommended approach.
    Private propValue As Char
    Public Property goodProp() As Char
        Get
            ' Insert code to update propValue.
            Return propValue
        End Get
        Set(ByVal Value As Char)
            propValue = Value
        End Set
    End Property
End Class
```

 默认情况下，此消息是一个警告。 有关隐藏警告或将警告视为错误的详细信息，请参阅 [Configuring Warnings in Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic)。

 **错误 ID：** BC42026

## <a name="to-correct-this-error"></a>更正此错误

- 如前面的示例所示，重写属性定义以使用建议的方法。

## <a name="see-also"></a>另请参阅

- [Property 过程](../../programming-guide/language-features/procedures/property-procedures.md)
- [Property Statement](../statements/property-statement.md)
- [Set 语句](../statements/set-statement.md)
