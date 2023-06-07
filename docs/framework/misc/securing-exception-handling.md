---
title: 保护异常处理
description: 请参阅如何在 .NET 代码中确保异常处理的安全。 如果有 try、except、catch 和 finally 语句，请查看代码的运行顺序。
ms.date: 03/30/2017
dev_langs:
- cpp
helpviewer_keywords:
- code security, exception handling
- security [.NET Framework], exception handling
- secure coding, exception handling
- exception handling, security
ms.assetid: 1f3da743-9742-47ff-96e6-d0dd1e9e1c19
ms.openlocfilehash: c7643bb34da0cbcbd267fc90e6294bc0b565985e
ms.sourcegitcommit: c37e8d4642fef647ebab0e1c618ecc29ddfe2a0f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87855772"
---
# <a name="securing-exception-handling"></a>保护异常处理

[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]

在 Visual C++ 和 Visual Basic 中，堆栈中的一个筛选器表达式将在任何语句之前运行 `finally` 。 与该筛选器相关联的**catch**块在 `finally` 语句之后运行。 有关详细信息，请参阅[使用用户筛选的异常](../../standard/exceptions/using-user-filtered-exception-handlers.md)。 本部分将介绍此顺序的安全隐患。 请考虑以下伪代码示例，其中阐释了筛选语句和 `finally` 语句的运行顺序。  
  
```cpp  
void Main()
{  
    try
    {  
        Sub();  
    }
    except (Filter())
    {  
        Console.WriteLine("catch");  
    }  
}  
bool Filter () {  
    Console.WriteLine("filter");  
    return true;  
}  
void Sub()
{  
    try
    {  
        Console.WriteLine("throw");  
        throw new Exception();  
    }
    finally
    {  
        Console.WriteLine("finally");  
    }  
}
```  
  
 此代码将打印以下代码。  
  
```output
Throw  
Filter  
Finally  
Catch  
```  
  
 筛选器在语句之前运行 `finally` ，因此，在执行其他代码的情况下，任何更改状态的内容都可以引入安全问题。 例如：  
  
```cpp  
try
{  
    Alter_Security_State();  
    // This means changing anything (state variables,  
    // switching unmanaged context, impersonation, and
    // so on) that could be exploited if malicious
    // code ran before state is restored.  
    Do_some_work();  
}
finally
{  
    Restore_Security_State();  
    // This simply restores the state change above.  
}  
```  
  
 此伪代码允许堆栈上较高的筛选器运行任意代码。 具有类似效果的其他操作示例是对其他标识的临时模拟、设置跳过某些安全检查的内部标志或更改与线程关联的区域性。 建议的解决方案是引入异常处理程序，以将代码的更改从调用方的筛选器块隔离到线程状态。 但是，正确地引入异常处理程序或不解决此问题很重要。 下面的示例将切换 UI 区域性，但任何类型的线程状态更改都可能会以同样的方式公开。  
  
```cpp  
YourObject.YourMethod()  
{  
   CultureInfo saveCulture = Thread.CurrentThread.CurrentUICulture;  
   try {  
      Thread.CurrentThread.CurrentUICulture = new CultureInfo("de-DE");  
      // Do something that throws an exception.  
}  
   finally {  
      Thread.CurrentThread.CurrentUICulture = saveCulture;  
   }  
}  
```  
  
```vb  
Public Class UserCode  
   Public Shared Sub Main()  
      Try  
         Dim obj As YourObject = new YourObject  
         obj.YourMethod()  
      Catch e As Exception When FilterFunc  
         Console.WriteLine("An error occurred: '{0}'", e)  
         Console.WriteLine("Current Culture: {0}",
Thread.CurrentThread.CurrentUICulture)  
      End Try  
   End Sub  
  
   Public Function FilterFunc As Boolean  
      Console.WriteLine("Current Culture: {0}", Thread.CurrentThread.CurrentUICulture)  
      Return True  
   End Sub  
  
End Class  
```  
  
 在这种情况下，正确的修复方法是**try** / 在**try**catch 块中包装现有 try**finally**块 / **catch** 。 只需将**catch throw**子句引入现有**try** / **finally**块中，就不能解决问题，如以下示例中所示。  
  
```cpp  
YourObject.YourMethod()  
{  
    CultureInfo saveCulture = Thread.CurrentThread.CurrentUICulture;  
  
    try
    {  
        Thread.CurrentThread.CurrentUICulture = new CultureInfo("de-DE");  
        // Do something that throws an exception.  
    }  
    catch { throw; }  
    finally
    {  
        Thread.CurrentThread.CurrentUICulture = saveCulture;  
    }  
}  
```  
  
 这不会解决此问题，因为在 `finally` 获取控件之前语句尚未运行 `FilterFunc` 。  
  
 下面的示例通过确保在将 `finally` 异常提供给调用方的异常筛选器块之前执行了子句来修复此问题。  
  
```cpp  
YourObject.YourMethod()  
{  
    CultureInfo saveCulture = Thread.CurrentThread.CurrentUICulture;  
    try
    {  
        try
        {  
            Thread.CurrentThread.CurrentUICulture = new CultureInfo("de-DE");  
            // Do something that throws an exception.  
        }  
        finally
        {  
            Thread.CurrentThread.CurrentUICulture = saveCulture;  
        }  
    }  
    catch { throw; }  
}  
```  
  
## <a name="see-also"></a>另请参阅

- [安全编码准则](../../standard/security/secure-coding-guidelines.md)
