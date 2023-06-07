---
title: 基于任务的异步编程 - .NET
description: 本文介绍如何通过 .NET 中的任务并行库 (TPL) 使用基于任务的异步编程。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- parallelism, task
ms.assetid: 458b5e69-5210-45e5-bc44-3888f86abd6f
ms.openlocfilehash: a1abe474628cd88e0c24f4152d83bd8ed7ad7950
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94830020"
---
# <a name="task-based-asynchronous-programming"></a>基于任务的异步编程

任务并行库 (TPL) 以“任务”的概念为基础，后者表示异步操作。 在某些方面，任务类似于线程或 <xref:System.Threading.ThreadPool> 工作项，但是抽象级别更高。 术语“任务并行”是指一个或多个独立的任务同时运行。 任务提供两个主要好处：

- 系统资源的使用效率更高，可伸缩性更好。

     在后台，任务排队到已使用算法增强的 <xref:System.Threading.ThreadPool>，这些算法能够确定线程数并随之调整，提供负载平衡以实现吞吐量最大化。 这会使任务相对轻量，你可以创建很多任务以启用细化并行。

- 对于线程或工作项，可以使用更多的编程控件。

     任务和围绕它们生成的框架提供了一组丰富的 API，这些 API 支持等待、取消、继续、可靠的异常处理、详细状态、自定义计划等功能。

出于这两个原因，在 .NET 中，TPL 是用于编写多线程、异步和并行代码的首选 API。

## <a name="creating-and-running-tasks-implicitly"></a>隐式创建和运行任务

<xref:System.Threading.Tasks.Parallel.Invoke%2A?displayProperty=nameWithType> 方法提供了一种简便方式，可同时运行任意数量的任意语句。 只需为每个工作项传入 <xref:System.Action> 委托即可。 创建这些委托的最简单方式是使用 lambda 表达式。 lambda 表达式可调用指定的方法，或提供内联代码。 下面的示例演示一个基本的 <xref:System.Threading.Tasks.Parallel.Invoke%2A> 调用，该调用创建并启动同时运行的两个任务。 第一个任务由调用名为 `DoSomeWork` 的方法的 lambda 表达式表示，第二个任务由调用名为 `DoSomeOtherWork` 的方法的 lambda 表达式表示。

> [!NOTE]
> 本文档使用 lambda 表达式在 TPL 中定义委托。 如果不熟悉 C# 或 Visual Basic 中的 lambda 表达式，请参阅 [PLINQ 和 TPL 中的 Lambda 表达式](lambda-expressions-in-plinq-and-tpl.md)。

