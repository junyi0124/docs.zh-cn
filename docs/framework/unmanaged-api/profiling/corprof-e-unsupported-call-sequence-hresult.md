---
title: CORPROF_E_UNSUPPORTED_CALL_SEQUENCE HRESULT
ms.date: 03/30/2017
f1_keywords:
- CORPROF_E_UNSUPPORTED_CALL_SEQUENCE
helpviewer_keywords:
- CORPROF_E_UNSUPPORTED_CALL_SEQUENCE HRESULT [.NET Framework profiling]
ms.assetid: f2fc441f-d62e-4f72-a011-354ea13c8c59
ms.openlocfilehash: 1a75b51b57bdf2923ca6386f42c19c0b2f44fd39
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95717469"
---
# <a name="corprof_e_unsupported_call_sequence-hresult"></a>CORPROF_E_UNSUPPORTED_CALL_SEQUENCE HRESULT

CORPROF_E_UNSUPPORTED_CALL_SEQUENCE HRESULT 是在 .NET Framework 版本2.0 中引入的。 .NET Framework 4 在两个方案中返回此 HRESULT：  
  
- 当劫持探查器随时强行重置线程的注册上下文时，使该线程尝试访问处于不一致状态的结构。  
  
- 当探查器尝试调用一个信息性方法，该方法从禁止垃圾回收的回调方法触发垃圾回收。  
  
以下各节将讨论这两种情况。  
  
## <a name="hijacking-profilers"></a>劫持探查器  

   (此方案主要是劫持探查器的问题，尽管在某些情况下，非劫持探查器可以看到此 HRESULT。 )   
  
 在这种情况下，劫持探查器会在任意时间强制重置线程的注册上下文，以便线程可以输入探查器代码或通过 [ICorProfilerInfo](icorprofilerinfo-interface.md) 方法重新输入公共语言运行时 (CLR) 。  
  
 许多 Id 分析 API 提供的指向 CLR 中的数据结构。 许多 `ICorProfilerInfo` 调用只是从这些数据结构中读取信息，然后再将其传递回来。 但是，CLR 可能会在运行时更改这些结构中的某些组件，并且它可能会使用锁来执行此操作。 假设 CLR 已经持有 (或尝试在探查器截获线程时获取锁) 。 如果线程重新输入 CLR 并尝试使用更多锁或检查正在修改的结构，则这些结构可能处于不一致的状态。 在这种情况下，可能很容易发生死锁和访问冲突。  
  
 通常，当非劫持探查器在 [ICorProfilerCallback](icorprofilercallback-interface.md) 方法中执行代码，并 `ICorProfilerInfo` 使用有效参数调用方法时，它不应死锁或收到访问冲突。 例如，在 [ICorProfilerCallback：： ClassLoadFinished](icorprofilercallback-classloadfinished-method.md) 方法内运行的探查器代码可以通过调用 [ICorProfilerInfo2：： GetClassIDInfo2](icorprofilerinfo2-getclassidinfo2-method.md) 方法来请求有关类的信息。 该代码可能会收到 CORPROF_E_DATAINCOMPLETE HRESULT，以指示该信息不可用。 但是，它不会死锁或收到访问冲突。 这些对的调用被 `ICorProfilerInfo` 视为同步的，因为它们是通过 `ICorProfilerCallback` 方法进行的。  
  
 但是，执行不在方法中的代码的托管线程被 `ICorProfilerCallback` 视为进行异步调用。 在 .NET Framework 版本1中，很难确定异步调用中可能会发生的情况。 此调用可能会死锁、崩溃或发出无效的答案。 .NET Framework 版本2.0 引入了一些简单的检查，以帮助避免此问题。 在 .NET Framework 2.0 中，如果异步调用 unsafe `ICorProfilerInfo` 函数，则会失败并出现 CORPROF_E_UNSUPPORTED_CALL_SEQUENCE HRESULT。  
  
 通常，异步调用是不安全的。 但是，以下方法是安全的，并且专门支持异步调用：  
  
- [GetEventMask](icorprofilerinfo-geteventmask-method.md)  
  
- [SetEventMask](icorprofilerinfo-seteventmask-method.md)  
  
- [GetCurrentThreadID](icorprofilerinfo-getcurrentthreadid-method.md)  
  
- [GetThreadContext](icorprofilerinfo-getthreadcontext-method.md)  
  
- [GetThreadAppDomain](icorprofilerinfo2-getthreadappdomain-method.md)  
  
- [GetFunctionFromIP](icorprofilerinfo-getfunctionfromip-method.md)  
  
- [GetFunctionInfo](icorprofilerinfo-getfunctioninfo-method.md)  
  
- [GetFunctionInfo2](icorprofilerinfo2-getfunctioninfo2-method.md)  
  
- [GetCodeInfo](icorprofilerinfo-getcodeinfo-method.md)  
  
- [GetCodeInfo2](icorprofilerinfo2-getcodeinfo2-method.md)  
  
- [GetModuleInfo](icorprofilerinfo-getmoduleinfo-method.md)  
  
- [GetClassIDInfo](icorprofilerinfo-getclassidinfo-method.md)  
  
- [GetClassIDInfo2](icorprofilerinfo2-getclassidinfo2-method.md)  
  
- [IsArrayClass](icorprofilerinfo-isarrayclass-method.md)  
  
