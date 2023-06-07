---
title: 调试、跟踪和分析
description: 了解如何在 .NET 中进行调试、跟踪和分析。 请参阅文章涵盖实时 (JIT) 调试、跟踪和检测应用程序等。
ms.date: 03/30/2017
helpviewer_keywords:
- debugging [.NET Framework]
- .NET Framework application configuration, debugging
- debugging [.NET Framework], .NET Framework application debugging
- troubleshooting applications [.NET Framework], profiling
- application development [.NET Framework], debugging
- .NET Framework application configuration, profiling
- profiling applications
- troubleshooting applications [.NET Framework], debugging
- troubleshooting applications [.NET Framework]
- application development [.NET Framework], profiling
ms.assetid: 4a04863e-2475-46f4-bc3f-3c11510c2a4b
ms.openlocfilehash: 33dd840f4c1421bbff54499af56ab3e147cc694b
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96272769"
---
# <a name="debugging-tracing-and-profiling"></a>调试、跟踪和分析

若要调试 .NET Framework 应用程序，编译器和运行时环境必须配置为可将调试程序附加到该应用程序，并且如果可能的话，为该应用程序及其相应 Microsoft 中间语言 (MSIL) 同时生成符号和行映射。 在对托管应用程序进行调试后，可对其进行分析以增强性能。 分析计算并描述可生成最常执行的代码的源代码行，以及执行它们所需的时间。  
  
 通过使用 Visual Studio 可轻松地调试.NET framework 应用程序，前者用于处理配置的许多详细信息。 如果未安装 Visual Studio，可以通过使用 .NET Framework <xref:System.Diagnostics> 命名空间中的调试类检查并提升 .NET Framework 应用程序的性能。 此命名空间包括用于跟踪执行流的 <xref:System.Diagnostics.Trace>、<xref:System.Diagnostics.Debug> 和 <xref:System.Diagnostics.TraceSource> 类，以及用于分析代码的 <xref:System.Diagnostics.Process>、<xref:System.Diagnostics.EventLog> 和 <xref:System.Diagnostics.PerformanceCounter> 类。  
  
## <a name="in-this-section"></a>本节内容  

 [启用 JIT 附加调试](enabling-jit-attach-debugging.md)  
 演示如何配置注册表从而将调试引擎以 JIT 方式附加到 .NET Framework 应用程序。  
  
 [令映像更易于调试](making-an-image-easier-to-debug.md)  
 演示如何打开 JIT 跟踪和关闭优化，以使程序集更易于调试。  
  
 [跟踪应用程序和在应用程序中插入检测点](tracing-and-instrumenting-applications.md)  
 描述如何监视应用程序在运行时的执行，以及如何来检测它以显示其执行状态或是否出现了问题。  
  
 [使用托管调试助手诊断错误](diagnosing-errors-with-managed-debugging-assistants.md)  
 描述托管调试助手 (MDA)，它是与公共语言运行时 (CLR) 联合工作以提供有关运行时状态信息的调试辅助程序。  
  
 [使用调试器显示特性增强调试](enhancing-debugging-with-the-debugger-display-attributes.md)  
 描述某种类型的开发人员可如何指定该类型在调试器中显示时的样子。  
  
 [性能计数器](performance-counters.md)  
 描述可用来跟踪应用程序性能的计数器。  
  
## <a name="related-sections"></a>相关章节  

 [在 Visual Studio 中调试 ASP.NET 或 ASP.NET Core 应用](/visualstudio/debugger/how-to-enable-debugging-for-aspnet-applications)  
 提供有关如何在开发期间或部署后调试 ASP.NET 应用程序的先决条件和说明。  
  
 [开发指南](../development-guide.md)  
 提供了有关应用程序开发的所有关键技术区域和任务（包括创建、配置、调试、保护和部署应用程序）的指南，以及有关动态编程、互操作性、扩展性、内存管理和线程处理的信息。
