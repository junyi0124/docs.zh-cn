---
title: 对 Func 和 Action 泛型委托使用变体 (C#)
description: 了解如何在 Func 和 Action 泛型委托中使用协变和逆变，以便为代码提供更多的灵活性。
ms.date: 07/20/2015
ms.assetid: 1826774f-2b7a-470f-b110-17cfdd6abdae
ms.openlocfilehash: 613470d7870aa6a917d19904a92e56f0e61f1ed9
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91176339"
---
# <a name="using-variance-for-func-and-action-generic-delegates-c"></a>对 Func 和 Action 泛型委托使用变体 (C#)

这些示例演示如何使用 `Func` 和 `Action` 泛型委托中的协变和逆变来启用重用方法并为代码中提供更多的灵活性。  
  
 有关协变和逆变的详细信息，请参阅[委托中的变体 (C#)](./variance-in-delegates.md)。  
  
## <a name="using-delegates-with-covariant-type-parameters"></a>使用具有协变类型参数的委托  

 下例阐释了泛型 `Func` 委托中的协变支持的益处。 `FindByTitle` 方法采用 `String` 类型的一个参数，并返回 `Employee` 类型的一个对象。 但是，可将此方法分配给 `Func<String, Person>` 委托，因为 `Employee` 继承 `Person`。  
  
```csharp  
// Simple hierarchy of classes.  
public class Person { }  
public class Employee : Person { }  
class Program  
{  
    static Employee FindByTitle(String title)  
    {  
        // This is a stub for a method that returns  
        // an employee that has the specified title.  
        return new Employee();  
    }  
  
    static void Test()  
    {  
        // Create an instance of the delegate without using variance.  
        Func<String, Employee> findEmployee = FindByTitle;  
  
        // The delegate expects a method to return Person,  
        // but you can assign it a method that returns Employee.  
        Func<String, Person> findPerson = FindByTitle;  
  
        // You can also assign a delegate
        // that returns a more derived type
        // to a delegate that returns a less derived type.  
        findPerson = findEmployee;  
  
    }  
}  
```  
  
## <a name="using-delegates-with-contravariant-type-parameters"></a>使用具有逆变类型参数的委托  

 下例阐释了泛型 `Action` 委托中的逆变支持的益处。 `AddToContacts` 方法采用 `Person` 类型的一个参数。 但是，可将此方法分配给 `Action<Employee>` 委托，因为 `Employee` 继承 `Person`。  
  
```csharp  
public class Person { }  
public class Employee : Person { }  
class Program  
{  
    static void AddToContacts(Person person)  
    {  
        // This method adds a Person object  
        // to a contact list.  
    }  
  
    static void Test()  
    {  
        // Create an instance of the delegate without using variance.  
        Action<Person> addPersonToContacts = AddToContacts;  
  
        // The Action delegate expects
        // a method that has an Employee parameter,  
        // but you can assign it a method that has a Person parameter  
        // because Employee derives from Person.  
        Action<Employee> addEmployeeToContacts = AddToContacts;  
  
        // You can also assign a delegate
        // that accepts a less derived parameter to a delegate
        // that accepts a more derived parameter.  
        addEmployeeToContacts = addPersonToContacts;  
    }  
}  
```  
  
## <a name="see-also"></a>请参阅

- [协变和逆变 (C#)](./index.md)
- [泛型](../../../../standard/generics/index.md)
