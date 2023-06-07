---
title: 安全和争用条件
'description:': Describes pitfalls to avoid around security holes exploited by race conditions, including dispose methods, constructors, cached objects, and finalizers.
ms.date: 07/15/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- security [.NET], race conditions
- race conditions
- secure coding, race conditions
- code security, race conditions
ms.assetid: ea3edb80-b2e8-4e85-bfed-311b20cb59b6
ms.openlocfilehash: 870dc0ac956bad045cb87b9c0968b4a8e9733812
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94824117"
---
# <a name="security-and-race-conditions"></a>安全和争用条件

还有一个需要注意的问题是争用情况可能会导致安全漏洞。 可以通过多种方式来执行此操作。 以下子主题概述了开发人员必须避免的一些主要缺陷。  
  
## <a name="race-conditions-in-the-dispose-method"></a>Dispose 方法中的争用条件  

如果类的 **Dispose** 方法 (详细信息，请参阅未同步的 [垃圾回收](../garbage-collection/index.md)) ，可以多次运行 **Dispose** 中的清理代码，如下面的示例中所示。  
  
```vb  
Sub Dispose()  
    If Not (myObj Is Nothing) Then  
       Cleanup(myObj)  
       myObj = Nothing  
    End If  
End Sub  
```  
  
```csharp  
void Dispose()
{  
    if (myObj != null)
    {  
        Cleanup(myObj);  
        myObj = null;  
    }  
}  
```  
  
由于此 **Dispose** 实现不同步，因此可以 `Cleanup` 通过前一个线程调用，然后在 `_myObj` 设置为 **null** 之前调用另一个线程。 这是否是一个安全问题取决于代码运行时发生的情况 `Cleanup` 。 不同步 **Dispose** 实现的主要问题涉及使用资源句柄（如文件）。 不当处置可能会导致使用错误的句柄，这通常会导致安全漏洞。  
  
## <a name="race-conditions-in-constructors"></a>构造函数中的争用条件

在某些应用程序中，其他线程可能在其类构造函数完全运行之前访问类成员。 应查看所有类构造函数，以确保在发生这种情况时不会出现安全问题，如有必要，还应同步线程。  
  
## <a name="race-conditions-with-cached-objects"></a>带有缓存对象的争用条件  

如果类的其他部分未进行适当同步，则缓存安全信息或使用代码访问安全 [断言](../../framework/misc/using-the-assert-method.md) 操作的代码也可能易受到争用条件的影响，如以下示例中所示。  
  
```vb  
Sub SomeSecureFunction()  
    If SomeDemandPasses() Then  
        fCallersOk = True  
        DoOtherWork()  
        fCallersOk = False  
    End If  
End Sub  
  
Sub DoOtherWork()  
    If fCallersOK Then  
        DoSomethingTrusted()  
    Else  
        DemandSomething()  
        DoSomethingTrusted()  
    End If  
End Sub  
```  
  
```csharp  
void SomeSecureFunction()
{  
    if (SomeDemandPasses())
    {  
        fCallersOk = true;  
        DoOtherWork();  
        fCallersOk = false;  
    }  
}  
void DoOtherWork()
{  
    if (fCallersOK)
    {  
        DoSomethingTrusted();  
    }  
    else
    {  
        DemandSomething();  
        DoSomethingTrusted();  
    }  
}  
```  
  
如果有其他路径 `DoOtherWork` 可以从具有相同对象的另一个线程调用，则不受信任的调用方可以按需求。  
  
如果你的代码缓存安全信息，请确保查看此漏洞。  
  
## <a name="race-conditions-in-finalizers"></a>终结器中的争用条件  

争用条件也可能出现在引用静态或非托管资源的对象中，该对象随后会在其终结器中释放。 如果多个对象共享在类的终结器中操作的资源，则这些对象必须同步对该资源的所有访问。  
  
## <a name="see-also"></a>另请参阅

- [安全编码准则](secure-coding-guidelines.md)
- [ASP.NET Core 安全性](/aspnet/core/security/)
