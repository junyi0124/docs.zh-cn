---
title: 异步编程模式
description: 了解 .NET 中的基于任务的异步模式 (TAP)、基于事件的异步模式 (EAP) 和异步编程模型 (APM)。
ms.date: 10/16/2018
helpviewer_keywords:
- asynchronous design patterns, .NET
- .NET, asynchronous design patterns
ms.assetid: 4ece5c0b-f8fe-4114-9862-ac02cfe5a5d7
ms.openlocfilehash: bc0e37c060ab6375f943b4b50053e3046c05a556
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94830332"
---
# <a name="asynchronous-programming-patterns"></a>异步编程模式

.NET 提供了执行异步操作的三种模式：  

- **基于任务的异步模式 (TAP)** ，该模式使用单一方法表示异步操作的开始和完成。 TAP 是在 .NET Framework 4 中引入的。 **这是在 .NET 中进行异步编程的推荐方法。** C# 中的 [async](../../csharp/language-reference/keywords/async.md) 和 [await](../../csharp/language-reference/operators/await.md) 关键词以及 Visual Basic 中的 [Async](../../visual-basic/language-reference/modifiers/async.md) 和 [Await](../../visual-basic/language-reference/operators/await-operator.md) 运算符为 TAP 添加了语言支持。 有关详细信息，请参阅[基于任务的异步模式 (TAP)](task-based-asynchronous-pattern-tap.md)。  

- 基于事件的异步模式 (EAP)，是提供异步行为的基于事件的旧模型。 这种模式需要后缀为 `Async` 的方法，以及一个或多个事件、事件处理程序委托类型和 `EventArg` 派生类型。 EAP 是在 .NET Framework 2.0 中引入的。 建议新开发中不再使用这种模式。 有关详细信息，请参阅[基于事件的异步模式 (EAP)](event-based-asynchronous-pattern-eap.md)。  

- 异步编程模型 (APM) 模式（也称为 <xref:System.IAsyncResult> 模式），这是使用 <xref:System.IAsyncResult> 接口提供异步行为的旧模型。 在这种模式下，同步操作需要 `Begin` 和 `End` 方法（例如，`BeginWrite` 和 `EndWrite`以实现异步写入操作）。 不建议新的开发使用此模式。 有关详细信息，请参阅[异步编程模型 (APM)](asynchronous-programming-model-apm.md)。  
  
## <a name="comparison-of-patterns"></a>模式的比较

为了快速比较这三种模式的异步操作方式，请考虑使用从指定偏移量处起将指定量数据读取到提供的缓冲区中的`Read`方法：  
  
```csharp  
public class MyClass  
{  
    public int Read(byte [] buffer, int offset, int count);  
}  
```  

此方法对应的 TAP 将公开以下单个 `ReadAsync` 方法：  
  
```csharp
public class MyClass  
{  
    public Task<int> ReadAsync(byte [] buffer, int offset, int count);  
}  
```

对应的 EAP 将公开以下类型和成员的集：  
  
```csharp  
public class MyClass  
{  
    public void ReadAsync(byte [] buffer, int offset, int count);  
    public event ReadCompletedEventHandler ReadCompleted;  
}  
```  
  
对应的 APM 将公开 `BeginRead` 和 `EndRead` 方法：  
  
```csharp  
public class MyClass  
{  
    public IAsyncResult BeginRead(  
        byte [] buffer, int offset, int count,
        AsyncCallback callback, object state);  
    public int EndRead(IAsyncResult asyncResult);  
}  
```  

## <a name="see-also"></a>请参阅

- [深入了解异步](../async-in-depth.md)
- [C# 中的异步编程](../../csharp/async.md)
- [F# 中的异步编程](../../fsharp/tutorials/asynchronous-and-concurrent-programming/async.md)
- [使用 Async 和 Await 的异步编程 (Visual Basic)](../../visual-basic/programming-guide/concepts/async/index.md)
