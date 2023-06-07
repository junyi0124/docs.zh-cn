---
title: 计时器
description: 了解在多线程环境中使用哪些 .NET 计时器。
ms.date: 07/03/2018
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- threading [.NET], timers
- timers, about timers
ms.assetid: 7091500d-be18-499b-a942-95366ce185e5
author: pkulikov
ms.openlocfilehash: 1ab612bc32a84c1248d311b0603a01a32a25199f
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94826100"
---
# <a name="timers"></a>计时器

.NET 提供两种可在多线程环境中使用的计时器：

- <xref:System.Threading.Timer?displayProperty=nameWithType>，用于按固定时间间隔在 <xref:System.Threading.ThreadPool> 线程上执行单个回叫方法。
- <xref:System.Timers.Timer?displayProperty=nameWithType>，默认情况下按固定时间间隔在 <xref:System.Threading.ThreadPool> 线程上引发事件。

> [!NOTE]
> 某些 .NET 实现可能包含其他计时器：
>
> - <xref:System.Windows.Forms.Timer?displayProperty=nameWithType>：一种 Windows 窗体组件，按固定时间间隔触发事件。 该组件没有用户界面，专门用于单线程环境。  
> - <xref:System.Web.UI.Timer?displayProperty=nameWithType>：一种 ASP.NET 组件，按固定时间间隔执行异步或同步网页回发。
> - <xref:System.Windows.Threading.DispatcherTimer?displayProperty=nameWithType>：集成到 <xref:System.Windows.Threading.Dispatcher> 队列中的计时器，该队列按指定时间间隔和指定优先级进行处理。

## <a name="the-systemthreadingtimer-class"></a>System.Threading.Timer 类

<xref:System.Threading.Timer?displayProperty=nameWithType> 类可用于按指定时间间隔持续调用委托。 此外，还可以使用此类计划按指定时间间隔对委托进行单次调用。 该委托在 <xref:System.Threading.ThreadPool> 线程上执行。

在创建 <xref:System.Threading.Timer?displayProperty=nameWithType> 对象时，需指定定义回叫方法的 <xref:System.Threading.TimerCallback> 委托、传递到该回叫的可选状态对象、首次调用该回叫前的延迟时间以及两次回叫调用之间的时间间隔。 若要取消挂起的计时器，请调用 <xref:System.Threading.Timer.Dispose%2A?displayProperty=nameWithType> 方法。

以下示例创建一个计时器，该计时器在一秒（1000 毫秒）后首次调用提供的委托，之后每两秒调用一次该委托。 示例中的状态对象用于计算调用该委托的次数。 当调用委托至少达 10 次时，计时器将停止。

[!code-cpp[System.Threading.Timer#2](../../../samples/snippets/cpp/VS_Snippets_CLR_System/system.Threading.Timer/CPP/source2.cpp#2)]
[!code-csharp[System.Threading.Timer#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.Threading.Timer/CS/source2.cs#2)]
[!code-vb[System.Threading.Timer#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.Threading.Timer/VB/source2.vb#2)]

有关更多信息和示例，请参见<xref:System.Threading.Timer?displayProperty=nameWithType>。

## <a name="the-systemtimerstimer-class"></a>System.Timers.Timer 类

<xref:System.Timers.Timer?displayProperty=nameWithType> 是另一个可在多线程环境中使用的计时器，该计时器默认情况下在 <xref:System.Threading.ThreadPool> 线程上引发事件。

在创建 <xref:System.Timers.Timer?displayProperty=nameWithType> 对象时，可以指定引发 <xref:System.Timers.Timer.Elapsed> 事件的时间间隔。 使用 <xref:System.Timers.Timer.Enabled%2A> 属性指示计时器是否应引发 <xref:System.Timers.Timer.Elapsed> 事件。 如果仅需在经过指定时间间隔后引发一次 <xref:System.Timers.Timer.Elapsed> 事件，请将 <xref:System.Timers.Timer.AutoReset%2A> 设置为 `false`。 <xref:System.Timers.Timer.AutoReset%2A> 属性的默认值为 `true`，表示将按 <xref:System.Timers.Timer.Interval%2A> 属性定义的时间间隔定期引发 <xref:System.Timers.Timer.Elapsed> 事件。

有关更多信息和示例，请参见<xref:System.Timers.Timer?displayProperty=nameWithType>。
  
## <a name="see-also"></a>请参阅

- <xref:System.Threading.Timer?displayProperty=nameWithType>
- <xref:System.Timers.Timer?displayProperty=nameWithType>
- [线程处理对象和功能](threading-objects-and-features.md)
