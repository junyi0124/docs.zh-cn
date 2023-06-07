---
title: .NET Framework 性能
description: 设计和规划 .NET 应用程序的性能。 使用 Microsoft 提供的工具来衡量应用程序的性能并做出改进。
ms.date: 03/30/2017
helpviewer_keywords:
- performance [.NET Framework]
- reliability [.NET Framework]
ms.assetid: c1676cca-3f1a-41ec-b469-9029566074fc
ms.openlocfilehash: f15cd9b38285e88dac727bcfe51c8dc542a735f6
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96277980"
---
# <a name="net-framework-performance"></a>.NET Framework 性能

如果想要创建一个具有卓越性能的应用，你应该像设计应用的任何其他功能一样设计和规划性能。 你可以使用由 Microsoft 所提供的用于测量你的应用性能的工具，并对内存使用、代码吞吐量和响应能力进行改进（如果需要）。 本主题列出了 Microsoft 提供的性能分析工具，并提供了介绍应用开发特定领域性能的其他主题的链接。  
  
## <a name="designing-and-planning-for-performance"></a>性能的设计和规划  

 如果你想要一个性能卓越的应用，你必须像设计任何其他功能一样，在设计应用时集成性能。 你必须确定应用中的性能关键方案，设定性能目标，以及尽早且经常为这些应用方案测量性能。 因为每个应用是不同的，并且具有不同的性能关键执行路径，尽早确定那些路径并集中精力，使你能最大限度地提高工作效率。  
  
 你不必完全熟悉目标平台来创建高性能的应用。 然而，你应对目标平台的哪些部分在性能方面成本高昂有基本的了解。 你可以通过在开发过程中尽早执行性能测量来做到这一点。  
  
 若要确定对性能至关重要的领域并建立性能目标，始终考虑用户体验。 启动时间和响应能力将是影响应用的用户感觉的两个关键领域。 如果你的应用使用大量内存，它对于用户可能显得缓慢或者影响系统上运行的其他应用，或者在某些情况下，它可能使 Windows 或 Windows Phone 应用商店的提交过程失败。 此外，如果你确定了执行更频繁的代码部分，则你可以确保代码的这些部分也进行了很好地优化。  
  
## <a name="analyzing-performance"></a>分析性能  

 作为你整体开发计划的一部分，在你将测量应用性能的开发过程中设置要点，并与你之前设定的目标比较结果。 在你希望用户所具有的环境和硬件中测量应用。 通过尽早并经常分析你的应用的性能，你可以更改那些在开发周期后期修复成本极为昂贵的体系结构决策。 以下各部分描述你可以用来分析应用和讨论事件跟踪的性能工具，该事件跟踪由这些工具使用。  
  
### <a name="performance-tools"></a>性能工具  

 以下是一些你可以与 .NET Framework 应用一起使用的性能工具。  
  
