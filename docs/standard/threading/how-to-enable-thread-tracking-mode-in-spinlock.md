---
title: 如何：在 SpinLock 中启用线程跟踪模式
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- SpinLock, how to enable thread-tracking
ms.assetid: 62ee2e68-0bdd-4869-afc9-f0a57a11ae01
ms.openlocfilehash: c33978226a02f65fdc495762af9286ba2daf9454
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723735"
---
# <a name="how-to-enable-thread-tracking-mode-in-spinlock"></a>如何：在 SpinLock 中启用线程跟踪模式

<xref:System.Threading.SpinLock?displayProperty=nameWithType> 是一种低级互斥锁，可用于等待时间非常短的方案。 <xref:System.Threading.SpinLock> 不可重入。 进入锁后，线程必须先正确退出锁定，然后才能再次进入。 通常，只要尝试重入锁，就会导致死锁发生，且死锁的调试难度可能非常大。 作为开发的辅助手段，<xref:System.Threading.SpinLock?displayProperty=nameWithType> 支持线程跟踪模式。也就是说，如果线程尝试重入已保留的锁，就会导致异常抛出。 这样一来，可以更轻松地定位未正确退出锁的点。 线程跟踪模式的启用方法为，使用需要使用布尔输入参数的 <xref:System.Threading.SpinLock> 构造函数，并传入参数 `true`。 完成开发和测试阶段后，禁用线程跟踪模式，以提升性能。  
  
## <a name="example"></a>示例  

 下面的示例展示了线程跟踪模式。 正确退出锁的代码行被注释掉，以模拟导致以下结果之一的编码错误：  
  
- 如果 <xref:System.Threading.SpinLock> 是通过使用参数 `true`（Visual Basic 中的 `True`）创建而成，异常就会抛出。  
  
- 如果 <xref:System.Threading.SpinLock> 是通过使用参数 `false`（Visual Basic 中的 `False`）创建而成，死锁就会发生。  
  
 [!code-csharp[CDS_SpinLock#01](../../../samples/snippets/csharp/VS_Snippets_Misc/cds_spinlock/cs/spinlockdemo.cs#01)]
 [!code-vb[CDS_SpinLock#01](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_spinlock/vb/spinlock_threadtracking.vb#01)]  
  
## <a name="see-also"></a>另请参阅

- [SpinLock](spinlock.md)
