---
title: 如何：使用 JoinBlock 从多个源读取数据
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Task Parallel Library, dataflows
- TPL dataflow library, joining blocks in
- dataflow blocks, joining in TPL
ms.assetid: e9c1ada4-ac57-4704-87cb-2f5117f8151d
ms.openlocfilehash: 381c77ee37b2ce03e7e45ad41bef31a9bd7b6ede
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95722643"
---
# <a name="how-to-use-joinblock-to-read-data-from-multiple-sources"></a>如何：使用 JoinBlock 从多个源读取数据

本文档介绍如何在来自多个源的数据可用时使用 <xref:System.Threading.Tasks.Dataflow.JoinBlock%602> 类执行操作。 还演示了如何使用非贪婪模式使多个联接块更有效地共享数据源。

[!INCLUDE [tpl-install-instructions](../../../includes/tpl-install-instructions.md)]

## <a name="example"></a>示例  

 下面的示例定义了三种资源类型（`NetworkResource`、`FileResource` 和 `MemoryResource`）并在资源可用时执行操作。 此示例需要使用 `NetworkResource` 和 `MemoryResource` 对才能执行第一个操作，需要使用 `FileResource` 和 `MemoryResource` 对才能执行第二个操作。 为了在所有所需资源可用时启用这些操作，此示例使用 <xref:System.Threading.Tasks.Dataflow.JoinBlock%602> 类。 当 <xref:System.Threading.Tasks.Dataflow.JoinBlock%602> 对象从所有源接收数据时，它会将该数据传播到其目标，其目标在本示例中为 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> 对象。 两个 <xref:System.Threading.Tasks.Dataflow.JoinBlock%602> 对象都从 `MemoryResource` 对象的共享池中读取。  
  
 [!code-csharp[TPLDataflow_NonGreedyJoin#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_nongreedyjoin/cs/nongreedyjoin.cs#1)]
 [!code-vb[TPLDataflow_NonGreedyJoin#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_nongreedyjoin/vb/nongreedyjoin.vb#1)]  
  
 为了实现 `MemoryResource` 对象共享池的高效使用，此示例指定了一个 <xref:System.Threading.Tasks.Dataflow.GroupingDataflowBlockOptions> 对象，该对象将 <xref:System.Threading.Tasks.Dataflow.GroupingDataflowBlockOptions.Greedy%2A> 属性设置为 `False` 以创建在非贪婪模式下运行的 <xref:System.Threading.Tasks.Dataflow.JoinBlock%602> 对象。 非贪婪联接块会推迟所有传入的消息，直至从每个源收到一条消息。 如果任何推迟的消息由另一个块接受，联接块将重新启动该进程。 非贪婪模式使联接块能够共享一个或多个源块，以便在其他块等待数据时能够使进程向前推进。 在此示例中，如果将 `MemoryResource` 对象添加到 `memoryResources` 池中，那么要接收第二个数据源的第一个联接块可以将进程向前推进。 如果此示例使用贪婪模式（默认模式），一个联接块可能会接受 `MemoryResource` 对象，然后等待第二个资源变为可用。 但是，如果另一个联接块有自己的第二个可用数据源，则它无法使进程向前推进，因为 `MemoryResource` 对象已被另一联接块占用。  
  
## <a name="robust-programming"></a>可靠编程  

 使用非贪婪联接还有助于防止应用程序中出现死锁。 在软件应用中，如果两个或多个进程分别留有资源，且相互等待另一进程释放其他资源，就会发生死锁  。 考虑一个定义两个 <xref:System.Threading.Tasks.Dataflow.JoinBlock%602> 对象的应用程序。 两个对象都从两个共享源块读取数据。 在贪婪模式下，如果一个联接块从第一个源读取，第二个联接块从第二个源读取，则应用程序可能发生死锁，原因是两个联接块相互等待另一个联接块释放其资源。 在非贪婪模式下，每个联接块只在所有数据可用时才从其源读取，因此消除了死锁风险。  
  
## <a name="see-also"></a>另请参阅

- [数据流](dataflow-task-parallel-library.md)
