---
title: 使用基于任务的异步模式
description: 了解如何在进行异步操作时使用基于任务的异步模式 (TAP)。
ms.date: 03/30/2017
helpviewer_keywords:
- .NET and TAP
- asynchronous design patterns, .NET
- TAP, .NET support for
- Task-based Asynchronous Pattern, .NET support for
- .NET, asynchronous design patterns
ms.assetid: 033cf871-ae24-433d-8939-7a3793e547bf
ms.openlocfilehash: 312a86f38335f1c16a4286795f04b9a80a9ea03f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95678124"
---
# <a name="consuming-the-task-based-asynchronous-pattern"></a>使用基于任务的异步模式

使用基于任务的异步模式 (TAP) 处理异步操作时，可以使用回叫实现等待，而不会阻塞。 对于任务，这可通过 <xref:System.Threading.Tasks.Task.ContinueWith%2A?displayProperty=nameWithType> 等方法实现。 通过允许在正常控制流中等待异步操纵，基于语言的异步支持可隐藏回叫，并且编译器生成的代码可提供此相同 API 级别的支持。

## <a name="suspending-execution-with-await"></a>使用 Await 挂起执行

可以使用 C# 中的 [await](../../csharp/language-reference/operators/await.md) 关键字和 Visual Basic 中的 [Await 运算符](../../visual-basic/language-reference/operators/await-operator.md)来异步等待 <xref:System.Threading.Tasks.Task> 和 <xref:System.Threading.Tasks.Task%601> 对象。 等待 <xref:System.Threading.Tasks.Task> 时，`await` 表达式的类型为 `void`。 等待 <xref:System.Threading.Tasks.Task%601> 时，`await` 表达式的类型为 `TResult`。 `await` 表达式必须出现在异步方法的正文内。 （.NET Framework 4.5 中引入了这些语言功能。）

 实际上，await 功能通过使用延续任务在任务上安装回叫。  此回叫在挂起点恢复异步方法。 恢复异步方法时，如果等待的操作已成功完成且为 <xref:System.Threading.Tasks.Task%601>，返回的是 `TResult`。  如果等待的 <xref:System.Threading.Tasks.Task> 或 <xref:System.Threading.Tasks.Task%601> 以 <xref:System.Threading.Tasks.TaskStatus.Canceled> 状态结束，就会抛出 <xref:System.OperationCanceledException> 异常。  如果等待的 <xref:System.Threading.Tasks.Task> 或 <xref:System.Threading.Tasks.Task%601> 以 <xref:System.Threading.Tasks.TaskStatus.Faulted> 状态结束，就会抛出导致它发生故障的异常。 一个 `Task` 可能由于多个异常而出错，但只会传播一个异常。 不过，<xref:System.Threading.Tasks.Task.Exception%2A?displayProperty=nameWithType> 属性会返回包含所有错误的 <xref:System.AggregateException> 异常。

 如果同步上下文（<xref:System.Threading.SynchronizationContext> 对象）与暂停时正在执行异步方法的线程（例如，<xref:System.Threading.SynchronizationContext.Current%2A?displayProperty=nameWithType> 属性不是 `null`）相关联，异步方法使用上下文的 <xref:System.Threading.SynchronizationContext.Post%2A> 方法，恢复相同的同步上下文。 否则，它依赖暂停时的当前任务计划程序（<xref:System.Threading.Tasks.TaskScheduler> 对象）。 通常情况下，这是定目标到线程池的默认任务计划程序 (<xref:System.Threading.Tasks.TaskScheduler.Default%2A?displayProperty=nameWithType>)。 此任务计划程序确定等待的异步操作是否应在该操作完成时恢复，或是否应计划该恢复。 默认计划程序通常允许在完成等待操作的线程上延续任务。

 调用异步方法时，将同步执行函数的正文，直到遇见尚未完成的可等待实例上的第一个 await 表达式，此时调用返回到调用方。 如果异步方法不返回 `void`，将会返回 <xref:System.Threading.Tasks.Task> 或 <xref:System.Threading.Tasks.Task%601> 对象，以表示正在进行的计算。 在非 void 异步方法中，如果遇到 return 语句或到达方法正文末尾，任务就以 <xref:System.Threading.Tasks.TaskStatus.RanToCompletion> 最终状态完成。 如果未经处理的异常导致无法控制异步方法正文，任务就以 <xref:System.Threading.Tasks.TaskStatus.Faulted> 状态结束。 如果异常为 <xref:System.OperationCanceledException>，任务改为以 <xref:System.Threading.Tasks.TaskStatus.Canceled> 状态结束。 通过此方式，最终将发布结果或异常。

 此行为有几种重要特殊情况。  出于性能原因，如果任务在等待时已完成，则不会生成控件，并且该函数将继续执行。  返回到原始上下文并不总是所需的行为，可对其进行更改；将在下一节中更详细地描述此内容。

