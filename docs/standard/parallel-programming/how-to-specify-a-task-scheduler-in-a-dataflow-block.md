---
title: 如何：在数据流块中指定任务计划程序
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- TPL dataflow library, linking to task scheduler in TPL
- Task Parallel Library, dataflows
- task scheduler, linking from TPL
ms.assetid: 27ece374-ed5b-49ef-9cec-b20db34a65e8
ms.openlocfilehash: b8c27c1ca61356b36183bb74b8360e41f5324d25
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95722435"
---
# <a name="how-to-specify-a-task-scheduler-in-a-dataflow-block"></a>如何：在数据流块中指定任务计划程序

本文档演示在应用程序中使用数据流时如何关联特定任务计划程序。 示例在 Windows 窗体应用程序中使用 <xref:System.Threading.Tasks.ConcurrentExclusiveSchedulerPair?displayProperty=nameWithType> 类来显示读取器任务处于活动状态的时间和编写器任务处于活动状态的时间。 它还使用 <xref:System.Threading.Tasks.TaskScheduler.FromCurrentSynchronizationContext%2A?displayProperty=nameWithType> 方法使数据流块能够在用户界面线程上运行。

[!INCLUDE [tpl-install-instructions](../../../includes/tpl-install-instructions.md)]

## <a name="to-create-the-windows-forms-application"></a>创建 Windows 窗体应用程序  
  
1. 创建一个 Visual C# 或 Visual Basic“Windows 窗体应用程序”项目  。 在以下步骤中，该项目命名为 `WriterReadersWinForms`。  
  
2. 在主窗体的窗体设计器中，Form1.cs（对于 Visual Basic 则为 Form1.vb）添加了四个 <xref:System.Windows.Forms.CheckBox> 控件。 将 `checkBox1`、`checkBox2`、`checkBox3`、`checkBox4` 的 <xref:System.Windows.Forms.Control.Text%2A> 属性分别设置为“读取器 1”、“读取器 2”、“读取器 3”和“编写器”。 将每个控件的 <xref:System.Windows.Forms.Control.Enabled%2A> 属性设置为 `False`。  
  
3. 在窗体上添加一个 <xref:System.Windows.Forms.Timer> 控件。 将 <xref:System.Windows.Forms.Timer.Interval%2A> 属性设置为 `2500`。  
  
## <a name="adding-dataflow-functionality"></a>添加数据流功能  

 本节介绍如何创建参与应用程序的数据流块以及如何将每个数据流块与任务计划程序关联。  
  
### <a name="to-add-dataflow-functionality-to-the-application"></a>在应用程序中添加数据流功能  
  
1. 在项目中，添加对 System.Threading.Tasks.Dataflow.dll 的引用。  
  
