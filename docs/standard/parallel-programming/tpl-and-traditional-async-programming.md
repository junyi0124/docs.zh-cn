---
title: TPL 和传统 .NET 异步编程
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tasks, with other asynchronous models
ms.assetid: e7b31170-a156-433f-9f26-b1fc7cd1776f
ms.openlocfilehash: bd6f32f8e77161007f4cfe49fa808167a17a3ec6
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94829981"
---
# <a name="tpl-and-traditional-net-asynchronous-programming"></a>TPL 和传统 .NET 异步编程

.NET 提供了以下两种标准模式，用于执行 I/O 密集型和计算密集型异步操作：  
  
- 异步编程模型 (APM)，在该模式中异步操作由一对 Begin/End 方法表示。 例如 <xref:System.IO.FileStream.BeginRead%2A?displayProperty=nameWithType> 和 <xref:System.IO.Stream.EndRead%2A?displayProperty=nameWithType>。  
  
- 基于事件的异步模式 (EAP)，在该模式中异步操作由名为 `<OperationName>Async` 和 `<OperationName>Completed` 的方法/事件对表示。 例如 <xref:System.Net.WebClient.DownloadStringAsync%2A?displayProperty=nameWithType> 和 <xref:System.Net.WebClient.DownloadStringCompleted?displayProperty=nameWithType>。
  
任务并行库 (TPL) 可采用各种方法与任一异步模式协同使用。 可将 APM 和 EAP 操作作为 `Task` 对象向库使用者公开，也可以公开 APM 模式但用 `Task` 对象在内部实现它们。 在这两种情况下，可使用 `Task` 对象简化代码以及利用以下有用的功能：  
  
- 在任务开始后随时以任务延续形式注册回调。  
  
- 使用 <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAll%2A> 和 <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAny%2A> 方法，或者 <xref:System.Threading.Tasks.Task.WaitAll%2A> 和 <xref:System.Threading.Tasks.Task.WaitAny%2A> 方法并列为响应 `Begin_` 方法而执行的多个操作。  
  
- 封装同一 `Task` 对象中的异步 I/O 密集型和计算密集型操作。  
  
- 监视 `Task` 对象的状态。  
  
- 使用 <xref:System.Threading.Tasks.TaskCompletionSource%601> 将操作状态封送处理至 `Task` 对象。  
  