### <a name="configuring-suspension-and-resumption-with-yield-and-configureawait"></a>使用 Yield 和 ConfigureAwait 配置挂起和恢复

 有多种方法可更好地控制异步方法的执行。 例如，可以使用 <xref:System.Threading.Tasks.Task.Yield%2A?displayProperty=nameWithType> 方法，将暂停点引入异步方法：

```csharp
public class Task : …
{
    public static YieldAwaitable Yield();
    …
}
```

 这相当于以异步方式发布或计划返回当前上下文。

```csharp
Task.Run(async delegate
{
    for(int i=0; i<1000000; i++)
    {
        await Task.Yield(); // fork the continuation into a separate work item
        ...
    }
});
```

 还可以使用 <xref:System.Threading.Tasks.Task.ConfigureAwait%2A?displayProperty=nameWithType> 方法，更好地控制异步方法中的暂停和恢复。  如前所述，默认情况下，异步方法挂起时会捕获当前上下文，捕获的上下文用于在恢复时调用异步方法的延续。  在多数情况下，这就是你所需的确切行为。  在其他情况下，你可能不关心延续上下文，则可以通过避免此类发布返回原始上下文来获得更好的性能。  若要启用此功能，请使用 <xref:System.Threading.Tasks.Task.ConfigureAwait%2A?displayProperty=nameWithType> 方法，指示等待操作不要捕获和恢复上下文，而是继续执行正在等待完成的所有异步操作：

```csharp
await someTask.ConfigureAwait(continueOnCapturedContext:false);
```

## <a name="canceling-an-asynchronous-operation"></a>取消异步操作

从 .NET Framework 4 开始，支持取消操作的 TAP 方法提供至少一个接受取消令牌（<xref:System.Threading.CancellationToken> 对象）的重载。

 可通过取消令牌源（<xref:System.Threading.CancellationTokenSource> 对象）创建取消令牌。  源的 <xref:System.Threading.CancellationTokenSource.Token%2A> 属性返回取消令牌，它在源的 <xref:System.Threading.CancellationTokenSource.Cancel%2A> 方法获得调用时收到信号。  例如，若要下载一个网页，并且希望能够取消此操作，请创建 <xref:System.Threading.CancellationTokenSource> 对象，将它的令牌传递给 TAP 方法，再在准备好取消此操作时，调用源的 <xref:System.Threading.CancellationTokenSource.Cancel%2A> 方法：

```csharp
var cts = new CancellationTokenSource();
string result = await DownloadStringAsync(url, cts.Token);
… // at some point later, potentially on another thread
cts.Cancel();
```

 若要取消多个异步调用，可以将相同令牌传递给所有调用：

```csharp
var cts = new CancellationTokenSource();
    IList<string> results = await Task.WhenAll(from url in urls select DownloadStringAsync(url, cts.Token));
    // at some point later, potentially on another thread
    …
    cts.Cancel();
```

 或者，将相同令牌传递给操作的选择性子集：

```csharp
var cts = new CancellationTokenSource();
    byte [] data = await DownloadDataAsync(url, cts.Token);
    await SaveToDiskAsync(outputPath, data, CancellationToken.None);
    … // at some point later, potentially on another thread
    cts.Cancel();
```

 可以从任意线程启动取消请求。

 可以将 <xref:System.Threading.CancellationToken.None%2A?displayProperty=nameWithType> 值传递给接受取消令牌的任何方法，指明绝不会请求取消操作。  这会导致 <xref:System.Threading.CancellationToken.CanBeCanceled%2A?displayProperty=nameWithType> 属性返回 `false`，并且调用的方法可以相应地进行优化。  出于测试目的，还可以通过传入预取消标记（该标记使用接受布尔值的构造函数进行实例化）来指示是否应以已取消或未取消状态启动标记。

 使用此方法进行取消具有以下优点：

- 可以将相同的取消标记传递给任意数量的异步和同步操作。

- 相同的取消请求可能会扩展到任意数量的侦听器。

- 异步 API 的开发人员可完全控制是否可以请求取消以及取消何时生效。

- 使用 API 的代码可以选择性地确定将对其传播取消请求的异步调用。

## <a name="monitoring-progress"></a>监视进度

 某些异步方法通过传入异步方法的进度接口来公开进度。  例如，设想某个函数以异步方式下载文本字符串，并在该过程中引发包括到目前为止下载完成百分比的进度更新。  此类方法可在 Windows Presentation Foundation (WPF) 应用程序中使用，如下所示：

```csharp
private async void btnDownload_Click(object sender, RoutedEventArgs e)
{
    btnDownload.IsEnabled = false;
    try
    {
        txtResult.Text = await DownloadStringAsync(txtUrl.Text,
            new Progress<int>(p => pbDownloadProgress.Value = p));
    }
    finally { btnDownload.IsEnabled = true; }
}
```

<a name="combinators"></a>

## <a name="using-the-built-in-task-based-combinators"></a>使用内置的基于任务的连结符

 <xref:System.Threading.Tasks> 命名空间包含多个方法，可用于撰写和处理任务。

