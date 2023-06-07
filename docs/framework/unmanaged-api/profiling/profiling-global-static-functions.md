---
title: 分析全局静态函数
ms.date: 03/30/2017
helpviewer_keywords:
- global static functions [.NET Framework profiling]
- profiling global static functions [.NET Framework]
- unmanaged global static functions [.NET Framework], profiling
ms.assetid: 08a13a57-dc49-488d-b937-31e3051fda97
ms.openlocfilehash: 1b0ad42e6b34e99212e112f6a594b0a36b6715e1
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723072"
---
# <a name="profiling-global-static-functions"></a>分析全局静态函数

本部分介绍分析 API 使用的非托管 API 函数。  
  
## <a name="in-this-section"></a>本节内容  
  
## <a name="net-framework-version-1-profiling-functions"></a>.NET Framework 版本1分析函数  

 [FunctionEnter 函数](functionenter-function.md)  
 通知探查器控制正在传递到函数。 .NET Framework 2.0 中弃用。  
  
 [FunctionLeave 函数](functionleave-function.md)  
 通知探查器某个函数将要返回到调用方。 .NET Framework 2.0 中弃用。  
  
 [FunctionTailcall 函数](functiontailcall-function.md)  
 通知探查器，当前正在执行的函数即将对另一个函数执行尾调用。 .NET Framework 2.0 中弃用。  
  
## <a name="net-framework-version-2-profiling-functions"></a>.NET Framework 版本2分析函数  

 [FunctionIDMapper 函数](functionidmapper-function.md)  
 通知探查器可能会将函数的给定标识符重新映射到备用 ID，以在该函数的 [FunctionEnter2](functionenter2-function.md)、 [FunctionLeave2](functionleave2-function.md)和 [FunctionTailcall2](functiontailcall2-function.md) 回调中使用。 还允许探查器指示是否要为该函数接收回调  
  
 [FunctionEnter2 函数](functionenter2-function.md)  
 通知探查器控制正在传递到函数，并提供有关堆栈帧和函数参数的信息。 在 .NET Framework 4 中已弃用。  
  
 [FunctionLeave2 函数](functionleave2-function.md)  
 通知探查器函数将要返回到调用方，并提供有关堆栈帧和函数返回值的信息。 在 .NET Framework 4 中已弃用。  
  
 [FunctionTailcall2 函数](functiontailcall2-function.md)  
 通知探查器，当前正在执行的函数即将对另一个函数执行尾调用，并提供有关堆栈帧的信息。 在 .NET Framework 4 中已弃用。  
  
 [StackSnapshotCallback 函数](stacksnapshotcallback-function.md)  
 在堆栈遍历期间，为探查器提供有关每个托管帧和每个托管帧的每个运行的信息，由 [ICorProfilerInfo2：:D ostacksnapshot](icorprofilerinfo2-dostacksnapshot-method.md) 方法启动。  
  
## <a name="net-framework-version-4-profiling-functions"></a>.NET Framework 版本4分析函数  

 [FunctionIDMapper2 函数](functionidmapper2-function.md)  
 通知探查器可能会将函数的给定标识符重新映射到备用 ID，该 ID 将在 [FunctionEnter3](functionenter3-function.md)、 [FunctionLeave3](functionleave3-function.md)和 [FunctionTailcall3](functiontailcall3-function.md)中使用，或者为该函数的[FunctionEnter3WithInfo](functionenter3withinfo-function.md)、 [FunctionLeave3WithInfo](functionleave3withinfo-function.md)和 [FunctionTailcall3WithInfo](functiontailcall3withinfo-function.md) 回调。 还允许探查器指示是否要接收该函数的回调。  
  
 `FunctionIDMapper2` 使用参数扩展 [FunctionIDMapper](functionidmapper-function.md) 函数 `clientData` ，探查器可能会使用该参数来消除运行时之间的歧义。  
  
 [FunctionEnter3 函数](functionenter3-function.md)  
 通知探查器控制正在传递到函数。  
  
 [FunctionEnter3WithInfo 函数](functionenter3withinfo-function.md)  
 通知探查器控制正在传递到函数，并提供一个可传递给 [ICorProfilerInfo3：： GetFunctionEnter3Info](icorprofilerinfo3-getfunctionenter3info-method.md) 以检索堆栈帧和函数参数的句柄。  
  
 [FunctionLeave3 函数](functionleave3-function.md)  
 通知探查器控制正在从函数返回。  
  
 [FunctionLeave3WithInfo 函数](functionleave3withinfo-function.md)  
 通知探查器正在从某个函数返回控件，并提供一个可传递给 [ICorProfilerInfo3：： GetFunctionLeave3Info](icorprofilerinfo3-getfunctionleave3info-method.md) 以检索堆栈帧和返回值的句柄。  
  
 [FunctionTailcall3 函数](functiontailcall3-function.md)  
 通知探查器，当前正在执行的函数即将对另一个函数执行尾调用。  
  
 [FunctionTailcall3WithInfo 函数](functiontailcall3withinfo-function.md)  
 通知探查器，当前正在执行的函数即将对另一个函数执行尾调用，并提供一个可传递给 [ICorProfilerInfo3：： GetFunctionTailcall3Info](icorprofilerinfo3-getfunctiontailcall3info-method.md) 以检索堆栈帧的句柄。  
  
## <a name="related-sections"></a>相关章节  

 [分析概述](profiling-overview.md)  
  
 [分析接口](profiling-interfaces.md)  
  
 [分析枚举](profiling-enumerations.md)  
  
 [分析结构](profiling-structures.md)
