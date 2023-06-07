---
title: 线程与线程处理
description: 了解线程处理，如进程和线程、何时使用多个线程，以及如何在 .NET 中使用多线程处理提高响应能力或吞吐量。
ms.date: 11/08/2018
helpviewer_keywords:
- multiple threads
- threading [.NET]
- threading [.NET], multiple threads
ms.assetid: 5baac3aa-e603-4fa6-9f89-0f2c1084e6b1
ms.openlocfilehash: 20550b597e27c75f00d16528871007988dd6b885
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94819534"
---
# <a name="threads-and-threading"></a>线程与线程处理

借助多线程处理，可以提高应用程序的响应能力，如果应用程序在多处理器或多核系统上运行，则可提高其吞吐量。

## <a name="processes-and-threads"></a>进程和线程

进程是一种正在执行的程序。 操作系统使用进程来分隔正在执行的应用程序。 线程是操作系统向其分配处理器时间的基本单元。 每个线程具有[计划优先级](scheduling-threads.md)并维护系统用于保存线程执行暂停时线程上下文的一组结构。 线程上下文包含线程顺畅继续执行所需的全部信息，包括线程的一组 CPU 寄存器和堆栈。 多个线程可在进程上下文中运行。 进程的所有线程共享其虚拟地址空间。 线程可执行任意部分的程序代码，包括其他线程正在执行的部分。

> [!NOTE]
> .NET Framework 通过使用应用程序域，提供在进程中隔离应用程序的方法。 （应用程序域不适用于 .NET Core。）有关详细信息，请参阅[应用程序域](../../framework/app-domains/application-domains.md)一文中的[应用程序域和线程](../../framework/app-domains/application-domains.md#application-domains-and-threads)部分。

默认情况下，.NET 程序由单个线程（通常称为主线程）启动。 但是，它可以创建其他线程，以与主线程并行或同时执行代码。 这些线程通常称为工作线程。

## <a name="when-to-use-multiple-threads"></a>何时使用多个线程

使用多线程可以提高应用程序的响应能力，并利用多处理器或多核系统提高应用程序吞吐量。

请思考桌面应用程序，其主线程负责用户界面元素并响应用户操作。 使用工作线程执行耗时的操作（这些操作会占用主线程并使用户界面无响应）。 此外，可以使用专用线程，提高网络或设备通信对传入消息或事件的响应能力。

如果程序执行可并行执行的操作，可通过在单独线程中执行这些操作并在多处理器或多核系统中运行程序减少总执行时间。 在此类系统中，使用多线程处理可能会提高吞吐量和响应能力。

## <a name="how-to-use-multithreading-in-net"></a>如何在 .NET 中使用多线程处理

从 .NET Framework 4 开始，使用多线程的推荐方法是使用[任务并行库 (TPL)](../parallel-programming/task-parallel-library-tpl.md) 和[并行 LINQ (PLINQ)](../parallel-programming/introduction-to-plinq.md)。 有关详细信息，请参阅[并行编程](../parallel-programming/index.md)。

TPL 和 PLINQ 依赖于 <xref:System.Threading.ThreadPool> 线程。 <xref:System.Threading.ThreadPool?displayProperty=nameWithType> 类为 .NET 应用程序提供工作线程池。 还可使用线程池线程。 有关详细信息，请参阅[托管线程池](the-managed-thread-pool.md)。

最后，可以使用表示托管线程的 <xref:System.Threading.Thread?displayProperty=nameWithType> 类。 有关详细信息，请参阅[使用线程和线程处理](using-threads-and-threading.md)。

多个线程可能需要访问共享的资源。 要使资源保持未损坏的状态并避免争用条件，必须同步对资源的线程访问。 可能还需要协调多个线程的交互。 .NET 提供了一系列可用于同步对共享资源或协调线程交互的访问的类型。 有关详细信息，请参阅[同步基元概述](overview-of-synchronization-primitives.md)。

请务必处理线程异常。 线程中未经处理的异常通常会终止进程。 有关详细信息，请参阅[托管线程异常](exceptions-in-managed-threads.md)。

## <a name="see-also"></a>请参阅

- [线程处理对象和功能](threading-objects-and-features.md)
- [托管线程处理的最佳做法](managed-threading-best-practices.md)
- [.NET 中的并行处理、并发和异步编程](../parallel-processing-and-concurrency.md)
- [关于进程和线程](/windows/desktop/procthread/about-processes-and-threads)