### <a name="taskrun"></a>Task.Run

 <xref:System.Threading.Tasks.Task> 类包含多个 <xref:System.Threading.Tasks.Task.Run%2A> 方法，以便于将工作作为 <xref:System.Threading.Tasks.Task> 或 <xref:System.Threading.Tasks.Task%601> 轻松卸载到线程池，例如：

```csharp
public async void button1_Click(object sender, EventArgs e)
{
    textBox1.Text = await Task.Run(() =>
    {
        // … do compute-bound work here
        return answer;
    });
}
```

 其中部分 <xref:System.Threading.Tasks.Task.Run%2A> 方法（如 <xref:System.Threading.Tasks.Task.Run%28System.Func%7BSystem.Threading.Tasks.Task%7D%29?displayProperty=nameWithType> 重载）以 <xref:System.Threading.Tasks.TaskFactory.StartNew%2A?displayProperty=nameWithType> 方法的简约表示形式存在。  借助其他重载（如 <xref:System.Threading.Tasks.Task.Run%28System.Func%7BSystem.Threading.Tasks.Task%7D%29?displayProperty=nameWithType>），可以在卸载的工作内使用 await，例如：

```csharp
public async void button1_Click(object sender, EventArgs e)
{
    pictureBox1.Image = await Task.Run(async() =>
    {
        using(Bitmap bmp1 = await DownloadFirstImageAsync())
        using(Bitmap bmp2 = await DownloadSecondImageAsync())
        return Mashup(bmp1, bmp2);
    });
}
```

 此类重载在逻辑上相当于结合使用 <xref:System.Threading.Tasks.TaskFactory.StartNew%2A?displayProperty=nameWithType> 方法和任务并行库中的 <xref:System.Threading.Tasks.TaskExtensions.Unwrap%2A> 扩展方法。

### <a name="taskfromresult"></a>Task.FromResult

 <xref:System.Threading.Tasks.Task.FromResult%2A> 方法的适用情景为，数据可能已存在，且只需通过提升到 <xref:System.Threading.Tasks.Task%601> 的任务返回方法返回：

```csharp
public Task<int> GetValueAsync(string key)
{
    int cachedValue;
    return TryGetCachedValue(out cachedValue) ?
        Task.FromResult(cachedValue) :
        GetValueAsyncInternal();
}

private async Task<int> GetValueAsyncInternal(string key)
{
    …
}
```

### <a name="taskwhenall"></a>Task.WhenAll

 <xref:System.Threading.Tasks.Task.WhenAll%2A> 方法可用于异步等待多个表示为任务的异步操作。  该方法所具有的多个重载支持一组非泛型任务或一组不统一的常规任务（如异步等待多个返回 void 的操作，或异步等待多个返回值的方法，其中每个值可能具有不同类型），并支持一组统一的常规任务（如异步等待多个 `TResult` 返回方法）。

 假设你想要向多个客户发送电子邮件。 你可以重叠发送邮件，因此发送邮件时无需等待上一封邮件完成发送。 还可以查看发送操作完成的时间，以及是否发生了错误：

```csharp
IEnumerable<Task> asyncOps = from addr in addrs select SendMailAsync(addr);
await Task.WhenAll(asyncOps);
```

 此代码不显式处理可能发生的异常，而是通过对 <xref:System.Threading.Tasks.Task.WhenAll%2A> 生成的任务执行 `await` 传播异常。  若要处理该异常，可以使用以下代码：

```csharp
IEnumerable<Task> asyncOps = from addr in addrs select SendMailAsync(addr);
try
{
    await Task.WhenAll(asyncOps);
}
catch(Exception exc)
{
    ...
}
```

 在这种情况下，如果任意异步操作失败，所有异常都会合并到 <xref:System.AggregateException> 异常中，此异常存储在 <xref:System.Threading.Tasks.Task.WhenAll%2A> 方法返回的 <xref:System.Threading.Tasks.Task> 中。  但是，仅通过 `await` 关键字传播其中一个异常。  如果想要检查所有异常，可以重写前面的代码，如下所示：

```csharp
Task [] asyncOps = (from addr in addrs select SendMailAsync(addr)).ToArray();
try
{
    await Task.WhenAll(asyncOps);
}
catch(Exception exc)
{
    foreach(Task faulted in asyncOps.Where(t => t.IsFaulted))
    {
        … // work with faulted and faulted.Exception
    }
}
```

 让我们考虑一下以异步方式从 Web 下载多个文件的示例。  在此示例中，所有异步操作具有相同的结果类型，并很容易对结果进行访问：

```csharp
string [] pages = await Task.WhenAll(
    from url in urls select DownloadStringAsync(url));
```

 可以使用上一个返回 void 方案中所讨论的异常处理技术：

