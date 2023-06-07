---
title: 通过结束异步操作来阻止应用程序执行
ms.date: 03/30/2017
helpviewer_keywords:
- blocks, asynchronous operations
- AsyncWaitHandle property
- asynchronous programming, blocking applications
- blocking application execution
ms.assetid: cc5e2834-a65b-4df8-b750-7bdb79997fee
dev_langs:
- csharp
- vb
ms.openlocfilehash: d99c09c4ac087152407fa8dc12894c216f9f43dc
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95716169"
---
# <a name="blocking-application-execution-by-ending-an-async-operation"></a>通过结束异步操作来阻止应用程序执行

如果应用无法在等待异步操作结果期间继续执行其他工作，必须阻止应用一直到操作完成。 请使用下列方法之一，在应用等待异步操作完成期间阻止应用的主线程：  
  
- 调用异步操作的 EndOperationName 方法   。 本主题介绍的就是这种方法。  
  
- 使用异步操作的 BeginOperationName 方法返回的 <xref:System.IAsyncResult> 的 <xref:System.IAsyncResult.AsyncWaitHandle%2A> 属性。 有关展示这种方法的示例，请参阅[使用 AsyncWaitHandle 阻止应用执行](blocking-application-execution-using-an-asyncwaithandle.md)。  
  
 在异步操作完成前使用 End _OperationName 方法阻止的应用程序，通常会调用 Begin _OperationName 方法，执行任何不需要等待操作结果也可以执行的工作，然后调用 End _OperationName___。  
  
## <a name="example"></a>示例  

 下面的代码示例展示了如何使用 <xref:System.Net.Dns> 类中的异步方法，检索用户指定计算机的域名系统信息。 请注意，对 <xref:System.Net.Dns.BeginGetHostByName%2A>`requestCallback` 和 `stateObject` 参数传递的是 `null`（Visual Basic 中的 `Nothing`），因为使用这种方法时这些是可选参数。  
  
 [!code-csharp[AsyncDesignPattern#1](../../../samples/snippets/csharp/VS_Snippets_CLR/AsyncDesignPattern/CS/Async_EndBlock.cs#1)]
 [!code-vb[AsyncDesignPattern#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/AsyncDesignPattern/VB/Async_EndBlock.vb#1)]  
  
## <a name="see-also"></a>另请参阅

- [基于事件的异步模式 (EAP)](event-based-asynchronous-pattern-eap.md)
- [基于事件的异步模式概述](event-based-asynchronous-pattern-overview.md)
