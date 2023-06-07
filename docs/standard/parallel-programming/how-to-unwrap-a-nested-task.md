---
title: 如何：解除嵌套任务的包装
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tasks, how to unwrap nested tasks
ms.assetid: a0769dd2-0f6d-48ca-8418-a9d39de7f450
ms.openlocfilehash: 3eec3b7e8cc76ee171d88a7886b4983000848084
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95722617"
---
# <a name="how-to-unwrap-a-nested-task"></a>如何：解除嵌套任务的包装

可以从方法返回任务，再等待或继续执行此任务，如下面的示例所示：  
  
 [!code-csharp[TPL_Unwrap#01](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_unwrap/cs/unwrapprogram.cs#01)]
 [!code-vb[TPL_Unwrap#01](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_unwrap/vb/snippets1-3.vb#01)]  
  
 在上面的示例中，<xref:System.Threading.Tasks.Task%601.Result%2A> 属性的类型为 `string`（Visual Basic 中的 `String`）。  
  
 不过，在某些情况下，建议在另一个任务中创建任务，再返回嵌套任务。 在这种情况下，封闭任务的 `TResult` 本身就是任务。 在下面的示例中，Result 属性是 C# 中的 `Task<Task<string>>` 或 Visual Basic 中的 `Task(Of Task(Of String))`。  
  
 [!code-csharp[TPL_Unwrap#02](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_unwrap/cs/unwrapprogram.cs#02)]
 [!code-vb[TPL_Unwrap#02](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_unwrap/vb/snippets1-3.vb#02)]  
  
 虽然可以编写代码来取消包装外部任务并检索原始任务及其 <xref:System.Threading.Tasks.Task%601.Result%2A> 属性，但此类代码不易编写，因为必须处理异常和取消请求。 在这种情况下，建议使用 <xref:System.Threading.Tasks.TaskExtensions.Unwrap%2A> 扩展方法之一，如下面的示例所示。  
  
 [!code-csharp[TPL_UnWrap#03](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_unwrap/cs/unwrapprogram.cs#03)]
 [!code-vb[TPL_UnWrap#03](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_unwrap/vb/snippets1-3.vb#03)]  
  
 <xref:System.Threading.Tasks.TaskExtensions.Unwrap%2A> 方法可用于将任何 `Task<Task>` 或 `Task<Task<TResult>>`（Visual Basic 中的 `Task(Of Task)` 或 `Task(Of Task(Of TResult))`）转换为 `Task` 或 `Task<TResult>`（Visual Basic 中的 `Task(Of TResult)`）。 新任务完全表示内部嵌套任务，并包含取消状态和所有异常。  
  
## <a name="example"></a>示例  

 下面的示例展示了如何使用 <xref:System.Threading.Tasks.TaskExtensions.Unwrap%2A> 扩展方法。  
  
 [!code-csharp[TPL_UnWrap#04](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_unwrap/cs/unwrapprogram.cs#04)]
 [!code-vb[TPL_UnWrap#04](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_unwrap/vb/snippet04.vb#04)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Threading.Tasks.TaskExtensions?displayProperty=nameWithType>
- [基于任务的异步编程](task-based-asynchronous-programming.md)