```csharp
Task<string> [] asyncOps =
    (from url in urls select DownloadStringAsync(url)).ToArray();
try
{
    string [] pages = await Task.WhenAll(asyncOps);
    ...
}
catch(Exception exc)
{
    foreach(Task<string> faulted in asyncOps.Where(t => t.IsFaulted))
    {
        … // work with faulted and faulted.Exception
    }
}
```

### <a name="taskwhenany"></a>Task.WhenAny

 <xref:System.Threading.Tasks.Task.WhenAny%2A> 方法可用于异步等待多个表示为要完成的任务的异步操作之一。  此方法适用于四个主要用例：

- 冗余：多次执行一个操作并选择最先完成的一次（例如，联系将生成一个结果的多个股市行情 Web 服务并选择最快完成的一个）。

- 交错：启动多个操作并等待所有这些操作完成，但在这些操作完成时对其进行处理。

- 限制：允许在其他操作完成时开始附加操作。  这是交错方案的扩展。

- 早期释放：例如，用任务 t1 表示的操作可以与任务 t2 组成 <xref:System.Threading.Tasks.Task.WhenAny%2A> 任务，并且可以等待 <xref:System.Threading.Tasks.Task.WhenAny%2A> 任务。 任务 t2 可以表示超时、取消或其他一些导致 <xref:System.Threading.Tasks.Task.WhenAny%2A> 任务先于 t1 完成的信号。

#### <a name="redundancy"></a>冗余

 假设你想要决定是否购买股票。  你信任一些股票建议 Web 服务，但每个服务最终会在不同的时间段变得很慢，具体取决于每日负载。  <xref:System.Threading.Tasks.Task.WhenAny%2A> 方法可用于在任何操作完成时接收通知：

```csharp
var recommendations = new List<Task<bool>>()
{
    GetBuyRecommendation1Async(symbol),
    GetBuyRecommendation2Async(symbol),
    GetBuyRecommendation3Async(symbol)
};
Task<bool> recommendation = await Task.WhenAny(recommendations);
if (await recommendation) BuyStock(symbol);
```

 <xref:System.Threading.Tasks.Task.WhenAll%2A> 返回已成功完成的所有任务的取消包装结果。与它不同，<xref:System.Threading.Tasks.Task.WhenAny%2A> 返回已完成的任意任务。 如果任务失败，重要的是知道该任务失败，如果任务成功，重要的是知道返回值与哪个任务相关联。  因此，你需要访问返回任务的结果，或进一步等待，如本示例中所示。

 与 <xref:System.Threading.Tasks.Task.WhenAll%2A> 一样，必须能够容纳异常。  因为接收到完成的任务后，可以等待返回的任务传播错误，并适当地进行 `try/catch`，例如：

```csharp
Task<bool> [] recommendations = …;
while(recommendations.Count > 0)
{
    Task<bool> recommendation = await Task.WhenAny(recommendations);
    try
    {
        if (await recommendation) BuyStock(symbol);
        break;
    }
    catch(WebException exc)
    {
        recommendations.Remove(recommendation);
    }
}
```

 此外，即使第一个任务成功完成，后续任务也可能会失败。  此时，有多个用于处理异常的选项：可以等待所有启动的任务完成，在这种情况下可以使用 <xref:System.Threading.Tasks.Task.WhenAll%2A> 方法；或者做出所有异常都重要且必须记录的决定。  为此，可以使用延续任务以在任务异步完成时接收通知：

```csharp
foreach(Task recommendation in recommendations)
{
    var ignored = recommendation.ContinueWith(
        t => { if (t.IsFaulted) Log(t.Exception); });
}
```

 或：

```csharp
foreach(Task recommendation in recommendations)
{
    var ignored = recommendation.ContinueWith(
        t => Log(t.Exception), TaskContinuationOptions.OnlyOnFaulted);
}
```

 或者甚至：

```csharp
private static async void LogCompletionIfFailed(IEnumerable<Task> tasks)
{
    foreach(var task in tasks)
    {
        try { await task; }
        catch(Exception exc) { Log(exc); }
    }
}
…
LogCompletionIfFailed(recommendations);
```

 最后，若要取消所有剩余操作：

```csharp
var cts = new CancellationTokenSource();
var recommendations = new List<Task<bool>>()
{
    GetBuyRecommendation1Async(symbol, cts.Token),
    GetBuyRecommendation2Async(symbol, cts.Token),
    GetBuyRecommendation3Async(symbol, cts.Token)
};

Task<bool> recommendation = await Task.WhenAny(recommendations);
cts.Cancel();
if (await recommendation) BuyStock(symbol);
```

#### <a name="interleaving"></a>交错

 假设你要从 Web 下载图像，并且处理每个图像（例如，将图像添加到 UI 控件）。 可以在 UI 线程上按顺序处理图像，但建议尽可能同时下载图像。 此外，建议不要直到所有图像都下载完成才将图像添加到 UI。 建议在完成下载时添加它们。

