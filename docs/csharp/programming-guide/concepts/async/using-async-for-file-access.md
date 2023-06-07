---
title: 异步文件访问 (C#)
description: 了解如何使用异步功能访问 C# 文件。 可以调用异步方法而无需使用回调，也不需要跨方法拆分代码。
ms.date: 08/19/2020
ms.assetid: bb018fea-5313-4c80-ab3f-7c24b2145bd9
ms.openlocfilehash: 9a8db6004e8fff2cb39f0c350b403b56ea619e54
ms.sourcegitcommit: 9c45035b781caebc63ec8ecf912dc83fb6723b1f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2020
ms.locfileid: "88812037"
---
# <a name="asynchronous-file-access-c"></a>异步文件访问 (C#)

可使用异步功能访问文件。 通过使用异步功能，你可以调用异步方法而无需使用回调，也不需要跨多个方法或 lambda 表达式来拆分代码。 若要使同步代码异步，只需调用异步方法而非同步方法，并向代码中添加几个关键字。

可能出于以下原因向文件访问调用中添加异步：

> [!div class="checklist"]
>
> - 异步使 UI 应用程序响应速度更快，因为启动该操作的 UI 线程可以执行其他操作。 如果 UI 线程必须执行耗时较长的代码（例如超过 50 毫秒），UI 可能会冻结，直到 I/O 完成，此时 UI 线程可以再次处理键盘和鼠标输入及其他事件。
> - 异步可减少对线程的需要，进而提高 ASP.NET 和其他基于服务器的应用程序的可伸缩性。 如果应用程序对每次响应都使用专用线程，同时处理 1000 个请求时，则需要 1000 个线程。 异步操作在等待期间通常不需要使用线程。 异步操作仅需在结束时短暂使用现有 I/O 完成线程。
> - 当前条件下，文件访问操作的延迟可能非常低，但以后可能大幅增加。 例如，文件可能会移动到覆盖全球的服务器。
> - 使用异步功能所增加的开销很小。
> - 异步任务可以轻松地并行运行。

## <a name="use-appropriate-classes"></a>使用适当的类

本主题中的简单示例演示 <xref:System.IO.File.WriteAllTextAsync%2A?displayProperty=nameWithType> 和 <xref:System.IO.File.ReadAllTextAsync%2A?displayProperty=nameWithType>。 要对文件 I/O 操作进行有限的控制，请使用 <xref:System.IO.FileStream> 类，该类有一个可导致在操作系统级别出现异步 I/O 的选项。 使用此选项可避免在许多情况下阻止线程池线程。 若要启用此选项，可在构造函数调用中指定 `useAsync=true` 或 `options=FileOptions.Asynchronous` 参数。

如果通过指定文件路径直接打开 <xref:System.IO.StreamReader> 和 <xref:System.IO.StreamWriter>，则无法将此选项与这二者配合使用。 但是，如果为二者提供已由 <xref:System.IO.FileStream> 类打开的 <xref:System.IO.Stream>，则可以使用此选项。 即使线程池线程受到阻止，UI 应用中的异步调用也会更快，因为 UI 线程在等待期间不会受到阻止。

## <a name="write-text"></a>写入文本

下面的示例将文本写入文件。 在每个 await 语句中，该方法会立即退出。 文件 I/O 完成后，该方法将在 await 语句后面的语句中继续。 Async 修饰符位于使用 await 语句的方法定义中。

### <a name="simple-example"></a>简单示例

:::code language="csharp" source="snippets/file-access/Program.cs" id="SimpleWrite":::

### <a name="finite-control-example"></a>有限控制示例

:::code language="csharp" source="snippets/file-access/Program.cs" id="WriteText":::

原始示例包含 `await sourceStream.WriteAsync(encodedText, 0, encodedText.Length);` 语句，它是下面两个语句的缩写式：

```csharp
Task theTask = sourceStream.WriteAsync(encodedText, 0, encodedText.Length);
await theTask;
```

第一条语句返回任务，并会导致文件处理启动。 具有 await 的第二条语句将使方法立即退出并返回一个不同的任务。 文件处理稍后完成后，执行将返回到 await 后面的语句中。

## <a name="read-text"></a>读取文本

下面的示例从文件中读取文本。

### <a name="simple-example"></a>简单示例

:::code language="csharp" source="snippets/file-access/Program.cs" id="SimpleRead":::

### <a name="finite-control-example"></a>有限控制示例

将会缓冲文本，并且在此情况下，会将其放入 <xref:System.Text.StringBuilder>。 与前一示例不同，await 的计算将生成一个值。 <xref:System.IO.Stream.ReadAsync%2A> 方法返回 <xref:System.Threading.Tasks.Task>\<<xref:System.Int32>>，因此在操作完成后 await 的评估会得出 `Int32` 值 `numRead`。 有关详细信息，请参阅[异步返回类型 (C#)](async-return-types.md)。

:::code language="csharp" source="snippets/file-access/Program.cs" id="ReadText":::

## <a name="parallel-asynchronous-io"></a>并行异步 I/O

下面的示例通过编写 10 个文本文件来演示并行处理。

### <a name="simple-example"></a>简单示例

:::code language="csharp" source="snippets/file-access/Program.cs" id="SimpleParallelWrite":::

### <a name="finite-control-example"></a>有限控制示例

对于每个文件，<xref:System.IO.Stream.WriteAsync%2A> 方法将返回一个任务，此任务随后将添加到任务列表中。 `await Task.WhenAll(tasks);` 语句将退出该方法，并在所有任务的文件处理完成时在此方法中继续。

该示例将在任务完成后关闭 `finally` 块中的所有 <xref:System.IO.FileStream> 实例。 如果每个 `FileStream` 均已在 `using` 语句中创建，则可能在任务完成前释放 `FileStream`。

任何性能提升都几乎完全来自并行处理而不是异步处理。 异步的优点在于它不会占用多个线程，也不会占用用户界面线程。

:::code language="csharp" source="snippets/file-access/Program.cs" id="ParallelWriteText":::

当使用 <xref:System.IO.Stream.WriteAsync%2A> 和 <xref:System.IO.Stream.ReadAsync%2A> 方法时，可以指定可用于取消操作中间流的 <xref:System.Threading.CancellationToken>。 有关详细信息，请参阅[托管线程中的取消](../../../../standard/threading/cancellation-in-managed-threads.md)。

## <a name="see-also"></a>另请参阅

- [使用 Async 和 Await 的异步编程 (C#)](index.md)
- [异步返回类型 (C#)](async-return-types.md)
