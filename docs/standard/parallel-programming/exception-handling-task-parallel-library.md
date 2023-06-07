---
title: 异常处理（任务并行库）
description: 探索 .NET 中使用任务并行库 (TPL) 的异常处理。 了解嵌套聚合异常、内部异常、未观察到的任务异常等。
ms.date: 04/20/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tasks, exceptions
ms.assetid: beb51e50-9061-4d3d-908c-56a4f7c2e8c1
ms.openlocfilehash: fd16fd4263c092f8678589e1a8dd9544955e798a
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94827329"
---
# <a name="exception-handling-task-parallel-library"></a>异常处理（任务并行库）

由在任务内部运行的用户代码引发的未处理异常会传播回调用线程，但本主题稍后部分介绍的某些情况除外。 如果使用静态或实例 <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType> 方法之一，异常会传播，异常处理方法为将调用封闭到 `try`/`catch` 语句中。 如果任务是所附加子任务的父级，或在等待多个任务，那么可能会引发多个异常。

为了将所有异常传播回调用线程，任务基础结构会将这些异常包装在 <xref:System.AggregateException> 实例中。 <xref:System.AggregateException> 异常具有 <xref:System.AggregateException.InnerExceptions%2A> 属性，可枚举该属性来检查引发的所有原始异常，并单独处理（或不处理）每个异常。 也可以使用 <xref:System.AggregateException.Handle%2A?displayProperty=nameWithType> 方法处理原始异常。

即使只引发了一个异常，仍会将该异常包装在 <xref:System.AggregateException> 中，如以下示例所示。

