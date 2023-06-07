---
title: 实现基于任务的异步模式
description: 本文介绍如何实现基于任务的异步模式。 可以使用它实现计算密集型和 I/O 密集型异步操作。
ms.date: 06/14/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- asynchronous design patterns, .NET
- TAP, .NET support for
- Task-based Asynchronous Pattern, .NET support for
- .NET, asynchronous design patterns
ms.assetid: fab6bd41-91bd-44ad-86f9-d8319988aa78
ms.openlocfilehash: 7613d93e1ca2ac9594759434966745a238ba166e
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95726725"
---
# <a name="implementing-the-task-based-asynchronous-pattern"></a>实现基于任务的异步模式

可使用以下三种方式实现基于任务的异步模式 (TAP)：使用 Visual Studio 中的 C# 和 Visual Basic 编译器、手动实现或编译器和手动方法相结合。 以下各节详细地讨论了每一种方法。 可以使用 TAP 模式实现计算密集型和 I/O 密集型异步操作。 [工作负载](#workloads)部分介绍了各种类型的操作。

## <a name="generating-tap-methods"></a>生成 TAP 方法

### <a name="using-the-compilers"></a>使用编译器

自 .NET Framework 4.5 起，任何归于 `async` 关键字（Visual Basic 中的 `Async`）的方法都被视为异步方法，并且 C# 和 Visual Basic 编译器会执行必要的转换，以使用 TAP 异步实现方法。 异步方法应返回 <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> 或 <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType> 对象。 对于后者，函数的主体应返回 `TResult`，并且编译器确保此结果是通过生成的任务对象获得。 同样，未在方法的主体中处理的任何异常都会被封送处理为输出任务并导致生成的任务结束以 <xref:System.Threading.Tasks.TaskStatus.Faulted?displayProperty=nameWithType> 状态结束。 此规则的异常发生在 <xref:System.OperationCanceledException>（或派生类型）未得到处理时，在这种情况下生成的任务以 <xref:System.Threading.Tasks.TaskStatus.Canceled?displayProperty=nameWithType> 状态结束。

### <a name="generating-tap-methods-manually"></a>手动生成 TAP 方法

你可以手动实现 TAP 模式以更好地控制实现。 编译器依赖从 <xref:System.Threading.Tasks?displayProperty=nameWithType> 命名空间公开的公共外围应用和 <xref:System.Runtime.CompilerServices?displayProperty=nameWithType> 命名空间中支持的类型。 如要自己实现 TAP，你需要创建一个 <xref:System.Threading.Tasks.TaskCompletionSource%601> 对象、执行异步操作，并在操作完成时，调用 <xref:System.Threading.Tasks.TaskCompletionSource%601.SetResult%2A>、<xref:System.Threading.Tasks.TaskCompletionSource%601.SetException%2A>、<xref:System.Threading.Tasks.TaskCompletionSource%601.SetCanceled%2A> 方法，或调用这些方法之一的`Try`版本。 手动实现 TAP 方法时，需在所表示的异步操作完成时完成生成的任务。 例如：

