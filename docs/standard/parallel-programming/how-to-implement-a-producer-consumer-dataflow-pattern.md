---
title: 如何：实现生成方-使用方数据流模式
description: 了解如何使用 .NET 中的 TPL 数据流库实现生成方-使用方数据流模式。
ms.date: 09/24/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- TPL dataflow library, implementing producer-consumer pattern
- Task Parallel Library, dataflows
- producer-consumer patterns, implementing [TPL]
ms.assetid: 47a1d38c-fe9c-44aa-bd15-937bd5659b0b
ms.openlocfilehash: 3bc97cce2a063e7d5286ad886401cb6ae2ce3026
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94826926"
---
# <a name="how-to-implement-a-producer-consumer-dataflow-pattern"></a>如何：实现生成方-使用方数据流模式

本文介绍如何使用 TPL 数据流库实现生成方-使用方模式。 在此模式下，制造者向消息块发送消息，使用者从该块读取消息。

[!INCLUDE [tpl-install-instructions](../../../includes/tpl-install-instructions.md)]

## <a name="example"></a>示例

下面的示例演示使用数据流的基本制造者-使用者模型。 `Produce` 方法将包含随机字节数据的数组写入 <xref:System.Threading.Tasks.Dataflow.ITargetBlock%601?displayProperty=nameWithType> 对象，而 `Consume` 方法从 <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601?displayProperty=nameWithType> 对象中读取字节。 通过对 <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601> 和 <xref:System.Threading.Tasks.Dataflow.ITargetBlock%601> 接口（而非其派生类型）的操作，可以编写可对多种数据流块类型执行的可重用代码。 此示例使用 <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> 类。 由于 <xref:System.Threading.Tasks.Dataflow.BufferBlock%601> 类既充当源块又充当目标块，制造者和使用者可以使用共享对象来传输数据。

 `Produce` 方法在循环中调用 <xref:System.Threading.Tasks.Dataflow.DataflowBlock.Post%2A> 方法，以将数据同步写入目标块。 在 `Produce` 方法将所有数据写入目标块后，它将调用 <xref:System.Threading.Tasks.Dataflow.IDataflowBlock.Complete%2A> 方法来表明此块将不再有其他数据可用。 `Consume` 方法使用 [async](../../csharp/language-reference/keywords/async.md) 和 [await](../../csharp/language-reference/operators/await.md) 运算符（Visual Basic 中的 [Async](../../visual-basic/language-reference/modifiers/async.md) 和 [Await](../../visual-basic/language-reference/operators/await-operator.md)），异步计算从 <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601> 对象收到的总字节数。 为了异步操作，`Consume` 方法会调用 <xref:System.Threading.Tasks.Dataflow.DataflowBlock.OutputAvailableAsync%2A> 方法，以便在源块有可用数据和源块不再有其他可用数据时收到通知。

 [!code-csharp[TPLDataflow_ProducerConsumer#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_producerconsumer/cs/dataflowproducerconsumer.cs#1)]
 [!code-vb[TPLDataflow_ProducerConsumer#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_producerconsumer/vb/dataflowproducerconsumer.vb#1)]

## <a name="robust-programming"></a>可靠编程

 前面的示例只使用一个使用者来处理源数据。 如果您的应用程序中有多个使用者，请使用 <xref:System.Threading.Tasks.Dataflow.IReceivableSourceBlock%601.TryReceive%2A> 方法从源块读取数据，如以下示例所示。

 [!code-csharp[TPLDataflow_ProducerConsumer#2](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_producerconsumer/cs/dataflowproducerconsumer.cs#2)]
 [!code-vb[TPLDataflow_ProducerConsumer#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_producerconsumer/vb/dataflowproducerconsumer.vb#2)]

 没有可用数据时，<xref:System.Threading.Tasks.Dataflow.IReceivableSourceBlock%601.TryReceive%2A> 方法将返回 `False`。 当多个使用者必须并发访问源块时，此机制可确保在调用 <xref:System.Threading.Tasks.Dataflow.DataflowBlock.OutputAvailableAsync%2A> 后数据仍然可用。

## <a name="see-also"></a>请参阅

- [数据流](dataflow-task-parallel-library.md)
