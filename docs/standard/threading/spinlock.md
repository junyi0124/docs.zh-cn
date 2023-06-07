---
title: SpinLock
ms.date: 03/30/2017
helpviewer_keywords:
- synchronization primitives, SpinLock
ms.assetid: f9af93bb-7a0d-4ba5-afe8-74f48b6b6958
ms.openlocfilehash: adb80ffb1917ea725c458dbe3c37f3d364276fb6
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95674289"
---
# <a name="spinlock"></a>SpinLock

<xref:System.Threading.SpinLock> 结构是低级互斥同步基元，在等待获取锁时旋转。 在多核计算机上，如果应缩短等待时间且争用最少，那么 <xref:System.Threading.SpinLock> 的性能优于其他种类的锁。 不过，建议仅在通过分析确定 <xref:System.Threading.Monitor?displayProperty=nameWithType> 方法或 <xref:System.Threading.Interlocked> 方法显著降低程序性能时，才使用 <xref:System.Threading.SpinLock>。  
  
 即使尚未获取锁，<xref:System.Threading.SpinLock> 也可能会生成线程的时间片。 这样做是为了避免线程优先级反转，并让垃圾回收器能够取得进展。 使用 <xref:System.Threading.SpinLock> 时，请确保没有线程可以将锁保留比较久的时间，也没有线程可以在保留锁时受阻止。  
  
 由于 SpinLock 是值类型，因此如果希望两个副本引用的是同一个锁，必须通过引用显式传递它。  
  
 若要详细了解如何使用此类型，请参阅 <xref:System.Threading.SpinLock?displayProperty=nameWithType>。 有关示例，请参阅[如何：使用 SpinLock 执行低级同步](how-to-use-spinlock-for-low-level-synchronization.md)。  
  
 <xref:System.Threading.SpinLock> 支持 *线程*-*跟踪* 模式，可以在开发阶段使用此模式，有助于跟踪在特定时间保留锁的线程。 虽然线程跟踪模式对于调试非常有用，但建议在程序的发行版中禁用它，因为它可能会降低性能。 有关详细信息，请参阅[如何：在 SpinLock 中启用线程跟踪模式](how-to-enable-thread-tracking-mode-in-spinlock.md)。  
  
## <a name="see-also"></a>另请参阅

- [线程处理对象和功能](threading-objects-and-features.md)