[!code-csharp[Conceptual.TAP_Patterns#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.tap_patterns/cs/patterns1.cs#1)]
[!code-vb[Conceptual.TAP_Patterns#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.tap_patterns/vb/patterns1.vb#1)]

### <a name="hybrid-approach"></a>混合方法

 你可能发现手动实现 TAP 模式、但将实现核心逻辑委托给编译器的这种方法很有用。 例如，当你想要验证编译器生成的异步方法之外的参数时，可能需要使用这种混合方法，以便异常可以转义到该方法的直接调用方而不是通过 <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> 对象被公开：

 [!code-csharp[Conceptual.TAP_Patterns#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.tap_patterns/cs/patterns1.cs#2)]
 [!code-vb[Conceptual.TAP_Patterns#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.tap_patterns/vb/patterns1.vb#2)]

 这种委托有用的另一种情况是：你在实施快速路径优化并想返回缓存的任务时。

## <a name="workloads"></a>工作负载

你可将计算密集型和 I/O 密集型异步操作作为 TAP 方法实现。 但是，当 TAP 方法从库中公开地公开时，应仅向涉及 I/O 密集型操作的工作负载提供这种方法（它们也可能涉及计算，但不是应仅仅是计算）。 如果是纯粹的计算密集型方法，应只公开为同步实现。 然后，使用它的代码可能会选择是将同步方法调用包装到任务中以将工作卸载到另一线程，还是实现并行。 如果方法是 I/O 密集型，应只公开为异步实现。

### <a name="compute-bound-tasks"></a>计算密集型任务

<xref:System.Threading.Tasks.Task?displayProperty=nameWithType> 类非常适合表示计算密集型操作。 默认情况下，它利用 <xref:System.Threading.ThreadPool> 类中的特殊支持来提供有效的执行，还对执行异步计算的时间、地点和方式提供重要控制。

你可以通过以下方式生成计算密集型任务：

- 在 .NET Framework 4.5 及更高版本（包括 .NET Core 和 .NET 5+）中，使用静态 <xref:System.Threading.Tasks.Task.Run%2A?displayProperty=nameWithType> 方法作为 <xref:System.Threading.Tasks.TaskFactory.StartNew%2A?displayProperty=nameWithType> 的快捷方式。 你可以使用 <xref:System.Threading.Tasks.Task.Run%2A> 来轻松启动针对线程池的计算密集型任务。 这是用于启动一项计算密集型任务的首选机制。 仅当需要更细化地控制任务时，才直接使用 `StartNew`。

- 在 .NET Framework 4 中，使用 <xref:System.Threading.Tasks.TaskFactory.StartNew%2A?displayProperty=nameWithType> 方法，这种方法接受异步执行委托（通常是 <xref:System.Action%601> 或 <xref:System.Func%601>）。 如果你提供 <xref:System.Action%601> 委托，该方法会返回表示异步执行该委托的 <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> 对象。 如果你提供 <xref:System.Func%601> 委托，该方法会返回 <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType> 对象。 <xref:System.Threading.Tasks.TaskFactory.StartNew%2A> 方法的重载接受一个取消标记（<xref:System.Threading.CancellationToken>）、任务创建选项（<xref:System.Threading.Tasks.TaskCreationOptions>）和一个任务计划程序（<xref:System.Threading.Tasks.TaskScheduler>），它们都对计划和任务执行提供细粒度控制。 定目标到当前任务计划程序的工厂实例可用作 <xref:System.Threading.Tasks.Task> 类的静态属性 (<xref:System.Threading.Tasks.Task.Factory%2A>)；例如：`Task.Factory.StartNew(…)`。

- 想要分别生成并计划任务时，请使用`Task`类型或`Start`方法的构造函数。 公共方法必须仅返回已开始的任务。

- 使用 <xref:System.Threading.Tasks.Task.ContinueWith%2A?displayProperty=nameWithType> 方法的重载。 此方法创建一项在另一任务完成时已排好计划的新任务。 某些 <xref:System.Threading.Tasks.Task.ContinueWith%2A> 重载接受一个取消标记、延续选项和一个任务计划程序，以更好地控制计划和执行延续任务。

- 使用 <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAll%2A?displayProperty=nameWithType> 和 <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAny%2A?displayProperty=nameWithType> 方法。 这些方法会在提供的全部任务或任意一组任务完成时创建已计划的新任务。 这些方法还提供了重载，用于控制这些任务的计划和执行。

在计算密集型任务中，如果系统在开始运行任务之前收到取消请求，则它可以防止执行已计划的任务。 同样，如果你提供一个取消标记（<xref:System.Threading.CancellationToken> 对象），则可以将标记传递给监视该标记的异步代码。 你也可以将此标记提供给先前提过的方法（如 `StartNew` 或 `Run`），以便`Task`运行时也能监视该标记。

例如，请考虑使用呈现图像的异步方法。 任务的主体可以轮询取消标记，如果在呈现过程中收到取消请求，代码可提前退出。 此外，如果呈现启动之前收到取消请求，你需要阻止呈现操作：

[!code-csharp[Conceptual.TAP_Patterns#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.tap_patterns/cs/patterns1.cs#3)]
[!code-vb[Conceptual.TAP_Patterns#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.tap_patterns/vb/patterns1.vb#3)]

如果满足下列条件之一，则计算密集型任务以 <xref:System.Threading.Tasks.TaskStatus.Canceled> 状态结束：

- 取消请求通过 <xref:System.Threading.CancellationToken> 对象到达，该对象在任务转换到 `StartNew` 状态前，作为创建方法的自变量（例如 `Run` 或 <xref:System.Threading.Tasks.TaskStatus.Running>）提供。

- <xref:System.OperationCanceledException> 异常在此类任务的主体内未得到处理，该异常包含传给该任务的同一 <xref:System.Threading.CancellationToken>，并且该标记显示已请求取消操作。

如果另一个异常在任务的主体内未得到处理，则此任务以 <xref:System.Threading.Tasks.TaskStatus.Faulted> 状态结束，并且任何等待该任务或访问其结果的尝试都将引发异常。

### <a name="io-bound-tasks"></a>I/O 密集型任务

若要创建一个不应由线程直接支持其全部执行的任务，请使用 <xref:System.Threading.Tasks.TaskCompletionSource%601> 类型。 此类型公开一个返回关联 <xref:System.Threading.Tasks.TaskCompletionSource%601.Task%2A> 实例的 <xref:System.Threading.Tasks.Task%601> 属性。 此任务的生命周期是由 <xref:System.Threading.Tasks.TaskCompletionSource%601> 方法控制的，比如 <xref:System.Threading.Tasks.TaskCompletionSource%601.SetResult%2A>、<xref:System.Threading.Tasks.TaskCompletionSource%601.SetException%2A>、<xref:System.Threading.Tasks.TaskCompletionSource%601.SetCanceled%2A> 以及它们的 `TrySet` 变形。

假设你想创建一个将在指定时间段后完成的任务。 例如，你可能想延迟用户界面中的活动。 <xref:System.Threading.Timer?displayProperty=nameWithType> 类已提供在指定时间段后以异步方式调用委托的能力，并且你可以通过使用 <xref:System.Threading.Tasks.TaskCompletionSource%601> 将 <xref:System.Threading.Tasks.Task%601> 前端放在计时器上，例如：

[!code-csharp[Conceptual.TAP_Patterns#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.tap_patterns/cs/patterns1.cs#4)]
[!code-vb[Conceptual.TAP_Patterns#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.tap_patterns/vb/patterns1.vb#4)]

<xref:System.Threading.Tasks.Task.Delay%2A?displayProperty=nameWithType> 方法正是为此目的而提供，并且你可以在另一个异步方法内使用它。例如，要实现异步轮询循环：

[!code-csharp[Conceptual.TAP_Patterns#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.tap_patterns/cs/patterns1.cs#5)]
[!code-vb[Conceptual.TAP_Patterns#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.tap_patterns/vb/patterns1.vb#5)]

<xref:System.Threading.Tasks.TaskCompletionSource%601> 类并没有对应的非泛型类。 然而 <xref:System.Threading.Tasks.Task%601> 派生自 <xref:System.Threading.Tasks.Task>，因此你可以为仅返回任务的 I/O 密集型方法使用泛型 <xref:System.Threading.Tasks.TaskCompletionSource%601> 对象。 为了做到这一点，你可以使用具有虚拟 `TResult`（<xref:System.Boolean> 是一个很好的默认选项，但是如果你担心 <xref:System.Threading.Tasks.Task> 用户将其向下转换成 <xref:System.Threading.Tasks.Task%601>，那么你可以转而使用私有 `TResult` 类型）。 例如，上一个示例中的 `Delay` 方法返回现有时间和所产生的偏移量（`Task<DateTimeOffset>`）。 如果结果值是不必要的，则可对该方法进行如下改写（注意对 <xref:System.Threading.Tasks.TaskCompletionSource%601.TrySetResult%2A> 的返回类型的更改和实参的更改）：

[!code-csharp[Conceptual.TAP_Patterns#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.tap_patterns/cs/patterns1.cs#6)]
[!code-vb[Conceptual.TAP_Patterns#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.tap_patterns/vb/patterns1.vb#6)]

### <a name="mixed-compute-bound-and-io-bound-tasks"></a>计算密集型和 I/O 密集型混合任务

异步方法不只局限于计算密集型或 I/O 密集型操作，还可以是两者的结合。 事实上，多个异步操作通常组合成较大的混合操作。 例如，请考虑前面示例中的 `RenderAsync` 方法，该方法执行计算密集型操作以根据某些输入 `imageData` 呈现图像。 此 `imageData` 可能来自你异步访问的 Web 服务：

[!code-csharp[Conceptual.TAP_Patterns#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.tap_patterns/cs/patterns1.cs#7)]
[!code-vb[Conceptual.TAP_Patterns#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.tap_patterns/vb/patterns1.vb#7)]

此示例还演示了如何通过多个异步操作使单个取消标记线程化。 有关详细信息，请参阅[使用基于任务的异步模式](consuming-the-task-based-asynchronous-pattern.md)中的取消用法部分。

## <a name="see-also"></a>请参阅

- [基于任务的异步模式 (TAP)](task-based-asynchronous-pattern-tap.md)
- [使用基于任务的异步模式](consuming-the-task-based-asynchronous-pattern.md)
- [与其他异步模式和类型互操作](interop-with-other-asynchronous-patterns-and-types.md)
