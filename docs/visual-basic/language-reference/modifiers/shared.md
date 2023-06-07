---
title: 共享
ms.date: 07/20/2015
f1_keywords:
- vb.Shared
helpviewer_keywords:
- Shared keyword [Visual Basic]
- members [Visual Basic], shared
- shared members
- nonshared
- shared [elements VB]
- elements [Visual Basic], shared
ms.assetid: 2bf7cf2c-b0dd-485e-8749-b5d674dab4cd
ms.openlocfilehash: b51c88e1af3a720912af8ba6aaf8ae4016af9cfa
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990198"
---
# <a name="shared-visual-basic"></a>Shared (Visual Basic)

指定一个或多个已声明的编程元素与较大的类或结构关联，而不是与类或结构的特定实例相关联。

## <a name="when-to-use-shared"></a>何时使用共享

共享类或结构的成员使其可用于每个实例，而不是*非共享*，其中每个实例都保留其自己的副本。 例如，如果将变量的值应用于整个应用程序，则共享会很有用。 如果将该变量声明为 `Shared` ，则所有实例都将访问相同的存储位置，并且如果一个实例更改该变量的值，则所有实例都将访问更新的值。

共享不会更改成员的访问级别。 例如，类成员可以是共享的，也可以是专用的（只能从类中访问），也可以是非共享的和公共的。 有关详细信息，请参阅[Visual Basic 中的访问级别](../../programming-guide/language-features/declared-elements/access-levels.md)。

## <a name="rules"></a>规则

- **声明上下文。** 只能在模块级别使用 `Shared`。 这意味着元素的声明上下文 `Shared` 必须是类或结构，不能是源文件、命名空间或过程。

- **组合修饰符。** 不能 `Shared` 在同一声明中同时指定[替代](overrides.md)、[重写](overridable.md)、 [NotOverridable](notoverridable.md)、 [MustOverride](mustoverride.md)或[静态](static.md)。

- **访问.** 使用共享元素的类或结构名称（而不是其类或结构的特定实例的变量名称）来限定共享元素。 甚至不必创建类或结构的实例即可访问其共享成员。

     下面的示例调用结构公开的共享过程 <xref:System.Double.IsNaN%2A> <xref:System.Double> 。

     ```vb
     If Double.IsNaN(result) Then Console.WriteLine("Result is mathematically undefined.")
     ```

- **隐式共享。** 不能 `Shared` 在[Const 语句](../statements/const-statement.md)中使用修饰符，而是隐式共享常量。 同样，不能将模块或接口的成员声明为 `Shared` ，但它们是隐式共享的。

## <a name="behavior"></a>行为

- **储存.** 共享的变量或事件只存储在内存中，无论您创建的是多少或几个实例的类或结构。 同样，共享过程或属性仅保存一组局部变量。

- **通过实例变量访问。** 可以通过使用包含其类或结构的特定实例的变量的名称来限定共享元素，从而访问该共享元素。 虽然这通常按预期方式工作，但编译器会生成一条警告消息，并通过类或结构名称而不是变量来进行访问。

- **通过实例表达式访问。** 如果通过返回类或结构的实例的表达式访问共享元素，编译器将通过类或结构名称进行访问，而不是计算表达式。 如果你希望表达式执行其他操作以及返回实例，则此访问会产生意外结果。 下面的示例说明了这种情况。
  
    ```vb
    Sub Main()
        ' The following line is the preferred way to access Total.
        ShareTotal.Total = 10

        ' The following line generates a compiler warning message and
        ' accesses total through class ShareTotal instead of through
        ' the variable instanceVar. This works as expected and adds
        ' 100 to Total.
        Dim instanceVar As New ShareTotal
        instanceVar.Total += 100

        ' The following line generates a compiler warning message and
        ' accesses total through class ShareTotal instead of calling
        ' ReturnClass(). This adds 1000 to total but does not work as
        ' expected, because the WriteLine in ReturnClass() does not run.
        Console.WriteLine("Value of total is " & CStr(ShareTotal.Total))
        ReturnClass().Total += 1000
    End Sub

    Public Function ReturnClass() As ShareTotal
        Console.WriteLine("Function ReturnClass() called")
        Return New ShareTotal
    End Function

    Public Class ShareTotal
        Public Shared Property Total As Integer
    End Class
    ```

     在前面的示例中，编译器会在代码通过实例访问共享属性时生成警告消息 `Total` 。 在每种情况下，它通过类直接进行访问 `ShareTotal` ，不使用任何实例。 对于对过程的预期调用 `ReturnClass` ，这意味着它甚至不会生成对的调用 `ReturnClass` ，因此，不会执行显示 "Function ReturnClass （）" 的其他操作。

`Shared` 修饰符可用于下面的上下文中：

- [Dim 语句](../statements/dim-statement.md)
- [Event 语句](../statements/event-statement.md)
- [Function 语句](../statements/function-statement.md)
- [Operator Statement](../statements/operator-statement.md)
- [Property Statement](../statements/property-statement.md)
- [Sub 语句](../statements/sub-statement.md)
  
## <a name="see-also"></a>另请参阅

- [Shadows](shadows.md)
- [静态](static.md)
- [Visual Basic 中的生存期](../../programming-guide/language-features/declared-elements/lifetime.md)
- [过程](../../programming-guide/language-features/procedures/index.md)
- [结构](../../programming-guide/language-features/data-types/structures.md)
- [对象和类](../../programming-guide/language-features/objects-and-classes/index.md)
