---
title: 分析概述
ms.date: 03/30/2017
helpviewer_keywords:
- managed code, profiling API support
- unmanaged code, combining with managed code in profiling
- notification threads [.NET Framework profiling]
- unmanaged code, profiling
- profiling API [.NET Framework], and COM
- profiling API [.NET Framework], unmanaged code profiling
- profilers, writing
- profiling API [.NET Framework], call stacks
- code profilers, writing
- profiling API [.NET Framework], security considerations
- profiling API [.NET Framework], managed code support
- common language runtime, profiling
- profiling API [.NET Framework], notification threads
- call stacks [.NET Framework profiling]
- profiling API [.NET Framework], stack depth
- common language runtime, writing a profiler
- profiling API [.NET Framework], information retrieval interfaces
- shadow stacks [.NET Framework profiling]
- COM, using in the profiling API
- stack snapshots [.NET Framework profiling]
- profiling API [.NET Framework], supported features
- profiling API [.NET Framework], overview
- security, profiling API considerations
- stack depth [.NET Framework profiling]
ms.assetid: 864c2344-71dc-46f9-96b2-ed59fb6427a8
ms.openlocfilehash: cf29260c36437aaf679498f648d0fcac5d65f321
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90558324"
---
# <a name="profiling-overview"></a>分析概述

探查器是一种工具，可监视另一个应用程序的执行情况。 公共语言运行时 (CLR) 探查器是一个动态链接库 (DLL)，具有使用分析 API 从 CLR 中接收消息以及向 CLR 发送消息的功能。 CLR 在运行时加载探查器 DLL。

传统的分析工具专注于测量应用程序的执行情况。 即测量随着时间的推移应用程序使用每个功能或内存所花费的时间。 而分析 API 则针对更广泛的诊断工具，如代码覆盖率实用程序，甚至高级的调试辅助工具。 实际上，它们全都用于诊断用途。 分析 API 不仅测量而且还监视应用程序的执行情况。 基于此原因，应用程序本身始终不应使用分析 API 并且应用程序的执行情况不应依赖于探查器或受探查器影响。

与分析常规编译的机器代码相比，分析 CLR 应用程序需要更多支持。 这是因为 CLR 引进了诸如应用程序域、垃圾回收、托管异常处理、实时 (JIT) 代码编译（将 Microsoft 中间语言、MSIL 或代码转换为本机代码）以及类似功能的概念。 常规的分析机制无法识别或提供关于这些功能的有用信息。 分析 API 则有效地提供缺少的信息，并对 CLR 和分析的应用程序的性能产生最小的影响。

运行时的 JIT 编译可以很好地进行分析。 分析 API 允许探查器在对某一例程进行 JIT 编译之前更改该例程的内存中 MSIL 代码流。 探查器可以利用这种方式向需要深入调查的特定例程动态添加检测代码。 尽管此种方法在常规方案中可用，但是通过使用分析 API 可以更轻松地实现 CLR。

## <a name="the-profiling-api"></a>分析 API

通常情况下，分析 API 用于编写 *代码探查器，代码探查器*是一种监视托管应用程序执行情况的程序。

分析 API 由探查器 DLL 使用，加载到与所分析应用程序相同的进程中。 探查器 DLL 实现 .NET Framework 版本1.0 和1.1，在版本2.0 和更高版本[) 中 (](icorprofilercallback2-interface.md) [ICorProfilerCallback](icorprofilercallback-interface.md)的回调接口。 CLR 调用该接口的方法，以通知探查器所分析进程中发生的事件。 探查器可以通过使用 [ICorProfilerInfo](icorprofilerinfo-interface.md) 和 [ICorProfilerInfo2](icorprofilerinfo2-interface.md) 接口中的方法返回到运行时，以获取有关所分析应用程序状态的信息。

> [!NOTE]
> 只有探查器解决方案的数据收集部分才能在与所分析应用程序相同的进程中运行。 所有用户界面和数据分析都应在单独的进程中执行。