```csharp
List<Task<Bitmap>> imageTasks =
    (from imageUrl in urls select GetBitmapAsync(imageUrl)).ToList();
while(imageTasks.Count > 0)
{
    try
    {
        Task<Bitmap> imageTask = await Task.WhenAny(imageTasks);
        imageTasks.Remove(imageTask);

        Bitmap image = await imageTask;
        panel.AddImage(image);
    }
    catch{}
}
```

 还可以将交错应用于涉及下载图像 <xref:System.Threading.ThreadPool> 的计算密集型处理的方案；例如：

```csharp
List<Task<Bitmap>> imageTasks =
    (from imageUrl in urls select GetBitmapAsync(imageUrl)
         .ContinueWith(t => ConvertImage(t.Result)).ToList();
while(imageTasks.Count > 0)
{
    try
    {
        Task<Bitmap> imageTask = await Task.WhenAny(imageTasks);
        imageTasks.Remove(imageTask);

        Bitmap image = await imageTask;
        panel.AddImage(image);
    }
    catch{}
}
```

#### <a name="throttling"></a>遏制

 请考虑交错示例，因用户大量下载图像而导致下载必须受到遏制除外；例如，你希望仅能同时下载特定数目的内容。 为此，可以启动异步操作的子集。  操作完成后，你可以启动其他操作对其进行替代：

```csharp
const int CONCURRENCY_LEVEL = 15;
Uri [] urls = …;
int nextIndex = 0;
var imageTasks = new List<Task<Bitmap>>();
while(nextIndex < CONCURRENCY_LEVEL && nextIndex < urls.Length)
{
    imageTasks.Add(GetBitmapAsync(urls[nextIndex]));
    nextIndex++;
}

while(imageTasks.Count > 0)
{
    try
    {
        Task<Bitmap> imageTask = await Task.WhenAny(imageTasks);
        imageTasks.Remove(imageTask);

        Bitmap image = await imageTask;
        panel.AddImage(image);
    }
    catch(Exception exc) { Log(exc); }

    if (nextIndex < urls.Length)
    {
        imageTasks.Add(GetBitmapAsync(urls[nextIndex]));
        nextIndex++;
    }
}
```

#### <a name="early-bailout"></a>早期释放

 假设正在异步等待某个操作完成的同时，对用户的取消请求（例如，用户单击取消按钮）进行响应。 以下代码阐释了此方案：

```csharp
private CancellationTokenSource m_cts;

public void btnCancel_Click(object sender, EventArgs e)
{
    if (m_cts != null) m_cts.Cancel();
}

public async void btnRun_Click(object sender, EventArgs e)
{
    m_cts = new CancellationTokenSource();
    btnRun.Enabled = false;
    try
    {
        Task<Bitmap> imageDownload = GetBitmapAsync(txtUrl.Text);
        await UntilCompletionOrCancellation(imageDownload, m_cts.Token);
        if (imageDownload.IsCompleted)
        {
            Bitmap image = await imageDownload;
            panel.AddImage(image);
        }
        else imageDownload.ContinueWith(t => Log(t));
    }
    finally { btnRun.Enabled = true; }
}

private static async Task UntilCompletionOrCancellation(
    Task asyncOp, CancellationToken ct)
{
    var tcs = new TaskCompletionSource<bool>();
    using(ct.Register(() => tcs.TrySetResult(true)))
        await Task.WhenAny(asyncOp, tcs.Task);
    return asyncOp;
}
```

 一旦决定退出，此实现将重新启用用户界面，但不会取消基础异步操作。 另一种选择是决定退出时，取消挂起的操作，但在操作完成之前不重新建立用户界面，可能会由于取消请求而提前结束：

```csharp
private CancellationTokenSource m_cts;

public async void btnRun_Click(object sender, EventArgs e)
{
    m_cts = new CancellationTokenSource();

    btnRun.Enabled = false;
    try
    {
        Task<Bitmap> imageDownload = GetBitmapAsync(txtUrl.Text, m_cts.Token);
        await UntilCompletionOrCancellation(imageDownload, m_cts.Token);
        Bitmap image = await imageDownload;
        panel.AddImage(image);
    }
    catch(OperationCanceledException) {}
    finally { btnRun.Enabled = true; }
}
```

 另一个早期释放示例涉及结合使用 <xref:System.Threading.Tasks.Task.WhenAny%2A> 方法和 <xref:System.Threading.Tasks.Task.Delay%2A> 方法，下一部分将对此进行介绍。

