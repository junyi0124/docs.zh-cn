---
title: 使用延续任务来链接任务
description: 了解如何在 .NET 中使用延续任务来链接任务。 延续任务是由其他任务调用的异步任务。
ms.date: 07/20/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tasks, continuations
ms.assetid: 0b45e9a2-de28-46ce-8212-1817280ed42d
ms.openlocfilehash: c7afbc1bb22a1743aed8e4ebd0a06c4f3fb57e86
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94830605"
---
# <a name="chaining-tasks-using-continuation-tasks"></a>使用延续任务来链接任务

在异步编程中，一个异步操作在完成时调用另一个操作的情况较常见。 延续使后续操作可以使用第一次操作的结果。 传统上，延续性是通过使用回调方法完成的。 在任务并行库中， _延续任务_ 提供了同样的功能。 延续任务（也简称为“延续”）是一个异步任务，在完成时由另一个任务（称为“先行任务”）调用。

尽管延续相对容易使用，但也十分强大和灵活。 例如，你可以：

- 将数据从前面的任务传递到延续。
- 指定将调用或不调用延续所依据的精确条件。
- 在延续启动之前取消延续，或在延续正在运行时以协作方式取消延续。
- 提供有关应如何计划延续的提示。
- 从同一前面的任务中调用多个延续。
- 在多个前面的任务中的全部或任意任务完成时调用一个延续。
- 将延续依次相连，形成任意长度。
- 使用延续来处理前面的任务所引发的异常。

## <a name="about-continuations"></a>关于延续

延续创建时的状态为 <xref:System.Threading.Tasks.TaskStatus.WaitingForActivation> 。 在一个或多个前面的任务完成时，它将自动激活。 若在用户代码中对延续调用 <xref:System.Threading.Tasks.Task.Start%2A?displayProperty=nameWithType> ，将引发 <xref:System.InvalidOperationException?displayProperty=nameWithType> 异常。

延续本身是 <xref:System.Threading.Tasks.Task> ，并不阻止它在其上启动的线程。 调用 <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType> 方法进行阻止，直到延续任务完成。

## <a name="create-a-continuation-for-a-single-antecedent"></a>为一个先行任务创建延续

通过调用 <xref:System.Threading.Tasks.Task.ContinueWith%2A?displayProperty=nameWithType> 方法创建在其前面的任务完成时执行的延续。 下面的示例演示基本模式（为清楚起见，省略了异常处理）。 它会执行一个先行任务 - `taskA`，将返回一个 <xref:System.DayOfWeek> 对象，指示当天为周几。 前面的任务完成时，将向延续任务 `continuation` 传递前面的任务，并显示包含其结果的字符串。

> [!NOTE]
> 本文中的 C# 示例利用 `Main` 方法的 `async` 修饰符。 此功能在 C# 7.1 及更高版本中提供。 以前的版本在编译此示例代码时生成 [`CS5001`](../../csharp/misc/cs5001.md)。 需要将语言版本设置为 C#7.1 或更高版本。 可以通过有关[配置语言版本](../../csharp/language-reference/configure-language-version.md)的文章了解如何配置语言版本。

:::code language="csharp" source="snippets/cs/simple1.cs":::

