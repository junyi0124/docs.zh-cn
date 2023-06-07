---
title: 创建变体泛型接口
ms.date: 07/20/2015
ms.assetid: d4037dd2-dfe9-4811-9150-93d4e8b20113
ms.openlocfilehash: 884349159d2738d8481b217f9dab383483616f2b
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400637"
---
# <a name="creating-variant-generic-interfaces-visual-basic"></a>创建变体泛型接口 (Visual Basic)

接口中的泛型类型参数可以声明为协变或逆变。 协变  允许接口方法具有与泛型类型参数定义的返回类型相比，派生程度更大的返回类型。 逆变  允许接口方法具有与泛型形参指定的实参类型相比，派生程度更小的实参类型。 具有协变或逆变泛型类型参数的泛型接口称为“变体”  。

> [!NOTE]
> .NET Framework 4 引入了对多个现有泛型接口的变体支持。 有关 .NET Framework 中的变体接口的列表，请参阅[泛型接口中的变体（Visual Basic）](variance-in-generic-interfaces.md)。

## <a name="declaring-variant-generic-interfaces"></a>声明变体泛型接口

可通过对泛型类型参数使用 `in` 和 `out` 关键字来声明变体泛型接口。

> [!IMPORTANT]
> `ByRef`Visual Basic 中的参数不能为变体。 值类型也不支持变体。

可以使用 `out` 关键字将泛型类型参数声明为协变。 协变类型必须满足以下条件：

- 类型仅用作接口方法的返回类型，不用作方法参数的类型。 下例演示了此要求，其中类型 `R` 为声明的协变。

    ```vb
    Interface ICovariant(Of Out R)
        Function GetSomething() As R
        ' The following statement generates a compiler error.
        ' Sub SetSomething(ByVal sampleArg As R)
    End Interface
    ```

    此规则有一个例外。 如果具有用作方法参数的逆变泛型委托，则可将类型用作该委托的泛型类型参数。 下例中的类型 `R` 演示了此情形。 有关详细信息，请参阅[委托中的变体（Visual Basic）](variance-in-delegates.md)和[对 Func 和 Action 泛型委托使用变体（Visual Basic）](using-variance-for-func-and-action-generic-delegates.md)。

    ```vb
    Interface ICovariant(Of Out R)
        Sub DoSomething(ByVal callback As Action(Of R))
    End Interface
    ```

- 类型不用作接口方法的泛型约束。 下面的代码阐释了这一点。

    ```vb
    Interface ICovariant(Of Out R)
        ' The following statement generates a compiler error
        ' because you can use only contravariant or invariant types
        ' in generic constraints.
        ' Sub DoSomething(Of T As R)()
    End Interface
    ```

可以使用 `in` 关键字将泛型类型参数声明为逆变。 逆变类型只能用作方法参数的类型，不能用作接口方法的返回类型。 逆变类型还可用于泛型约束。 以下代码演示如何声明逆变接口，以及如何将泛型约束用于其方法之一。

```vb
Interface IContravariant(Of In A)
    Sub SetSomething(ByVal sampleArg As A)
    Sub DoSomething(Of T As A)()
    ' The following statement generates a compiler error.
    ' Function GetSomething() As A
End Interface
```

此外，还可以在同一接口中同时支持协变和逆变，但需应用于不同的类型参数，如以下代码示例所示。

```vb
Interface IVariant(Of Out R, In A)
    Function GetSomething() As R
    Sub SetSomething(ByVal sampleArg As A)
    Function GetSetSomething(ByVal sampleArg As A) As R
End Interface
```

在 Visual Basic 中，如果不指定委托类型，则不能在变体接口中声明事件。 此外，变体接口不能具有嵌套的类、枚举或结构，但它可以具有嵌套接口。 下面的代码阐释了这一点。

```vb
Interface ICovariant(Of Out R)
    ' The following statement generates a compiler error.
    ' Event SampleEvent()
    ' The following statement specifies the delegate type and
    ' does not generate an error.
    Event AnotherEvent As EventHandler

    ' The following statements generate compiler errors,
    ' because a variant interface cannot have
    ' nested enums, classes, or structures.

    'Enum SampleEnum : test : End Enum
    'Class SampleClass : End Class
    'Structure SampleStructure : Dim value As Integer : End Structure

    ' Variant interfaces can have nested interfaces.
    Interface INested : End Interface
End Interface
```

## <a name="implementing-variant-generic-interfaces"></a>实现变体泛型接口

在类中实现变体泛型接口时，所用语法和用于固定接口的语法相同。 以下代码示例演示如何在泛型类中实现协变接口。

