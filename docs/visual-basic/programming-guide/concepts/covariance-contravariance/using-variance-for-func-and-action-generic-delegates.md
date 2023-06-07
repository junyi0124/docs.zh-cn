---
title: 对 Func 和 Action 泛型委托使用变体
ms.date: 07/20/2015
ms.assetid: 36c3012f-b39c-493b-b90f-079b5912ac1b
ms.openlocfilehash: f824d2422d67f1395d21a0863ca8c95d9f108989
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84375753"
---
# <a name="using-variance-for-func-and-action-generic-delegates-visual-basic"></a>对 Func 和 Action 泛型委托使用变体 (Visual Basic)

这些示例演示如何使用 `Func` 和 `Action` 泛型委托中的协变和逆变来启用重用方法并为代码中提供更多的灵活性。

有关协变和逆变的详细信息，请参阅[委托中的变体（Visual Basic）](variance-in-delegates.md)。

## <a name="using-delegates-with-covariant-type-parameters"></a>使用具有协变类型参数的委托

下例阐释了泛型 `Func` 委托中的协变支持的益处。 `FindByTitle` 方法采用 `String` 类型的一个参数，并返回 `Employee` 类型的一个对象。 但是，可将此方法分配给 `Func(Of String, Person)` 委托，因为 `Employee` 继承 `Person`。

```vb
' Simple hierarchy of classes.
Public Class Person
End Class

Public Class Employee
    Inherits Person
End Class

Class Finder
    Public Shared Function FindByTitle(
        ByVal title As String) As Employee
        ' This is a stub for a method that returns
        ' an employee that has the specified title.
        Return New Employee
    End Function

    Sub Test()
        ' Create an instance of the delegate without using variance.
        Dim findEmployee As Func(Of String, Employee) =
            AddressOf FindByTitle

        ' The delegate expects a method to return Person,
        ' but you can assign it a method that returns Employee.
        Dim findPerson As Func(Of String, Person) =
            AddressOf FindByTitle

        ' You can also assign a delegate
        ' that returns a more derived type to a delegate
        ' that returns a less derived type.
        findPerson = findEmployee
    End Sub
End Class
```

## <a name="using-delegates-with-contravariant-type-parameters"></a>使用具有逆变类型参数的委托

下例阐释了泛型 `Action` 委托中的逆变支持的益处。 `AddToContacts` 方法采用 `Person` 类型的一个参数。 但是，可将此方法分配给 `Action(Of Employee)` 委托，因为 `Employee` 继承 `Person`。

```vb
Public Class Person
End Class

Public Class Employee
    Inherits Person
End Class

Class AddressBook
    Shared Sub AddToContacts(ByVal person As Person)
        ' This method adds a Person object
        ' to a contact list.
    End Sub

    Sub Test()
        ' Create an instance of the delegate without using variance.
        Dim addPersonToContacts As Action(Of Person) =
            AddressOf AddToContacts

        ' The Action delegate expects
        ' a method that has an Employee parameter,
        ' but you can assign it a method that has a Person parameter
        ' because Employee derives from Person.
        Dim addEmployeeToContacts As Action(Of Employee) =
            AddressOf AddToContacts

        ' You can also assign a delegate
        ' that accepts a less derived parameter
        ' to a delegate that accepts a more derived parameter.
        addEmployeeToContacts = addPersonToContacts
    End Sub
End Class
```

## <a name="see-also"></a>另请参阅

- [协变和逆变 (Visual Basic)](index.md)
- [泛型](../../../../standard/generics/index.md)