下图显示探查器 DLL 如何与所分析应用程序和 CLR 交互。

![显示分析体系结构的屏幕截图。](./media/profiling-overview/profiling-architecture.png)

### <a name="the-notification-interfaces"></a>通知接口

可以将[ICorProfilerCallback](icorprofilercallback-interface.md)和[ICorProfilerCallback2](icorprofilercallback2-interface.md)视为通知接口。 这些接口包括 [ClassLoadStarted](icorprofilercallback-classloadstarted-method.md)、 [ClassLoadFinished](icorprofilercallback-classloadfinished-method.md)和 [JITCompilationStarted](icorprofilercallback-jitcompilationstarted-method.md)等方法。 每次 CLR 进行加载或卸载类、编译函数等操作时，都会调用探查器的 `ICorProfilerCallback` 或 `ICorProfilerCallback2` 接口中的相应方法。

例如，探查器可以通过两个通知函数（ [FunctionEnter2](functionenter2-function.md) 和 [FunctionLeave2](functionleave2-function.md)）来度量代码性能。 它会对每个通知添加时间戳、累积结果并输出一个列表指示在应用程序执行期间哪个函数占用的 CPU 最多或消耗的时钟时间最长。

### <a name="the-information-retrieval-interfaces"></a>信息检索接口

分析中涉及的其他主要界面是 [ICorProfilerInfo](icorprofilerinfo-interface.md) 和 [ICorProfilerInfo2](icorprofilerinfo2-interface.md)。 探查器根据需要调用这些接口，以获取更多的信息来帮助进行分析。 例如，每当 CLR 调用 [FunctionEnter2](functionenter2-function.md) 函数时，它都会提供函数标识符。 探查器可以通过调用 [ICorProfilerInfo2：： GetFunctionInfo2](icorprofilerinfo2-getfunctioninfo2-method.md) 方法来获取有关该函数的详细信息，以发现该函数的父类、名称，等等。

## <a name="supported-features"></a>支持的功能

分析 API 提供公共语言运行时中发生的各种事件和操作的相关信息。 你可以使用此信息来监视进程的内部工作情况，也可分析 .NET Framework 应用程序的性能。

分析 API 检索 CLR 中发生的以下操作和事件的相关信息：

- CLR 启动和关闭事件。

- 应用程序域创建和关闭事件。

- 程序集加载和卸载事件。

- 模块加载和卸载事件。

- COM vtable 创建和析构事件。

- 实时 (JIT) 编译和代码间距调整事件。

- 类加载和卸载事件。

- 线程创建和析构事件。

- 函数入口和退出事件。

- 异常。

- 托管和非托管代码执行之间的转换。

- 不同运行时上下文之间的转换。

- 有关运行时挂起的信息。

- 有关运行时内存堆和垃圾回收活动的信息。

分析 API 可从任何（非托管）COM 兼容语言调用。

API 可高效减少 CPU 和内存占用。 分析不包括对所分析应用程序进行足以导致误导性结果的更改。

分析 API 有益于采样和非采样探查器。 *采样探查器*定期检查配置文件，如5毫秒。 *非采样探查器*与导致事件的线程同步通知事件。

### <a name="unsupported-functionality"></a>不支持的功能

分析 API 不支持以下功能：

- 非托管代码，此类代码必须使用常规 Win32 方法进行分析。 然而，CLR 探查器包括过渡事件，以确定托管与非托管代码之间的边界。

- 自修改应用程序，即出于某种目的（如面向方面的编程）修改自己的代码。

- 边界检查，原因是分析 API 不提供此信息。 CLR 为所有托管代码的边界检查提供内部支持。

- 远程分析，在以下情况不受支持：

  - 远程分析延长执行时间。 当使用分析接口时，必须最大限度地缩短执行时间，确保分析结果不会受到不良影响。 在执行性能受到监视时尤为如此。 然而，当分析接口用于监视内存使用情况或用于获取有关堆栈帧、对象等的运行时信息时，远程分析并不是一个限制。

  - CLR 代码探查器必须向运行所分析应用程序的本地计算机上的运行时注册一个或多个回调接口。 这便限制了创建远程代码探查器的功能。

