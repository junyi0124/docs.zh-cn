---
title: 以协作方式取消线程
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- threads, cancellation
ms.assetid: d2d6d5fd-e263-4fa0-847b-2fc3e0d82337
ms.openlocfilehash: 9e9224e9dc9ac57defe75e916dd6b9844bba7f12
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94826607"
---
# <a name="canceling-threads-cooperatively"></a>以协作方式取消线程

在 .NET Framework 4 之前，.NET 不提供内置方法在线程启动后以协作方式取消线程。 不过，从 .NET Framework 4 开始，可以使用 <xref:System.Threading.CancellationToken?displayProperty=nameWithType> 来取消线程，就像使用它们取消 <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> 对象或 PLINQ 查询一样。 虽然 <xref:System.Threading.Thread?displayProperty=nameWithType> 类不提供对取消标记的内置支持，但可以使用采用 <xref:System.Threading.ParameterizedThreadStart> 委托的 <xref:System.Threading.Thread> 构造函数将一个标记传递给一个线程过程。 下面的示例演示如何执行此操作。  
  
 [!code-csharp[Cancellation#14](../../../samples/snippets/csharp/VS_Snippets_Misc/cancellation/cs/CooperativeThreads.cs#14)]
 [!code-vb[Cancellation#14](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cancellation/vb/CooperativeThreads.vb#14)]  
  
## <a name="see-also"></a>另请参阅

- [使用线程和线程处理](using-threads-and-threading.md)
