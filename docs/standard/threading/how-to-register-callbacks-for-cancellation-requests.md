---
title: 注册取消请求的回叫
ms.date: 08/14/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- cancellation, how to register callbacks
ms.assetid: 8838dd75-18ed-4b8b-b322-cd4531faac64
ms.openlocfilehash: f551055fc6e5e4565329201e9e0be6e4a83487a1
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94826399"
---
# <a name="register-callbacks-for-cancellation-requests"></a>注册取消请求的回叫

了解如何注册当 <xref:System.Threading.CancellationToken.IsCancellationRequested%2A> 属性为 true 时调用的委托。 对创建令牌的对象调用 <xref:System.Threading.CancellationTokenSource.Cancel%2A> 时，值将从 false 更改为 true。 使用此技术可取消本地不支持统一取消框架的异步操作，也可取消阻止可能等待异步操作结束的方法。

> [!NOTE]
> 某些情况下，当启用“仅我的代码”后，Visual Studio 会在引发异常的行中断运行并显示一条错误消息，该消息显示“用户代码未处理异常”。 此错误是良性的。 可以按 <kbd>F5</kbd> 继续运行，并查看下面示例中所示的异常处理行为。 为了阻止 Visual Studio 在第一个错误出现时中断，只需依次转到“工具”、“选项”、“调试”、“常规”下，取消选中“仅我的代码”复选框即可。

## <a name="example"></a>示例

在下面的示例中，<xref:System.Net.WebClient.CancelAsync%2A> 方法注册为在通过取消标记请求取消时要调用的方法。

:::code language="csharp" source="../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.cancellation.callback/cs/howtoexample1.cs":::
:::code language="vb" source="../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.cancellation.callback/vb/howtoexample1.vb":::

如果注册回调时已经请求了取消，那么仍然要保证调用回调。 在此特定情况下，如果当前未执行异步操作，则 <xref:System.Net.WebClient.CancelAsync%2A> 方法将不进行任何操作，因此调用此方法始终是安全的。

## <a name="see-also"></a>请参阅

- [托管线程中的取消](cancellation-in-managed-threads.md)
- <xref:System.Net.WebClient.DownloadStringTaskAsync(System.String)?displayProperty=nameWithType>
