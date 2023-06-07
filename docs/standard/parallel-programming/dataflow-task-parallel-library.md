---
title: 数据流（任务并行库）
description: 了解如何使用任务并行库 (TPL) 中的数据流组件来提高启用并发的应用程序的可靠性。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Task Parallel Library, dataflows
- TPL dataflow library
ms.assetid: 643575d0-d26d-4c35-8de7-a9c403e97dd6
ms.openlocfilehash: 67b5cfaa987b3f37d68597d22ad5f87e8d1bd018
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95728688"
---
# <a name="dataflow-task-parallel-library"></a>数据流（任务并行库）

任务并行库 (TPL) 提供数据流组件，可帮助提高启用并发的应用程序的可靠性。 这些数据流组件统称为 TPL 数据流库。 这种数据流模型通过向粗粒度的数据流和管道任务提供进程内消息传递来促进基于角色的编程。 数据流组件基于 TPL 的类型和计划基础结构，并集成了 C#、Visual Basic 和 F# 语言的异步编程支持。 当您有必须相互异步沟通的多个操作或者想要在数据可用时对其处理时，这些数据流组件就非常有用。 例如，请考虑一个处理网络摄像机图像数据的应用程序。 通过使用数据流模型，当图像帧可用时，应用程序就可以处理它们。 如果应用程序增强图像帧（例如执行灯光修正或消除红眼），则可以创建数据流组件的管道。 管道的每个阶段可以使用更粗粒度的并行功能（例如 TPL 提供的功能）来转换图像。  
  
 本文档对 TPL 数据流库进行了概述。 它介绍编程模型，预定义的数据流块类型，以及如何配置数据流块来满足应用程序的特定要求。  

[!INCLUDE [tpl-install-instructions](../../../includes/tpl-install-instructions.md)]

## <a name="programming-model"></a>编程模型

 TPL 数据流库向具有高吞吐量和低滞后时间的占用大量 CPU 和 I/O 操作的应用程序的并行化和消息传递提供了基础。 它还能显式控制缓存数据的方式以及在系统中移动的方式。 为了更好地了解数据流编程模型，请考虑一个以异步方式从磁盘加载图像并创建复合图像的应用程序。 传统编程模型通常需要使用回调和同步对象（例如锁）来协调任务和访问共享数据。 通过使用数据流编程模型，您可以从磁盘读取时创建处理图像的数据流对象。 在数据流模型下，您可以声明当数据可用时的处理方式，以及数据之间的所有依赖项。 由于运行时管理数据之间的依赖项，因此通常可以避免这种要求来同步访问共享数据。 此外，因为运行时计划基于数据的异步到达，所以数据流可以通过有效管理基础线程提高响应能力和吞吐量。 有关在 Windows 窗体应用程序中使用数据流编程模型实现图像处理的示例，请参阅[演练：在 Windows 窗体应用程序中使用数据流](walkthrough-using-dataflow-in-a-windows-forms-application.md)。  
  
