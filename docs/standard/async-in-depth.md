---
title: 深入了解异步
description: 了解如何使用基于 .NET 任务的异步模型直接编写绑定 I/O 和 CPU 的异步代码。
author: cartermp
ms.author: wiwagn
ms.date: 06/20/2016
ms.assetid: 1e38f9d9-8f84-46ee-a15f-199aec4f2e34
ms.openlocfilehash: 7fcc41c4ea5037d643402fc722e8f16f28d560ee
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94823324"
---
# <a name="async-in-depth"></a>深入了解异步

使用基于 .NET 任务的异步模型可直接编写绑定 I/O 和 CPU 的异步代码。 该模型由 `Task` 和 `Task<T>` 类型以及 C# 和 Visual Basic 中的 `async` 和 `await` 关键字公开。 （有关特定语言的资源，请参见[另请参阅](#see-also)部分。）本文解释如何使用 .NET 异步，并深入介绍其中使用的异步框架。

## <a name="task-and-taskt"></a>任务和 Task\<T>

任务是用于实现称之为[并发 Promise 模型](https://en.wikipedia.org/wiki/Futures_and_promises)的构造。  简单地说，它们“承诺”，会在稍后完成工作，让你使用干净的 API 与 promise 协作。

- `Task` 表示不返回值的单个操作。
- `Task<T>` 表示返回 `T` 类型的值的单个操作。

请务必将任务理解为工作的异步抽象，而非  在线程之上的抽象。 默认情况下，任务在当前线程上执行，且在适当时会将工作委托给操作系统。 可选择性地通过 `Task.Run` API 显式请求任务在独立线程上运行。

任务会公开一个 API 协议来监视、等候和访问任务的结果值（如 `Task<T>`）。 含有 `await` 关键字的语言集成可提供高级别抽象来使用任务。

任务运行时，使用 `await` 在任务完成前将控制让步于其调用方，可让应用程序和服务执行有用工作。 任务完成后代码无需依靠回调或事件便可继续执行。 语言和任务 API 集成会为你完成此操作。 如果正在使用 `Task<T>`，任务完成时，`await` 关键字还将“打开”返回的值。  下面进一步详细介绍了此工作原理。

可在[基于任务的异步模式 (TAP)](./asynchronous-programming-patterns/task-based-asynchronous-pattern-tap.md) 主题中了解有关任务以及与任务交互的不同方法的详细信息。

## <a name="deeper-dive-into-tasks-for-an-io-bound-operation"></a>深入了解针对绑定 I/O 的操作的任务

以下部分介绍了使用典型异步 I/O 调用时会出现的各种情况。 我们先看两个例子。

第一个示例调用异步方法，并返回活动任务，很可能尚未完成。

```csharp
public Task<string> GetHtmlAsync()
{
    // Execution is synchronous here
    var client = new HttpClient();

    return client.GetStringAsync("https://www.dotnetfoundation.org");
}
```

第二个示例还使用了 `async` 和 `await` 关键字对任务进行操作。

```csharp
public async Task<string> GetFirstCharactersCountAsync(string url, int count)
{
    // Execution is synchronous here
    var client = new HttpClient();

    // Execution of GetFirstCharactersCountAsync() is yielded to the caller here
    // GetStringAsync returns a Task<string>, which is *awaited*
    var page = await client.GetStringAsync("https://www.dotnetfoundation.org");

    // Execution resumes when the client.GetStringAsync task completes,
    // becoming synchronous again.

    if (count > page.Length)
    {
        return page;
    }
    else
    {
        return page.Substring(0, count);
    }
}
```

对 `GetStringAsync()` 的调用通过低级别 .NET 库进行（可能是调用其他异步方法），直到其到达 P/Invoke 互操作调用，进入本机网络库。 本机库随后可能会调入系统 API 调用（例如 Linux 上套接字的 `write()`）。 可能会使用 [TaskCompletionSource](xref:System.Threading.Tasks.TaskCompletionSource%601.SetResult(%600)) 在本机/托管边界创建一个任务对象。 将通过层向上传递任务对象，对其进行操作或直接返回，最后返回到初始调用方。

在上述第二个示例中，`Task<T>` 对象直接从 `GetStringAsync` 返回。 由于使用了 `await` 关键字，因此该方法会返回一个新建的任务对象。 在 `GetFirstCharactersCountAsync` 方法中，控制权从此位置返回给调用方。 [Task&lt;T&gt;](xref:System.Threading.Tasks.Task%601) 对象的方法和属性使调用者能够监视任务的进度。GetFirstCharactersCountAsync 中剩余的代码执行完毕时，该任务便完成。

调用系统 API 后，请求位于内核空间，一路来到操作系统的网络子系统（例如 Linux 内核中的 `/net`）。  此处操作系统将对网络请求进行异步  处理。  所用操作系统不同，细节可能有所不同（可能会将设备驱动程序调用安排为发送回运行时的信号，或者会执行设备驱动程序调用然后  有一个信号发送回来），但最终都会通知运行时网络请求正在进行中。  此时，设备驱动程序工作处于已计划、正在进行或是已完成（请求已“通过网络”发出），但由于这些均为异步进行，设备驱动程序可立即着手处理其他事项！

例如，在 Windows 中操作系统线程调用网络设备驱动程序并要求它通过表示操作的中断请求数据包 (IRP) 执行网络操作。  设备驱动程序接收 IRP，调用网络，将 IRP 标记为“待定”，并返回到操作系统。  由于现在操作系统线程了解到 IRP 为“待定”，因此无需再为此作业进行进一步操作，将其“返回”，这样它就可用于完成其他工作。

请求完成且数据通过设备驱动程序返回后，会经由中断通知 CPU 新接收到的数据。  处理中断的方式因操作系统不同而有所不同，但最终都会通过操作系统将数据传递到系统互操作调用（例如，Linux 中的中断处理程序将安排 IRQ 的下半部分通过操作系统异步向上传递数据）。  请注意这仍  是异步进行的！  在下一个可用线程能执行异步方法且“解包”已完成任务的结果前，结果会排入队列。

在整个过程中，关键点在于 **没有线程专用于运行任务**。  尽管需要在一些上下文中执行工作（即，操作系统确实必须将数据传递到设备驱动程序并响应中断），但没有专用于 *等待* 数据从请求返回的线程。  这让系统能处理更多的工作而不是等待某些 I/O 调用结束。

虽从上述内容来看需要完成许多工作，但以实际时间来计量，这远少于执行实际 I/O 工作所花费的时间。 虽然不是完全精确，但此类调用可能的时间线如下所示：

0-1————————————————————————————————————————————————–2-3

- 从点 `0` 到 `1` 所花费时间很长，直到异步方法将控制让步于其调用方才结束。
- 从点 `1` 到点 `2` 所用时间是花费在 I/O 上的时间，且 CPU 没有耗时。
- 最后，点 `2` 到点 `3` 所花费时间用于将控制（和可能的值）传递回异步方法，此时将再次执行。

### <a name="what-does-this-mean-for-a-server-scenario"></a>这对服务器方案而言意味着什么？

此模型可很好地处理典型的服务器方案工作负荷。  由于没有专用于阻止未完成任务的线程，服务器线程池可服务更多的 Web 请求。

考虑使用两个服务器：一个运行异步代码，一个不运行异步代码。  鉴于本示例的目的，每个服务器只有 5 个可用于服务请求的线程。  请注意，这样小的数目仅可用于演示。

假设两个服务器都接收到 6 个并发请求。 每个请求执行一个 I/O 操作。  未  运行异步代码的服务器必须对第 6 个请求排队，直到 5 个线程中的一个完成了绑定 I/O 的工作并编写了响应。 此时收到了第 20 个请求，由于队列过长，服务器可能会开始变慢。

运行有  异步代码的服务器也需对第 6 个请求排队，但由于使用了 `async` 和 `await`，绑定了 I/O 的工作开始时，每个线程都会得到释放，无需等到工作结束。  收到第 20 个请求时，传入请求队列将变得很小（如果其中还有请求的话），且服务器不会变慢。

尽管这是一个人为想象的示例，但在现实世界中其工作方式与此类似。  事实上，相比服务器将线程专用于接收到的每个请求，使用 `async` 和 `await` 能够使服务器多处理一个数量级的请求。

### <a name="what-does-this-mean-for-client-scenario"></a>这对客户端方案而言意味着什么？

使用 `async` 和 `await` 对客户端应用带来的最大好处在于提高了响应能力。  尽管可以手动生成线程让应用响应，但相比仅使用 `async` 和 `await`，生成线程的操作更加昂贵。  特别是对于手机游戏等应用而言，在涉及 I/O 时尽可能少地影响 UI 线程，这点至关重要。

更重要的是，由于绑定 I/O 的工作在 CPU 上几乎没有耗时，所以将整个 CPU 线程专用于执行几乎没有任何作用的工作将是一种资源浪费。

此外，使用 `async` 方法将工作调度到 UI 线程（例如，更新 UI）十分简单，且无需额外的工作（例如调用线程安全的委托）。

## <a name="deeper-dive-into-task-and-taskt-for-a-cpu-bound-operation"></a>深入了解绑定 CPU 的操作的任务和 Task\<T>

绑定 CPU 的 `async` 代码与绑定 I/O 的 `async` 代码有些许不同。  由于工作在 CPU 上执行，无法解决线程专用于计算的问题。  `async` 和 `await` 的运用使得可以与后台线程交互并让异步方法调用方可响应。  请注意这不会为共享数据提供任何保护。  如果正在使用共享数据，仍需要采用合适的同步策略。

这里详细介绍了绑定 CPU 的异步调用的方方面面：

```csharp
public async Task<int> CalculateResult(InputData data)
{
    // This queues up the work on the threadpool.
    var expensiveResultTask = Task.Run(() => DoExpensiveCalculation(data));

    // Note that at this point, you can do some other work concurrently,
    // as CalculateResult() is still executing!

    // Execution of CalculateResult is yielded here!
    var result = await expensiveResultTask;

    return result;
}
```

`CalculateResult()` 在调用它的线程上执行。  调用 `Task.Run` 时，它会在线程池上对昂贵的绑定 CPU 的操作 `DoExpensiveCalculation()` 进行排队，并收到一个 `Task<int>` 句柄。  `DoExpensiveCalculation()` 最终在下一个可用线程上并行运行（很可能在另一个 CPU 内核上）。  当 `DoExpensiveCalculation()` 忙于处理另一线程时它可能进行并行工作，因为调用 `CalculateResult()` 的线程仍在执行。

一旦遇到 `await`，`CalculateResult()` 执行会让步于调用方，在 `DoExpensiveCalculation()` 执行运算的同时，允许其他任务在当前线程执行。  `DoExpensiveCalculation()` 完成后，结果会在主线程上排队等待运行。  最后，主线程将返回执行得到 `DoExpensiveCalculation()` 结果的 `CalculateResult()`，。

### <a name="why-does-async-help-here"></a>异步为什么在此处会起作用？

`async` 和 `await` 是在需要可响应性时管理绑定 CPU 的工作的最佳实践。 存在多个可将异步用于绑定 CPU 的工作的模式。 请务必注意，使用异步成本有少许费用，不推荐紧凑循环使用它。  如何编写此新功能的代码完全取决于你。

## <a name="see-also"></a>请参阅

- [C# 中的异步编程](../csharp/async.md)
- [使用 Async 和 Await 的异步编程 (C#)](../csharp/programming-guide/concepts/async/index.md)
- [F# 中的异步编程](../fsharp/tutorials/asynchronous-and-concurrent-programming/async.md)
- [使用 Async 和 Await 的异步编程 (Visual Basic)](../visual-basic/programming-guide/concepts/async/index.md)