### <a name="taskdelay"></a>Task.Delay

 <xref:System.Threading.Tasks.Task.Delay%2A?displayProperty=nameWithType> 方法可用于将暂停引入异步方法的执行中。  这对于许多功能都非常有用，包括构建轮询循环和延迟预定时间段的用户输入处理。  <xref:System.Threading.Tasks.Task.Delay%2A?displayProperty=nameWithType> 方法还可以与 <xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType> 结合使用，以对 await 实现超时。

 如果某任务属于较大型异步操作（如 ASP.NET Web 服务）中的一部分，由于花费时间过长而不能完成，则整体操作可能会受到影响（尤其是此任务一直不能完成的情况下）。  因此，等待异步操作时可以超时非常重要。  虽然同步 <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType>、<xref:System.Threading.Tasks.Task.WaitAll%2A?displayProperty=nameWithType> 和 <xref:System.Threading.Tasks.Task.WaitAny%2A?displayProperty=nameWithType> 方法接受超时值，但相应的 <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAll%2A?displayProperty=nameWithType>/<xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType> 和前述 <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType>/<xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType> 方法不接受。  相反，可以将 <xref:System.Threading.Tasks.Task.Delay%2A?displayProperty=nameWithType> 与 <xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType>结合使用，以实现超时。

 例如，在 UI 应用程序中，假设你想要下载图像，并在图像下载期间禁用该 UI。 但是，如果下载时间过长，你希望重新启用 UI 并放弃下载：

```csharp
public async void btnDownload_Click(object sender, EventArgs e)
{
    btnDownload.Enabled = false;
    try
    {
        Task<Bitmap> download = GetBitmapAsync(url);
        if (download == await Task.WhenAny(download, Task.Delay(3000)))
        {
            Bitmap bmp = await download;
            pictureBox.Image = bmp;
            status.Text = "Downloaded";
        }
        else
        {
            pictureBox.Image = null;
            status.Text = "Timed out";
            var ignored = download.ContinueWith(
                t => Trace("Task finally completed"));
        }
    }
    finally { btnDownload.Enabled = true; }
}
```

 这同样适用于多个下载，因为 <xref:System.Threading.Tasks.Task.WhenAll%2A> 返回任务：

```csharp
public async void btnDownload_Click(object sender, RoutedEventArgs e)
{
    btnDownload.Enabled = false;
    try
    {
        Task<Bitmap[]> downloads =
            Task.WhenAll(from url in urls select GetBitmapAsync(url));
        if (downloads == await Task.WhenAny(downloads, Task.Delay(3000)))
        {
            foreach(var bmp in downloads.Result) panel.AddImage(bmp);
            status.Text = "Downloaded";
        }
        else
        {
            status.Text = "Timed out";
            downloads.ContinueWith(t => Log(t));
        }
    }
    finally { btnDownload.Enabled = true; }
}
```

## <a name="building-task-based-combinators"></a>构建基于任务的连结符

 因为任务可以完全代表异步操作、提供同步和异步功能来加入操作、检索其结果等，所以可以构建组成任务的连结符的库以构建更大的模式。 如前一部分所述，.NET 包括一些内置连结符，但是，你也可以构建自己的连结符。 以下各节提供了一些潜在的连结符方法和类型的示例。

### <a name="retryonfault"></a>RetryOnFault

 在许多情况下，如果上次尝试失败，你可能想要重试操作。  对于同步代码，你可能会构建一个帮助器方法来实现此目的，如下例中的 `RetryOnFault`：

```csharp
public static T RetryOnFault<T>(
    Func<T> function, int maxTries)
{
    for(int i=0; i<maxTries; i++)
    {
        try { return function(); }
        catch { if (i == maxTries-1) throw; }
    }
    return default(T);
}
```

 你可以为异步操作（使用 TAP 实现，因此返回任务）构建几乎相同的帮助器方法：

```csharp
public static async Task<T> RetryOnFault<T>(
    Func<Task<T>> function, int maxTries)
{
    for(int i=0; i<maxTries; i++)
    {
        try { return await function().ConfigureAwait(false); }
        catch { if (i == maxTries-1) throw; }
    }
    return default(T);
}
```

 然后，可以使用此连结符将重试编码到应用程序的逻辑中，例如：

```csharp
// Download the URL, trying up to three times in case of failure
string pageContents = await RetryOnFault(
    () => DownloadStringAsync(url), 3);
```

 可以进一步扩展 `RetryOnFault` 函数。 例如，该函数可以接受另一个 `Func<Task>`（在重试间隔期间调用以确定何时重试该操作）：

```csharp
public static async Task<T> RetryOnFault<T>(
    Func<Task<T>> function, int maxTries, Func<Task> retryWhen)
{
    for(int i=0; i<maxTries; i++)
    {
        try { return await function().ConfigureAwait(false); }
        catch { if (i == maxTries-1) throw; }
        await retryWhen().ConfigureAwait(false);
    }
    return default(T);
}
```

 重试该操作前，可以使用以下函数等待片刻：

```csharp
// Download the URL, trying up to three times in case of failure,
// and delaying for a second between retries
string pageContents = await RetryOnFault(
    () => DownloadStringAsync(url), 3, () => Task.Delay(1000));
```