```vb
Interface ICovariant(Of Out R)
    Function GetSomething() As R
End Interface

Class SampleImplementation(Of R)
    Implements ICovariant(Of R)
    Public Function GetSomething() As R _
    Implements ICovariant(Of R).GetSomething
        ' Some code.
    End Function
End Class
```

实现变体接口的类是固定类。 例如，考虑下面的代码。

```vb
 The interface is covariant.
Dim ibutton As ICovariant(Of Button) =
    New SampleImplementation(Of Button)
Dim iobj As ICovariant(Of Object) = ibutton

' The class is invariant.
Dim button As SampleImplementation(Of Button) =
    New SampleImplementation(Of Button)
' The following statement generates a compiler error
' because classes are invariant.
' Dim obj As SampleImplementation(Of Object) = button
```

## <a name="extending-variant-generic-interfaces"></a>扩展变体泛型接口

扩展变体泛型接口时，必须使用 `in` 和 `out` 关键字来显式指定派生接口是否支持变体。 编译器不会根据正在扩展的接口来推断变体。 例如，考虑以下接口。

```vb
Interface ICovariant(Of Out T)
End Interface

Interface IInvariant(Of T)
    Inherits ICovariant(Of T)
End Interface

Interface IExtCovariant(Of Out T)
    Inherits ICovariant(Of T)
End Interface
```

在 `Invariant(Of T)` 接口中，泛型类型参数 `T` 是固定的，而在 `IExtCovariant (Of Out T)` 类型参数中是协变的，尽管这两个接口都扩展了同一接口。 此规则也适用于逆变泛型类型参数。

无论泛型类型参数 `T` 在接口中是协变还是逆变，都可以创建一个接口来扩展这两类接口，只要在扩展接口中，该 `T` 泛型类型参数为固定参数。 以下代码示例阐释了这一点。

```vb
Interface ICovariant(Of Out T)
End Interface

Interface IContravariant(Of In T)
End Interface

Interface IInvariant(Of T)
    Inherits ICovariant(Of T), IContravariant(Of T)
End Interface
```

但是，如果泛型类型参数 `T` 在一个接口中声明为协变，则无法在扩展接口中将其声明为逆变，反之亦然。 以下代码示例阐释了这一点。

```vb
Interface ICovariant(Of Out T)
End Interface

' The following statements generate a compiler error.
' Interface ICoContraVariant(Of In T)
'     Inherits ICovariant(Of T)
' End Interface
```

### <a name="avoiding-ambiguity"></a>避免多义性

实现变体泛型接口时，变体有时可能会导致多义性。 应避免这种情况。

例如，如果在一个类中使用不同的泛型类型参数来显式实现同一变体泛型接口，便会产生多义性。 在这种情况下，编译器不会产生错误，但未指定将在运行时选择哪个接口实现。 这可能导致代码中出现微妙的 bug。 请考虑以下代码示例。

> [!NOTE]
> 对于 `Option Strict Off` ，当存在不明确的接口实现时，Visual Basic 会生成编译器警告。 对于 `Option Strict On` ，Visual Basic 会生成编译器错误。

```vb
' Simple class hierarchy.
Class Animal
End Class

Class Cat
    Inherits Animal
End Class

Class Dog
    Inherits Animal
End Class

' This class introduces ambiguity
' because IEnumerable(Of Out T) is covariant.
Class Pets
    Implements IEnumerable(Of Cat), IEnumerable(Of Dog)

    Public Function GetEnumerator() As IEnumerator(Of Cat) _
        Implements IEnumerable(Of Cat).GetEnumerator
        Console.WriteLine("Cat")
        ' Some code.
    End Function

    Public Function GetEnumerator1() As IEnumerator(Of Dog) _
        Implements IEnumerable(Of Dog).GetEnumerator
        Console.WriteLine("Dog")
        ' Some code.
    End Function

    Public Function GetEnumerator2() As IEnumerator _
        Implements IEnumerable.GetEnumerator
        ' Some code.
    End Function
End Class

Sub Main()
    Dim pets As IEnumerable(Of Animal) = New Pets()
    pets.GetEnumerator()
End Sub
```

在此示例中，没有指定 `pets.GetEnumerator` 方法如何在 `Cat` 和 `Dog` 之间选择。 这可能导致代码中出现问题。

## <a name="see-also"></a>另请参阅

- [泛型接口中的变体 (Visual Basic)](variance-in-generic-interfaces.md)
- [对 Func 和 Action 泛型委托使用变体 (Visual Basic)](using-variance-for-func-and-action-generic-delegates.md)
