---
title: 如何：取消链接数据流块
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- dataflow blocks, unlinking in TPL
- Task Parallel Library, dataflows
- TPL dataflow library, unlinking dataflow blocks
ms.assetid: 40f0208d-4618-47f7-85cf-4913d07d2d7d
ms.openlocfilehash: e981d1b9b3adc638a84de1c119ad849ec533d16e
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95722357"
---
# <a name="how-to-unlink-dataflow-blocks"></a>如何：取消链接数据流块

本文档介绍如何取消目标数据流块与其源的链接。

[!INCLUDE [tpl-install-instructions](../../../includes/tpl-install-instructions.md)]

## <a name="example"></a>示例  

 下面的示例创建了三个 <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> 对象，每个对象调用 `TrySolution` 方法来计算值。 此示例只需使用第一次调用 `TrySolution` 的结果即可完成。  
  
 [!code-csharp[TPLDataflow_ReceiveAny#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_receiveany/cs/dataflowreceiveany.cs#1)]
 [!code-vb[TPLDataflow_ReceiveAny#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_receiveany/vb/dataflowreceiveany.vb#1)]  
  
 为了接收完成的第一个 <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> 对象的值，该示例定义了 `ReceiveFromAny(T)` 方法。 `ReceiveFromAny(T)` 方法接受一个 <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601> 对象的数组，并将其中每个对象链接到 <xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601> 对象。 当使用 <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601.LinkTo%2A> 方法将源数据流块链接到目标块时，源会在数据可用时将消息传播到目标。 因为 <xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601> 类只接受为其提供的第一条消息，`ReceiveFromAny(T)` 方法通过调用 <xref:System.Threading.Tasks.Dataflow.DataflowBlock.Receive%2A> 方法来生成其结果。 这将生成提供给 <xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601> 对象的第一条消息。 <xref:System.Threading.Tasks.Dataflow.ISourceBlock%601.LinkTo%2A> 方法有一个重载版本，其含有一个具有 <xref:System.Threading.Tasks.Dataflow.DataflowLinkOptions.MaxMessages> 属性的 <xref:System.Threading.Tasks.Dataflow.DataflowLinkOptions> 对象，当该属性设置为 `1` 时，则指示源块在目标收到来自源的一条消息后取消与目标的链接。 取消 <xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601> 对象与其源的链接非常重要，因为当 <xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601> 对象收到一条消息后，便不再需要源数组与 <xref:System.Threading.Tasks.Dataflow.WriteOnceBlock%601> 对象之间的关系。  
  
 为了使 `TrySolution` 的剩余调用能够在其中一个调用计算了一个值后结束，`TrySolution` 方法采用一个 <xref:System.Threading.CancellationToken> 对象，该对象在对 `ReceiveFromAny(T)` 的调用返回后将被取消。 当此 <xref:System.Threading.SpinWait.SpinUntil%2A> 对象取消时，<xref:System.Threading.CancellationToken> 方法将返回。  
  
## <a name="see-also"></a>另请参阅

- [数据流](dataflow-task-parallel-library.md)