### <a name="sources-and-targets"></a>源和目标  

 TPL 数据流库包括 *数据流块*，它是缓冲并处理数据的数据结构。 TPL 定义了三种数据流块：源块、目标块和传播器块。 源块作为数据源，可以读取。 目标块作为数据接收方，可以写入。 传播器块作为源块和目标块，可以读取和写入。 TPL 定义 <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601?displayProperty=nameWithType> 接口来表示源，<xref:System.Threading.Tasks.Dataflow.ITargetBlock%601?displayProperty=nameWithType> 表示目标以及 <xref:System.Threading.Tasks.Dataflow.IPropagatorBlock%602?displayProperty=nameWithType> 表示传播器。 <xref:System.Threading.Tasks.Dataflow.IPropagatorBlock%602> 继承自 <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601> 和 <xref:System.Threading.Tasks.Dataflow.ITargetBlock%601>。  
  
 TPL 数据流库提供了多个预定义的数据流块类型，可以实现 <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601>、<xref:System.Threading.Tasks.Dataflow.ITargetBlock%601> 和 <xref:System.Threading.Tasks.Dataflow.IPropagatorBlock%602> 接口。 这些数据流块类型在本文档的[预定义的数据流块类型](#predefined-dataflow-block-types)部分进行了说明。  
  
### <a name="connecting-blocks"></a>连接块  

 可以连接数据流块来形成管道（这是数据流块的线性序列），或网络（这是数据流块的图形）。 管道是网络的一种形式。 在管道或网络中，当数据可用时源向目标异步传播数据。 <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601.LinkTo%2A?displayProperty=nameWithType> 方法将源数据流块链接到目标块。 源可以链接到零个或多个目标；目标可以从零个或多个源进行链接。 您可以同时向管道或网络中添加或从其移除数据流块。 预定义的数据流块类型处理所有的建立或释放链接的线程安全性。  
  
 有关连接数据流块以形成基本管道的示例，请参阅[演练：创建数据流管道](walkthrough-creating-a-dataflow-pipeline.md)。 有关连接数据流块以形成更复杂网络的示例，请参阅[演练：在 Windows 窗体应用程序中使用数据流](walkthrough-using-dataflow-in-a-windows-forms-application.md)。 有关在源向目标传递消息后从源取消目标链接的示例，请参阅[如何：取消链接数据流块](how-to-unlink-dataflow-blocks.md)。  
  
#### <a name="filtering"></a>筛选  

 当您调用 <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601.LinkTo%2A?displayProperty=nameWithType> 方法将源链接到目标时，您可以根据消息的值提供一个委托来决定目标块是接受还是拒绝该消息。 这种筛选机制很有用，它可以保证数据流块只接收特定值。 对于大多数预定义的数据流块类型，如果源块连接到多个目标块，那么当目标块拒绝消息时，源将向下一个目标提供该消息。 源向目标提供消息的顺序是按源定义的，可以根据源类型的不同而不同。 一个目标接受消息后，大多数源块类型会停止提供该消息。 此规则的例外情况是 <xref:System.Threading.Tasks.Dataflow.BroadcastBlock%601> 类，这个类向所有目标提供每条消息，即使某些目标拒绝消息。 有关使用筛选功能来仅处理特定消息的示例，请参阅[演练：在 Windows 窗体应用程序中使用数据流](walkthrough-using-dataflow-in-a-windows-forms-application.md)。  
  
> [!IMPORTANT]
> 由于每个预定义源数据流块类型确保了消息是按照它们接收的顺序来传播的，因此每一条消息都必须在源块可以处理下一条消息之前从源块读取。 因此，当您使用筛选向一个源连接多个目标时，请确保至少一个目标块能够接收每一条消息。 否则，您的应用程序可能发生死锁。  
  
### <a name="message-passing"></a>消息传递  

 数据流编程模型与 *消息传递* 这一概念相关，其中程序的独立组件通过发送消息相互通信。 在应用组件间传播消息的一种方法是，调用 <xref:System.Threading.Tasks.Dataflow.DataflowBlock.Post%2A> 和 <xref:System.Threading.Tasks.Dataflow.DataflowBlock.SendAsync%2A?displayProperty=nameWithType> 方法，向目标数据流块发送消息（<xref:System.Threading.Tasks.Dataflow.DataflowBlock.Post%2A> 同步运行，<xref:System.Threading.Tasks.Dataflow.DataflowBlock.SendAsync%2A> 异步运行），再调用 <xref:System.Threading.Tasks.Dataflow.DataflowBlock.Receive%2A>、<xref:System.Threading.Tasks.Dataflow.DataflowBlock.ReceiveAsync%2A> 和 <xref:System.Threading.Tasks.Dataflow.DataflowBlock.TryReceive%2A> 方法接收源数据流块发送的消息。 您可以通过向头节点（目标块）发送输入数据，从管道的终端节点或网络的终端节点（一个或多个源块）接收输出数据来使用数据流管道或网络组合使用这些方法。 您还可以使用 <xref:System.Threading.Tasks.Dataflow.DataflowBlock.Choose%2A> 方法从提供的第一个拥有可用数据的源读取数据，并对该数据执行操作。  
  
 源数据流块通过调用方法 <xref:System.Threading.Tasks.Dataflow.ITargetBlock%601.OfferMessage%2A?displayProperty=nameWithType> 向目标数据流块提供数据。 目标块通过以下三种方式之一来回应提供的消息：它可以接受消息，拒绝消息或推迟消息。 当目标接受消息时，<xref:System.Threading.Tasks.Dataflow.ITargetBlock%601.OfferMessage%2A> 方法会返回 <xref:System.Threading.Tasks.Dataflow.DataflowMessageStatus.Accepted>。 当目标拒绝消息时，<xref:System.Threading.Tasks.Dataflow.ITargetBlock%601.OfferMessage%2A> 方法会返回 <xref:System.Threading.Tasks.Dataflow.DataflowMessageStatus.Declined>。 当目标要求它不再接收来自源的任何消息时，<xref:System.Threading.Tasks.Dataflow.ITargetBlock%601.OfferMessage%2A> 会返回 <xref:System.Threading.Tasks.Dataflow.DataflowMessageStatus.DecliningPermanently>。 预定义的源块类型在这些返回值接收后不会向链接的目标提供消息，并且它们会自动取消这些目标的链接。  
  
 当目标块推迟消息以备日后使用时，<xref:System.Threading.Tasks.Dataflow.ITargetBlock%601.OfferMessage%2A> 方法会返回 <xref:System.Threading.Tasks.Dataflow.DataflowMessageStatus.Postponed>。 推迟消息的目标块可以稍后调用 <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601.ReserveMessage%2A?displayProperty=nameWithType> 方法，以尝试暂留所提供的消息。 此时，消息仍可用，并且可由该目标块使用，否则表明该消息已由另一个目标接收。 如果目标数据流块稍后需要消息或不再需要消息，它会分别调用 <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601.ConsumeMessage%2A?displayProperty=nameWithType> 或 <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601.ReleaseReservation%2A> 方法。 消息预留通常由以非贪婪模式运行的数据流块类型使用。 非贪婪模式将在本文档的后面详细介绍。 除了保留推迟的消息，目标块也可以使用 <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601.ConsumeMessage%2A?displayProperty=nameWithType> 方法来尝试直接使用推迟的消息。  
  
### <a name="dataflow-block-completion"></a>数据流块完成  

 数据流块也支持完成概念。 完成状态的数据流块不执行任何进一步的工作。 每个数据流块都有相关的 <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> 对象（称为“完成任务”），表示数据流块的完成状态。 因为您可以使用完成任务等待 <xref:System.Threading.Tasks.Task> 对象完成，所以您可以等待数据流网络的一个或更多终端节点来完成任务。 <xref:System.Threading.Tasks.Dataflow.IDataflowBlock> 接口定义 <xref:System.Threading.Tasks.Dataflow.IDataflowBlock.Complete%2A> 方法（该方法向数据流块通知它完成的请求）和 <xref:System.Threading.Tasks.Dataflow.IDataflowBlock.Completion%2A> 属性（该属性返回数据流块的完成任务）。 <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601> 和 <xref:System.Threading.Tasks.Dataflow.ITargetBlock%601> 都继承 <xref:System.Threading.Tasks.Dataflow.IDataflowBlock> 接口。  
  
 有两种方法来确定数据流块完成时是否没有出错、遇到一个或多个错误或已取消。 第一种方法是在 `try`-`catch` 块（在 Visual Basic 中为 `Try`-`Catch`）中对完成任务调用 <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType> 方法。 下面的示例创建一个 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> 对象，该对象在其输入值小于零时会引发 <xref:System.ArgumentOutOfRangeException>。 当此示例在完成任务后调用 <xref:System.AggregateException> 时，将引发 <xref:System.Threading.Tasks.Task.Wait%2A>。 通过 <xref:System.ArgumentOutOfRangeException> 对象的 <xref:System.AggregateException.InnerExceptions%2A> 属性来访问 <xref:System.AggregateException>。  
  
 [!code-csharp[TPLDataflow_Overview#10](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_overview/cs/program.cs#10)]
 [!code-vb[TPLDataflow_Overview#10](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_overview/vb/program.vb#10)]  
  
 此示例演示在执行数据流块的委托中异常变成不可处理的情况。 建议您在这样的块主体中处理异常。 然而，你如果没能这么做，块就表现得好像是它被取消了，而且不会处理传入消息。  
  
 当显式取消数据流块时，<xref:System.AggregateException> 对象在 <xref:System.OperationCanceledException> 属性中包含 <xref:System.AggregateException.InnerExceptions%2A>。 有关数据流取消的详细信息，请参阅[启用取消](#enabling-cancellation)部分。  
  
 第二种确定数据流块的完成状态的方法是使用延续执行完成任务，或者使用 C# 和 Visual Basic 的异步语言功能以异步方式等待完成任务。 您提供给 <xref:System.Threading.Tasks.Task.ContinueWith%2A?displayProperty=nameWithType> 方法的委托采用表示前面任务的 <xref:System.Threading.Tasks.Task> 对象。 就 <xref:System.Threading.Tasks.Dataflow.IDataflowBlock.Completion%2A> 属性来说，延续的委托自行采用完成任务。 下面的示例与前一个示例相似，不同之处在于它也使用 <xref:System.Threading.Tasks.Task.ContinueWith%2A> 方法创建输出整个数据流操作状态的延续任务。  
  
 [!code-csharp[TPLDataflow_Overview#11](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_overview/cs/program.cs#11)]
 [!code-vb[TPLDataflow_Overview#11](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_overview/vb/program.vb#11)]  
  
 您也可以使用类似延续任务主体中的属性（例如 <xref:System.Threading.Tasks.Task.IsCanceled%2A>）来确定有关数据流块的完成状态的其他信息。 若要深入了解延续任务及其与取消和错误处理如何相关，请参阅[使用延续任务链接任务](chaining-tasks-by-using-continuation-tasks.md)、[任务取消](task-cancellation.md)和[异常处理](exception-handling-task-parallel-library.md)。  

## <a name="predefined-dataflow-block-types"></a>预定义的数据流块类型  

 TPL 数据流库提供了多个预定义的数据流块类型。 这些类型分为三个类别：缓冲块、执行块和分组块。 以下部分描述了组成这些类别的块类型。  
  
### <a name="buffering-blocks"></a>缓冲块  

 缓冲块存放的数据供数据使用者使用。 TPL 数据流库提供三种缓冲块类型：<xref:System.Threading.Tasks.Dataflow.BufferBlock%601?displayProperty=nameWithType>、<xref:System.Threading.Tasks.Dataflow.BroadcastBlock%601?displayProperty=nameWithType> 和 <xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601?displayProperty=nameWithType>。  
  
#### <a name="bufferblockt"></a>BufferBlock(T)  

 <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> 类表示一般用途的异步消息结构。 此类存储先进先出 (FIFO) 消息队列，此消息队列可由多个源写入或从多个目标读取。 在目标收到来自 <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> 对象的消息时，将从消息队列中删除此消息。 因此，虽然一个 <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> 对象可以具有多个目标，但只有一个目标将接收每条消息。 需将多条消息传递给另一个组件，且该组件必须接收每条消息时，<xref:System.Threading.Tasks.Dataflow.BufferBlock%601> 类十分有用。  
  
 下面的基本示例将多个 <xref:System.Int32> 值发送到 <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> 对象，然后从该对象读回这些值。  
  
 [!code-csharp[TPLDataflow_Overview#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_overview/cs/program.cs#1)]
 [!code-vb[TPLDataflow_Overview#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_overview/vb/program.vb#1)]  
  
 有关演示如何将消息写入到 <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> 对象并从该对象读取消息的完整示例，请参阅[如何：将消息写入数据流块和从数据流块读取消息](how-to-write-messages-to-and-read-messages-from-a-dataflow-block.md)。  
  
#### <a name="broadcastblockt"></a>BroadcastBlock(T)  

 若您必须将多条消息传递给另一个组件，而该组件只需要最新的值，则 <xref:System.Threading.Tasks.Dataflow.BroadcastBlock%601> 类很有用。 需向多个组件广播消息时，此类也很有用。  
  
 下面的基本示例将 <xref:System.Double> 值发送给 <xref:System.Threading.Tasks.Dataflow.BroadcastBlock%601> 对象，然后多次从该对象读回该值。 由于值在被读取之后不会从 <xref:System.Threading.Tasks.Dataflow.BroadcastBlock%601> 对象中移除，因此每一次的可用值都相同。  
  
 [!code-csharp[TPLDataflow_Overview#2](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_overview/cs/program.cs#2)]
 [!code-vb[TPLDataflow_Overview#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_overview/vb/program.vb#2)]  
  
 有关演示如何使用 <xref:System.Threading.Tasks.Dataflow.BroadcastBlock%601> 来将一条消息广播给多个目标块的完整示例，请参阅[如何：在数据流块中指定任务计划程序](how-to-specify-a-task-scheduler-in-a-dataflow-block.md)。  
  
#### <a name="writeonceblockt"></a>WriteOnceBlock(T)  

 <xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601> 类与 <xref:System.Threading.Tasks.Dataflow.BroadcastBlock%601> 类相似，不同之处在于 <xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601> 对象仅可被写入一次。 可以将 <xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601> 视作类似于 C# 中的 [readonly](../../csharp/language-reference/keywords/readonly.md)（Visual Basic 中的 [ReadOnly](../../visual-basic/language-reference/modifiers/readonly.md)）关键字，不同之处在于 <xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601> 对象在收到值后（而不是在构造时）成为不可变对象。 与 <xref:System.Threading.Tasks.Dataflow.BroadcastBlock%601> 类相似，在目标收到来自 <xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601> 对象的消息时，不会从该目标删除此消息。 因此，多个目标将接收到该消息的副本。 当您想要仅传播多条消息中的第一条时，<xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601> 类很有用。  
  
 下面的基本示例将多个 <xref:System.String> 值发送给 <xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601> 对象，然后从该对象读回该值。 由于 <xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601> 对象在 <xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601> 对象接收消息后只能写入一次，因此它放弃后续消息。  
  
 [!code-csharp[TPLDataflow_Overview#3](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_overview/cs/program.cs#3)]
 [!code-vb[TPLDataflow_Overview#3](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_overview/vb/program.vb#3)]  
  
 有关演示如何使用 <xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601> 接收完成的第一个操作值的完整示例，请参阅[如何：取消链接数据流块](how-to-unlink-dataflow-blocks.md)。  
  
### <a name="execution-blocks"></a>执行块  

 执行块为每条接收数据调用用户提供的委托。 TPL 数据流库提供三种执行块类型：<xref:System.Threading.Tasks.Dataflow.ActionBlock%601>、<xref:System.Threading.Tasks.Dataflow.TransformBlock%602?displayProperty=nameWithType> 和 <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602?displayProperty=nameWithType>。  
  
#### <a name="actionblockt"></a>ActionBlock(T)  

 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> 类在接收数据时是调用委托的目标块。 将 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> 对象视为数据可用时异步运行的委托。 您提供给 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> 对象的委托可以是类型 <xref:System.Action%601> 或类型 `System.Func<TInput, Task>`。 当通过 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> 使用 <xref:System.Action%601> 对象时，每个输入元素的处理在委托返回时视为已完成。 当您通过 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> 使用 `System.Func<TInput, Task>` 对象时，只有当返回的 <xref:System.Threading.Tasks.Task> 对象完成时，每个输入元素的处理才可以视为已完成。 使用这两种机制，您可使用 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> 同步和异步处理每个输入元素。  
  
 下面的基本示例将多个 <xref:System.Int32> 值发送给 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> 对象。 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> 对象将这些值输出到控制台中。 然后此示例将该块设置为已完成状态，并等待所有数据流任务完成。  
  
 [!code-csharp[TPLDataflow_Overview#4](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_overview/cs/program.cs#4)]
 [!code-vb[TPLDataflow_Overview#4](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_overview/vb/program.vb#4)]  
  
 有关演示如何在 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> 类中使用委托的完整示例，请参阅[如何：在数据流块收到数据时执行操作](how-to-perform-action-when-a-dataflow-block-receives-data.md)。  
  
#### <a name="transformblocktinput-toutput"></a>TransformBlock(TInput, TOutput)  

 <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> 类与 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> 类相似，不同之处在于它可以同时充当源和目标。 传递给 <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> 对象的委托返回类型为 `TOutput` 的值。 您提供给 <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> 对象的委托可以是类型 `System.Func<TInput, TOutput>` 或类型 `System.Func<TInput, Task<TOutput>>`。 当您搭配使用 <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> 和 `System.Func<TInput, TOutput>` 对象时，每个输入元素的处理在委托返回时视为已完成。 当您搭配使用 <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> 和 `System.Func<TInput, Task<TOutput>>` 对象时，只有当返回的 <xref:System.Threading.Tasks.Task%601> 对象完成时，每个输入元素的处理才可以视为已完成。 像 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> 一样，通过使用这两种机制，您可使用 <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> 同步和异步处理每个输入元素。  
  
 下面的基本示例所创建的 <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> 对象用于计算输入的平方根。 <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> 对象采用 <xref:System.Int32> 值作为输入并生成 <xref:System.Double> 值作为输出。  
  
 [!code-csharp[TPLDataflow_Overview#5](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_overview/cs/program.cs#5)]
 [!code-vb[TPLDataflow_Overview#5](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_overview/vb/program.vb#5)]  
  
 有关在数据流块网络（用于在 Windows 窗体应用程序中执行图像处理）中使用 <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> 的完整示例，请参阅[演练：在 Windows 窗体应用程序中使用数据流](walkthrough-using-dataflow-in-a-windows-forms-application.md)。  
  
#### <a name="transformmanyblocktinput-toutput"></a>TransformManyBlock(TInput, TOutput)  

 <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602> 类与 <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> 类相似，不同之处在于 <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602> 为每一个输入值生成零个或多个输出值，而不是为每个输入值仅生成一个输出值。 您提供给 <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602> 对象的委托可以是类型 `System.Func<TInput, IEnumerable<TOutput>>` 或类型 `System.Func<TInput, Task<IEnumerable<TOutput>>>`。 当您搭配使用 <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602> 和 `System.Func<TInput, IEnumerable<TOutput>>` 对象时，每个输入元素的处理在委托返回时视为已完成。 当您搭配使用 <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602> 和 `System.Func<TInput, Task<IEnumerable<TOutput>>>` 对象时，只有当返回的 `System.Threading.Tasks.Task<IEnumerable<TOutput>>` 对象完成时，每个输入元素的处理才可以视为已完成。  
  
 下面的基本示例所创建的 <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602> 对象将字符串拆分为单个字符序列。 <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602> 对象采用 <xref:System.String> 值作为输入并生成 <xref:System.Char> 值作为输出。  
  
 [!code-csharp[TPLDataflow_Overview#6](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_overview/cs/program.cs#6)]
 [!code-vb[TPLDataflow_Overview#6](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_overview/vb/program.vb#6)]  
  
 有关使用 <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602> 在一个数据流管道中为每一个输入生成多个独立输出的完整示例，请参阅[演练：创建数据流管道](walkthrough-creating-a-dataflow-pipeline.md)。  
  
#### <a name="degree-of-parallelism"></a>并行度  

 每个 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601>、<xref:System.Threading.Tasks.Dataflow.TransformBlock%602> 和 <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602> 对象都缓冲输入消息，直到块准备处理它们。 默认情况下，这些类以接收消息的顺序处理消息，一次处理一条消息。 您还可以指定并行度，使 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601>、<xref:System.Threading.Tasks.Dataflow.TransformBlock%602> 和 <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602> 对象同时处理多条消息。 有关并行执行的详细信息，请参阅本文档后面的“指定并行度”部分。 有关设置并行度使执行数据流块能够一次处理多条消息的示例，请参阅[如何：指定数据流块中的并行度](how-to-specify-the-degree-of-parallelism-in-a-dataflow-block.md)。  
  
#### <a name="summary-of-delegate-types"></a>委托类型摘要  

 下表汇总了可提供给 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601>、<xref:System.Threading.Tasks.Dataflow.TransformBlock%602> 和 <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602> 对象的委托类型。 此表还指出委托类型是同步执行还是异步执行。  
  
|类型|同步委托类型|异步委托类型|  
|----------|-------------------------------|--------------------------------|  
|<xref:System.Threading.Tasks.Dataflow.ActionBlock%601>|`System.Action`|`System.Func<TInput, Task>`|  
|<xref:System.Threading.Tasks.Dataflow.TransformBlock%602>|`System.Func<TInput, TOutput>`|`System.Func<TInput, Task<TOutput>>`|  
|<xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602>|`System.Func<TInput, IEnumerable<TOutput>>`|`System.Func<TInput, Task<IEnumerable<TOutput>>>`|  
  
 当处理执行块类型时，还可以使用 lambda 表达式。 有关演示如何使用 lambda 表达式处理执行块的示例，请参阅[如何：在数据流块收到数据时执行操作](how-to-perform-action-when-a-dataflow-block-receives-data.md)。  
  
### <a name="grouping-blocks"></a>分组块  

 分组块在各种约束下合并一个或多个源的数据。 TPL 数据流库提供三种联接块类型：<xref:System.Threading.Tasks.Dataflow.BatchBlock%601>、<xref:System.Threading.Tasks.Dataflow.JoinBlock%602> 和 <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602>。  
  
#### <a name="batchblockt"></a>BatchBlock(T)  

 <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> 类将一系列输入数据合并到输出数据数组，即批处理。 在创建 <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> 对象时，指定每个批的大小。 当 <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> 对象接收指定数量的输入元素时，它会异步传播含这些元素的数组。 如果 <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> 对象设置为已完成状态，但不包含足够的元素形成批，则会传播包含其余输入元素的最终数组。  
  
 <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> 类可以在贪婪或非贪婪模式下运行。 在默认贪婪模式下，<xref:System.Threading.Tasks.Dataflow.BatchBlock%601> 对象接受它提供的每条消息，并在接收指定数量的元素后传播数组。 在非贪婪模式下，<xref:System.Threading.Tasks.Dataflow.BatchBlock%601> 对象推迟所有传入的消息，直到足够的源给块提供消息来形成批。 贪婪模式处理开销较少，所以通常比非贪婪模式执行得更有效。 但是，当您必须以基本方式协调来自多个源的消耗时，可以使用非贪婪模式。 在 <xref:System.Threading.Tasks.Dataflow.GroupingDataflowBlockOptions.Greedy%2A> 构造函数的 `False` 参数中，通过将 `dataflowBlockOptions` 设置为 <xref:System.Threading.Tasks.Dataflow.BatchBlock%601.%23ctor%2A> 来指定非贪婪模式。  
  
 下面的基本示例将多个 <xref:System.Int32> 值发送给一批中能容纳十个元素的 <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> 对象。 为了确保所有值从 <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> 传播，此示例调用 <xref:System.Threading.Tasks.Dataflow.IDataflowBlock.Complete%2A> 方法。 <xref:System.Threading.Tasks.Dataflow.IDataflowBlock.Complete%2A> 方法将 <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> 对象设置为已完成状态，因此，<xref:System.Threading.Tasks.Dataflow.BatchBlock%601> 对象作为最终批传播剩余的所有元素。  
  
 [!code-csharp[TPLDataflow_Overview#7](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_overview/cs/program.cs#7)]
 [!code-vb[TPLDataflow_Overview#7](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_overview/vb/program.vb#7)]  
  
 有关使用 <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> 改进数据库插入操作效率的完整示例，请参阅[演练：使用 BatchBlock 和 BatchedJoinBlock 提高效率](walkthrough-using-batchblock-and-batchedjoinblock-to-improve-efficiency.md)。  
  
#### <a name="joinblockt1-t2-"></a>JoinBlock(T1, T2, ...)  

 <xref:System.Threading.Tasks.Dataflow.JoinBlock%602> 和 <xref:System.Threading.Tasks.Dataflow.JoinBlock%603> 类收集输入元素并传播包含这些元素的 <xref:System.Tuple%602?displayProperty=nameWithType> 或 <xref:System.Tuple%603?displayProperty=nameWithType> 对象。 <xref:System.Threading.Tasks.Dataflow.JoinBlock%602> 和 <xref:System.Threading.Tasks.Dataflow.JoinBlock%603> 类不能从 <xref:System.Threading.Tasks.Dataflow.ITargetBlock%601> 继承。 而是提供属性 <xref:System.Threading.Tasks.Dataflow.JoinBlock%602.Target1%2A>、<xref:System.Threading.Tasks.Dataflow.JoinBlock%602.Target2%2A> 和 <xref:System.Threading.Tasks.Dataflow.JoinBlock%603.Target3%2A> 来实现 <xref:System.Threading.Tasks.Dataflow.ITargetBlock%601>。  
  
 像在贪婪或非贪婪模式下运行的 <xref:System.Threading.Tasks.Dataflow.BatchBlock%601>、<xref:System.Threading.Tasks.Dataflow.JoinBlock%602> 和 <xref:System.Threading.Tasks.Dataflow.JoinBlock%603> 一样。 在默认贪婪模式下，<xref:System.Threading.Tasks.Dataflow.JoinBlock%602> 或 <xref:System.Threading.Tasks.Dataflow.JoinBlock%603> 对象在其每个目标接收至少一条消息之后接受提供的每条消息并传播元组。 在非贪婪模式下，<xref:System.Threading.Tasks.Dataflow.JoinBlock%602> 或 <xref:System.Threading.Tasks.Dataflow.JoinBlock%603> 对象推迟所有传入的消息，直到向任何目标提供了创建元组所需的数据。 此时，块参与两阶段提交协议，以原子方式从源中检索所有必需的项。 此延迟使得其他实体可以同时使用数据，这使整个系统取得进展。  
  
 下面的基本示例演示 <xref:System.Threading.Tasks.Dataflow.JoinBlock%603> 对象需要多个数据来计算值的情况。 此示例创建了需要两个 <xref:System.Threading.Tasks.Dataflow.JoinBlock%603> 值和一个 <xref:System.Int32> 值来执行算术运算的 <xref:System.Char> 对象。  
  
 [!code-csharp[TPLDataflow_Overview#8](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_overview/cs/program.cs#8)]
 [!code-vb[TPLDataflow_Overview#8](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_overview/vb/program.vb#8)]  
  
 有关在非贪婪模式下使用 <xref:System.Threading.Tasks.Dataflow.JoinBlock%602> 对象合作共享资源的完整示例，请参阅[如何：使用 JoinBlock 从多个源读取数据](how-to-use-joinblock-to-read-data-from-multiple-sources.md)。  
  
#### <a name="batchedjoinblockt1-t2-"></a>BatchedJoinBlock(T1, T2, ...)  

 <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602> 和 <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%603> 类收集各批输入元素，并传播包含这些元素的 `System.Tuple(IList(T1), IList(T2))` 或 `System.Tuple(IList(T1), IList(T2), IList(T3))` 对象。 将 <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602> 视为 <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> 和 <xref:System.Threading.Tasks.Dataflow.JoinBlock%602> 的组合。 在创建 <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602> 对象时，指定每个批的大小。 <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602> 还提供了属性 <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602.Target1%2A> 和 <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602.Target2%2A> 来实现 <xref:System.Threading.Tasks.Dataflow.ITargetBlock%601>。 当从所有目标收到指定数量的输入元素时，<xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602> 对象会异步传播包含这些元素的 `System.Tuple(IList(T1), IList(T2))` 对象。  
  
 下面的基本示例创建了一个包含结果、<xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602> 值和 <xref:System.Int32> 对象错误的 <xref:System.Exception> 对象。 此示例执行多个操作，然后将结果写入 <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602.Target1%2A> 属性，将错误写入 <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602.Target2%2A> 对象的 <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602> 属性。 由于成功和失败操作的计数事先是未知的，因此 <xref:System.Collections.Generic.IList%601> 对象使每个目标都能收到零个或多个值。  
  
 [!code-csharp[TPLDataflow_Overview#9](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_overview/cs/program.cs#9)]
 [!code-vb[TPLDataflow_Overview#9](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_overview/vb/program.vb#9)]  
  
 有关使用 <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602> 捕获程序从数据库中读取数据时发生的结果和任何异常的完整示例，请参阅[演练：使用 BatchBlock 和 BatchedJoinBlock 提高效率](walkthrough-using-batchblock-and-batchedjoinblock-to-improve-efficiency.md)。  

## <a name="configuring-dataflow--block-behavior"></a>配置数据流块行为  

 可以通过给数据流块类型的构造函数提供 <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions?displayProperty=nameWithType> 对象来启用其他选项。 这些选项控制这类管理基础任务和并行度的调度程序的行为。 <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions> 还包含派生类型，用以指定特定于某些数据流块类型的行为。 下表汇总了与每个数据流块类型相关的选项类型。  
  
|数据流块类型|<xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions> 类型|  
|-------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|  
|<xref:System.Threading.Tasks.Dataflow.BufferBlock%601>|<xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions>|  
|<xref:System.Threading.Tasks.Dataflow.BroadcastBlock%601>|<xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions>|  
|<xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601>|<xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions>|  
|<xref:System.Threading.Tasks.Dataflow.ActionBlock%601>|<xref:System.Threading.Tasks.Dataflow.ExecutionDataflowBlockOptions>|  
|<xref:System.Threading.Tasks.Dataflow.TransformBlock%602>|<xref:System.Threading.Tasks.Dataflow.ExecutionDataflowBlockOptions>|  
|<xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602>|<xref:System.Threading.Tasks.Dataflow.ExecutionDataflowBlockOptions>|  
|<xref:System.Threading.Tasks.Dataflow.BatchBlock%601>|<xref:System.Threading.Tasks.Dataflow.GroupingDataflowBlockOptions>|  
|<xref:System.Threading.Tasks.Dataflow.JoinBlock%602>|<xref:System.Threading.Tasks.Dataflow.GroupingDataflowBlockOptions>|  
|<xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602>|<xref:System.Threading.Tasks.Dataflow.GroupingDataflowBlockOptions>|  
  
 以下各节提供了有关重要数据流块选项（通过 <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions?displayProperty=nameWithType>、<xref:System.Threading.Tasks.Dataflow.ExecutionDataflowBlockOptions?displayProperty=nameWithType> 和 <xref:System.Threading.Tasks.Dataflow.GroupingDataflowBlockOptions?displayProperty=nameWithType> 类提供）的其他信息。  
  
### <a name="specifying-the-task-scheduler"></a>指定任务计划程序  

 每个预定义的数据流块在数据可用时使用 TPL 任务计划机制执行一些活动，例如，将数据传播到目标、接收来自源的数据并运行用户定义的委托。 <xref:System.Threading.Tasks.TaskScheduler> 是抽象类，表示将任务排队成线程的任务计划程序。 默认任务计划程序 <xref:System.Threading.Tasks.TaskScheduler.Default%2A> 使用 <xref:System.Threading.ThreadPool> 类进行排队并执行工作。 构造数据流块对象时，您可以通过设置 <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.TaskScheduler%2A> 属性重写默认任务计划程序。  
  
 当同一个任务计划程序管理多个数据流块时，它可在它们之间强制实施策略。 例如，如果多个数据流块分别配置为面向同一 <xref:System.Threading.Tasks.ConcurrentExclusiveSchedulerPair> 对象的独占计划程序，则会序列化这些块间运行的所有工作。 同样，如果这些块配置为面向同一 <xref:System.Threading.Tasks.ConcurrentExclusiveSchedulerPair> 对象的并发计划程序，而该计划程序配置为具有最大并发级，则这些块中所有的工作都会受到并发操作数的限制。 有关使用 <xref:System.Threading.Tasks.ConcurrentExclusiveSchedulerPair> 类启用并发读取操作，但以排除所有其他操作的独占方式执行写入操作的示例，请参阅[如何：在数据流块中指定任务计划程序](how-to-specify-a-task-scheduler-in-a-dataflow-block.md)。 有关 TPL 中的任务计划程序的详细信息，请参阅 <xref:System.Threading.Tasks.TaskScheduler> 类主题。  
  
### <a name="specifying-the-degree-of-parallelism"></a>指定并行度  

 默认情况下，TPL 数据流库提供三种执行块类型（<xref:System.Threading.Tasks.Dataflow.ActionBlock%601>、<xref:System.Threading.Tasks.Dataflow.TransformBlock%602> 和 <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602>），一次处理一条消息。 这些数据流块类型也会按照接收消息的顺序对消息进行处理。 若要使这些数据流块同时处理该消息，请在构造数据流对象块时设置 <xref:System.Threading.Tasks.Dataflow.ExecutionDataflowBlockOptions.MaxDegreeOfParallelism%2A?displayProperty=nameWithType> 属性。  
  
 <xref:System.Threading.Tasks.Dataflow.ExecutionDataflowBlockOptions.MaxDegreeOfParallelism%2A> 的默认值为 1，这保证了数据流块一次处理一条消息。 将该属性设置为大于 1 的值将使数据流块可以同时处理多条消息。 将该属性设置为 <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.Unbounded?displayProperty=nameWithType> 将使基础任务计划程序管理最大并发程度。  
  
> [!IMPORTANT]
> 当指定大于 1 的最大并行度时，会同时处理多条消息，因此，消息可能不会按照接收的顺序进行处理。 然而，从块输出消息的顺序与接收消息的顺序相同。  
  
 由于 <xref:System.Threading.Tasks.Dataflow.ExecutionDataflowBlockOptions.MaxDegreeOfParallelism%2A> 属性表示最大并行度，因此数据流块执行时的并行度可能小于指定的值。 为了达到功能要求或因为缺少可用的系统资源，数据流块可能使用较小的并行度。 数据流块选择的并行度不会超过您指定的值。  
  
 <xref:System.Threading.Tasks.Dataflow.ExecutionDataflowBlockOptions.MaxDegreeOfParallelism%2A> 属性的值对于每个数据流块对象而言，都是特有的。 例如，如果四个数据流对象块中的每一个都指定 1 作为最大并行度，则所有四个数据流对象块可以并行运行。  
  
 有关设置最大并行度以启用并行冗长操作的示例，请参阅[如何：指定数据流块中的并行度](how-to-specify-the-degree-of-parallelism-in-a-dataflow-block.md)。  
  
### <a name="specifying-the-number-of-messages-per-task"></a>指定每个任务的消息数  

 预定义的数据流块类型使用任务来处理多个输入元素。 这有助于最大限度地减少需要处理数据的任务对象数，从而使应用程序可以更有效地运行。 但是，当一个数据流块集合中的任务处理数据时，其他数据流块的任务可能需要按照队列消息等待处理时间。 若要使数据流任务更加公平，请设置 <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.MaxMessagesPerTask%2A> 属性。 当 <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.MaxMessagesPerTask%2A> 设置为 <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.Unbounded?displayProperty=nameWithType> 默认值时，数据流块使用的任务会处理尽可能多的消息。 当 <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.MaxMessagesPerTask%2A> 设置为 <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.Unbounded> 以外的值时，数据流块为每个 <xref:System.Threading.Tasks.Task> 对象至多处理这个数量的消息。 虽然设置 <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.MaxMessagesPerTask%2A> 属性可以提高任务间的公平性，但它可能会导致该系统创建多个非必要的任务，这会降低性能。  
  
### <a name="enabling-cancellation"></a>启用取消  

 TPL 提供了一种机制，能使任务以一种合作的方式协调取消。 若要启用数据流块参与此取消机制，请设置 <xref:System.Threading.Tasks.Dataflow.DataflowBlockOptions.CancellationToken%2A> 属性。 当此 <xref:System.Threading.CancellationToken> 对象设置为已取消状态时，所有监视该标记的数据流块都会完成当前项目的执行，但不会开始处理后续项。 这些数据流块也会清除所有缓冲的消息，释放所有源和目标块的连接，并转换为已取消状态。 通过转换为已取消状态，<xref:System.Threading.Tasks.Dataflow.IDataflowBlock.Completion%2A> 属性具有设置为 <xref:System.Threading.Tasks.Task.Status%2A> 的 <xref:System.Threading.Tasks.TaskStatus.Canceled> 属性，除非在处理过程中出现异常。 在这种情况下，<xref:System.Threading.Tasks.Task.Status%2A> 会设置为 <xref:System.Threading.Tasks.TaskStatus.Faulted>。  
  
 有关演示如何在 Windows 窗体应用程序中使用取消的示例，请参阅[如何：取消数据流块](how-to-cancel-a-dataflow-block.md)。 若要深入了解 TPL 中的取消，请参阅[任务取消](task-cancellation.md)。  
  
### <a name="specifying-greedy-versus-non-greedy-behavior"></a>指定贪婪与非贪婪行为  

 几个分组数据流块类型可以在贪婪或非贪婪模式下运行。 默认情况下，预定义的数据流块类型在贪婪模式下运行。  
  
 对于联接块类型（如 <xref:System.Threading.Tasks.Dataflow.JoinBlock%602>），贪婪模式意味着块立即接受数据，即使相应的数据联接不可用。 非贪婪模式意味着块推迟所有传入的消息，直到在其每个目标上有一个可完成联接。 如果任何推迟的消息不再可用，则联接块会释放所有推迟的消息并重新启动该过程。 对于 <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> 类，贪婪和非贪婪行为非常相似，不同之处在于在非贪婪模式下，<xref:System.Threading.Tasks.Dataflow.BatchBlock%601> 对象推迟所有传入的消息，直到不同源中有足够消息可用于完成批作业。  
  
 若要为数据流块指定非贪婪模式，请将 <xref:System.Threading.Tasks.Dataflow.GroupingDataflowBlockOptions.Greedy%2A> 设置为 `False`。 有关演示如何使用非贪婪模式使多个联接块更高效地共享数据源的示例，请参阅[如何：使用 JoinBlock 从多个源读取数据](how-to-use-joinblock-to-read-data-from-multiple-sources.md)。

## <a name="custom-dataflow-blocks"></a>自定义数据流块  

 尽管 TPL 数据流库提供了许多预定义块类型，但是您还是可以创建执行自定义行为的其他块类型。 直接实现 <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601> 或 <xref:System.Threading.Tasks.Dataflow.ITargetBlock%601> 接口或使用 <xref:System.Threading.Tasks.Dataflow.DataflowBlock.Encapsulate%2A> 方法生成封装现有块类型行为的复杂块。 有关演示如何实现自定义数据流块功能的示例，请参阅[演练：创建自定义数据流块类型](walkthrough-creating-a-custom-dataflow-block-type.md)。

## <a name="related-topics"></a>相关主题  
  
|Title|描述|  
|-----------|-----------------|  
|[如何：将消息写入数据流块和从数据流块读取消息](how-to-write-messages-to-and-read-messages-from-a-dataflow-block.md)|演示如何向 <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> 对象写入和读取消息。|  
|[如何：实现制造者-使用者数据流模式](how-to-implement-a-producer-consumer-dataflow-pattern.md)|描述如何使用数据流模型实现制造者-使用方模式，在这个模型中制造者向数据流块发送消息，而使用方从该块中读取消息。|  
|[如何：在数据流块收到数据时执行操作](how-to-perform-action-when-a-dataflow-block-receives-data.md)|描述如何向执行数据流块类型（<xref:System.Threading.Tasks.Dataflow.ActionBlock%601>、<xref:System.Threading.Tasks.Dataflow.TransformBlock%602> 和 <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602>）提供委托。|  
|[演练：创建数据流管道](walkthrough-creating-a-dataflow-pipeline.md)|描述如何创建从 Web 下载文本并对该文本执行操作的数据流管道。|  
|[如何：取消链接数据流块](how-to-unlink-dataflow-blocks.md)|展示了如何在源向目标提供消息后，使用 <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601.LinkTo%2A> 方法取消链接源数据流块和目标数据流块。|  
|[演练：在 Windows 窗体应用程序中使用数据流](walkthrough-using-dataflow-in-a-windows-forms-application.md)|演示如何创建在 Windows 窗体应用程序中执行图像处理的数据流块网络。|  
|[如何：取消数据流块](how-to-cancel-a-dataflow-block.md)|演示如何在 Windows 窗体应用程序中使用取消。|  
|[如何：使用 JoinBlock 从多个源读取数据](how-to-use-joinblock-to-read-data-from-multiple-sources.md)|解释如何在多个源的数据可用时使用 <xref:System.Threading.Tasks.Dataflow.JoinBlock%602> 类执行操作，以及如何使用非贪婪模式使多个联接块更有效地共享数据源。|  
|[如何：指定数据流块中的并行度](how-to-specify-the-degree-of-parallelism-in-a-dataflow-block.md)|描述如何设置 <xref:System.Threading.Tasks.Dataflow.ExecutionDataflowBlockOptions.MaxDegreeOfParallelism%2A> 属性使执行数据流块一次处理多条消息。|  
|[如何：在数据流块中指定任务计划程序](how-to-specify-a-task-scheduler-in-a-dataflow-block.md)|演示在应用程序中使用数据流时如何关联特定任务计划程序。|  
|[演练：使用 BatchBlock 和 BatchedJoinBlock 提高效率](walkthrough-using-batchblock-and-batchedjoinblock-to-improve-efficiency.md)|描述如何使用 <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> 类改进数据库插入操作的效率，以及如何使用 <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602> 类获取程序从数据库中读取数据时产生的结果和发生的任何异常。|  
|[演练：创建自定义数据流块类型](walkthrough-creating-a-custom-dataflow-block-type.md)|演示创建实现自定义行为的数据流块类型的两种方法。|  
|[任务并行库 (TPL)](task-parallel-library-tpl.md)|介绍 TPL，一个可在 .NET Framework 应用程序里简化并行和并发编程的库。|