## <a name="notification-threads"></a>通知线程

在大多数情况下，生成事件的线程也会执行通知。 此类通知 (例如， [FunctionEnter](functionenter-function.md) 和 [FunctionLeave](functionleave-function.md)) 无需提供显式 `ThreadID` 。 此外，探查器还可能决定使用线程本地存储来存储和更新其分析块，而不是基于受影响线程的 `ThreadID` 对全局存储中的分析块建立索引。

注意，这些回调未经过序列化。 用户必须通过创建线程安全数据结构并在必要时锁定探查器代码以防止从多个线程并行访问的方式保护代码。 因此，在某些情况下，会收到不正常的回调序列。 例如，假设托管应用程序正在生成执行相同代码的两个线程。 在这种情况下，在[ICorProfilerCallback::JITCompilationStarted](icorprofilercallback-jitcompilationstarted-method.md) `FunctionEnter` 接收[ICorProfilerCallback：： JITCompilationFinished](icorprofilercallback-jitcompilationfinished-method.md)回调之前，可以从一个线程接收某个函数的 ICorProfilerCallback：： JITCompilationStarted 事件，并从另一个线程接收回调。 在这种情况下，用户将收到可能尚未完全实时 (JIT) 编译的函数的 `FunctionEnter` 回调。

## <a name="security"></a>安全性

探查器 DLL 是作为公共语言运行时执行引擎的一部分运行的非托管 DLL。 因此，探查器 DLL 中的代码并不受到托管代码访问安全性的约束。 探查器 DLL 的唯一限制是操作系统强加在运行所分析应用程序的用户身上的限制。

探查器作者应采取适当的预防措施以避免安全相关问题。 例如，在安装期间，应将探查器 DLL 添加到访问控制列表 (ACL)，以确保恶意用户无法对其进行修改。

## <a name="combining-managed-and-unmanaged-code-in-a-code-profiler"></a>在代码探查器中组合托管和非托管代码

编写错误的探查器可能会引起自身的循环引用，从而导致不可预知的行为。

审查 CLR 分析 API 可能会形成这样的印象：可以编写包含托管和非托管组件的探查器，在探查器中这些组件通过 COM 互操作或间接调用相互调用。

虽然从设计角度而言这是可行的，但分析 API 并不支持托管组件。 CLR 探查器必须完全处于非托管状态。 尝试在 CLR 探查器中组合托管和非托管代码可能会导致访问冲突、程序故障或死锁。 探查器的托管组件将激发事件返回其非托管组件，随后将再次调用托管组件，从而导致循环引用。

CLR 探查器可以安全调用托管代码的唯一位置是方法的 Microsoft 中间语言 (MSIL) 体。 修改 MSIL 体的建议做法是在 [ICorProfilerCallback4](icorprofilercallback4-interface.md) 接口中使用 JIT 重新编译方法。

还可以使用较早的检测方法修改 MSIL。 在实时 (JIT) 完成函数的编译之前，探查器可以在方法的 MSIL 体中插入托管调用，然后对其进行 JIT 编译， (参见 [ICorProfilerInfo：： GetILFunctionBody](icorprofilerinfo-getilfunctionbody-method.md) 方法) 。 这种技术可成功用于托管代码的选择性检测，也可用于收集有关 JIT 的统计信息和性能数据。

代码探查器也可以在调用到非托管代码的每个托管函数的 MSIL 体中插入本机挂钩。 这种技术可用于检测和覆盖。 例如，代码探查器可以在每个 MSIL 块后插入检测挂钩，以确保该块已执行。 修改方法的 MSIL 体是一项非常细致的操作，因此必须将许多因素考虑在内。

## <a name="profiling-unmanaged-code"></a>分析非托管代码

