---
title: 工作站和服务器垃圾回收 (GC)
description: 了解 .NET 中的工作站和服务器垃圾回收。
ms.date: 04/21/2020
helpviewer_keywords:
- garbage collection, workstation
- garbage collection, server
- workstation garbage collection
- server garbage collection
ms.openlocfilehash: 640b5f42c1f841c2537284e4721e827248e3d300
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87917016"
---
# <a name="workstation-and-server-garbage-collection"></a>工作站和服务器垃圾回收

垃圾回收器可自行优化并且适用于多种方案。 不过，你可以基于工作负载的特征[设置垃圾回收的类型](../../core/run-time-config/garbage-collector.md#flavors-of-garbage-collection)。 CLR 提供了以下类型的垃圾回收：

- 工作站垃圾回收 (GC) 是为客户端应用设计的。 它是独立应用的默认 GC 风格。 对于托管应用（例如由 ASP.NET 托管的应用），由主机确定默认 GC 风格。

  工作站垃圾回收既可以是并发的，也可以是非并发的。 并发（或后台  ）垃圾回收使托管线程能够在垃圾回收期间继续操作。 [后台垃圾回收](background-gc.md)替换 .NET Framework 4 及更高版本中的[并行垃圾回收](background-gc.md#concurrent-garbage-collection)。

- 服务器垃圾回收，用于需要高吞吐量和可伸缩性的服务器应用程序。

  - 在 .NET Core 中，服务器垃圾回收既可以是非并发也可以是后台执行。

  - 在 .NET Framework 4.5 和更高版本中，服务器垃圾回收既可以是非并发也可以是后台执行。 在 .NET Framework 4 和以前的版本中，服务器垃圾回收非并行运行。

下图演示了服务器上执行垃圾回收的专用线程：

![服务器垃圾回收线程](media/gc-server.png)

## <a name="performance-considerations"></a>性能注意事项

### <a name="workstation-gc"></a>工作站 GC

以下是工作站垃圾回收的线程处理和性能注意事项：

- 回收发生在触发垃圾回收的用户线程上，并保留相同优先级。 因为用户线程通常以普通优先级运行，所以垃圾回收器（在普通优先级线程上运行）必须与其他线程竞争 CPU 时间。 （运行本机代码的线程不会由于服务器或工作站垃圾回收而挂起。）

- 工作站垃圾回收始终用于只有一个处理器的计算机，无论[配置设置](../../core/run-time-config/garbage-collector.md#workstation-vs-server)如何。

### <a name="server-gc"></a>服务器 GC

以下是服务器垃圾回收的线程处理和性能注意事项：

- 回收发生在以 `THREAD_PRIORITY_HIGHEST` 优先级运行的多个专用线程上。

- 为每个 CPU 提供一个用于执行垃圾回收的一个堆和专用线程，并将同时回收这些堆。 每个堆都包含一个小对象堆和一个大对象堆，并且所有的堆都可由用户代码访问。 不同堆上的对象可以相互引用。

- 因为多个垃圾回收线程一起工作，所以对于相同大小的堆，服务器垃圾回收比工作站垃圾回收更快一些。

- 服务器垃圾回收通常具有更大的段。 但是，这是通常情况：段大小特定于实现且可能更改。 调整应用程序时，不要假设垃圾回收器分配的段大小。

- 服务器垃圾回收会占用大量资源。 例如，假设在一台有 4 个处理器的计算机上，运行着 12 个使用服务器 GC 的进程。 如果所有进程碰巧同时回收垃圾，它们会相互干扰，因为将在同一个处理器上调度 12 个线程。 如果进程处于活动状态，则最好不要让它们都使用服务器 GC。

如果运行应用程序的数百个实例，请考虑使用工作站垃圾回收并禁用并发垃圾回收。 这可以减少上下文切换，从而提高性能。

## <a name="see-also"></a>请参阅

- [后台垃圾回收](background-gc.md)
- [用于垃圾回收的运行时配置选项](../../core/run-time-config/garbage-collector.md)
