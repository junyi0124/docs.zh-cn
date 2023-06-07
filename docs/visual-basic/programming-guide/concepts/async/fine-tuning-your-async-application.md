---
title: 微调异步应用程序
ms.date: 07/20/2015
ms.assetid: 4c3e7997-a95f-4fbe-a6ac-60ba042d30b9
ms.openlocfilehash: 6ad4f9a526e0497029ff8ddc3e93637a4f9acb00
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91075429"
---
# <a name="fine-tuning-your-async-application-visual-basic"></a>微调异步应用程序 (Visual Basic)

可以使用由 <xref:System.Threading.Tasks.Task> 类型提供的方法和属性将精度和灵活性添加到异步应用程序。 本部分中的主题介绍使用 <xref:System.Threading.CancellationToken> 的示例和一些重要的 `Task` 方法，例如 <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> 和 <xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType>。  
  
 使用 `WhenAny` 和 `WhenAll` 可以更轻松地启动多个任务并通过监视单个任务待其完成。  
  
- 集合中的任何任务完成时，`WhenAny` 将返回完成的任务。  
  
     有关使用的示例 `WhenAny` ，请参阅  [在完成一个异步任务后取消操作 (Visual Basic) ](cancel-remaining-async-tasks-after-one-is-complete.md)并 [启动多个异步任务并在它们完成 (Visual Basic) 时进行处理 ](start-multiple-async-tasks-and-process-them-as-they-complete.md)。  
  
- 集合中的所有任务完成时，`WhenAll` 将返回完成的任务。  
  
     有关使用的详细信息和示例 `WhenAll` ，请参阅 [如何：使用 system.threading.tasks.task.whenall (Visual Basic) 扩展 Async 演练 ](how-to-extend-the-async-walkthrough-by-using-task-whenall.md)。  
  
 本部分包括下列示例。  
  
- [取消异步任务或任务列表 (Visual Basic) ](cancel-an-async-task-or-a-list-of-tasks.md)。  
  
- [在一段时间后取消异步任务 (Visual Basic)](cancel-async-tasks-after-a-period-of-time.md)  
  
- [在完成一个异步任务后取消剩余任务 (Visual Basic)](cancel-remaining-async-tasks-after-one-is-complete.md)  
  
- [启动多个异步任务并在其完成时进行处理 (Visual Basic)](start-multiple-async-tasks-and-process-them-as-they-complete.md)  
  
> [!NOTE]
> 若要运行该示例，计算机上必须安装有 Visual Studio 2012 或更高版本和 .NET Framework 4.5 或更高版本。  
  
 项目将创建一个 UI，其中包含用于启动进程和取消进程的按钮，如下图所示。 这些按钮名为 `startButton` 和 `cancelButton`。  
  
 ![WPF 窗口与“取消”按钮](./media/fine-tuning-your-async-application/cancellation-and-start-button.png "带有“开始”和“停止”按钮的对话框")  
  
 要下载完整的 Windows Presentation Foundation (WPF) 项目，请参阅 [Async Sample:Fine Tuning Your Application](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea)（异步示例：微调应用程序）。  
  
## <a name="see-also"></a>请参阅

- [使用 Async 和 Await 的异步编程 (Visual Basic)](index.md)
