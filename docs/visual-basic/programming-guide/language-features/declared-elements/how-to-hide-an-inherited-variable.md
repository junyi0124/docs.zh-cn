---
title: 如何：隐藏继承的变量
ms.date: 07/20/2015
helpviewer_keywords:
- qualification [Visual Basic], of element names
- element names [Visual Basic], qualification
- references [Visual Basic], declared elements
- declaration statements [Visual Basic], declared elements
- referencing declared elements [Visual Basic]
- declared elements [Visual Basic], referencing
- declared elements [Visual Basic], about declared elements
- variables [Visual Basic], hiding inherited
ms.assetid: 765728d9-7351-4a30-999d-b5f34f024412
ms.openlocfilehash: f49bba0497f9f4f2774b01284c815bba9aaed119
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84357265"
---
# <a name="how-to-hide-an-inherited-variable-visual-basic"></a>如何：隐藏继承的变量 (Visual Basic)

派生类继承其基类的所有定义。 如果要使用与基类元素相同的名称来定义变量，则可以在派生类中定义变量时*隐藏或隐藏*该基类元素。 如果执行此操作，派生类中的代码将访问你的变量，除非它显式跳过隐藏机制。

您可能想要隐藏继承的变量的另一个原因是为了防止基类修订。 基类可能会改变要继承的元素。 如果发生这种情况，则 `Shadows` 修饰符强制将派生类中的引用解析为你的变量，而不是基类元素。

## <a name="to-hide-an-inherited-variable"></a>隐藏继承的变量

1. 确保要隐藏的变量是在类级别（在任何过程外部）声明的。 否则，你无需隐藏它。
  
2. 在派生类中，编写声明变量的[Dim 语句](../../../language-reference/statements/dim-statement.md)。 使用与继承变量相同的名称。

3. 在声明中包含[Shadows](../../../language-reference/modifiers/shadows.md)关键字。

     当派生类中的代码引用变量名时，编译器会将对变量的引用解析为。

     下面的示例说明了继承变量的隐藏：
  
    ```vb  
    Public Class ShadowBaseClass  
        Public shadowString As String = "This is the base class string."  
    End Class  
    Public Class ShadowDerivedClass  
        Inherits ShadowBaseClass  
        Public Shadows shadowString As String = "This is the derived class string."  
        Public Sub ShowStrings()  
            Dim s As String = $"Unqualified shadowString: {shadowString}{vbCrLf}MyBase.shadowString: {MyBase.shadowString}"
            MsgBox(s)  
        End Sub  
    End Class  
    ```  
  
     前面的示例声明 `shadowString` 基类中的变量，并将其隐藏在派生类中。 `ShowStrings`派生类中的过程在名称不合格时显示字符串的隐藏版本 `shadowString` 。 当用关键字限定时，它会显示隐藏的版本 `shadowString` `MyBase` 。  
  
## <a name="robust-programming"></a>可靠编程

隐藏引入了一个具有相同名称的变量的多个版本。 当代码语句引用变量名时，编译器解析引用的版本取决于一些因素，如代码语句的位置和符合条件的字符串的状态。 这可能会增加引用隐藏变量的意外版本的风险。 您可以通过完全限定对隐藏变量的所有引用来降低风险。

## <a name="see-also"></a>另请参阅

- [References to Declared Elements](references-to-declared-elements.md)
- [Visual Basic 中的隐藏](shadowing.md)
- [隐藏和重写之间的差异](differences-between-shadowing-and-overriding.md)
- [如何：隐藏与变量同名的变量](how-to-hide-a-variable-with-the-same-name-as-your-variable.md)
- [如何：访问被派生类隐藏的变量](how-to-access-a-variable-hidden-by-a-derived-class.md)
- [替代](../../../language-reference/modifiers/overrides.md)
- [Me、My、MyBase 和 MyClass](../../program-structure/me-my-mybase-and-myclass.md)
- [继承基础知识](../objects-and-classes/inheritance-basics.md)
