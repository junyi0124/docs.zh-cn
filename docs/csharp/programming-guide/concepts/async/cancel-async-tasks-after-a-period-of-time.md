---
title: 在一段时间后取消异步任务 (C#)
description: 了解如何计划取消未在一段时间内完成的任何关联任务。
ms.date: 08/19/2020
ms.topic: tutorial
ms.assetid: 194282c2-399f-46da-a7a6-96674e00b0b3
ms.openlocfilehash: ad9064f8f45a737982ffc35ab4ea2395ddae9016
ms.sourcegitcommit: 9c45035b781caebc63ec8ecf912dc83fb6723b1f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2020
ms.locfileid: "88811413"
---
# <a name="cancel-async-tasks-after-a-period-of-time-c"></a>在一段时间后取消异步任务 (C#)

如果不希望等待操作结束，可使用 <xref:System.Threading.CancellationTokenSource.CancelAfter%2A?displayProperty=nameWithType> 方法在一段时间后取消异步操作。 此方法会计划取消未在 `CancelAfter` 表达式指定的时间段内完成的任何关联任务。

此示例添加到[取消任务列表 (C#)](cancel-an-async-task-or-a-list-of-tasks.md) 中开发的代码，以下载网站列表并显示每个网站的内容长度。

本教程涉及：

> [!div class="checklist"]
>
> - 更新现有的 .NET 控制台应用程序
> - 计划取消

## <a name="prerequisites"></a>必备条件

本教程需要的内容如下：

- 在[取消任务列表 (C#)](cancel-an-async-task-or-a-list-of-tasks.md) 教程中已创建应用程序
- [.NET 5.0 或更高版本的 SDK](https://dotnet.microsoft.com/download/dotnet/5.0)
- 集成开发环境 (IDE)
  - [建议使用 Visual Studio、Visual Studio Code 或 Visual Studio for Mac](https://visualstudio.microsoft.com)

## <a name="update-application-entry-point"></a>更新应用程序入口点

将现有的 `Main` 方法替换为以下内容：

```csharp
static async Task Main()
{
    Console.WriteLine("Application started.");

    try
    {
        s_cts.CancelAfter(3500);

        await SumPageSizesAsync();
    }
    catch (TaskCanceledException)
    {
        Console.WriteLine("\nTasks cancelled: timed out.\n");
    }

    Console.WriteLine("Application ending.");
}
```

更新后的 `Main` 方法将一些说明性消息写入控制台。 在[尝试捕获](../../../language-reference/keywords/try-catch.md)内，调用 <xref:System.Threading.CancellationTokenSource.CancelAfter(System.Int32)?displayProperty=nameWithType> 以计划取消。 一段时间后将发出取消信号。

接下来，等待 `SumPageSizesAsync` 方法。 如果处理所有 URL 的速度比计划取消的速度快，应用程序将结束。 但如果在处理所有 URL 之前触发了计划取消，则会引发 <xref:System.Threading.Tasks.TaskCanceledException>。

### <a name="example-application-output"></a>示例应用程序输出

```console
Application started.

https://docs.microsoft.com                                       37,357
https://docs.microsoft.com/aspnet/core                           85,589
https://docs.microsoft.com/azure                                398,939
https://docs.microsoft.com/azure/devops                          73,663

Tasks cancelled: timed out.

Application ending.
```

## <a name="complete-example"></a>完整示例

下列代码是示例的 Program.cs 文件的完整文本。

:::code language="csharp" source="snippets/cancel-tasks/cancel-task-after-period-of-time/Program.cs":::

## <a name="see-also"></a>另请参阅

- <xref:System.Threading.CancellationToken>
- <xref:System.Threading.CancellationTokenSource>
- [使用 Async 和 Await 的异步编程 (C#)](index.md)
- [取消任务列表 (C#)](cancel-an-async-task-or-a-list-of-tasks.md)