2. 确保 Form1.cs（对于 Visual Basic 则为 Form1.vb）包含以下 `using` 语句（Visual Basic 中为 `Imports`）。  
  
     [!code-csharp[TPLDataflow_WriterReadersWinForms#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/cs/writerreaderswinforms/form1.cs#1)]
     [!code-vb[TPLDataflow_WriterReadersWinForms#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/vb/writerreaderswinforms/form1.vb#1)]  
  
3. 将 <xref:System.Threading.Tasks.Dataflow.BroadcastBlock%601> 数据成员添加到 `Form1` 类。  
  
     [!code-csharp[TPLDataflow_WriterReadersWinForms#2](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/cs/writerreaderswinforms/form1.cs#2)]
     [!code-vb[TPLDataflow_WriterReadersWinForms#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/vb/writerreaderswinforms/form1.vb#2)]  
  
4. 在 `Form1` 构造函数中，在调用 `InitializeComponent` 后，创建一个 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> 对象，该对象将切换 <xref:System.Windows.Forms.CheckBox> 对象的状态。  
  
     [!code-csharp[TPLDataflow_WriterReadersWinForms#3](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/cs/writerreaderswinforms/form1.cs#3)]
     [!code-vb[TPLDataflow_WriterReadersWinForms#3](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/vb/writerreaderswinforms/form1.vb#3)]  
  
5. 在 `Form1` 构造函数中，创建一个 <xref:System.Threading.Tasks.ConcurrentExclusiveSchedulerPair> 对象和四个 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> 对象，每个 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> 对象对应一个 <xref:System.Windows.Forms.CheckBox> 对象。 对每个 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> 对象，指定一个 <xref:System.Threading.Tasks.Dataflow.ExecutionDataflowBlockOptions> 对象，该对象将读取器的 <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.TaskScheduler%2A> 属性设置为 <xref:System.Threading.Tasks.ConcurrentExclusiveSchedulerPair.ConcurrentScheduler%2A> 属性，将编写器的设置为 <xref:System.Threading.Tasks.ConcurrentExclusiveSchedulerPair.ExclusiveScheduler%2A> 属性。  
  
     [!code-csharp[TPLDataflow_WriterReadersWinForms#4](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/cs/writerreaderswinforms/form1.cs#4)]
     [!code-vb[TPLDataflow_WriterReadersWinForms#4](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/vb/writerreaderswinforms/form1.vb#4)]  
  
6. 在 `Form1` 构造函数中，启动 <xref:System.Windows.Forms.Timer> 对象。  
  
     [!code-csharp[TPLDataflow_WriterReadersWinForms#5](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/cs/writerreaderswinforms/form1.cs#5)]
     [!code-vb[TPLDataflow_WriterReadersWinForms#5](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/vb/writerreaderswinforms/form1.vb#5)]  
  
7. 在主窗体的窗体设计器中，为计时器的 <xref:System.Windows.Forms.Timer.Tick> 事件创建事件处理程序。  
  
8. 实现计时器的 <xref:System.Windows.Forms.Timer.Tick> 事件。  
  
     [!code-csharp[TPLDataflow_WriterReadersWinForms#6](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/cs/writerreaderswinforms/form1.cs#6)]
     [!code-vb[TPLDataflow_WriterReadersWinForms#6](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/vb/writerreaderswinforms/form1.vb#6)]  
  
 因为 `toggleCheckBox` 数据流块是在用户界面上操作，所以此操作要在用户界面线程上执行，这一点很重要。 为此，在构造期间，此对象提供一个将 <xref:System.Threading.Tasks.Dataflow.ExecutionDataflowBlockOptions> 属性设置为 <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.TaskScheduler%2A> 的 <xref:System.Threading.Tasks.TaskScheduler.FromCurrentSynchronizationContext%2A?displayProperty=nameWithType> 对象。 <xref:System.Threading.Tasks.TaskScheduler.FromCurrentSynchronizationContext%2A> 方法会创建一个在当前同步上下文中执行工作的 <xref:System.Threading.Tasks.TaskScheduler> 对象。 因为 `Form1` 构造函数是从用户界面线程中调用的，所以 `toggleCheckBox` 数据流块的操作也会在用户线程中运行。  
  
 对于在同一 <xref:System.Threading.Tasks.ConcurrentExclusiveSchedulerPair> 对象上运行的所有其他数据流块，此示例还使用了 <xref:System.Threading.Tasks.ConcurrentExclusiveSchedulerPair> 类使某些数据流块能够并发操作，而使其他数据流块能够单独执行。 当多个数据流块共享资源，但有些需要对该资源独占访问时，这种技术很有用，因为它不需要手动同步对该资源的访问。 手动同步的消除会使代码更高效。  
  
## <a name="example"></a>示例  

 下面的示例演示 Form1.cs（对于 Visual Basic 则为 Form1.vb）的完整代码。  
  
 [!code-csharp[TPLDataflow_WriterReadersWinForms#100](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/cs/writerreaderswinforms/form1.cs#100)]
 [!code-vb[TPLDataflow_WriterReadersWinForms#100](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_writerreaderswinforms/vb/writerreaderswinforms/form1.vb#100)]  
  
## <a name="see-also"></a>另请参阅

- [数据流](dataflow-task-parallel-library.md)
