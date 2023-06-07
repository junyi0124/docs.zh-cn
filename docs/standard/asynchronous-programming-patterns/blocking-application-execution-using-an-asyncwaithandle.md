---
title: 使用 AsyncWaitHandle 阻止应用程序的执行
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- blocks, asynchronous operations
- ending asynchronous operations
- asynchronous programming, ending operations
- asynchronous programming, blocking applications
- stopping asynchronous operations
- blocking application execution
ms.assetid: 3e32daf2-8161-4e8f-addd-9fd9ff101b03
ms.openlocfilehash: 1e7fc71ef5a4a44b2bb4c039305e06c157c65f75
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95716156"
---
# <a name="blocking-application-execution-using-an-asyncwaithandle"></a>使用 AsyncWaitHandle 阻止应用程序的执行

如果应用无法在等待异步操作结果期间继续执行其他工作，必须阻止应用一直到操作完成。 请使用下列方法之一，在应用等待异步操作完成期间阻止应用的主线程：  
  
- 使用异步操作的 BeginOperationName 方法返回的 <xref:System.IAsyncResult> 的 <xref:System.IAsyncResult.AsyncWaitHandle%2A> 属性。 本主题介绍的就是这种方法。  
  
- 调用异步操作的 EndOperationName   方法。 有关展示这种方法的示例，请参阅[通过结束异步操作阻止应用执行](blocking-application-execution-by-ending-an-async-operation.md)。  
  
 在异步操作完成前使用一个或多个 <xref:System.Threading.WaitHandle> 对象阻止的应用，通常会调用 BeginOperationName   方法，执行任何不需要等待操作结果也可以执行的工作，并在一个或多个异步操作完成前一直处于阻止状态。 可以使用 <xref:System.IAsyncResult.AsyncWaitHandle%2A> 调用 <xref:System.Threading.WaitHandle.WaitOne%2A> 方法之一，对单个操作阻止应用。 若要在等待一组异步操作完成期间阻止应用，请将关联的 <xref:System.IAsyncResult.AsyncWaitHandle%2A> 对象存储到数组中，并调用 <xref:System.Threading.WaitHandle.WaitAll%2A> 方法之一。 若要在等待一组异步操作中任一操作完成期间阻止应用，请将关联的 <xref:System.IAsyncResult.AsyncWaitHandle%2A> 对象存储到数组中，并调用 <xref:System.Threading.WaitHandle.WaitAny%2A> 方法之一。  
  
## <a name="example"></a>示例  

 下面的代码示例展示了如何使用 DNS 类中的异步方法，检索用户指定计算机的域名系统信息。 此示例展示了如何使用与异步操作关联的 <xref:System.Threading.WaitHandle> 阻止应用。 请注意，对 <xref:System.Net.Dns.BeginGetHostByName%2A>`requestCallback` 和 `stateObject` 参数传递的是 `null`（Visual Basic 中的 `Nothing`），因为使用这种方法时这些是可选参数。  
  
 [!code-csharp[AsyncDesignPattern#2](../../../samples/snippets/csharp/VS_Snippets_CLR/AsyncDesignPattern/CS/Async_EndBlockWait.cs#2)]
 [!code-vb[AsyncDesignPattern#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/AsyncDesignPattern/VB/Async_EndBlockWait.vb#2)]  
  
## <a name="see-also"></a>另请参阅

- [基于事件的异步模式 (EAP)](event-based-asynchronous-pattern-eap.md)
- [基于事件的异步模式概述](event-based-asynchronous-pattern-overview.md)