|工具|说明|  
|----------|-----------------|  
|Visual Studio 性能分析|用于分析你的 .NET Framework 应用的 CPU 使用情况，该应用将部署到运行 Windows 操作系统的计算机。<br /><br /> 在打开一个项目后，此工具可以从 Visual Studio 中的“调试”菜单处获取。 有关更多信息，请参见 [性能资源管理器](/visualstudio/profiling/performance-explorer)。 **注意**：面向 Windows Phone 时，请使用 Windows Phone 应用程序分析（参见下一行）。|  
|Windows Phone 应用程序分析|用于分析在你的 Windows Phone 应用中的 CPU 和内存、网络数据传输率、应用响应能力和电池消耗。<br /><br /> 安装 [Windows Phone SDK](https://go.microsoft.com/fwlink/?LinkId=265773) 后，此工具可以从 Visual Studio 中 Windows Phone 项目的“调试”菜单获取。 有关详细信息，请参阅 [Windows Phone 8 的应用程序分析](/previous-versions/windows/apps/jj215908(v=vs.105))。|  
|[PerfView](https://www.microsoft.com/download/details.aspx?id=28567)|用于识别 CPU 以及与内存相关的性能问题。 此工具使用针对 Windows (ETW) 和 CLR 分析 API 的事件跟踪以提供高级内存和 CPU 调查以及关于垃圾回收和 JIT 编译的信息。 有关如何使用 PerfView 的详细信息，请参阅应用附带的教程和帮助文件：[第 9 频道视频教程](https://channel9.msdn.com/Series/PerfView-Tutorial)和[博客文章](/archive/blogs/vancem/)。<br /><br /> 对于特定于内存的问题，请参阅[使用 PerfView 进行内存调查](https://channel9.msdn.com/Series/PerfView-Tutorial/PerfView-Tutorial-9-NET-Memory-Investigation-Basics-of-GC-Heap-Snapshots)。|  
|[Windows Performance Analyzer](https://www.microsoft.com/p/windows-performance-analyzer/9n0w1b2bxgnz?activetab=pivot:overviewtab)|用于确定整体系统性能，如在多个应用在同一台计算机上运行时，你的应用的内存和存储使用情况。 在 windows 8 (ADK) 的 Windows 评估和部署工具包中，此工具可从下载中心获取。 有关详细信息，请参阅 [Windows Performance Analyzer](/windows-hardware/test/wpt/windows-performance-analyzer)。|
  
### <a name="event-tracing-for-windows-etw"></a>针对 Windows (ETW) 的事件跟踪  

 ETW 是一种技术，可以让你获取有关运行代码的诊断信息，并且对于很多前面提到的性能工具而言必不可少。 在特定的事件由 .NET Framework 应用和 Windows 引发时，ETW 将创建日志。 使用 ETW，你可以动态启用和禁用日志记录，这样就可以在不重新启动应用的情况下在生产环境中进行详细的跟踪。 .NET Framework 为 ETW 事件提供支持，并且 ETW 供许多分析和性能工具使用以生成性能数据。 这些工具经常启用和禁用 ETW 事件，因此熟悉它们很有帮助。 你可以使用特定 ETW 事件以收集关于你的应用的特定组件的性能信息。 有关 .NET Framework 中 ETW 支持的详细信息，请参阅[公共语言运行时中的 ETW 事件](etw-events-in-the-common-language-runtime.md)和[任务并行库和 PLINQ 中的 ETW 事件](etw-events-in-task-parallel-library-and-plinq.md)。  
  
## <a name="performance-by-app-type"></a>按应用程序类型的性能  

 每种 .NET Framework 应用都具有它自己的最佳实践、注意事项以及评估性能的工具。 下表提供指向特定 .NET Framework 应用类型的性能主题的链接。  
  
|应用类型|查看|  
|--------------|---------|  
|针对所有平台的 .NET Framework 应用|[垃圾回收和性能](../../standard/garbage-collection/performance.md)<br /><br /> [性能提示](performance-tips.md)|  
|用 c + +、c # 和 Visual Basic 编写的 Windows 8.x 应用商店应用|[使用 C++、C# 和 Visual Basic 的 Windows 应用商店应用的性能最佳做法](/previous-versions/windows/apps/hh750313(v=win.10))|  
|Windows Presentation Foundation (WPF)|[WPF 性能套件](/previous-versions/dotnet/netframework-4.0/aa969767(v=vs.100))|  
|ASP.NET|[ASP.NET 性能概述](/previous-versions/aspnet/cc668225(v=vs.100))|  
  
## <a name="related-topics"></a>相关主题  
  
|Title|描述|  
|-----------|-----------------|  
|[.NET Framework 应用程序中的缓存](caching-in-net-framework-applications.md)|描述用于缓存数据以提高应用性能的技术。|  
|[延迟初始化](lazy-initialization.md)|描述如何按需初始化对象以提高性能，尤其是在应用启动时。|  
|[可靠性](reliability.md)|提供有关在服务器环境中防止异步异常的信息。|  
|[编写大型的响应式 .NET Framework 应用](writing-large-responsive-apps.md)|提供从托管代码中重写的 C# 和 Visual Basic 编译器收集的性能提示，并且包括来自 C# 编译器的几个真实示例。|