### <a name="needonlyone"></a>NeedOnlyOne

 有时，你可以利用冗余改进操作延迟和提高成功的可能性。  假设有多个 Web 服务提供股票报价，但在一天中的不同时间，每个服务可能提供不同级别的质量和响应时间。  为了应对这些波动，你可能会向所有 Web 服务发出请求，并且只要从其中一个获得响应，立刻取消剩余的请求。  你可以通过 helper 函数更轻松地实现此启动多个操作的通用模式：等待任何操作，然后取消其余部分。 以下示例中的 `NeedOnlyOne` 函数阐释了这种方案：

```csharp
public static async Task<T> NeedOnlyOne(
    params Func<CancellationToken,Task<T>> [] functions)
{
    var cts = new CancellationTokenSource();
    var tasks = (from function in functions
                 select function(cts.Token)).ToArray();
    var completed = await Task.WhenAny(tasks).ConfigureAwait(false);
    cts.Cancel();
    foreach(var task in tasks)
    {
        var ignored = task.ContinueWith(
            t => Log(t), TaskContinuationOptions.OnlyOnFaulted);
    }
    return completed;
}
```

 然后，你可以使用此函数，如下所示：

```csharp
double currentPrice = await NeedOnlyOne(
    ct => GetCurrentPriceFromServer1Async("msft", ct),
    ct => GetCurrentPriceFromServer2Async("msft", ct),
    ct => GetCurrentPriceFromServer3Async("msft", ct));
```

### <a name="interleaved-operations"></a>交错操作

 处理大型任务集时，如果使用 <xref:System.Threading.Tasks.Task.WhenAny%2A> 方法支持交错方案，可能存在潜在性能问题。 每次调用 <xref:System.Threading.Tasks.Task.WhenAny%2A> 都会向每个任务注册延续。 对于 N 个任务，这将导致在交错操作的操作期间创建 O(N<sup>2</sup>) 次延续。 如果处理大型任务集，则可以使用连结符（以下示例中的 `Interleaved`）来解决性能问题：

```csharp
static IEnumerable<Task<T>> Interleaved<T>(IEnumerable<Task<T>> tasks)
{
    var inputTasks = tasks.ToList();
    var sources = (from _ in Enumerable.Range(0, inputTasks.Count)
                   select new TaskCompletionSource<T>()).ToList();
    int nextTaskIndex = -1;
    foreach (var inputTask in inputTasks)
    {
        inputTask.ContinueWith(completed =>
        {
            var source = sources[Interlocked.Increment(ref nextTaskIndex)];
            if (completed.IsFaulted)
                source.TrySetException(completed.Exception.InnerExceptions);
            else if (completed.IsCanceled)
                source.TrySetCanceled();
            else
                source.TrySetResult(completed.Result);
        }, CancellationToken.None,
           TaskContinuationOptions.ExecuteSynchronously,
           TaskScheduler.Default);
    }
    return from source in sources
           select source.Task;
}
```

 然后，可以在任务完成时，使用连结符来处理任务的结果，例如：

```csharp
IEnumerable<Task<int>> tasks = ...;
foreach(var task in Interleaved(tasks))
{
    int result = await task;
    …
}
```

### <a name="whenallorfirstexception"></a>WhenAllOrFirstException

 在特定的分散/集中情况下，你可能想要等待集中的所有任务，除非某个任务发生错误。在这种情况下，你希望在异常发生时停止等待。  你可以通过使用连结符方法（如 `WhenAllOrFirstException`）实现该目的，如下所示：

```csharp
public static Task<T[]> WhenAllOrFirstException<T>(IEnumerable<Task<T>> tasks)
{
    var inputs = tasks.ToList();
    var ce = new CountdownEvent(inputs.Count);
    var tcs = new TaskCompletionSource<T[]>();

    Action<Task> onCompleted = (Task completed) =>
    {
        if (completed.IsFaulted)
            tcs.TrySetException(completed.Exception.InnerExceptions);
        if (ce.Signal() && !tcs.Task.IsCompleted)
            tcs.TrySetResult(inputs.Select(t => t.Result).ToArray());
    };

    foreach (var t in inputs) t.ContinueWith(onCompleted);
    return tcs.Task;
}
```

## <a name="building-task-based-data-structures"></a>构建基于任务的数据结构

 除了能够生成基于任务的自定义组合器，在 <xref:System.Threading.Tasks.Task> 和 <xref:System.Threading.Tasks.Task%601>（表示异步操作结果和联接所必需的同步操作结果）中包含数据结构，还可以使其成为功能非常强大的类型，基于该类型可生成在异步方案中使用的自定义数据结构。

### <a name="asynccache"></a>AsyncCache

 任务的重要方面之一是，它可能会分发到多个使用者，所有使用者都可以等待任务、向任务注册延续、获取任务结果或异常（如果是 <xref:System.Threading.Tasks.Task%601> 的话）等。  这样一来，<xref:System.Threading.Tasks.Task> 和 <xref:System.Threading.Tasks.Task%601> 就非常适用于异步缓存基础结构。  下面的示例演示了基于 <xref:System.Threading.Tasks.Task%601> 生成的功能非常强大的小型异步缓存：

