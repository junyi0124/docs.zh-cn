---
title: 在泛型集合的接口中使用变体
ms.date: 07/20/2015
ms.assetid: c867fcea-7462-4995-b9c5-542feec74036
ms.openlocfilehash: b762ce42215f9b24371313446637e95962677bfb
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84375636"
---
# <a name="using-variance-in-interfaces-for-generic-collections-visual-basic"></a>在泛型集合的接口中使用变体 (Visual Basic)

协变接口允许其方法返回的派生类型多于接口中指定的派生类型。 逆变接口允许其方法接受派生类型少于接口中指定的类型的参数。

在.NET Framework 4 中，多个现有接口已变为协变和逆变接口。 包括 <xref:System.Collections.Generic.IEnumerable%601> 和 <xref:System.IComparable%601>。 这使你可将对基类型的泛型集合进行操作的那些方法重用于派生类型的集合。

有关 .NET Framework 中的变体接口的列表，请参阅[泛型接口中的变体（Visual Basic）](variance-in-generic-interfaces.md)。

## <a name="converting-generic-collections"></a>转换泛型集合

下例阐释了 <xref:System.Collections.Generic.IEnumerable%601> 接口中的协变支持的益处。 `PrintFullName` 方法接受 `IEnumerable(Of Person)` 类型的集合作为参数。 但可将该方法重用于 `IEnumerable(Of Person)` 类型的集合，因为 `Employee` 继承 `Person`。

```vb
' Simple hierarchy of classes.
Public Class Person
    Public Property FirstName As String
    Public Property LastName As String
End Class

Public Class Employee
    Inherits Person
End Class

' The method has a parameter of the IEnumerable(Of Person) type.
Public Sub PrintFullName(ByVal persons As IEnumerable(Of Person))
    For Each person As Person In persons
        Console.WriteLine(
            "Name: " & person.FirstName & " " & person.LastName)
    Next
End Sub

Sub Main()
    Dim employees As IEnumerable(Of Employee) = New List(Of Employee)

    ' You can pass IEnumerable(Of Employee),
    ' although the method expects IEnumerable(Of Person).

    PrintFullName(employees)

End Sub
```

## <a name="comparing-generic-collections"></a>比较泛型集合

下例阐释了 <xref:System.Collections.Generic.IComparer%601> 接口中的逆变支持的益处。 `PersonComparer` 类实现 `IComparer(Of Person)` 接口。 但可以重用此类来比较 `Employee` 类型的对象序列，因为 `Employee` 继承 `Person`。

```vb
' Simple hierarchy of classes.
Public Class Person
    Public Property FirstName As String
    Public Property LastName As String
End Class

Public Class Employee
    Inherits Person
End Class
' The custom comparer for the Person type
' with standard implementations of Equals()
' and GetHashCode() methods.
Class PersonComparer
    Implements IEqualityComparer(Of Person)

    Public Function Equals1(
        ByVal x As Person,
        ByVal y As Person) As Boolean _
        Implements IEqualityComparer(Of Person).Equals

        If x Is y Then Return True
        If x Is Nothing OrElse y Is Nothing Then Return False
        Return (x.FirstName = y.FirstName) AndAlso
            (x.LastName = y.LastName)
    End Function
    Public Function GetHashCode1(
        ByVal person As Person) As Integer _
        Implements IEqualityComparer(Of Person).GetHashCode

        If person Is Nothing Then Return 0
        Dim hashFirstName =
            If(person.FirstName Is Nothing,
            0, person.FirstName.GetHashCode())
        Dim hashLastName = person.LastName.GetHashCode()
        Return hashFirstName Xor hashLastName
    End Function
End Class

Sub Main()
    Dim employees = New List(Of Employee) From {
        New Employee With {.FirstName = "Michael", .LastName = "Alexander"},
        New Employee With {.FirstName = "Jeff", .LastName = "Price"}
    }

    ' You can pass PersonComparer,
    ' which implements IEqualityComparer(Of Person),
    ' although the method expects IEqualityComparer(Of Employee)

    Dim noduplicates As IEnumerable(Of Employee) = employees.Distinct(New PersonComparer())

    For Each employee In noduplicates
        Console.WriteLine(employee.FirstName & " " & employee.LastName)
    Next
End Sub
```

## <a name="see-also"></a>另请参阅

- [泛型接口中的变体 (Visual Basic)](variance-in-generic-interfaces.md)
