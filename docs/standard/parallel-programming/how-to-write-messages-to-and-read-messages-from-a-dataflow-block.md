---
title: 如何：将消息写入数据流块和从数据流块读取消息
ms.date: 09/10/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Task Parallel Library, dataflows
- TPL dataflow library, reading and writing messages
ms.assetid: 1a9bf078-aa82-46eb-b95a-f87237f028c5
ms.openlocfilehash: be0e78989105cc59bd041ceb8c6f31073a702f83
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94820782"
---
# <a name="how-to-write-and-read-messages-from-a-dataflow-block"></a>如何：将消息写入数据流块和从数据流块读取消息

本文介绍如何使用任务并行库 (TPL) 数据流库将消息写入数据流块和从数据流块读取消息。 TPL 数据流库同时提供用于从数据流块写入和读取消息的同步和异步方法。 本文介绍如何使用 <xref:System.Threading.Tasks.Dataflow.BufferBlock%601?displayProperty=nameWithType> 类。 <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> 类可缓冲消息，既充当消息源，又充当消息目标。

[!INCLUDE [tpl-install-instructions](../../../includes/tpl-install-instructions.md)]

## <a name="writing-and-reading-synchronously"></a>同步写入和读取

下面的示例使用 <xref:System.Threading.Tasks.Dataflow.DataflowBlock.Post%2A> 方法写入 <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> 数据流块，使用 <xref:System.Threading.Tasks.Dataflow.DataflowBlock.Receive%2A> 方法从同一对象读取。

:::code language="csharp" source="../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_readwrite/cs/dataflowreadwrite.cs" id="2":::
:::code language="vb" source="../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_readwrite/vb/dataflowreadwrite.vb" id="2":::

如以下示例所示，还可以使用 <xref:System.Threading.Tasks.Dataflow.IReceivableSourceBlock%601.TryReceive%2A> 方法从数据流块读取。 <xref:System.Threading.Tasks.Dataflow.IReceivableSourceBlock%601.TryReceive%2A> 方法不会阻止当前线程，而且在偶尔轮询数据时非常有用。

:::code language="csharp" source="../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_readwrite/cs/dataflowreadwrite.cs" id="3":::
:::code language="vb" source="../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_readwrite/vb/dataflowreadwrite.vb" id="3":::

由于 <xref:System.Threading.Tasks.Dataflow.DataflowBlock.Post%2A> 方法是同步运行的，前面示例中的 <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> 对象会在第二个循环读取数据之前接收所有数据。 下面的示例对第一个示例进行了扩展，使用 <xref:System.Threading.Tasks.Task.WhenAll(System.Threading.Tasks.Task[])?displayProperty=nameWithType> 同时读取和写入消息块。 由于 <xref:System.Threading.Tasks.Task.WhenAll%2A> 并发执行操作，因此不会按任何特定的顺序将值写入 <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> 对象。

:::code language="csharp" source="../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_readwrite/cs/dataflowreadwrite.cs" id="4":::
:::code language="vb" source="../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_readwrite/vb/dataflowreadwrite.vb" id="4":::

## <a name="writing-and-reading-asynchronously"></a>异步写入和读取

下面的示例使用 <xref:System.Threading.Tasks.Dataflow.DataflowBlock.SendAsync%2A> 方法异步写入 <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> 对象，使用 <xref:System.Threading.Tasks.Dataflow.DataflowBlock.ReceiveAsync%2A> 方法从同一对象异步读取。 本示例使用 [async](../../csharp/language-reference/keywords/async.md) 和 [await](../../csharp/language-reference/operators/await.md) 运算符（Visual Basic 中为 [Async](../../visual-basic/language-reference/modifiers/async.md) 和 [Await](../../visual-basic/language-reference/operators/await-operator.md)）以异步方式向目标块发送数据以及从中读取数据。 必须启用数据流块来推迟消息时，<xref:System.Threading.Tasks.Dataflow.DataflowBlock.SendAsync%2A> 方法很有用。 希望在数据可用时对此数据进行操作时，<xref:System.Threading.Tasks.Dataflow.DataflowBlock.ReceiveAsync%2A> 方法很有用。 有关消息在消息块之间如何传播的详细信息，请参阅[数据流](dataflow-task-parallel-library.md)中的“消息传递”一节。

:::code language="csharp" source="../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_readwrite/cs/dataflowreadwrite.cs" id="5":::
:::code language="vb" source="../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_readwrite/vb/dataflowreadwrite.vb" id="5":::

## <a name="a-complete-example"></a>完整示例

以下示例显示本文中的所有代码。

:::code language="csharp" source="../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_readwrite/cs/dataflowreadwrite.cs" id="1":::
:::code language="vb" source="../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_readwrite/vb/dataflowreadwrite.vb" id="1":::

## <a name="next-steps"></a>后续步骤

本示例演示如何直接从消息块读取和写入。 还可以连接数据流块来形成管道  （这是数据流块的线性序列）或网络  （这是数据流块的图形）。 在管道或网络中，当数据可用时源向目标异步传播数据。 有关创建基本数据流管道的示例，请参阅[演练：创建数据流管道](walkthrough-creating-a-dataflow-pipeline.md)。 有关创建更复杂的数据流网络的示例，请参阅[演练：在 Windows 窗体应用程序中使用数据流](walkthrough-using-dataflow-in-a-windows-forms-application.md)。

## <a name="see-also"></a>另请参阅

- [数据流（任务并行库）](dataflow-task-parallel-library.md)
