---
title: 演练：创建数据流管道
description: 创建数据流管道，它是一系列组件或数据流块。 数据流块执行特定任务来贡献更大的目标。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- dataflow pipelines, creating with TPL
- Task Parallel Library, dataflows
- TPL dataflow library, creating dataflow pipeline
ms.assetid: 69308f82-aa22-4ac5-833d-e748533b58e8
ms.openlocfilehash: 9efa77062be35dd93e72c88c67cccd06ff485141
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95689844"
---
# <a name="walkthrough-creating-a-dataflow-pipeline"></a>演练：创建数据流管道

尽管可以使用 <xref:System.Threading.Tasks.Dataflow.DataflowBlock.Receive%2A?displayProperty=nameWithType>、<xref:System.Threading.Tasks.Dataflow.DataflowBlock.ReceiveAsync%2A?displayProperty=nameWithType> 和 <xref:System.Threading.Tasks.Dataflow.DataflowBlock.TryReceive%2A?displayProperty=nameWithType> 方法从源块接收消息，但也可以连接消息块来形成一个 *数据流管道*。 数据流管道是一系列组件或“数据流块”，每个组件或数据流块执行一个有助于实现更大目标的特定任务。 数据流管道中的每个数据流块会在收到来自另一数据流块的消息时执行工作。 这就好比是汽车制造装配线。 每辆汽车通过装配线时，一站组装车架，下一站则安装引擎，以此类推。 因为装配线可以同时装配多辆汽车，所以比一次装配整辆车拥有更高的产出。

 本文档演示了一个数据流管道，用于从网站上下载书籍《The Iliad of Homer》并搜索文本以将各个单词与反转第一个单词字符的单词相匹配。 本文档中数据流管道的形成包括以下步骤：  
  
1. 创建参与管道的数据流块。  
  
2. 连接每个数据流块与管道中的下一个块。 每个块将管道中前一个块的输出作为输入接收。  
  
3. 对每个数据流块，创建一个延续任务，该延续任务在上一个块完成后将下一个块的状态设置为已完成状态。  
  
4. 将数据发布到管道的开头。  
  
5. 将管道的开头标记为已完成。  
  
6. 等待管道完成所有工作。  
  
## <a name="prerequisites"></a>先决条件  

 开始本演练之前，请阅读[数据流](dataflow-task-parallel-library.md)。  
  
## <a name="creating-a-console-application"></a>创建控制台应用程序  

 在 Visual Studio 中，创建 Visual C# 或 Visual Basic“控制台应用程序”项目。 安装 System.Threading.Tasks.Dataflow NuGet 包。

