---
title: 如何：使用并行任务遍历二叉树
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tasks, how to traverse a tree
ms.assetid: 4265d169-6c69-4f36-b10d-b7ae7f72f4df
ms.openlocfilehash: 6c8708f2879671573ab870bf7d9df520a6118c7a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95722367"
---
# <a name="how-to-traverse-a-binary-tree-with-parallel-tasks"></a>如何：使用并行任务遍历二叉树

下面的示例展示了两种使用并行任务遍历树数据结构的方式。 树创建本身是留给大家练练手的。  
  
## <a name="example"></a>示例  

 [!code-csharp[TPL#16](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl/cs/tpl.cs#16)]
 [!code-vb[TPL#16](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl/vb/treewalk.vb#16)]  
  
 上面的两种方法在功能上是相当的。 通过使用 <xref:System.Threading.Tasks.TaskFactory.StartNew%2A> 方法创建和运行任务，可以从可用于等待任务和处理异常的任务中返回句柄。  
  
## <a name="see-also"></a>另请参阅

- [任务并行库 (TPL)](task-parallel-library-tpl.md)
