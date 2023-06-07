---
title: 任务取消
description: 了解 Task 类和 Task<TResult> 类中通过使用 .NET 中的取消令牌支持的任务取消。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tasks, cancellation
- asynchronous task cancellation
ms.assetid: 3ecf1ea9-e399-4a6a-a0d6-8475f48dcb28
ms.openlocfilehash: deea3eaaa1e652d44a71e953975d776d4898a9b3
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94830007"
---
# <a name="task-cancellation"></a>任务取消

<xref:System.Threading.Tasks.Task?displayProperty=nameWithType> 和 <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType> 类支持通过使用取消标记进行取消。 有关详细信息，请参阅[托管线程中的取消](../threading/cancellation-in-managed-threads.md)。 在 Task 类中，取消涉及用户委托间的协作，这表示可取消的操作和请求取消的代码。 成功取消涉及调用 <xref:System.Threading.CancellationTokenSource.Cancel%2A?displayProperty=nameWithType> 方法的请求代码，以及及时终止操作的用户委托。 可以使用以下选项之一终止操作：  
  
- 简单地从委托中返回。 在许多情况下，这样已足够；但是，采用这种方式取消的任务实例会转换为 <xref:System.Threading.Tasks.TaskStatus.RanToCompletion?displayProperty=nameWithType> 状态，而不是 <xref:System.Threading.Tasks.TaskStatus.Canceled?displayProperty=nameWithType> 状态。  
  
- 引发 <xref:System.OperationCanceledException> ，并将其传递到在其上请求了取消的标记。 完成此操作的首选方式是使用 <xref:System.Threading.CancellationToken.ThrowIfCancellationRequested%2A> 方法。 采用这种方式取消的任务会转换为 Canceled 状态，调用代码可使用该状态来验证任务是否响应了其取消请求。  
  
 下面的示例演示引发异常的任务取消的基本模式。 请注意，标记将传递到用户委托和任务实例本身。  
  
 [!code-csharp[TPL_Cancellation#02](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_cancellation/cs/snippet02.cs#02)]
 [!code-vb[TPL_Cancellation#02](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_cancellation/vb/module1.vb#02)]  
  
 有关更完整的示例，请参见[如何：取消任务及其子级](how-to-cancel-a-task-and-its-children.md)。  
  
 当任务实例观察到用户代码引发的 <xref:System.OperationCanceledException> 时，它会将该异常的标记与其关联的标记（传递到创建任务的 API 的标记）进行比较。 如果这两个标记相同，并且标记的 <xref:System.Threading.CancellationToken.IsCancellationRequested%2A> 属性返回 true，则任务会将此解释为确认取消并转换为 Canceled 状态。 如果您不使用 <xref:System.Threading.Tasks.Task.Wait%2A> 或 <xref:System.Threading.Tasks.Task.WaitAll%2A> 方法来等待任务，则任务只会将其状态设置为 <xref:System.Threading.Tasks.TaskStatus.Canceled>。  
  
 如果你在等待转换为 Canceled 状态的任务，则会引发 <xref:System.Threading.Tasks.TaskCanceledException?displayProperty=nameWithType> 异常（包装在 <xref:System.AggregateException> 异常中）。 请注意，此异常指示成功的取消，而不是有错误的情况。 因此，任务的 <xref:System.Threading.Tasks.Task.Exception%2A> 属性返回 `null`。  
  
 如果标记的 <xref:System.Threading.CancellationToken.IsCancellationRequested%2A> 属性返回 false，或者异常的标记与任务的标记不匹配，则会将 <xref:System.OperationCanceledException> 按照普通的异常来处理，从而导致任务转换为 Faulted 状态。 另外还要注意，其他异常的存在将也会导致任务转换为 Faulted 状态。 您可以在 <xref:System.Threading.Tasks.Task.Status%2A> 属性中获取已完成任务的状态。  
  
 在请求取消操作之后，任务可能还可以继续处理一些项目。  
  
## <a name="see-also"></a>请参阅

- [托管线程中的取消](../threading/cancellation-in-managed-threads.md)
- [如何：取消任务及其子级](how-to-cancel-a-task-and-its-children.md)