```csharp
public class AsyncCache<TKey, TValue>
{
    private readonly Func<TKey, Task<TValue>> _valueFactory;
    private readonly ConcurrentDictionary<TKey, Lazy<Task<TValue>>> _map;

    public AsyncCache(Func<TKey, Task<TValue>> valueFactory)
    {
        if (valueFactory == null) throw new ArgumentNullException("loader");
        _valueFactory = valueFactory;
        _map = new ConcurrentDictionary<TKey, Lazy<Task<TValue>>>();
    }

    public Task<TValue> this[TKey key]
    {
        get
        {
            if (key == null) throw new ArgumentNullException("key");
            return _map.GetOrAdd(key, toAdd =>
                new Lazy<Task<TValue>>(() => _valueFactory(toAdd))).Value;
        }
    }
}
```

 [AsyncCache\<TKey,TValue>](https://devblogs.microsoft.com/pfxteam/parallelextensionsextras-tour-12-asynccache/) 类接受需要使用 `TKey` 且返回 <xref:System.Threading.Tasks.Task%601> 的函数作为构造函数的委托。  以前从缓存访问的所有值都存储在内部字典中，`AsyncCache` 可以确保每个密钥仅生成一个任务，即便同时访问缓存也是如此。

 例如，你可以生成下载网页的缓存：

```csharp
private AsyncCache<string,string> m_webPages =
    new AsyncCache<string,string>(DownloadStringAsync);
```

 然后可以在任何需要网页内容的时候，以异步方式使用此缓存。 `AsyncCache` 类可确保下载尽可能少的页面，并缓存结果。

```csharp
private async void btnDownload_Click(object sender, RoutedEventArgs e)
{
    btnDownload.IsEnabled = false;
    try
    {
        txtContents.Text = await m_webPages["https://www.microsoft.com"];
    }
    finally { btnDownload.IsEnabled = true; }
}
```

### <a name="asyncproducerconsumercollection"></a>AsyncProducerConsumerCollection

 你还可以使用任务来构建协调异步活动的数据结构。  请考虑经典的并行设计模式之一：制造者/使用者。  在此模式下，制造者生成数据，使用者使用数据，制造者和使用者可能会并行运行。 例如，使用者处理之前由制造者生成的第 1 项，而制造者现在正在制造第 2 项。  对于制造者/使用者模式，总是需要某种数据结构来存储制造者创建的工作，以便使用者可以收到新数据的通知并及时发现新数据。

 以下是基于任务构建的简单数据结构，可以将异步方法用作生成方和使用方：

```csharp
public class AsyncProducerConsumerCollection<T>
{
    private readonly Queue<T> m_collection = new Queue<T>();
    private readonly Queue<TaskCompletionSource<T>> m_waiting =
        new Queue<TaskCompletionSource<T>>();

    public void Add(T item)
    {
        TaskCompletionSource<T> tcs = null;
        lock (m_collection)
        {
            if (m_waiting.Count > 0) tcs = m_waiting.Dequeue();
            else m_collection.Enqueue(item);
        }
        if (tcs != null) tcs.TrySetResult(item);
    }

    public Task<T> Take()
    {
        lock (m_collection)
        {
            if (m_collection.Count > 0)
            {
                return Task.FromResult(m_collection.Dequeue());
            }
            else
            {
                var tcs = new TaskCompletionSource<T>();
                m_waiting.Enqueue(tcs);
                return tcs.Task;
            }
        }
    }
}
```

 通过该数据结构，可以编写如下所示的代码：

```csharp
private static AsyncProducerConsumerCollection<int> m_data = …;
…
private static async Task ConsumerAsync()
{
    while(true)
    {
        int nextItem = await m_data.Take();
        ProcessNextItem(nextItem);
    }
}
…
private static void Produce(int data)
{
    m_data.Add(data);
}
```

<xref:System.Threading.Tasks.Dataflow> 命名空间包括 <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> 类型，可以类似方式使用它，但无需生成自定义集合类型：

```csharp
private static BufferBlock<int> m_data = …;
…
private static async Task ConsumerAsync()
{
    while(true)
    {
        int nextItem = await m_data.ReceiveAsync();
        ProcessNextItem(nextItem);
    }
}
…
private static void Produce(int data)
{
    m_data.Post(data);
}
```

> [!NOTE]
> <xref:System.Threading.Tasks.Dataflow> 命名空间作为 NuGet 包提供。 若要安装包含 <xref:System.Threading.Tasks.Dataflow> 命名空间的程序集，请在 Visual Studio 中打开项目，选择“项目”菜单中的“管理 NuGet 包”，再在线搜索 `System.Threading.Tasks.Dataflow` 包。

## <a name="see-also"></a>请参阅

- [基于任务的异步模式 (TAP)](task-based-asynchronous-pattern-tap.md)
- [实现基于任务的异步模式](implementing-the-task-based-asynchronous-pattern.md)
- [与其他异步模式和类型互操作](interop-with-other-asynchronous-patterns-and-types.md)