- [SetFunctionIDMapper](icorprofilerinfo-setfunctionidmapper-method.md)  
  
- [DoStackSnapshot](icorprofilerinfo2-dostacksnapshot-method.md)  
  
 有关详细信息，请参阅 CLR 分析 API 博客中的 [CORPROF_E_UNSUPPORTED_CALL_SEQUENCE](/archive/blogs/davbr/why-we-have-corprof_e_unsupported_call_sequence) 条目。  
  
## <a name="triggering-garbage-collections"></a>触发垃圾回收  

 此方案涉及在回调方法内运行的探查器 (例如， `ICorProfilerCallback` 禁止垃圾回收的方法之一) 。 如果探查器尝试调用信息方法 (例如，接口上的方法 `ICorProfilerInfo`) 可能触发垃圾回收，则信息性方法将失败，并 CORPROF_E_UNSUPPORTED_CALL_SEQUENCE HRESULT。  
  
 下表显示了禁止垃圾回收的回调方法，以及可能触发垃圾回收的信息性方法。 如果探查器在列出的回调方法之一内执行并调用列出的信息性方法之一，则该信息性方法将失败，并 CORPROF_E_UNSUPPORTED_CALL_SEQUENCE HRESULT。  
  
|禁止垃圾回收的回调方法|触发垃圾回收的信息性方法|  
|------------------------------------------------------|------------------------------------------------------------|  
|[ThreadAssignedToOSThread](icorprofilercallback-threadassignedtoosthread-method.md)<br /><br /> [ExceptionUnwindFunctionEnter](icorprofilercallback-exceptionunwindfunctionenter-method.md)<br /><br /> [ExceptionUnwindFunctionLeave](icorprofilercallback-exceptionunwindfunctionleave-method.md)<br /><br /> [ExceptionUnwindFinallyEnter](icorprofilercallback-exceptionunwindfinallyenter-method.md)<br /><br /> [ExceptionUnwindFinallyLeave](icorprofilercallback-exceptionunwindfinallyleave-method.md)<br /><br /> [ExceptionCatcherEnter](icorprofilercallback-exceptioncatcherenter-method.md)<br /><br /> [RuntimeSuspendStarted](icorprofilercallback-runtimesuspendstarted-method.md)<br /><br /> [RuntimeSuspendFinished](icorprofilercallback-runtimesuspendfinished-method.md)<br /><br /> [RuntimeSuspendAborted](icorprofilercallback-runtimesuspendaborted-method.md)<br /><br /> [RuntimeThreadSuspended](icorprofilercallback-runtimethreadsuspended-method.md)<br /><br /> [RuntimeThreadResumed](icorprofilercallback-runtimethreadresumed-method.md)<br /><br /> [MovedReferences](icorprofilercallback-movedreferences-method.md)<br /><br /> [ObjectReferences](icorprofilercallback-objectreferences-method.md)<br /><br /> [ObjectsAllocatedByClass](icorprofilercallback-objectsallocatedbyclass-method.md)<br /><br /> [RootReferences2](icorprofilercallback-rootreferences-method.md)<br /><br /> [HandleCreated](icorprofilercallback2-handlecreated-method.md)<br /><br /> [HandleDestroyed](icorprofilercallback2-handledestroyed-method.md)<br /><br /> [GarbageCollectionStarted](icorprofilercallback2-garbagecollectionstarted-method.md)<br /><br /> [GarbageCollectionFinished](icorprofilercallback2-garbagecollectionfinished-method.md)|[GetILFunctionBodyAllocator](icorprofilerinfo-getilfunctionbodyallocator-method.md)<br /><br /> [SetILFunctionBody](icorprofilerinfo-setilfunctionbody-method.md)<br /><br /> [SetILInstrumentedCodeMap](icorprofilerinfo-setilinstrumentedcodemap-method.md)<br /><br /> [ForceGC](icorprofilerinfo-forcegc-method.md)<br /><br /> [GetClassFromToken](icorprofilerinfo-getclassfromtoken-method.md)<br /><br /> [GetClassFromTokenAndTypeArgs](icorprofilerinfo2-getclassfromtokenandtypeargs-method.md)<br /><br /> [GetFunctionFromTokenAndTypeArgs](icorprofilerinfo2-getfunctionfromtokenandtypeargs-method.md)<br /><br /> [GetAppDomainInfo](icorprofilerinfo-getappdomaininfo-method.md)<br /><br /> [EnumModules](icorprofilerinfo3-enummodules-method.md)<br /><br /> [RequestProfilerDetach](icorprofilerinfo3-requestprofilerdetach-method.md)<br /><br /> [GetAppDomainsContainingModule](icorprofilerinfo3-getappdomainscontainingmodule-method.md)|  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerCallback 接口](icorprofilercallback-interface.md)
- [ICorProfilerCallback2 接口](icorprofilercallback2-interface.md)
- [ICorProfilerCallback3 接口](icorprofilercallback3-interface.md)
- [ICorProfilerInfo 接口](icorprofilerinfo-interface.md)
- [ICorProfilerInfo2 接口](icorprofilerinfo2-interface.md)
- [ICorProfilerInfo3 接口](icorprofilerinfo3-interface.md)
- [分析接口](profiling-interfaces.md)
- [分析](index.md)