[!code-csharp[TPL#21](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl/cs/tpl.cs#21)]
[!code-vb[TPL#21](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl/vb/tpl_vb.vb#21)]

> [!NOTE]
> <xref:System.Threading.Tasks.Task> 在后台创建的 <xref:System.Threading.Tasks.Parallel.Invoke%2A> 实例数不一定与所提供的委托数相等。 TPL 可能会使用各种优化，特别是对于大量的委托。

有关详细信息，请参阅[如何：使用 Parallel.Invoke 来执行并行操作](how-to-use-parallel-invoke-to-execute-parallel-operations.md)。

为了更好地控制任务执行或从任务返回值，必须更加显式地使用 <xref:System.Threading.Tasks.Task> 对象。

## <a name="creating-and-running-tasks-explicitly"></a>显式创建和运行任务

不返回值的任务由 <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> 类表示。 返回值的任务由 <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType> 类表示，该类从 <xref:System.Threading.Tasks.Task> 继承。 任务对象处理基础结构详细信息，并提供可在任务的整个生存期内从调用线程访问的方法和属性。 例如，可以随时访问任务的 <xref:System.Threading.Tasks.Task.Status%2A> 属性，以确定它是已开始运行、已完成运行、已取消还是引发了异常。 状态由 <xref:System.Threading.Tasks.TaskStatus> 枚举表示。

在创建任务时，你赋予它一个用户委托，该委托封装该任务将执行的代码。 该委托可以表示为命名的委托、匿名方法或 lambda 表达式。 lambda 表达式可以包含对命名方法的调用，如下面的示例所示。 请注意，该示例包含对 <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType> 方法的调用，以确保任务在控制台模式应用程序结束之前完成执行。

[!code-csharp[TPL_TaskIntro#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/lambda1.cs#1)]
[!code-vb[TPL_TaskIntro#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/lambda1.vb#1)]

你还可以使用 <xref:System.Threading.Tasks.Task.Run%2A?displayProperty=nameWithType> 方法通过一个操作创建并启动任务。 无论是哪个任务计划程序与当前线程关联，<xref:System.Threading.Tasks.Task.Run%2A> 方法都将使用默认的任务计划程序来管理任务。 不需要对任务的创建和计划进行更多控制时，首选 <xref:System.Threading.Tasks.Task.Run%2A> 方法创建并启动任务。

[!code-csharp[TPL_TaskIntro#2](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/run1.cs#2)]
[!code-vb[TPL_TaskIntro#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/run1.vb#2)]

你还可以使用 <xref:System.Threading.Tasks.TaskFactory.StartNew%2A?displayProperty=nameWithType> 方法在一个操作中创建并启动任务。 不必将创建和计划分开并且需要其他任务创建选项或使用特定计划程序时，或者需要将其他状态传递到可以通过 <xref:System.Threading.Tasks.Task.AsyncState%2A?displayProperty=nameWithType> 属性检索到的任务时，请使用此方法，如下例所示。

[!code-csharp[TPL_TaskIntro#3](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/asyncstate.cs#23)]
[!code-vb[TPL_TaskIntro#3](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/asyncstate.vb#23)]

<xref:System.Threading.Tasks.Task> 和 <xref:System.Threading.Tasks.Task%601> 均公开静态 <xref:System.Threading.Tasks.Task.Factory%2A> 属性，该属性返回 <xref:System.Threading.Tasks.TaskFactory> 的默认实例，因此你可以调用该方法为 `Task.Factory.StartNew()`。 此外，在以下示例中，由于任务的类型为 <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType>，因此每个任务都具有包含计算结果的公共 <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=nameWithType> 属性。 任务以异步方式运行，可以按任意顺序完成。 如果在计算完成之前访问 <xref:System.Threading.Tasks.Task%601.Result%2A> 属性，则该属性将阻止调用线程，直到值可用为止。

[!code-csharp[TPL_TaskIntro#4](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/result1.cs#4)]
[!code-vb[TPL_TaskIntro#4](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/result1.vb#4)]

有关详细信息，请参阅[如何：从任务中返回值](how-to-return-a-value-from-a-task.md)。

使用 lambda 表达式创建委托时，你有权访问源代码中当时可见的所有变量。 然而，在某些情况下，特别是在循环中，lambda 不按照预期的方式捕获变量。 它仅捕获最终值，而不是它每次迭代后更改的值。 以下示例演示了该问题。 它将循环计数器传递给实例化 `CustomData` 对象并使用循环计数器作为对象标识符的 lambda 表达式。 如示例输出所示，每个 `CustomData` 对象都具有相同的标识符。

[!code-csharp[TPL_TaskIntro#22](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/iteration1b.cs#22)]
[!code-vb[TPL_TaskIntro#22](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/iteration1b.vb#22)]

通过使用构造函数向任务提供状态对象，可以在每次迭代时访问该值。 以下示例在上一示例的基础上做了修改，在创建 `CustomData` 对象时使用循环计数器，该对象继而传递给 lambda 表达式。  如示例输出所示，每个 `CustomData` 对象现在都具有唯一的一个标识符，该标识符基于该对象实例化时循环计数器的值。

[!code-csharp[TPL_TaskIntro#21](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/iteration1a.cs#21)]
[!code-vb[TPL_TaskIntro#21](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/iteration1a.vb#21)]

此状态作为参数传递给任务委托，并且可通过使用 <xref:System.Threading.Tasks.Task.AsyncState%2A?displayProperty=nameWithType> 属性从任务对象访问。  以下示例在上一示例的基础上演变而来。 它使用 <xref:System.Threading.Tasks.Task.AsyncState%2A> 属性显示关于传递到 lambda 表达式的 `CustomData` 对象的信息。

[!code-csharp[TPL_TaskIntro#23](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/asyncstate.cs#23)]
[!code-vb[TPL_TaskIntro#23](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/asyncstate.vb#23)]

## <a name="task-id"></a>任务 ID

每个任务都获得一个在应用程序域中唯一标识自己的整数 ID，可以使用 <xref:System.Threading.Tasks.Task.Id%2A?displayProperty=nameWithType> 属性访问该 ID。 该 ID 可有效用于在 Visual Studio 调试器的“并行堆栈”和“任务”窗口中查看任务信息。 该 ID 是惰式创建的，这意味着它不会在被请求之前创建；因此每次运行该程序时，任务可能具有不同的 ID。 有关如何在调试器中查看任务 ID 的详细信息，请参阅[使用任务窗口](/visualstudio/debugger/using-the-tasks-window)和[使用并行堆栈窗口](/visualstudio/debugger/using-the-parallel-stacks-window)。

## <a name="task-creation-options"></a>任务创建选项

创建任务的大多数 API 提供接受 <xref:System.Threading.Tasks.TaskCreationOptions> 参数的重载。 通过指定下列某个或多个选项，可指示任务计划程序在线程池中安排任务计划的方式。 可以使用位 OR 运算组合选项。

下面的示例演示一个具有 <xref:System.Threading.Tasks.TaskCreationOptions.LongRunning> 和 <xref:System.Threading.Tasks.TaskContinuationOptions.PreferFairness> 选项的任务。

[!code-csharp[TPL_TaskIntro#03](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/taskintro.cs#03)]
[!code-vb[TPL_TaskIntro#03](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/tpl_intro.vb#03)]

## <a name="tasks-threads-and-culture"></a>任务、线程和区域性

每个线程都具有一个关联的区域性和 UI 区域性，分别由 <xref:System.Threading.Thread.CurrentCulture%2A?displayProperty=nameWithType> 和 <xref:System.Threading.Thread.CurrentUICulture%2A?displayProperty=nameWithType> 属性定义。 线程的区域性用在诸如格式、分析、排序和字符串比较操作中。 线程的 UI 区域性用于查找资源。

除非使用 <xref:System.Globalization.CultureInfo.DefaultThreadCurrentCulture%2A?displayProperty=nameWithType> 和 <xref:System.Globalization.CultureInfo.DefaultThreadCurrentUICulture%2A?displayProperty=nameWithType> 属性在应用程序域中为所有线程指定默认区域性，线程的默认区域性和 UI 区域性则由系统区域性定义。 如果你显式设置线程的区域性并启动新线程，则新线程不会继承正在调用的线程的区域性；相反，其区域性就是默认系统区域性。 但是，在基于任务的编程中，任务使用调用线程的区域性，即使任务在不同线程上以异步方式运行也是如此。

下面的示例提供了简单的演示。 它将应用的当前区域性更改为 French (France)；或者，如果 French (France) 已为当前区域性，则将其更改为 English (United States)。 然后，调用一个名为 `formatDelegate` 的委托，该委托返回在新区域性中格式化为货币值的数字。 无论委托是由任务同步调用还是异步调用，该任务都将使用调用线程的区域性。

:::code language="csharp" source="snippets/cs/asyncculture1.cs" id="1":::

:::code language="vbnet" source="snippets/vb/asyncculture1.vb" id="1":::

> [!NOTE]
> 在 .NET Framework 4.6 之前的 .NET Framework 版本中，任务的区域性由它在其上运行的线程区域性确定，而不是调用线程的区域性 。 对于异步任务，这意味着任务使用的区域性可能不同于调用线程的区域性。

有关异步任务和区域性的详细信息，请参阅 <xref:System.Globalization.CultureInfo> 主题中的“区域性和基于异步任务的操作”部分。

## <a name="creating-task-continuations"></a>创建任务延续

使用 <xref:System.Threading.Tasks.Task.ContinueWith%2A?displayProperty=nameWithType> 和 <xref:System.Threading.Tasks.Task%601.ContinueWith%2A?displayProperty=nameWithType> 方法，可以指定要在先行任务完成时启动的任务。 延续任务的委托已传递了对先行任务的引用，因此它可以检查先行任务的状态，并通过检索 <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=nameWithType> 属性的值将先行任务的输出用作延续任务的输入。

在下面的示例中，`getData` 任务通过调用 <xref:System.Threading.Tasks.TaskFactory.StartNew%60%601%28System.Func%7B%60%600%7D%29?displayProperty=nameWithType> 方法来启动。 当 `processData` 完成时，`getData` 任务自动启动，当 `displayData` 完成时，`processData` 启动。 `getData` 产生一个整数数组，通过 `processData` 任务的 `getData` 属性，<xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=nameWithType> 任务可访问该数组。 `processData` 任务处理该数组并返回结果，结果的类型从传递到 <xref:System.Threading.Tasks.Task%601.ContinueWith%60%601%28System.Func%7BSystem.Threading.Tasks.Task%7B%600%7D%2C%60%600%7D%29?displayProperty=nameWithType> 方法的 Lambda 表达式的返回类型推断而来。 `displayData` 完成时，`processData` 任务自动执行，而 <xref:System.Tuple%603> 任务可通过 `processData` 任务的 `displayData` 属性访问由 `processData` lambda 表达式返回的 <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=nameWithType> 对象。 `displayData` 任务采用 `processData` 任务的结果，继而得出自己的结果，其类型以相似方式推断而来，且可由程序中的 <xref:System.Threading.Tasks.Task%601.Result%2A> 属性使用。

[!code-csharp[TPL_TaskIntro#5](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/continuations1.cs#5)]
[!code-vb[TPL_TaskIntro#5](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/continuations1.vb#5)]

因为 <xref:System.Threading.Tasks.Task.ContinueWith%2A?displayProperty=nameWithType> 是实例方法，所以你可以将方法调用链接在一起，而不是为每个先行任务去实例化 <xref:System.Threading.Tasks.Task%601> 对象。 以下示例与上一示例在功能上等同，唯一的不同在于它将对 <xref:System.Threading.Tasks.Task.ContinueWith%2A?displayProperty=nameWithType> 方法的调用链接在一起。 请注意，通过方法调用链返回的 <xref:System.Threading.Tasks.Task%601> 对象是最终延续任务。

[!code-csharp[TPL_TaskIntro#24](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/continuations2.cs#24)]
[!code-vb[TPL_TaskIntro#24](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/continuations2.vb#24)]

使用 <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAll%2A> 和 <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAny%2A> 方法，可以从多个任务继续。

有关详细信息，请参阅[使用延续任务链接任务](chaining-tasks-by-using-continuation-tasks.md)。

## <a name="creating-detached-child-tasks"></a>创建分离的子任务

如果在任务中运行的用户代码创建一个新任务，且未指定 <xref:System.Threading.Tasks.TaskCreationOptions.AttachedToParent> 选项，则该新任务不采用任何特殊方式与父任务同步。 这种不同步的任务类型称为“分离的嵌套任务”或“分离的子任务”。 以下示例展示了创建一个分离子任务的任务。

[!code-csharp[TPL_TaskIntro#07](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/taskintro.cs#07)]
[!code-vb[TPL_TaskIntro#07](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/tpl_intro.vb#07)]

请注意，父任务不会等待分离子任务完成。

## <a name="creating-child-tasks"></a>创建子任务

如果任务中运行的用户代码在创建任务时指定了 <xref:System.Threading.Tasks.TaskCreationOptions.AttachedToParent> 选项，新任务就称为父任务的附加子任务。 因为父任务隐式地等待所有附加子任务完成，所以你可以使用 <xref:System.Threading.Tasks.TaskCreationOptions.AttachedToParent> 选项表示结构化的任务并行。 以下示例展示了创建十个附加子任务的父任务。 请注意，虽然此示例调用 <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType> 方法等待父任务完成，但不必显式等待附加子任务完成。

[!code-csharp[TPL_TaskIntro#8](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/child1.cs#8)]
[!code-vb[TPL_TaskIntro#8](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/child1.vb#8)]

父任务可使用 <xref:System.Threading.Tasks.TaskCreationOptions.DenyChildAttach?displayProperty=nameWithType> 选项阻止其他任务附加到父任务。 有关详细信息，请参阅[附加和分离的子任务](attached-and-detached-child-tasks.md)。

## <a name="waiting-for-tasks-to-finish"></a>等待任务完成

<xref:System.Threading.Tasks.Task?displayProperty=nameWithType> 和 <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType> 类型提供了 <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType> 方法的若干重载，以便能够等待任务完成。 此外，使用静态 <xref:System.Threading.Tasks.Task.WaitAll%2A?displayProperty=nameWithType> 和 <xref:System.Threading.Tasks.Task.WaitAny%2A?displayProperty=nameWithType> 方法的重载可以等待一批任务中的任一任务或所有任务完成。

通常，会出于以下某个原因等待任务：

- 主线程依赖于任务计算的最终结果。

- 你必须处理可能从任务引发的异常。

- 应用程序可以在所有任务执行完毕之前终止。 例如，执行 `Main`（应用程序入口点）中的所有同步代码后，控制台应用程序将立即终止。

下面的示例演示不包含异常处理的基本模式。

[!code-csharp[TPL_TaskIntro#06](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/taskintro.cs#06)]
[!code-vb[TPL_TaskIntro#06](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/tpl_intro.vb#06)]

有关演示异常处理的示例，请参见[异常处理](exception-handling-task-parallel-library.md)。

某些重载允许你指定超时，而其他重载采用额外的 <xref:System.Threading.CancellationToken> 作为输入参数，以便可以通过编程方式或根据用户输入来取消等待。

等待任务时，其实是在隐式等待使用 <xref:System.Threading.Tasks.TaskCreationOptions.AttachedToParent?displayProperty=nameWithType> 选项创建的该任务的所有子级。 <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType> 在该任务已完成时立即返回。 <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType> 方法将抛出由某任务引发的任何异常，即使 <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType> 方法是在该任务完成之后调用的。

## <a name="composing-tasks"></a>组合任务

<xref:System.Threading.Tasks.Task> 类和 <xref:System.Threading.Tasks.Task%601> 类提供多种方法，这些方法能够帮助你组合多个任务以实现常见模式，并更好地使用由 C#、Visual Basic 和 F# 提供的异步语言功能。 本节介绍了 <xref:System.Threading.Tasks.Task.WhenAll%2A>、<xref:System.Threading.Tasks.Task.WhenAny%2A>、<xref:System.Threading.Tasks.Task.Delay%2A> 和 <xref:System.Threading.Tasks.Task.FromResult%2A> 方法。

### <a name="taskwhenall"></a>Task.WhenAll

<xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> 方法异步等待多个 <xref:System.Threading.Tasks.Task> 或 <xref:System.Threading.Tasks.Task%601> 对象完成。 通过它提供的重载版本可以等待非均匀任务组。 例如，你可以等待多个 <xref:System.Threading.Tasks.Task> 和 <xref:System.Threading.Tasks.Task%601> 对象在一个方法调用中完成。

### <a name="taskwhenany"></a>Task.WhenAny

<xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType> 方法异步等待多个 <xref:System.Threading.Tasks.Task> 或 <xref:System.Threading.Tasks.Task%601> 对象中的一个完成。 与在 <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> 方法中一样，该方法提供重载版本，让你能等待非均匀任务组。 <xref:System.Threading.Tasks.Task.WhenAny%2A> 方法在下列情境中尤其有用。

- 冗余运算。 请考虑可以用多种方式执行的算法或运算。 你可使用 <xref:System.Threading.Tasks.Task.WhenAny%2A> 方法来选择先完成的运算，然后取消剩余的运算。

- 交叉运算。 你可启动必须全部完成的多项运算，并使用 <xref:System.Threading.Tasks.Task.WhenAny%2A> 方法在每项运算完成时处理结果。 在一项运算完成后，可以启动一个或多个其他任务。

- 受限制的运算。 你可使用 <xref:System.Threading.Tasks.Task.WhenAny%2A> 方法通过限制并发运算的数量来扩展前面的情境。

- 过期的运算。 你可使用 <xref:System.Threading.Tasks.Task.WhenAny%2A> 方法在一个或多个任务与特定时间后完成的任务（例如 <xref:System.Threading.Tasks.Task.Delay%2A> 方法返回的任务）间进行选择。 下节描述了 <xref:System.Threading.Tasks.Task.Delay%2A> 方法。

### <a name="taskdelay"></a>Task.Delay

<xref:System.Threading.Tasks.Task.Delay%2A?displayProperty=nameWithType> 方法将生成在指定时间后完成的 <xref:System.Threading.Tasks.Task> 对象。 你可使用此方法来生成偶尔轮询数据的循环，引入超时，将对用户输入的处理延迟预定的一段时间等。

### <a name="tasktfromresult"></a>Task(T).FromResult

通过使用 <xref:System.Threading.Tasks.Task.FromResult%2A?displayProperty=nameWithType> 方法，你可以创建包含预计算结果的 <xref:System.Threading.Tasks.Task%601> 对象。 执行返回 <xref:System.Threading.Tasks.Task%601> 对象的异步运算，且已计算该 <xref:System.Threading.Tasks.Task%601> 对象的结果时，此方法将十分有用。 有关使用 <xref:System.Threading.Tasks.Task.FromResult%2A> 检索缓存中包含的异步下载运算结果的示例，请参阅[如何：创建预先计算的任务](how-to-create-pre-computed-tasks.md)。

## <a name="handling-exceptions-in-tasks"></a>处理任务中的异常

当某个任务抛出一个或多个异常时，异常包装在 <xref:System.AggregateException> 异常中。 该异常传播回与该任务联接的线程，通常该线程正在等待该任务完成或该线程访问 <xref:System.Threading.Tasks.Task%601.Result%2A> 属性。 此行为用于强制实施 .NET Framework 策略 - 默认所有未处理的异常应终止进程。 调用代码可以通过使用 `try`/`catch` 块中的以下任意方法来处理异常：

- <xref:System.Threading.Tasks.Task.Wait%2A> 方法

- <xref:System.Threading.Tasks.Task.WaitAll%2A> 方法

- <xref:System.Threading.Tasks.Task.WaitAny%2A> 方法

- <xref:System.Threading.Tasks.Task%601.Result%2A> 属性

联接线程也可以通过在对任务进行垃圾回收之前访问 <xref:System.Threading.Tasks.Task.Exception%2A> 属性来处理异常。 通过访问此属性，可防止未处理的异常在对象完成时触发终止进程的异常传播行为。

有关异常和任务的的详细信息，请参阅[异常处理](exception-handling-task-parallel-library.md)。

## <a name="canceling-tasks"></a>取消任务

<xref:System.Threading.Tasks.Task> 类支持协作取消，并与 .NET Framework 4 中新增的 <xref:System.Threading.CancellationTokenSource?displayProperty=nameWithType> 类和 <xref:System.Threading.CancellationToken?displayProperty=nameWithType> 类完全集成。 <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> 类中的大多数构造函数采用 <xref:System.Threading.CancellationToken> 对象作为输入参数。 许多 <xref:System.Threading.Tasks.TaskFactory.StartNew%2A> 和 <xref:System.Threading.Tasks.Task.Run%2A> 重载还包括 <xref:System.Threading.CancellationToken> 参数。

你可以创建标记，并使用 <xref:System.Threading.CancellationTokenSource> 类在以后某一时间发出取消请求。 可以将该标记作为参数传递给 <xref:System.Threading.Tasks.Task>，还可以在执行响应取消请求的工作的用户委托中引用同一标记。

有关详细信息，请参阅[任务取消](task-cancellation.md)和[如何：取消任务及其子级](how-to-cancel-a-task-and-its-children.md)。

## <a name="the-taskfactory-class"></a>TaskFactory 类

<xref:System.Threading.Tasks.TaskFactory> 类提供静态方法，这些方法封装了用于创建和启动任务和延续任务的一些常用模式。

- 最常用模式为 <xref:System.Threading.Tasks.TaskFactory.StartNew%2A>，它在一个语句中创建并启动任务。

- 如果通过多个先行任务创建延续任务，请使用 <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAll%2A> 方法或 <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAny%2A> 方法，或它们在 <xref:System.Threading.Tasks.Task%601> 类中的相当方法。 有关详细信息，请参阅[使用延续任务链接任务](chaining-tasks-by-using-continuation-tasks.md)。

- 若要在 `BeginX` 或 `EndX` 实例中封装异步编程模型 <xref:System.Threading.Tasks.Task> 和 <xref:System.Threading.Tasks.Task%601> 方法，请使用 <xref:System.Threading.Tasks.TaskFactory.FromAsync%2A> 方法。 有关详细信息，请参阅 [TPL 和传统 .NET Framework 异步编程](tpl-and-traditional-async-programming.md)。

默认的 <xref:System.Threading.Tasks.TaskFactory> 可作为 <xref:System.Threading.Tasks.Task> 类或 <xref:System.Threading.Tasks.Task%601> 类上的静态属性访问。 你还可以直接实例化 <xref:System.Threading.Tasks.TaskFactory> 并指定各种选项，包括 <xref:System.Threading.CancellationToken>、<xref:System.Threading.Tasks.TaskCreationOptions> 选项、<xref:System.Threading.Tasks.TaskContinuationOptions> 选项或 <xref:System.Threading.Tasks.TaskScheduler>。 创建任务工厂时所指定的任何选项将应用于它创建的所有任务，除非 <xref:System.Threading.Tasks.Task> 是通过使用 <xref:System.Threading.Tasks.TaskCreationOptions> 枚举创建的（在这种情况下，任务的选项重写任务工厂的选项）。

## <a name="tasks-without-delegates"></a>无委托的任务

在某些情况下，可能需要使用 <xref:System.Threading.Tasks.Task> 封装由外部组件（而不是你自己的用户委托）执行的某个异步操作。 如果该操作基于异步编程模型 Begin/End 模式，你可以使用 <xref:System.Threading.Tasks.TaskFactory.FromAsync%2A> 方法。 如果不是这种情况，你可以使用 <xref:System.Threading.Tasks.TaskCompletionSource%601> 对象将该操作包装在任务中，并因而获得 <xref:System.Threading.Tasks.Task> 可编程性的一些好处，例如对异常传播和延续的支持。 有关详细信息，请参阅 <xref:System.Threading.Tasks.TaskCompletionSource%601>。

## <a name="custom-schedulers"></a>自定义计划程序

大多数应用程序或库开发人员并不关心任务在哪个处理器上运行、任务如何将其工作与其他任务同步以及如何在 <xref:System.Threading.ThreadPool?displayProperty=nameWithType> 中计划任务。 他们只需要它在主机上尽可能高效地执行。 如果需要对计划细节进行更细化的控制，可以使用任务并行库在默认任务计划程序上配置一些设置，甚至是提供自定义计划程序。 有关详细信息，请参阅 <xref:System.Threading.Tasks.TaskScheduler>。

## <a name="related-data-structures"></a>相关数据结构

TPL 有几种在并行和顺序方案中都有用的新公共类型。 它们包括 <xref:System.Collections.Concurrent?displayProperty=nameWithType> 命名空间中的一些线程安全的、快速且可缩放的集合类，还包括一些新的同步类型（例如 <xref:System.Threading.Semaphore?displayProperty=nameWithType> 和 <xref:System.Threading.ManualResetEventSlim?displayProperty=nameWithType>），对特定类型的工作负荷而言，这些新同步类型比旧的同步类型效率更高。 .NET Framework 4 中的其他新类型（例如 <xref:System.Threading.Barrier?displayProperty=nameWithType> 和 <xref:System.Threading.SpinLock?displayProperty=nameWithType>）提供了早期版本中未提供的功能。 有关详细信息，请参阅[用于并行编程的数据结构](data-structures-for-parallel-programming.md)。

## <a name="custom-task-types"></a>自定义任务类型

建议不要从 <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> 或 <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType> 继承。 相反，我们建议你使用 <xref:System.Threading.Tasks.Task.AsyncState%2A> 属性将其他数据或状态与 <xref:System.Threading.Tasks.Task> 或 <xref:System.Threading.Tasks.Task%601> 对象相关联。 还可以使用扩展方法扩展 <xref:System.Threading.Tasks.Task> 和 <xref:System.Threading.Tasks.Task%601> 类的功能。 有关扩展方法的详细信息，请参阅[扩展方法](../../csharp/programming-guide/classes-and-structs/extension-methods.md)和[扩展方法](../../visual-basic/programming-guide/language-features/procedures/extension-methods.md)。

如果必须从 <xref:System.Threading.Tasks.Task> 或 <xref:System.Threading.Tasks.Task%601> 继承，则不能使用 <xref:System.Threading.Tasks.Task.Run%2A> 或 <xref:System.Threading.Tasks.TaskFactory?displayProperty=nameWithType>，<xref:System.Threading.Tasks.TaskFactory%601?displayProperty=nameWithType> 或 <xref:System.Threading.Tasks.TaskCompletionSource%601?displayProperty=nameWithType> 类创建自定义任务类型的实例，因为这些类仅创建 <xref:System.Threading.Tasks.Task> 和 <xref:System.Threading.Tasks.Task%601> 对象。 此外，不能使用 <xref:System.Threading.Tasks.Task>、<xref:System.Threading.Tasks.Task%601>、<xref:System.Threading.Tasks.TaskFactory> 和 <xref:System.Threading.Tasks.TaskFactory%601> 提供的任务延续机制创建自定义任务类型的实例，因为这些机制也只创建 <xref:System.Threading.Tasks.Task> 和 <xref:System.Threading.Tasks.Task%601> 对象。

## <a name="related-topics"></a>相关主题

|Title|描述|
|-|-|
|[使用延续任务来链接任务](chaining-tasks-by-using-continuation-tasks.md)|描述延续任务的工作方式。|
|[附加和分离的子任务](attached-and-detached-child-tasks.md)|描述附加子任务和分离子任务之间的差异。|
|[任务取消](task-cancellation.md)|描述在 <xref:System.Threading.Tasks.Task> 对象中内置的取消支持。|
|[异常处理](exception-handling-task-parallel-library.md)|描述如何处理并行线程中的异常。|
|[如何：使用 Parallel.Invoke 来执行并行操作](how-to-use-parallel-invoke-to-execute-parallel-operations.md)|描述如何使用 <xref:System.Threading.Tasks.Parallel.Invoke%2A>。|
|[如何：从任务中返回值](how-to-return-a-value-from-a-task.md)|描述如何从任务中返回值。|
|[如何：取消任务及其子级](how-to-cancel-a-task-and-its-children.md)|描述如何取消任务。|
|[如何：创建预先计算的任务](how-to-create-pre-computed-tasks.md)|描述如何使用 <xref:System.Threading.Tasks.Task.FromResult%2A?displayProperty=nameWithType> 方法去检索缓存中包含的异步下载运算结果。|
|[如何：使用并行任务遍历二叉树](how-to-traverse-a-binary-tree-with-parallel-tasks.md)|描述如何使用任务遍历二叉树。|
|[如何：解除嵌套任务的包装](how-to-unwrap-a-nested-task.md)|演示如何使用 <xref:System.Threading.Tasks.TaskExtensions.Unwrap%2A> 扩展方法。|
|[数据并行](data-parallelism-task-parallel-library.md)|描述如何使用 <xref:System.Threading.Tasks.Parallel.For%2A> 和 <xref:System.Threading.Tasks.Parallel.ForEach%2A> 来创建循环访问数据的并行循环。|
|[并行编程](index.md)|.NET Framework 并行编程的顶级节点。|

## <a name="see-also"></a>请参阅

- [并行编程](index.md)
- [使用 .NET Core 和 .NET Standard 并行编程的示例](/samples/browse/?products=dotnet-core%2Cdotnet-standard&term=parallel)
