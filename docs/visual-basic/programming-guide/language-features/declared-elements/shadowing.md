---
title: 阴影操作
ms.date: 07/20/2015
helpviewer_keywords:
- inheritance [Visual Basic], shadowing
- overriding, and shadowing
- shadowing
- duplicate names [Visual Basic]
- shadowing, by inheritance
- declared elements [Visual Basic], referencing
- shadowing, by scope
- declared elements [Visual Basic], hiding
- naming conflicts, shadowing
- declared elements [Visual Basic], shadowing
- shadowing, and overriding
- scope [Visual Basic], shadowing
- Shadows keyword [Visual Basic], about
- objects [Visual Basic], names
- names [Visual Basic], shadowing
ms.assetid: 54bb4c25-12c4-4181-b4a0-93546053964e
ms.openlocfilehash: 81e54875a3c1a4bbc5f5631e7ebac649a2e5afaf
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91085888"
---
# <a name="shadowing-in-visual-basic"></a>Visual Basic 中的隐藏

当两个编程元素共享同一名称时，其中一个元素可以 *隐藏或隐藏*另一个。 在这种情况下，隐藏的元素不可用于引用;相反，当你的代码使用元素名称时，Visual Basic 编译器会将其解析为隐藏元素。  
  
## <a name="purpose"></a>用途  

 隐藏的主要目的是保护类成员的定义。 基类可能会发生更改，该更改将创建一个与已定义的元素同名的元素。 如果发生这种情况， `Shadows` 修饰符会强制通过类的引用解析为你定义的成员而不是新的基类元素。  
  
## <a name="types-of-shadowing"></a>隐藏类型  

 元素可通过两种不同的方式隐藏其他元素。 可以在包含隐藏元素的区域的子区域内声明隐藏元素，在这种情况下，隐藏是 *通过范围*实现的。 或派生类可以重新定义基类的成员，在这种情况下，将 *通过继承*来完成隐藏。  
  
### <a name="shadowing-through-scope"></a>通过范围隐藏  

 同一模块、类或结构中的编程元素可以具有相同的名称，但范围不同。 如果以这种方式声明了两个元素，而代码引用了它们共享的名称，则范围较窄的元素隐藏其他元素 (块范围是最窄的) 。  
  
 例如，模块可以定义一个 `Public` 名为的变量 `temp` ，并且该模块中的一个过程可以声明同样名为的局部变量 `temp` 。 对中的的引用 `temp` 访问本地变量，而 `temp` 从该过程外部引用将访问该 `Public` 变量。 在这种情况下，过程变量 `temp` 隐藏了模块变量 `temp` 。  
  
 下图显示了两个名为 `temp` 的变量。 局部变量在 `temp` `temp` 从其自己的过程中被访问时隐藏了成员变量 `p` 。 不过， `MyClass` 关键字会绕过隐藏并访问成员变量。  
  
 ![显示通过范围隐藏的图形。](./media/shadowing/shadow-scope-diagram.gif)
  
 有关通过范围进行隐藏的示例，请参阅 [如何：隐藏与您的变量](how-to-hide-a-variable-with-the-same-name-as-your-variable.md)同名的变量。  
  
### <a name="shadowing-through-inheritance"></a>通过继承隐藏  

 如果派生类重定义了从基类继承的编程元素，则重定义元素将隐藏原始元素。 可以用任何其他类型隐藏任何类型的已声明元素或重载元素集。 例如， `Integer` 变量可以隐藏 `Function` 过程。 如果使用另一个过程来隐藏过程，则可以使用不同的参数列表和不同的返回类型。  
  
 下图显示了 `b` 继承自的基类和派生类 `d` `b` 。 基类定义了一个名为的过程 `proc` ，派生类将使用另一个同名的过程来隐藏该过程。 第一 `Call` 条语句访问 `proc` 派生类中的隐藏。 不过， `MyBase` 关键字会绕过隐藏并访问基类中的隐藏过程。  
  
 ![通过继承进行隐藏示意图](./media/shadowing/shadowing-inherit-diagram.gif)  
  
 有关通过继承进行隐藏的示例，请参阅 [如何：隐藏与您的变量](how-to-hide-a-variable-with-the-same-name-as-your-variable.md) 同名的变量和 [如何：隐藏继承的变量](how-to-hide-an-inherited-variable.md)。  
  
#### <a name="shadowing-and-access-level"></a>隐藏和访问级别  

 不能始终使用派生类从代码访问隐藏元素。 例如，它可能是声明的 `Private` 。 在这种情况下，隐藏会失效，如果没有隐藏，编译器将解析对同一元素的任何引用。 此元素是可访问的元素，它是从隐藏类向后 derivational 的最少步骤。 如果隐藏的元素是一个过程，则将其解析为具有相同名称、参数列表和返回类型的最接近的可访问版本。  
  
 下面的示例显示了三个类的继承层次结构。 每个类都定义一个 `Sub` 过程 `display` ，并且每个派生类都会 `display` 在其基类中隐藏该过程。  
  
