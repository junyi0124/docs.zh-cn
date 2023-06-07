---
title: 后台垃圾回收
description: 了解 .NET 中的后台垃圾回收，以及工作站和服务器垃圾回收有哪些区别。
ms.date: 04/21/2020
helpviewer_keywords:
- garbage collection, background
- background garbage collection
ms.openlocfilehash: bf88c14b2aeed94a548b6116749fa8669576afe1
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87916994"
---
# <a name="background-garbage-collection"></a>后台垃圾回收

在后台垃圾回收 (GC) 中，在进行第 2 代回收的过程中，将会根据需要收集暂时代（第 0 代和第 1 代）。 后台垃圾回收是在一个或多个专用线程上执行的，具体取决于它是后台还是服务器 GC，它只适用于第 2 代回收。

默认启用后台垃圾回收。 可以在 .NET Framework 应用中使用 [gcConcurrent](../../framework/configure-apps/file-schema/runtime/gcconcurrent-element.md) 配置设置或 .NET Core 和 .NET 5 及更高版本应用中的 [System.GC.Concurrent](../../core/run-time-config/garbage-collector.md#background-gc) 来启用或禁用后台垃圾回收。

> [!NOTE]
> 后台垃圾回收替换在 .NET Framework 4 及更高版本中可用的[并行垃圾回收](#concurrent-garbage-collection)。 在 .NET Framework 4 中，仅支持工作站垃圾回收。 从 .NET Framework 4.5 开始，后台垃圾回收可用于工作站和服务器垃圾回收 。

后台垃圾回收期间对暂时代的回收称为“前台”垃圾回收。 发生前台垃圾回收时，所有托管线程都将被挂起。

当后台垃圾回收正在进行并且你已在第 0 代中分配了足够的对象时，CLR 将执行第 0 代或第 1 代前台垃圾回收。 专用的后台垃圾回收线程将在常见的安全点上进行检查以确定是否存在对前台垃圾回收的请求。 如果存在，则后台回收将挂起自身以便前台垃圾回收可以发生。 在前台垃圾回收完成之后，专用的后台垃圾回收线程和用户线程将继续。

后台垃圾回收可以消除并发垃圾回收所带来的分配限制，因为在后台垃圾回收期间，可发生暂时垃圾回收。 后台垃圾回收可以删除暂存世代中的死对象。 如果需要，它还可以在第 1 代垃圾回收期间扩展堆。

## <a name="background-workstation-vs-server-gc"></a>后台工作站与服务器 GC

从 .NET Framework 4.5 开始，后台垃圾回收可用于服务器 GC。 服务器 GC 是服务器垃圾回收的默认模式。

后台服务器垃圾回收与后台工作站垃圾回收具有类似功能，但有一些不同之处：

- 后台工作区域垃圾回收使用一个专用的后台垃圾回收线程，而后台服务器垃圾回收使用多个线程。 通常一个逻辑处理器有一个专用线程。

- 不同于工作站后台垃圾回收线程，这些后台服务器 GC 线程不会超时。

下图显示对独立专用线程执行的后台工作站垃圾回收：

![后台工作站垃圾回收](media/fundamentals/background-workstation-garbage-collection.png)

下图显示对独立专用线程执行的后台服务器垃圾回收：

![后台服务器垃圾回收](media/fundamentals/background-server-garbage-collection.png)

## <a name="concurrent-garbage-collection"></a>并行垃圾回收

> [!TIP]
> 本部分仅适用于：
>
> - 用于工作站垃圾回收的 .NET Framework 3.5 及更早版本
> - 用于服务器垃圾回收的 .NET Framework 4 及更早版本
>
> 在更高的版本中，后台垃圾回收取代了并行垃圾回收。

在工作站或服务器垃圾回收中，你可以[启用并发垃圾回收](../../framework/configure-apps/file-schema/runtime/gcconcurrent-element.md)，以便在大多数回收期间，让各线程与执行垃圾回收的专用线程并发运行。 此选项只影响第 2 代中的垃圾回收；第 0 代和第 1 代中的垃圾回收始终是非并发的，因为它们完成的速度很快。

并发垃圾回收通过最大程度地减少因回收引起的暂停，使交互应用程序能够更快地响应。 在运行并发垃圾回收线程的大多数时间，托管线程可以继续运行。 此设计使得在发生垃圾回收时的暂停时间更短。

并发垃圾回收在一个专用线程上执行。 默认情况下，CLR 将运行工作站垃圾回收，并在单处理器和多处理器计算机上同时启用并发垃圾回收。

下图演示了在单独的专用线程上执行的并发垃圾回收。

![并发垃圾回收线程](media/gc-concurrent.png)

## <a name="see-also"></a>请参阅

- [工作站和服务器垃圾回收](workstation-server-gc.md)
- [用于垃圾回收的运行时配置选项](../../core/run-time-config/garbage-collector.md)
