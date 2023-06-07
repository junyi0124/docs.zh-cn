---
title: SYSLIB0006 警告
description: 了解有关生成编译时警告 SYSLIB0006 的过时信息。
ms.topic: reference
ms.date: 10/20/2020
ms.openlocfilehash: c1eecade9280ac37917bc24aa2973756c7f08d87
ms.sourcegitcommit: e301979e3049ce412d19b094c60ed95b316a8f8c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/16/2020
ms.locfileid: "97596273"
---
# <a name="syslib0006-threadabort-is-not-supported"></a>SYSLIB0006：不支持 Thread.Abort

从 .NET 5.0 开始，以下 API 标记为已过时。 使用这些 API 会在编译时生成警告 `SYSLIB0006`。

- <xref:System.Threading.Thread.Abort?displayProperty=nameWithType>
- <xref:System.Threading.Thread.Abort(System.Object)?displayProperty=nameWithType>

## <a name="workarounds"></a>工作区

使用 <xref:System.Threading.CancellationToken> 中止对工作单元的处理，而不是调用 <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType>。 以下示例说明了 <xref:System.Threading.CancellationToken> 的用法。

```csharp
void ProcessPendingWorkItemsNew(CancellationToken cancellationToken)
{
    if (QueryIsMoreWorkPending())
    {
        // If the CancellationToken is marked as "needs to cancel",
        // this will throw the appropriate exception.
        cancellationToken.ThrowIfCancellationRequested();

        WorkItem work = DequeueWorkItem();
        ProcessWorkItem(work);
    }
}
```

[!INCLUDE [suppress-syslib-warning](../../../../includes/suppress-syslib-warning.md)]

## <a name="see-also"></a>另请参阅

- [Thread.Abort 已过时](../core-libraries/5.0/thread-abort-obsolete.md)
- [托管线程中的取消](../../../standard/threading/cancellation-in-managed-threads.md)