[!code-csharp[TPL_Exceptions#21](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_exceptions/cs/handling21.cs#21)]
[!code-vb[TPL_Exceptions#21](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_exceptions/vb/handling21.vb#21)]

可以通过只捕获 <xref:System.AggregateException> 而不观察任何内部异常来避免未处理的异常。 但是，我们建议你不要这样做，因为这样相当于在非并行情况下捕获基 <xref:System.Exception> 类型。 捕获异常而不采取具体措施从中恢复可能会使程序进入不确定状态。

如果不想调用 <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType> 方法来等待任务完成，也可以通过任务的 <xref:System.Threading.Tasks.Task.Exception%2A> 属性检索 <xref:System.AggregateException> 异常，如下面的示例所示。 有关详细信息，请参阅本主题中的[通过使用 Task.Exception 属性观察异常](#observing-exceptions-by-using-the-taskexception-property)部分。

[!code-csharp[TPL_Exceptions#29](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_exceptions/cs/handling22.cs#29)]
[!code-vb[TPL_Exceptions#29](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_exceptions/vb/handling22.vb#29)]

如果不等待传播异常的任务，或要访问其 <xref:System.Threading.Tasks.Task.Exception%2A> 属性，则会在对该任务进行垃圾回收时根据 .NET 异常策略提升异常。

如果允许异常向上冒泡回到联接线程，则一个任务也许可以在引发异常后继续处理一些项。

> [!NOTE]
> 某些情况下，当启用“仅我的代码”后，Visual Studio 会在引发异常的行中断运行并显示一条错误消息，该消息显示“用户代码未处理异常”。 此错误是良性的。 可以按 F5 继续并查看在这些示例中演示的异常处理行为。 若要阻止 Visual Studio 在出现第一个错误时中断运行，只需在“工具”-&gt;“选项”-&gt;“调试”-&gt;“常规”  下取消选中“启用‘仅我的代码’” 复选框即可。

## <a name="attached-child-tasks-and-nested-aggregateexceptions"></a>附加子任务和嵌套 AggregateExceptions

如果某个任务具有引发异常的附加子任务，则会在将该异常传播到父任务之前将其包装在 <xref:System.AggregateException> 中，父任务将该异常包装在自己的 <xref:System.AggregateException> 中，然后再将其传播回调用线程。 在这种情况下，在 <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType>、<xref:System.Threading.Tasks.Task.WaitAny%2A>、或 <xref:System.Threading.Tasks.Task.WaitAll%2A> 方法处捕获的 <xref:System.AggregateException> 异常的 <xref:System.AggregateException.InnerExceptions%2A> 属性包含一个或多个 <xref:System.AggregateException> 实例，而不包含导致错误的原始异常。 为了避免必须循环访问嵌套 <xref:System.AggregateException> 异常，可以使用 <xref:System.AggregateException.Flatten%2A> 方法删除所有嵌套 <xref:System.AggregateException> 异常，以便 <xref:System.AggregateException.InnerExceptions%2A?displayProperty=nameWithType> 属性包含原始异常。 在下面的示例中，嵌套 <xref:System.AggregateException> 实例已经平展，并且仅在一个循环中处理。

[!code-csharp[TPL_Exceptions#22](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_exceptions/cs/flatten2.cs#22)]
[!code-vb[TPL_Exceptions#22](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_exceptions/vb/flatten2.vb#22)]

还可以使用 <xref:System.AggregateException.Flatten%2A?displayProperty=nameWithType> 方法，通过多个任务在一个 <xref:System.AggregateException> 实例中抛出的多个 <xref:System.AggregateException> 实例重新抛出内部异常，如下面的示例所示。

[!code-csharp[TPL_Exceptions#13](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_exceptions/cs/taskexceptions2.cs#13)]
[!code-vb[TPL_Exceptions#13](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_exceptions/vb/taskexceptions2.vb#13)]

## <a name="exceptions-from-detached-child-tasks"></a>分离子任务中的异常

默认情况下，子任务在创建时处于分离状态。 必须在直接父任务中处理或重新引发从分离任务引发的异常；将不会采用与附加子任务传播回异常相同的方式将这些异常传播回调用线程。 最顶层的父级可以手动重新引发分离子级中的异常，以使其包装在 <xref:System.AggregateException> 中并传播回调用线程。

[!code-csharp[TPL_Exceptions#23](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_exceptions/cs/detached21.cs#23)]
[!code-vb[TPL_Exceptions#23](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_exceptions/vb/detached21.vb#23)]

即使使用延续观察子任务中的异常，该异常仍然必须由父任务观察。

## <a name="exceptions-that-indicate-cooperative-cancellation"></a>指示协作取消的异常

在任务中的用户代码响应取消请求时，正确的过程是引发传入在其上传达请求的取消标记中的 <xref:System.OperationCanceledException> 。 在尝试传播异常之前，任务实例会将异常中的标记与创建异常时传递给异常的标记进行比较。 如果标记相同，则任务会传播包装在 <xref:System.Threading.Tasks.TaskCanceledException> 中的 <xref:System.AggregateException>，并且将可以在检查内部异常时看到它。 但是，如果调用线程未在等待任务，则将不会传播此特定异常。 有关详细信息，请参阅[任务取消](task-cancellation.md)。

[!code-csharp[TPL_Exceptions#4](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_exceptions/cs/exceptions.cs#4)]
[!code-vb[TPL_Exceptions#4](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_exceptions/vb/tpl_exceptions.vb#4)]

## <a name="using-the-handle-method-to-filter-inner-exceptions"></a>使用 Handle 方法筛选内部异常

可以使用 <xref:System.AggregateException.Handle%2A?displayProperty=nameWithType> 方法，筛选掉可视为“已处理”的异常，而无需进一步使用任何逻辑。 在提供给 <xref:System.AggregateException.Handle%28System.Func%7BSystem.Exception%2CSystem.Boolean%7D%29?displayProperty=nameWithType> 方法的用户委托中，可以检查异常类型及其 <xref:System.Exception.Message%2A> 属性，或可便于确定异常是否为良性的其他任何信息。 在 <xref:System.AggregateException.Handle%2A?displayProperty=nameWithType> 方法返回结果后，便会立即在新实例 <xref:System.AggregateException> 中重新抛出委托针对其返回 `false` 的任何异常。

下面的示例在功能上相当于本主题中的第一个示例（检查 <xref:System.AggregateException.InnerExceptions%2A?displayProperty=nameWithType> 集合中的所有异常）。  相反，此异常处理程序对每个异常调用 <xref:System.AggregateException.Handle%2A?displayProperty=nameWithType> 方法对象，并仅重新抛出不是 `CustomException` 实例的异常。

[!code-csharp[TPL_Exceptions#26](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_exceptions/cs/handlemethod21.cs#26)]
[!code-vb[TPL_Exceptions#26](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_exceptions/vb/handlemethod21.vb#26)]

下面是更完整的示例，在枚举文件时，使用 <xref:System.AggregateException.Handle%2A?displayProperty=nameWithType> 方法提供 <xref:System.UnauthorizedAccessException> 异常的特殊处理。

[!code-csharp[TPL_Exceptions#12](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_exceptions/cs/taskexceptions.cs#12)]
[!code-vb[TPL_Exceptions#12](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_exceptions/vb/taskexceptions.vb#12)]

## <a name="observing-exceptions-by-using-the-taskexception-property"></a>通过使用 Task.Exception 属性观察异常

如果任务完成时的状态为 <xref:System.Threading.Tasks.TaskStatus.Faulted?displayProperty=nameWithType>，可以检查它的 <xref:System.Threading.Tasks.Task.Exception%2A> 属性，以发现是哪个异常导致错误发生。 观察 <xref:System.Threading.Tasks.Task.Exception%2A> 属性的一个好方法是使用仅在前面的任务出错时才运行的延续，如以下示例所示。

[!code-csharp[TPL_Exceptions#27](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_exceptions/cs/exceptionprop21.cs#27)]
[!code-vb[TPL_Exceptions#27](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_exceptions/vb/exceptionprop21.vb#27)]

在有意义的应用程序中，延续委托可能会记录有关异常的详细信息，并可能生成新任务以从异常中恢复。 如果任务出错，以下表达式将引发异常：

- `await task`
- `task.Wait()`
- `task.Result`
- `task.GetAwaiter().GetResult()`

使用 [`try-catch`](../../csharp/language-reference/keywords/try-catch.md) 语句来处理和观察引发的异常。 或者，通过访问 <xref:System.Threading.Tasks.Task.Exception%2A?displayProperty=nameWithType> 属性来观察异常。

## <a name="unobservedtaskexception-event"></a>UnobservedTaskException 事件

在某些情况下（例如承载不受信任的插件时），良性异常可能比较普遍，因此很难以手动方式观察到所有异常。 在这些情况下，可以处理 <xref:System.Threading.Tasks.TaskScheduler.UnobservedTaskException?displayProperty=nameWithType> 事件。 传递到处理程序的 <xref:System.Threading.Tasks.UnobservedTaskExceptionEventArgs?displayProperty=nameWithType> 实例可用于阻止未观察到的异常传播回联接线程。

## <a name="see-also"></a>请参阅

- [任务并行库 (TPL)](task-parallel-library-tpl.md)