[!code-vb[TPL_Continuations#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/simple1.vb#1)]

## <a name="create-a-continuation-for-multiple-antecedents"></a>为多个先行任务创建延续

还可以创建一个将在一组任务中的任意或全部任务完成时运行的延续任务。 若要在所有前面的任务都完成后执行延续，则可以调用静态（在 Visual Basic 中为`Shared` ） <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> 方法或实例 <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAll%2A?displayProperty=nameWithType> 方法。 若要在多个前面的任务中的任意任务完成时执行延续，则可以调用静态（在 Visual Basic 中为`Shared` ） <xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType> 方法或实例 <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAny%2A?displayProperty=nameWithType> 方法。

调用 <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> 和 <xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType> 重载不会阻止调用线程。 不过，通常调用除 <xref:System.Threading.Tasks.Task.WhenAll%28System.Collections.Generic.IEnumerable%7BSystem.Threading.Tasks.Task%7D%29?displayProperty=nameWithType> 和 <xref:System.Threading.Tasks.Task.WhenAll%28System.Threading.Tasks.Task%5B%5D%29?displayProperty=nameWithType> 方法外的其他所有方法来检索返回的 <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=nameWithType> 属性，这样不会阻止调用线程。

下面的示例调用 <xref:System.Threading.Tasks.Task.WhenAll%28System.Collections.Generic.IEnumerable%7BSystem.Threading.Tasks.Task%7D%29?displayProperty=nameWithType> 方法来创建反映其 10 个前面的任务的结果的延续任务。 每个前面的任务计算从 1 到 10 的索引值的平方值。 如果前面的任务成功完成（其 <xref:System.Threading.Tasks.Task.Status%2A?displayProperty=nameWithType> 属性为 <xref:System.Threading.Tasks.TaskStatus.RanToCompletion?displayProperty=nameWithType>），则延续任务的 <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=nameWithType> 属性为由每个前面的任务返回的 <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=nameWithType> 值组成的数组。 该示例计算它们的总和，得出 1 到 10 之间的所有数字的平方和。

:::code language="csharp" source="snippets/cs/whenall1.cs":::

[!code-vb[TPL_Continuations#5](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/whenall1.vb#5)]

## <a name="continuation-options"></a>延续选项

在创建单任务延续时，你可以使用 <xref:System.Threading.Tasks.Task.ContinueWith%2A> 重载，该重载采用 <xref:System.Threading.Tasks.TaskContinuationOptions?displayProperty=nameWithType> 枚举值来指定启动延续所依据的条件。 例如，可以将延续指定为仅在前面的任务已完成运行时运行，或仅在前面的任务完成时处于错误状态时运行。 如果该条件在前面的任务准备调用延续时未得到满足，则延续将直接转换为 <xref:System.Threading.Tasks.TaskStatus.Canceled?displayProperty=nameWithType> 状态，之后将无法启动。

许多多任务延续方法（如 <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAll%2A?displayProperty=nameWithType> 方法的重载）也包括 <xref:System.Threading.Tasks.TaskContinuationOptions?displayProperty=nameWithType> 参数。 但是，只有所有 <xref:System.Threading.Tasks.TaskContinuationOptions?displayProperty=nameWithType> 枚举成员的一个子集有效。 你可以指定在 <xref:System.Threading.Tasks.TaskContinuationOptions?displayProperty=nameWithType> 枚举中具有对应的值的 <xref:System.Threading.Tasks.TaskCreationOptions?displayProperty=nameWithType> 值，如 <xref:System.Threading.Tasks.TaskContinuationOptions.AttachedToParent?displayProperty=nameWithType>、 <xref:System.Threading.Tasks.TaskContinuationOptions.LongRunning?displayProperty=nameWithType>和 <xref:System.Threading.Tasks.TaskContinuationOptions.PreferFairness?displayProperty=nameWithType>。 如果为多任务延续指定 `NotOn` 或 `OnlyOn` 选项中的任意一个，则在运行时将引发 <xref:System.ArgumentOutOfRangeException> 异常。

有关任务延续选项的详细信息，请参阅 <xref:System.Threading.Tasks.TaskContinuationOptions> 主题。

## <a name="pass-data-to-a-continuation"></a>将数据传递到延续

<xref:System.Threading.Tasks.Task.ContinueWith%2A?displayProperty=nameWithType> 方法将对前面的任务的引用以参数形式传递到延续的用户委托。 如果前面的任务是一个 <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType> 对象，并且任务在完成前保持运行，则延续可以访问任务的 <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=nameWithType> 属性。

在任务完成之前，将阻止 <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=nameWithType> 属性。 但是，如果任务已取消或出错，则尝试访问 <xref:System.Threading.Tasks.Task%601.Result%2A> 属性将引发 <xref:System.AggregateException> 异常。 可通过使用 <xref:System.Threading.Tasks.TaskContinuationOptions.OnlyOnRanToCompletion> 选项避免此问题，如下面的示例所示。

:::code language="csharp" source="snippets/cs/result1.cs":::

[!code-vb[TPL_Continuations#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/result1.vb#2)]

如果希望延续即使在前面的任务未完成运行时也运行，则必须防止出现异常。 一种方法是测试前面的任务的 <xref:System.Threading.Tasks.Task.Status%2A?displayProperty=nameWithType> 属性，并且仅在状态不是 <xref:System.Threading.Tasks.Task%601.Result%2A> 或 <xref:System.Threading.Tasks.TaskStatus.Faulted> 时才尝试访问 <xref:System.Threading.Tasks.TaskStatus.Canceled>属性。 也可以检查前面的任务的 <xref:System.Threading.Tasks.Task.Exception%2A> 属性。 有关详细信息，请参阅[异常处理](exception-handling-task-parallel-library.md)。 下面的示例将之前的示例修改为仅在前面的任务的状态为 <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=nameWithType> 时，才访问该任务的 <xref:System.Threading.Tasks.TaskStatus.RanToCompletion?displayProperty=nameWithType>属性。

:::code language="csharp" source="snippets/cs/result2.cs":::

[!code-vb[TPL_Continuations#7](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/result2.vb#7)]

## <a name="cancel-a-continuation"></a>取消延续

在以下情况下，延续的 <xref:System.Threading.Tasks.Task.Status%2A?displayProperty=nameWithType> 属性将设置为 <xref:System.Threading.Tasks.TaskStatus.Canceled?displayProperty=nameWithType> ：

- 延续引发 <xref:System.OperationCanceledException> 以响应取消请求。 就像任何任务一样，如果异常包含已传递到延续的相同标记，则会将其视为确认协作取消。
- 将 <xref:System.Threading.CancellationToken?displayProperty=nameWithType> 属性为 <xref:System.Threading.CancellationToken.IsCancellationRequested%2A> 的 `true`传递到延续。 在这种情况下，延续不会启动，并且将转换为 <xref:System.Threading.Tasks.TaskStatus.Canceled?displayProperty=nameWithType> 状态。
- 延续由于其 <xref:System.Threading.Tasks.TaskContinuationOptions> 参数设置的条件未得到满足而从不运行。 例如，如果前面的任务进入 <xref:System.Threading.Tasks.TaskStatus.Faulted?displayProperty=nameWithType> 状态，则该任务被传递了 <xref:System.Threading.Tasks.TaskContinuationOptions.NotOnFaulted?displayProperty=nameWithType> 选项的延续将不会运行，而是将转换为 <xref:System.Threading.Tasks.TaskStatus.Canceled> 状态。

如果一项任务及其延续表示同一逻辑操作的两个部分，则可以将相同的取消标记传递到这两个任务，如下面的示例所示。 它包含的前面的任务可生成由可被 33 的整数组成的列表，并将该列表传递给延续。 而延续反过来显示该列表。 前面的任务和延续任务都将定期以随机间隔暂停。 此外， <xref:System.Threading.Timer?displayProperty=nameWithType> 对象用于在五秒的超时间隔后执行 `Elapsed` 方法。 此示例调用 <xref:System.Threading.CancellationTokenSource.Cancel%2A?displayProperty=nameWithType> 方法，从而将导致当前正在执行的任务调用 <xref:System.Threading.CancellationToken.ThrowIfCancellationRequested%2A?displayProperty=nameWithType> 方法。 是否在前面的任务或其延续正在执行时调用 <xref:System.Threading.CancellationTokenSource.Cancel%2A?displayProperty=nameWithType> 方法取决于随机生成的暂停的持续时间。 如果已取消前面的任务，则延续将不会启动。 如果未取消前面的任务，则仍然可以使用标记来取消延续。

:::code language="csharp" source="snippets/cs/cancellation1.cs":::

[!code-vb[TPL_Continuations#3](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/cancellation1.vb#3)]

通过在创建延续时指定 <xref:System.Threading.Tasks.TaskContinuationOptions.NotOnCanceled?displayProperty=nameWithType> 选项，在不向延续提供取消标记而取消其前面的任务的情况下，你也可阻止延续的执行。 下面是一个简单的示例。

:::code language="csharp" source="snippets/cs/cancellation2.cs":::

[!code-vb[TPL_Continuations#8](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/cancellation2.vb#8)]

在延续转换为 <xref:System.Threading.Tasks.TaskStatus.Canceled> 状态后，它可能会影响后面的延续，具体情况取决于为这些延续指定的 <xref:System.Threading.Tasks.TaskContinuationOptions> 。

已释放的延续将不会启动。

## <a name="continuations-and-child-tasks"></a>延续和子任务

在前面的任务及其所有附加的子任务完成之前，延续将不会运行。 延续不会等待分离的子任务完成。 以下两个示例阐释了两个子任务，其中一个附加到创建了延续的前面的任务，另一个从创建了延续的前面的任务中分离。 在下面的示例中，延续仅在所有子任务都完成后才会运行，并且多次运行该示例时，每次生成的输出相同。 该示例通过调用 <xref:System.Threading.Tasks.TaskFactory.StartNew%2A?displayProperty=nameWithType> 方法来启动前面的任务，因为在默认情况下， <xref:System.Threading.Tasks.Task.Run%2A?displayProperty=nameWithType> 方法将创建一个默认任务创建选项为 <xref:System.Threading.Tasks.TaskCreationOptions.DenyChildAttach?displayProperty=nameWithType>的父任务。

:::code language="csharp" source="snippets/cs/attached1.cs":::

[!code-vb[TPL_Continuations#9](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/attached1.vb#9)]

但是，如果子任务与前面的任务分离，则前面的任务任务一旦终止，延续就将立即开始运行，而无论子任务的状态。 因此，多次运行下面的示例可能生成可变输出，具体取决于任务计划程序处理每个子任务的方式。

:::code language="csharp" source="snippets/cs/detached1.cs":::

[!code-vb[TPL_Continuations#10](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/detached1.vb#10)]

前面的任务的最终状态取决于任何附加的子任务的最终状态。 分离的子任务的状态不影响父级。 有关详细信息，请参阅[附加和分离的子任务](attached-and-detached-child-tasks.md)。

## <a name="associate-state-with-continuations"></a>将状态与延续关联

可以将任意状态与任务延续关联。 <xref:System.Threading.Tasks.Task.ContinueWith%2A> 方法提供重载版本，每个重载版本都带有一个表示延续状态的 <xref:System.Object> 值。 可以之后通过使用 <xref:System.Threading.Tasks.Task.AsyncState%2A?displayProperty=nameWithType> 属性访问此状态对象。 如果未提供值，则此状态对象为 `null` 。

将使用 [异步编程模型 (APM)](../asynchronous-programming-patterns/asynchronous-programming-model-apm.md) 的现有代码转换为使用 TPL 时，延续状态非常有用。 在 APM 中，通常在 **Begin**_Method_ 方法中提供对象状态，并在之后通过使用 <xref:System.IAsyncResult.AsyncState%2A?displayProperty=nameWithType> 属性访问该状态。 通过使用 <xref:System.Threading.Tasks.Task.ContinueWith%2A> 方法，你可在将使用 APM 的代码转换为使用 TPL 时保留此状态。

在 Visual Studio 调试器中处理 <xref:System.Threading.Tasks.Task> 对象时，延续状态也非常有用。 例如，在“并行任务”  窗口中，“任务”  列显示每个任务的状态对象的字符串表示形式。 有关“并行任务”窗口的详细信息，请参阅[使用并行任务窗口](/visualstudio/debugger/using-the-tasks-window)。

下面的示例演示如何使用延续状态。 它将创建一个延续任务链。 每个任务都将为 <xref:System.DateTime> 方法的 `state` 参数提供当前时间，即一个 <xref:System.Threading.Tasks.Task.ContinueWith%2A> 对象。 每个 <xref:System.DateTime> 对象都表示创建延续任务的时间。 每个任务都将生成第二个 <xref:System.DateTime> 对象作为其结果，该对象表示任务的完成时间。 所有任务都完成后，本示例将显示每个延续任务的创建时间和完成时间。

:::code language="csharp" source="snippets/cs/continuationstate.cs":::

[!code-vb[TPL_ContinuationState#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuationstate/vb/continuationstate.vb#1)]

## <a name="continuations-that-return-task-types"></a>返回 Task 类型的延续

有时，可能需要链接可返回 <xref:System.Threading.Tasks.Task> 类型的延续。 这些任务称为嵌套任务，它们很常见。 当父任务调用 <xref:System.Threading.Tasks.Task%601.ContinueWith%2A?displayProperty=nameWithType>，并提供作为任务返回的 `continuationFunction` 时，你可以调用 <xref:System.Threading.Tasks.TaskExtensions.Unwrap%2A> 来创建表示 `<Task<Task<T>>>` 或 `Task(Of Task(Of T))` (Visual Basic) 的异步操作的代理任务。

下面的示例展示如何使用包装附加任务返回函数的延续。 每个延续都可以进行解包，并公开已包装的内部任务。

:::code language="csharp" source="snippets/cs/unwrap.cs":::
:::code language="vb" source="snippets/vb/unwrap.vb":::

有关使用 <xref:System.Threading.Tasks.TaskExtensions.Unwrap%2A> 的详细信息，请参阅[如何：展开嵌套任务](how-to-unwrap-a-nested-task.md)。

## <a name="handle-exceptions-thrown-from-continuations"></a>处理从延续中引发的异常

前面的任务与延续之间的关系不是父/子关系。 由延续引发的异常不会传播到前面的任务。 因此，请按在任何其他任务中处理异常的方式来处理由延续引发的异常，如下所示：

- 你可以使用 <xref:System.Threading.Tasks.Task.Wait%2A>、 <xref:System.Threading.Tasks.Task.WaitAll%2A>或 <xref:System.Threading.Tasks.Task.WaitAny%2A> 法或其对应的泛型方法来等待延续。 你可以在同一 `try` 语句中等待前面的任务及其延续，如下面的示例所示。

:::code language="csharp" source="snippets/cs/exception1.cs":::

[!code-vb[TPL_Continuations#6](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/exception1.vb#6)]

- 可以使用第二个延续来观察第一个延续的 <xref:System.Threading.Tasks.Task.Exception%2A> 属性。 在下面的示例中，某个任务尝试从不存在的文件中进行读取。 然后，延续将显示有关前面的任务中的异常的信息。

:::code language="csharp" source="snippets/cs/exception2.cs" id="example":::

[!code-vb[TPL_Continuations#4](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/exception2.vb#4)]

因为使用 <xref:System.Threading.Tasks.TaskContinuationOptions.OnlyOnFaulted?displayProperty=nameWithType> 选项运行，所以仅当前面的任务中发生异常时才会执行延续，并且可以假设前面的任务的 <xref:System.Threading.Tasks.Task.Exception%2A> 属性不为 `null`。 如果无论前面的任务中是否引发了异常都将执行延续，则必须先检查前面的任务的 <xref:System.Threading.Tasks.Task.Exception%2A> 属性是否不为 `null` ，然后再尝试处理该异常，如以下代码片段所示。

:::code language="csharp" source="snippets/cs/exception2.cs" id="exception":::

[!code-vb[TPL_Continuations#11](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_continuations/vb/exception2.vb#11)]

有关详细信息，请参阅[异常处理](exception-handling-task-parallel-library.md)。

- 如果延续为附加子任务并且是通过使用 <xref:System.Threading.Tasks.TaskContinuationOptions.AttachedToParent?displayProperty=nameWithType> 选项创建的，则父级会将该延续的异常传播回调用线程，就像任何其他附加子级的情况一样。 有关详细信息，请参阅[附加和分离的子任务](attached-and-detached-child-tasks.md)。

## <a name="see-also"></a>请参阅

- [任务并行库 (TPL)](task-parallel-library-tpl.md)
