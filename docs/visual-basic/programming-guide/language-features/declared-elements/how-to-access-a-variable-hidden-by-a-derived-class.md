---
title: 如何：访问被派生类隐藏的变量
ms.date: 07/20/2015
helpviewer_keywords:
- qualification [Visual Basic], of element names
- base classes [Visual Basic], accessing elements
- element names [Visual Basic], qualification
- references [Visual Basic], declared elements
- declared elements [Visual Basic], referencing
- variables [Visual Basic], accessing hidden
ms.assetid: ae21a8ac-9cd4-4fba-a3ec-ecc4321ef93c
ms.openlocfilehash: c5ff802a0f6e081acd00d7cdfab4a8296b4daad9
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84392852"
---
# <a name="how-to-access-a-variable-hidden-by-a-derived-class-visual-basic"></a>如何：访问被派生类隐藏的变量 (Visual Basic)

当派生类中的代码访问某个变量时，编译器通常会将该引用解析为最接近的可访问版本，即，可访问的版本 derivational 从访问类向后翻的步骤。 如果在派生类中定义了该变量，则代码通常会访问该定义。

如果派生类变量隐藏了基类中的变量，则会隐藏基类版本。 但是，您可以通过使用关键字限定基类变量来访问它 `MyBase` 。

### <a name="to-access-a-base-class-variable-hidden-by-a-derived-class"></a>访问由派生类隐藏的基类变量

- 在表达式或赋值语句中，在变量名称之前加上 `MyBase` 关键字和一个句点（ `.` ）。

    编译器将引用解析为对变量的基类版本的引用。

    下面的示例演示通过继承进行的隐藏。 它创建两个引用，一个用于访问隐藏变量，另一个用于绕过隐藏。

    ```vb
    Public Class shadowBaseClass
        Public shadowString As String = "This is the base class string."
    End Class
    Public Class shadowDerivedClass
        Inherits shadowBaseClass
        Public Shadows shadowString As String = "This is the derived class string."
        Public Sub showStrings()
            Dim s As String = "Unqualified shadowString: " & shadowString &
                vbCrLf & "MyBase.shadowString: " & MyBase.shadowString
            MsgBox(s)
        End Sub
    End Class
    ```

    前面的示例声明 `shadowString` 基类中的变量，并将其隐藏在派生类中。 `showStrings`派生类中的过程在名称不合格时显示字符串的隐藏版本 `shadowString` 。 当用关键字限定时，它会显示隐藏的版本 `shadowString` `MyBase` 。

## <a name="robust-programming"></a>可靠编程

为了降低引用隐藏变量的意外版本的风险，可以完全限定对隐藏变量的所有引用。 隐藏引入了一个具有相同名称的变量的多个版本。 当代码语句引用变量名时，编译器解析引用的版本取决于一些因素，如代码语句的位置和符合条件的字符串的状态。 这可能会增加引用错误的变量版本的风险。

## <a name="see-also"></a>另请参阅

- [References to Declared Elements](references-to-declared-elements.md)
- [Visual Basic 中的隐藏](shadowing.md)
- [隐藏和重写之间的差异](differences-between-shadowing-and-overriding.md)
- [如何：隐藏与变量同名的变量](how-to-hide-a-variable-with-the-same-name-as-your-variable.md)
- [如何：隐藏继承的变量](how-to-hide-an-inherited-variable.md)
- [Shadows](../../../language-reference/modifiers/shadows.md)
- [替代](../../../language-reference/modifiers/overrides.md)
- [Me、My、MyBase 和 MyClass](../../program-structure/me-my-mybase-and-myclass.md)
- [继承基础知识](../objects-and-classes/inheritance-basics.md)
