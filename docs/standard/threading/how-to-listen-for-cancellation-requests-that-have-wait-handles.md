---
title: 如何：侦听具有等待句柄的取消请求
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- cancellation, waiting with wait handles
ms.assetid: 6e2aa49b-fc84-4bcf-962b-17db98b7edcb
ms.openlocfilehash: 74b32144a1c49fe6adfc5aa1fbdcf5dc9c8a4dd7
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95728493"
---
# <a name="how-to-listen-for-cancellation-requests-that-have-wait-handles"></a>如何：侦听具有等待句柄的取消请求

如果方法在等待事件收到信号时受阻止，既无法检查取消令牌的值，也无法及时响应。 第一个示例展示了如何在使用 <xref:System.Threading.ManualResetEvent?displayProperty=nameWithType> 等不本机支持统一取消框架的事件时解决此问题。 第二个示例展示了一种更简化的方法，即使用确实支持统一取消的 <xref:System.Threading.ManualResetEventSlim?displayProperty=nameWithType>。  
  
> [!NOTE]
> 某些情况下，当启用“仅我的代码”后，Visual Studio 会在引发异常的行中断运行并显示一条错误消息，该消息显示“用户代码未处理异常”。 此错误是良性的。 可以按 F5 继续运行，并请参阅下面示例中所示的异常处理行为。 为了阻止 Visual Studio 在第一个错误出现时中断，只需依次转到“工具”、“选项”、“调试”、“常规”  下，取消选中“仅我的代码”复选框即可。  
  
## <a name="example"></a>示例  

 下面的示例使用 <xref:System.Threading.ManualResetEvent> 展示了如何取消阻止不支持统一取消的等待句柄。  
  
 [!code-csharp[Cancellation#9](../../../samples/snippets/csharp/VS_Snippets_Misc/cancellation/cs/cancellationex9.cs#9)]
 [!code-vb[Cancellation#9](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cancellation/vb/cancellationex9.vb#9)]  
  
## <a name="example"></a>示例  

 下面的示例使用 <xref:System.Threading.ManualResetEventSlim> 展示了如何取消阻止确实支持统一取消的协调基元。 相同的方法可以与其他轻型协调基元（如 <xref:System.Threading.Semaphore>`Slim` 和 <xref:System.Threading.CountdownEvent>）结合使用。  
  
 [!code-csharp[Cancellation#10](../../../samples/snippets/csharp/VS_Snippets_Misc/cancellation/cs/cancellationex10.cs#10)]
 [!code-vb[Cancellation#10](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cancellation/vb/cancellationex10.vb#10)]  
  
## <a name="see-also"></a>另请参阅

- [托管线程中的取消](cancellation-in-managed-threads.md)