[!INCLUDE [tpl-install-instructions](../../../includes/tpl-install-instructions.md)]

 将以下代码添加到项目中以创建基本应用程序。  
  
 [!code-csharp[TPLDataflow_Palindromes#2](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_palindromes/cs/dataflowpalindromes.cs#2)]
 [!code-vb[TPLDataflow_Palindromes#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_palindromes/vb/dataflowpalindromesemptymain.vb#2)]  
  
## <a name="creating-the-dataflow-blocks"></a>创建数据流块  

 将以下代码添加到 `Main` 方法以创建参与管道的数据流块。 下表总结了管道的每个成员的角色。  
  
 [!code-csharp[TPLDataflow_Palindromes#3](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_palindromes/cs/dataflowpalindromes.cs#3)]
 [!code-vb[TPLDataflow_Palindromes#3](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_palindromes/vb/dataflowpalindromes.vb#3)]  
  
|成员|类型|描述|  
|------------|----------|-----------------|  
|`downloadString`|<xref:System.Threading.Tasks.Dataflow.TransformBlock%602>|从 Web 下载该书的文本。|  
|`createWordList`|<xref:System.Threading.Tasks.Dataflow.TransformBlock%602>|将该书的文本分成单词的数组。|  
|`filterWordList`|<xref:System.Threading.Tasks.Dataflow.TransformBlock%602>|删除短词和单词数组中的重复项。|  
|`findReversedWords`|<xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602>|查找经过筛选的单词数组集合中所有将字母反转后仍在单词数组中出现的单词。|  
|`printReversedWords`|<xref:System.Threading.Tasks.Dataflow.ActionBlock%601>|向控制台显示单词及对应的倒序词。|  
  
 虽然可以将本示例中数据流管道的多个步骤合并为一个步骤，不过本示例阐释了组合多个独立数据流任务来执行较大任务的概念。 示例通过使用 <xref:System.Threading.Tasks.Dataflow.TransformBlock%602> 使管道的每个成员能对其输入数据执行操作并将结果发送到管道中的下一步骤。 管道的 `findReversedWords` 成员是一个 <xref:System.Threading.Tasks.Dataflow.TransformManyBlock%602> 对象，因为该成员会为每个输入生成多个独立输出。 管道的结尾 `printReversedWords` 是一个 <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> 对象，因为它会对其输入执行一个操作，但不产生结果。  
  
## <a name="forming-the-pipeline"></a>形成管道  

 添加以下代码将每个块与管道中的下一个块连接。  
  
 当您调用 <xref:System.Threading.Tasks.Dataflow.DataflowBlock.LinkTo%2A> 方法将源数据流块连接到目标数据流块时，源数据流块会在数据可用时将数据传播到目标块。 如果你还提供 <xref:System.Threading.Tasks.Dataflow.DataflowLinkOptions>，并将 <xref:System.Threading.Tasks.Dataflow.DataflowLinkOptions.PropagateCompletion> 设置为 true，则在管道中成功或未成功完成一个块都将导致管道中下一个块的完成。
  
 [!code-csharp[TPLDataflow_Palindromes#4](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_palindromes/cs/dataflowpalindromes.cs#4)]
 [!code-vb[TPLDataflow_Palindromes#4](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_palindromes/vb/dataflowpalindromes.vb#4)]  
  
## <a name="posting-data-to-the-pipeline"></a>将数据发布到管道  

 添加以下代码，以将《The Iliad of Homer》一书的 URL 发布到数据流管道的开头。  
  
 [!code-csharp[TPLDataflow_Palindromes#6](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_palindromes/cs/dataflowpalindromes.cs#6)]
 [!code-vb[TPLDataflow_Palindromes#6](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_palindromes/vb/dataflowpalindromes.vb#6)]  
  
 此示例使用 <xref:System.Threading.Tasks.Dataflow.DataflowBlock.Post%2A?displayProperty=nameWithType> 将数据同步发送到管道的开头。 在必须将数据异步发送到数据流节点时，请使用 <xref:System.Threading.Tasks.Dataflow.DataflowBlock.SendAsync%2A?displayProperty=nameWithType> 方法。  
  
## <a name="completing-pipeline-activity"></a>完成管道活动  

 添加以下代码将管道的开头标记为已完成。 管道的开头在处理了所有缓冲的消息后传播其完成。
  
 [!code-csharp[TPLDataflow_Palindromes#7](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_palindromes/cs/dataflowpalindromes.cs#7)]
 [!code-vb[TPLDataflow_Palindromes#7](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_palindromes/vb/dataflowpalindromes.vb#7)]  
  
 此示例通过要处理的数据流管道发送一个 URL。 如果要通过管道发送多个输入，请在提交了所有输入后调用 <xref:System.Threading.Tasks.Dataflow.IDataflowBlock.Complete%2A?displayProperty=nameWithType> 方法。 如果您的应用程序没有表示数据不再可用或应用程序不必等待管道完成的定义完善的点，则可以忽略此步骤。  
  
## <a name="waiting-for-the-pipeline-to-finish"></a>等待管道完成  

 添加以下代码以等待管道完成。 管道的结尾完成时完成整个操作。  
  
 [!code-csharp[TPLDataflow_Palindromes#8](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_palindromes/cs/dataflowpalindromes.cs#8)]
 [!code-vb[TPLDataflow_Palindromes#8](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_palindromes/vb/dataflowpalindromes.vb#8)]  
  
 可以同时等待任一线程或多个线程的数据流完成。  
  
## <a name="the-complete-example"></a>完整示例  

 下面的示例显示此演练的完整代码。  
  
 [!code-csharp[TPLDataflow_Palindromes#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_palindromes/cs/dataflowpalindromes.cs#1)]
 [!code-vb[TPLDataflow_Palindromes#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_palindromes/vb/dataflowpalindromes.vb#1)]  
  
## <a name="next-steps"></a>后续步骤  

 此示例发送一个通过数据流管道处理的 URL。 如果要通过管道发送多个输入值，可以将并行的形式引入应用程序，这与零件在汽车厂中移动的方式类似。 当管道的第一个成员将其结果发送给第二个成员时，它可以在第二个成员处理第一个结果时并行处理另一个项。  
  
 通过使用数据流管道实现的并行称为 *粗粒度并行*，因为它通常由几个较大的任务组成。 此外，你也可以在数据流管道中对短时间运行的较小任务使用 *粒度较细的并行*。 在本示例中，管道的 `findReversedWords` 成员使用 [PLINQ](introduction-to-plinq.md) 并行处理工作列表中的多个项。 在粗粒度的管道中使用细粒度并行可以提高总吞吐量。  
  
 另外，还可以将数据流块连接到多个目标块，以创建“数据流网络”。 <xref:System.Threading.Tasks.Dataflow.DataflowBlock.LinkTo%2A> 方法采用一个 <xref:System.Predicate%601> 对象，该对象定义了目标块是否根据其值来接受每个消息。 大多数充当源的数据流块类型按目标块连接的顺序向所有已连接的目标块提供消息，直到其中一个块接受此消息。 通过使用此筛选机制，您可以创建已连接数据流块的系统，指示某些数据通过一条路径，其他数据通过另一条路径。 有关使用筛选来创建数据流网络的示例，请参阅[演练：在 Windows 窗体应用程序中使用数据流](walkthrough-using-dataflow-in-a-windows-forms-application.md)。  
  
## <a name="see-also"></a>请参阅

- [数据流](dataflow-task-parallel-library.md)