公共语言运行时 (CLR) 分析 API 为分析非托管代码提供最低支持。 它具有以下功能：

- 堆栈链的枚举。 借助此功能，代码探查器能够确定托管代码与非托管代码之间的边界。

- 确定堆栈链是否与托管代码或本机代码对应。

在 .NET Framework 1.0 和 1.1 版中，可通过 CLR 调试 API 的进程内子集使用这些方法。 它们在 CorDebug.idl 文件中定义。

在 .NET Framework 2.0 及更高版本中，可以使用 [ICorProfilerInfo2：:D ostacksnapshot](icorprofilerinfo2-dostacksnapshot-method.md) 方法来实现此功能。

## <a name="using-com"></a>使用 COM

尽管将分析接口定义为 COM 接口，但公共语言运行时 (CLR) 并不会实际初始化 COM 以使用这些接口。 原因是为了避免在托管应用程序有机会指定其所需的线程模型之前，必须使用 [CoInitialize](/windows/desktop/api/objbase/nf-objbase-coinitialize) 函数设置线程模型。 同样，探查器本身不应调用 `CoInitialize`，因为它可能会选取与所分析应用程序不兼容的线程模型，并可能会导致应用程序失败。

## <a name="call-stacks"></a>调用堆栈

分析 API 提供两种方法来获取调用堆栈：堆栈快照方法和阴影堆栈方法，前者以分散方式收集调用堆栈，后者时刻跟踪调用堆栈。

### <a name="stack-snapshot"></a>堆栈快照

堆栈快照是线程堆栈在某一时刻的跟踪。 分析 API 支持在堆栈上跟踪托管函数，但它会将跟踪非托管函数的工作交给探查器自己的堆栈审核器来完成。

有关如何对探查器进行编程以遍历托管堆栈的详细信息，请参阅本文档集中的 [ICorProfilerInfo2：:D ostacksnapshot](icorprofilerinfo2-dostacksnapshot-method.md) 方法和 [探查器堆栈遍历 .NET Framework 2.0：基础和更](/previous-versions/dotnet/articles/bb264782(v=msdn.10))高版本。

### <a name="shadow-stack"></a>阴影堆栈

过度频繁使用快照方法很快就会产生性能问题。 如果希望频繁地使用堆栈跟踪，探查器应改为通过使用 [FunctionEnter2](functionenter2-function.md)、 [FunctionLeave2](functionleave2-function.md)、 [FunctionTailcall2](functiontailcall2-function.md)和 [ICorProfilerCallback2](icorprofilercallback2-interface.md) 异常回调来生成阴影堆栈。 阴影堆栈始终是最新的，并且在需要堆栈快照时可以快速复制到存储区。

阴影堆栈可以获取函数自变量、返回值和有关泛型实例化的信息。 泛型实例化信息只能通过阴影堆栈获取，并可能在将控件传递到函数时获取。 然而，后续运行此函数时，此信息可能不可用。

## <a name="callbacks-and-stack-depth"></a>回调和堆栈深度

在堆栈受到严重限制的情况下，可能会引发探查器回调；并且探查器回调中的堆栈溢出将导致进程立即退出。 探查器应确保尽可能少使用堆栈来响应回调。 如果希望将探查器用于抑制堆栈溢出的可靠进程中，那么探查器本身也应避免触发堆栈溢出。

## <a name="related-topics"></a>相关主题

|Title|说明|
|-----------|-----------------|
|[设置分析环境](setting-up-a-profiling-environment.md)|说明如何初始化探查器、设置事件通知和分析 Windows 服务。|
|[分析接口](profiling-interfaces.md)|描述分析 API 使用的非托管接口。|
|[分析全局静态函数](profiling-global-static-functions.md)|描述分析 API 使用的非托管全局静态函数。|
|[分析枚举](profiling-enumerations.md)|描述分析 API 使用的非托管枚举。|
|[分析结构](profiling-structures.md)|描述分析 API 使用的非托管结构。|
