---
title: 与其他异步模式和类型互操作
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- asynchronous design patterns, .NET
- TAP, .NET support for
- Task-based Asynchronous Pattern, .NET support for
- .NET, asynchronous design patterns
ms.assetid: f120a5d9-933b-4d1d-acb6-f034a57c3749
ms.openlocfilehash: 2ae1c514185152dd709fe06018df513fb54b874b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95726712"
---
# <a name="interop-with-other-asynchronous-patterns-and-types"></a>与其他异步模式和类型互操作

.NET 中异步模式的简短历史记录：

- .NET Framework 1.0 引进了 <xref:System.IAsyncResult> 模式，也称为[异步编程模型 (APM)](asynchronous-programming-model-apm.md) 或 `Begin/End` 模式。
- .NET Framework 2.0 增加了[基于事件的异步模式 (EAP)](event-based-asynchronous-pattern-eap.md)。
- .NET Framework 4 引进了[基于任务的异步模式 (TAP)](task-based-asynchronous-pattern-tap.md)，它取代了 APM 和 EAP，并能够轻松构建从早期模式中迁移的例程。
  
## <a name="tasks-and-the-asynchronous-programming-model-apm"></a>任务和异步编程模型 (APM)

### <a name="from-apm-to-tap"></a>从 APM 到 TAP  

 因为[异步编程模型 (APM)](asynchronous-programming-model-apm.md) 模式的结构合理，而且能够轻松生成包装，将 APM 实现公开为 TAP 实现。 .NET Framework 4 及更高版本包含采用 <xref:System.Threading.Tasks.TaskFactory.FromAsync%2A> 方法重载形式的帮助器例程来实现这种转换。  
  
 请考虑 <xref:System.IO.Stream> 类及其 <xref:System.IO.Stream.BeginRead%2A> 和 <xref:System.IO.Stream.EndRead%2A> 方法，它们代表与同步 <xref:System.IO.Stream.Read%2A> 方法对应的 APM：  
  
 [!code-csharp[Conceptual.AsyncInterop#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.AsyncInterop/cs/Stream1.cs#1)]
 [!code-vb[Conceptual.AsyncInterop#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.AsyncInterop/vb/stream1.vb#1)]  
[!code-csharp[Conceptual.AsyncInterop#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.AsyncInterop/cs/Stream1.cs#2)]
[!code-vb[Conceptual.AsyncInterop#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.AsyncInterop/vb/stream1.vb#2)]  
[!code-csharp[Conceptual.AsyncInterop#3](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.AsyncInterop/cs/Stream1.cs#3)]
[!code-vb[Conceptual.AsyncInterop#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.AsyncInterop/vb/stream1.vb#3)]  
  
 可以使用 <xref:System.Threading.Tasks.TaskFactory%601.FromAsync%2A?displayProperty=nameWithType> 方法来实现此操作的 TAP 包装，如下所示：  
  
 [!code-csharp[Conceptual.AsyncInterop#4](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.AsyncInterop/cs/Wrap1.cs#4)]
 [!code-vb[Conceptual.AsyncInterop#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.AsyncInterop/vb/Wrap1.vb#4)]  
  
 此实现类似于以下内容：  
  
 [!code-csharp[Conceptual.AsyncInterop#5](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.AsyncInterop/cs/Wrap2.cs#5)]
 [!code-vb[Conceptual.AsyncInterop#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.AsyncInterop/vb/Wrap2.vb#5)]  
  
### <a name="from-tap-to-apm"></a>从 TAP 到 APM  

 如果现有的基础结构需要 APM 模式，则还需要采用 TAP 实现并在需要 APM 实现的地方使用它。  由于任务可以组合，并且 <xref:System.Threading.Tasks.Task> 类实现 <xref:System.IAsyncResult>，您可以使用一个简单的 helper 函数执行此操作。 以下代码使用 <xref:System.Threading.Tasks.Task%601> 类的扩展，但可以对非泛型任务使用几乎相同的函数。  
  
 [!code-csharp[Conceptual.AsyncInterop#6](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.AsyncInterop/cs/APM1.cs#6)]
 [!code-vb[Conceptual.AsyncInterop#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.AsyncInterop/vb/APM1.vb#6)]  
  
 现在，请考虑具有以下 TAP 实现的用例：  
  
 [!code-csharp[Conceptual.AsyncInterop#7](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.AsyncInterop/cs/APM2.cs#7)]
 [!code-vb[Conceptual.AsyncInterop#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.AsyncInterop/vb/APM2.vb#7)]  
  
 并且想要提供此 APM 实现：  
  
 [!code-csharp[Conceptual.AsyncInterop#8](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.AsyncInterop/cs/APM2.cs#8)]
 [!code-vb[Conceptual.AsyncInterop#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.AsyncInterop/vb/APM2.vb#8)]  
[!code-csharp[Conceptual.AsyncInterop#9](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.AsyncInterop/cs/APM2.cs#9)]
[!code-vb[Conceptual.AsyncInterop#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.AsyncInterop/vb/APM2.vb#9)]  
  
 以下示例演示了一种向 APM 迁移的方法：  
  
 [!code-csharp[Conceptual.AsyncInterop#10](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.AsyncInterop/cs/APM2.cs#10)]
 [!code-vb[Conceptual.AsyncInterop#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.AsyncInterop/vb/APM2.vb#10)]  
  
## <a name="tasks-and-the-event-based-asynchronous-pattern-eap"></a>任务和基于事件的异步模式 (EAP)  

 包装 [基于事件的异步模式 (EAP)](event-based-asynchronous-pattern-eap.md) 实现比包装 APM 模式更为复杂，因为与 APM 模式相比，EAP 模式的变体更多，结构更少。  为了演示，以下代码包装了 `DownloadStringAsync` 方法。  `DownloadStringAsync` 接受 URI，在下载时引发 `DownloadProgressChanged` 事件，以报告进度的多个统计信息，并在完成时引发 `DownloadStringCompleted` 事件。  最终在指定 URI 中返回一个字符串，其中包含页面内容。  
  
 [!code-csharp[Conceptual.AsyncInterop#11](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.AsyncInterop/cs/EAP1.cs#11)]
 [!code-vb[Conceptual.AsyncInterop#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.AsyncInterop/vb/EAP1.vb#11)]  
  
## <a name="tasks-and-wait-handles"></a>任务和等待句柄  
  
### <a name="from-wait-handles-to-tap"></a>从等待句柄到 TAP  

 虽然等待句柄不能实现异步模式，但高级开发人员可以在设置等待句柄时使用 <xref:System.Threading.WaitHandle> 类和 <xref:System.Threading.ThreadPool.RegisterWaitForSingleObject%2A?displayProperty=nameWithType> 方法实现异步通知。  可以包装 <xref:System.Threading.ThreadPool.RegisterWaitForSingleObject%2A> 方法以在等待句柄中启用针对任何同步等待的基于任务的替代方法：  
  
 [!code-csharp[Conceptual.AsyncInterop#12](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.AsyncInterop/cs/Wait1.cs#12)]
 [!code-vb[Conceptual.AsyncInterop#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.AsyncInterop/vb/Wait1.vb#12)]  
  
 使用此方法，可以在异步方法中使用现有 <xref:System.Threading.WaitHandle> 实现。  例如，若要限制在任何特定时间执行的异步操作数，可以利用信号灯（<xref:System.Threading.SemaphoreSlim?displayProperty=nameWithType> 对象）。  可以将并发运行的操作数目限制到 N，方法为：初始化到 N 的信号量的数目、在想要执行操作时等待信号量，并在完成操作时释放信号量 ：  
  
 [!code-csharp[Conceptual.AsyncInterop#13](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.AsyncInterop/cs/Semaphore1.cs#13)]
 [!code-vb[Conceptual.AsyncInterop#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.AsyncInterop/vb/Semaphore1.vb#13)]  
  
 还可以构建不依赖等待句柄就完全可以处理任务的异步信号量。 若要执行此操作，可以使用 [使用基于任务的异步模式](consuming-the-task-based-asynchronous-pattern.md) 中所述的用于在 <xref:System.Threading.Tasks.Task>。  
  
### <a name="from-tap-to-wait-handles"></a>从 TAP 到等待句柄  

 正如前面所述， <xref:System.Threading.Tasks.Task> 类实现 <xref:System.IAsyncResult>，且该实现公开 <xref:System.Threading.Tasks.Task.System%23IAsyncResult%23AsyncWaitHandle%2A> 属性，该属性会返回在 <xref:System.Threading.Tasks.Task> 完成时设置的等待句柄。  可以获得 <xref:System.Threading.WaitHandle> 的 <xref:System.Threading.Tasks.Task> ，如下所示：  
  
 [!code-csharp[Conceptual.AsyncInterop#14](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.AsyncInterop/cs/Wait1.cs#14)]
 [!code-vb[Conceptual.AsyncInterop#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.AsyncInterop/vb/Wait1.vb#14)]  
  
## <a name="see-also"></a>请参阅

- [基于任务的异步模式 (TAP)](task-based-asynchronous-pattern-tap.md)
- [实现基于任务的异步模式](implementing-the-task-based-asynchronous-pattern.md)
- [使用基于任务的异步模式](consuming-the-task-based-asynchronous-pattern.md)