```vb  
Public Class firstClass  
    Public Sub display()  
        MsgBox("This is firstClass")  
    End Sub  
End Class  
Public Class secondClass  
    Inherits firstClass  
    Private Shadows Sub display()  
        MsgBox("This is secondClass")  
    End Sub  
End Class  
Public Class thirdClass  
    Inherits secondClass  
    Public Shadows Sub display()  
        MsgBox("This is thirdClass")  
    End Sub  
End Class  
Module callDisplay  
    Dim first As New firstClass  
    Dim second As New secondClass  
    Dim third As New thirdClass  
    Public Sub callDisplayProcedures()  
        ' The following statement displays "This is firstClass".  
        first.display()  
        ' The following statement displays "This is firstClass".  
        second.display()  
        ' The following statement displays "This is thirdClass".  
        third.display()  
    End Sub  
End Module  
```  
  
 在前面的示例中，派生类 `secondClass` `display` 具有一个 `Private` 过程。 当模块 `callDisplay` `display` 在中调用时 `secondClass` ，调用代码在外部， `secondClass` 因此无法访问私有 `display` 过程。 隐藏操作已失效，编译器将引用解析为基类 `display` 过程。  
  
 但是，进一步的派生类 `thirdClass` 声明 `display` 为 `Public` ，因此中的代码 `callDisplay` 可以访问它。  
  
## <a name="shadowing-and-overriding"></a>隐藏和重写  

 不要将隐藏与重写混淆。 当派生类从基类继承时，同时使用这两种方法，并将一个声明的元素重定义为另一个。 但两者之间存在重大差异。 有关比较，请参阅 [隐藏和重写之间的差异](differences-between-shadowing-and-overriding.md)。  
  
## <a name="shadowing-and-overloading"></a>隐藏和重载  

 如果在派生类中隐藏具有多个元素的同一个基类元素，则隐藏元素将成为该元素的重载版本。 有关更多信息，请参见 [Procedure Overloading](../procedures/procedure-overloading.md)。  
  
## <a name="accessing-a-shadowed-element"></a>访问隐藏的元素  

 从派生类访问某个元素时，通常通过该派生类的当前实例执行此操作，方法是使用关键字限定元素名称 `Me` 。 如果派生类隐藏了基类中的元素，则可以通过使用关键字限定基类元素来访问它 `MyBase` 。  
  
 有关访问隐藏的元素的示例，请参阅 [如何：访问由派生类隐藏的变量](how-to-access-a-variable-hidden-by-a-derived-class.md)。  
  
### <a name="declaration-of-the-object-variable"></a>对象变量的声明  

 创建对象变量的方式还会影响派生类是访问隐藏元素还是访问隐藏的元素。 下面的示例从派生类创建两个对象，但一个对象声明为基类，另一个对象作为派生类。  
  
```vb  
Public Class baseCls  
    ' The following statement declares the element that is to be shadowed.  
    Public z As Integer = 100  
End Class  
Public Class dervCls  
    Inherits baseCls  
    ' The following statement declares the shadowing element.  
    Public Shadows z As String = "*"  
End Class  
Public Class useClasses  
    ' The following statement creates the object declared as the base class.  
    Dim basObj As baseCls = New dervCls()  
    ' Note that dervCls widens to its base class baseCls.  
    ' The following statement creates the object declared as the derived class.  
    Dim derObj As dervCls = New dervCls()  
    Public Sub showZ()
    ' The following statement outputs 100 (the shadowed element).  
        MsgBox("Accessed through base class: " & basObj.z)  
    ' The following statement outputs "*" (the shadowing element).  
        MsgBox("Accessed through derived class: " & derObj.z)  
    End Sub  
End Class  
```  
  
 在前面的示例中，变量 `basObj` 被声明为基类。 `dervCls`为对象分配对象将形成扩大转换，因此有效。 但是，基类无法访问派生类中的变量的隐藏版本 `z` ，因此编译器解析 `basObj.z` 为原始基类值。  
  
## <a name="see-also"></a>请参阅

- [References to Declared Elements](references-to-declared-elements.md)
- [Visual Basic 中的范围](scope.md)
- [Widening and Narrowing Conversions](../data-types/widening-and-narrowing-conversions.md)
- [Shadows](../../../language-reference/modifiers/shadows.md)
- [替代](../../../language-reference/modifiers/overrides.md)
- [Me、My、MyBase 和 MyClass](../../program-structure/me-my-mybase-and-myclass.md)
- [继承基础知识](../objects-and-classes/inheritance-basics.md)