## <a name="wrap-apm-operations-in-a-task"></a>在 Task 中包装 APM 操作  

 <xref:System.Threading.Tasks.TaskFactory?displayProperty=nameWithType> 和 <xref:System.Threading.Tasks.TaskFactory%601?displayProperty=nameWithType> 类都提供了 <xref:System.Threading.Tasks.TaskFactory.FromAsync%2A?displayProperty=nameWithType> 和 <xref:System.Threading.Tasks.TaskFactory%601.FromAsync%2A?displayProperty=nameWithType> 方法的几个重载，由此可将 APM begin/end 方法对封装在 <xref:System.Threading.Tasks.Task> 或 <xref:System.Threading.Tasks.Task%601> 实例中。 各种重载都可容纳任何具有零至三个输入参数的 begin/end 方法对。  
  
 对于具有返回值（在 Visual Basic 中为 `Function`）的 `End` 方法的对，使用 <xref:System.Threading.Tasks.TaskFactory%601> 中创建 <xref:System.Threading.Tasks.Task%601> 的方法。 对于具有返回 void（在 Visual Basic 中为 `Sub`）的 `End` 方法，使用 <xref:System.Threading.Tasks.TaskFactory> 中创建 <xref:System.Threading.Tasks.Task> 的方法。  
  
 在极少情况下，如果 `Begin` 方法具有三个以上参数或包含 `ref` 或 `out` 参数，则提供仅封装 `End` 方法的其他 `FromAsync` 重载。  
  
 下面的示例显示了匹配 <xref:System.IO.FileStream.BeginRead%2A?displayProperty=nameWithType> 和 <xref:System.IO.FileStream.EndRead%2A?displayProperty=nameWithType> 方法的 `FromAsync` 重载的签名。
  
 [!code-csharp[FromAsync#01](../../../samples/snippets/csharp/VS_Snippets_Misc/fromasync/cs/fromasync.cs#01)]
 [!code-vb[FromAsync#01](../../../samples/snippets/visualbasic/VS_Snippets_Misc/fromasync/vb/module1.vb#01)]  
  
此重载采用三个输入参数，如下所示。 第一个参数是匹配 <xref:System.IO.FileStream.BeginRead%2A?displayProperty=nameWithType> 方法签名的 <xref:System.Func%606> 委托。 第二个参数使用 <xref:System.IAsyncResult> 并返回 `TResult` 的 <xref:System.Func%602> 委托。 由于 <xref:System.IO.FileStream.EndRead%2A> 返回一个整数，因此编译器会将 `TResult` 类型推断为 <xref:System.Int32> 并将任务类型推断为 <xref:System.Threading.Tasks.Task>。 最后第四个参数与 <xref:System.IO.FileStream.BeginRead%2A?displayProperty=nameWithType> 方法中的参数相同：  
  
- 存储文件数据的缓冲区。  
  
- 开始写入数据的缓冲区的偏移量。  
  
- 要从文件中读取的最大数据量。  
  
- 存储要传递至回调的用户定义状态数据的可选对象。  
  
### <a name="use-continuewith-for-the-callback-functionality"></a>使用 ContinueWith 执行回调功能

 如果需要访问文件中的数据，而不仅仅访问字节数，则 <xref:System.Threading.Tasks.TaskFactory.FromAsync%2A> 方法不能满足此操作。 请改用 <xref:System.Threading.Tasks.Task>，其 `Result` 属性包含文件数据。 可以通过向原始任务添加延续来实现这种操作。 延续执行通常由 <xref:System.AsyncCallback> 委托执行的任务。 先前任务完成且填充了数据缓冲区后调用此操作。 （<xref:System.IO.FileStream> 对象应在返回前关闭。）  
  
 下面的示例演示如何返回封装 <xref:System.IO.FileStream> 类的 `BeginRead`/`EndRead` 对的 <xref:System.Threading.Tasks.Task>。  
  
 [!code-csharp[FromAsync#03](../../../samples/snippets/csharp/VS_Snippets_Misc/fromasync/cs/fromasync.cs#03)]
 [!code-vb[FromAsync#03](../../../samples/snippets/visualbasic/VS_Snippets_Misc/fromasync/vb/module1.vb#03)]  
  
 然后可调用此方法，如下所示。  
  
 [!code-csharp[FromAsync#04](../../../samples/snippets/csharp/VS_Snippets_Misc/fromasync/cs/fromasync.cs#04)]
 [!code-vb[FromAsync#04](../../../samples/snippets/visualbasic/VS_Snippets_Misc/fromasync/vb/module1.vb#04)]  
  
### <a name="provide-custom-state-data"></a>提供自定义状态数据

 在通常的 <xref:System.IAsyncResult> 操作中，如果<xref:System.AsyncCallback> 委托需要一些自定义状态数据，则必须通过 `Begin` 方法中的最后一个参数将它传入，以便可将数据打包到最终要传递至回调方法的 <xref:System.IAsyncResult> 对象中。 当使用 `FromAsync` 方法时，通常无需此操作。 如果延续知道自定义数据，可直接在延续委托中捕获它。 下面的示例与以前的示例类似，但延续检查此延续的用户委托可直接访问的自定义状态数据，而不是检查历史任务的 `Result` 属性。  
  
 [!code-csharp[FromAsync#05](../../../samples/snippets/csharp/VS_Snippets_Misc/fromasync/cs/fromasync.cs#05)]
 [!code-vb[FromAsync#05](../../../samples/snippets/visualbasic/VS_Snippets_Misc/fromasync/vb/module1.vb#05)]  
  
### <a name="synchronize-multiple-fromasync-tasks"></a>同步多个 FromAsync 任务

 当结合使用 `FromAsync` 方法时，静态 <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAll%2A> 和 <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAny%2A> 方法具有更大的灵活性。 下面的示例显示如何启动多个异步 I/O 操作，然后等待所有这些操作都完成后再执行延续。  
  
 [!code-csharp[FromAsync#06](../../../samples/snippets/csharp/VS_Snippets_Misc/fromasync/cs/fromasync.cs#06)]
 [!code-vb[FromAsync#06](../../../samples/snippets/visualbasic/VS_Snippets_Misc/fromasync/vb/module1.vb#06)]  
  
### <a name="fromasync-tasks-for-only-the-end-method"></a>仅用于 End 方法的 FromAsync 任务

 在极少情况下，如果 `Begin` 方法需要三个以上的输入参数，或具有 `ref` 或 `out` 参数，可以使用仅表示 `End` 方法的 `FromAsync` 重载，例如，<xref:System.Threading.Tasks.TaskFactory%601.FromAsync%28System.IAsyncResult%2CSystem.Func%7BSystem.IAsyncResult%2C%600%7D%29?displayProperty=nameWithType>。 这些方法还可用于传递 <xref:System.IAsyncResult> 并将其封装到 Task 的任何方案中。  
  
 [!code-csharp[FromAsync#07](../../../samples/snippets/csharp/VS_Snippets_Misc/fromasync/cs/fromasync.cs#07)]
 [!code-vb[FromAsync#07](../../../samples/snippets/visualbasic/VS_Snippets_Misc/fromasync/vb/module1.vb#07)]  
  
### <a name="start-and-cancel-fromasync-tasks"></a>开始和取消 FromAsync 任务

 `FromAsync` 方法返回的任务具有 `WaitingForActivation` 状态，并在创建任务后在某个时刻由操作系统启动。 如果尝试调用此类任务上的“启动”，将引发异常。  
  
 无法取消 `FromAsync` 任务，因为基础 .NET API 目前不支持取消正在进行中的文件或网络 I/O。 可以将取消功能添加到封装 `FromAsync` 调用的方法中，但只能在调用 `FromAsync` 之前或在调用完成之后响应取消（例如，在延续任务中）。  
  
 一些支持 EAP 的类（如 <xref:System.Net.WebClient>）不支持取消，但可以通过使用取消标记集成该本机取消功能。  
  
## <a name="expose-complex-eap-operations-as-tasks"></a>将复杂的 EAP 操作公开为任务

 TPL 不提供任何专用于以 `FromAsync` 系列方法包装 <xref:System.IAsyncResult> 模式相同的方式封装基于事件的异步操作的方法。 但是，TPL 会提供 <xref:System.Threading.Tasks.TaskCompletionSource%601?displayProperty=nameWithType> 类，此类可用于将任意一组操作表示为 <xref:System.Threading.Tasks.Task%601>。 这些操作可能同步、可能异步，可能是 I/O 密集型、也可能是计算密集型，还可能两者都是。  
  
 下面的示例显示如何使用 <xref:System.Threading.Tasks.TaskCompletionSource%601> 将一组异步 <xref:System.Net.WebClient> 操作作为基础 <xref:System.Threading.Tasks.Task%601> 向客户端代码公开。 此方法允许输入 Web URL 数组和术语或名称来进行搜索，然后返回每个站点搜索字词出现的次数。  
  
 [!code-csharp[FromAsync#10](../../../samples/snippets/csharp/VS_Snippets_Misc/fromasync/cs/snippet10.cs#10)]
 [!code-vb[FromAsync#10](../../../samples/snippets/visualbasic/VS_Snippets_Misc/fromasync/vb/snippet10.vb#10)]  
  
 有关包括其他异常处理且展示了如何通过客户端代码调用方法的更完整示例，请参阅[如何：在任务中包装 EAP 模式](how-to-wrap-eap-patterns-in-a-task.md)。  
  
 请记住，通过 <xref:System.Threading.Tasks.TaskCompletionSource%601> 创建的任何任务均由 `TaskCompletionSource` 启动，因此用户代码不应在此任务中调用 `Start` 方法。  
  
## <a name="implement-the-apm-pattern-by-using-tasks"></a>使用任务实现 APM 模式

 在某些情况下，可能需要通过使用 API 中 begin/end 方法对直接公开 <xref:System.IAsyncResult> 模式。 例如，可能想要与现有的 API 保持一致，或者可能具有需要这种模式的自动化工具。 在这种情况下，可使用 `Task` 对象来简化在内部实现 APM 模式的方式。  
  
 下面的示例显示如何使用任务实现长时间运行计算密集型方法的 APM begin/end 方法对。  
  
 [!code-csharp[FromAsync#09](../../../samples/snippets/csharp/VS_Snippets_Misc/fromasync/cs/fromasync.cs#09)]
 [!code-vb[FromAsync#09](../../../samples/snippets/visualbasic/VS_Snippets_Misc/fromasync/vb/module1.vb#09)]  
  
## <a name="use-the-streamextensions-sample-code"></a>使用 StreamExtensions 示例代码

  [.NET Standard parallel extensions extras](/samples/dotnet/samples/parallel-programming-extensions-extras-cs/) 存储库中的 StreamExtensions.cs 文件包含将 `Task` 对象用于异步文件和网络 I/O 的若干参考实现。
  
## <a name="see-also"></a>请参阅

- [任务并行库 (TPL)](task-parallel-library-tpl.md)
