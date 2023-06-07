---
title: ICorProfilerCallback 接口
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback
helpviewer_keywords:
- ICorProfilerCallback interface [.NET Framework profiling]
ms.assetid: 4bae06f7-94d7-4ba8-b250-648b2da78674
topic_type:
- apiref
ms.openlocfilehash: 8451f100f9e1b8d68045050d1b584ae44c29195d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95684053"
---
# <a name="icorprofilercallback-interface"></a>ICorProfilerCallback 接口

提供公共语言运行时 (CLR) 用于在探查器订阅的事件发生时通知代码探查器的方法。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[AppDomainCreationFinished 方法](icorprofilercallback-appdomaincreationfinished-method.md)|通知探查器已创建应用程序域。|  
|[AppDomainCreationStarted 方法](icorprofilercallback-appdomaincreationstarted-method.md)|通知探查器正在创建应用程序域。|  
|[AppDomainShutdownFinished 方法](icorprofilercallback-appdomainshutdownfinished-method.md)|通知探查器已从进程中卸载应用程序域。|  
|[AppDomainShutdownStarted 方法](icorprofilercallback-appdomainshutdownstarted-method.md)|通知探查器正在从进程中卸载应用程序域。|  
|[AssemblyLoadFinished 方法](icorprofilercallback-assemblyloadfinished-method.md)|通知探查器程序集已完成加载。|  
|[AssemblyLoadStarted 方法](icorprofilercallback-assemblyloadstarted-method.md)|通知探查器正在加载程序集。|  
|[AssemblyUnloadFinished 方法](icorprofilercallback-assemblyunloadfinished-method.md)|通知探查器已卸载程序集。|  
|[AssemblyUnloadStarted 方法](icorprofilercallback-assemblyunloadstarted-method.md)|通知探查器正在卸载程序集。|  
|[ClassLoadFinished 方法](icorprofilercallback-classloadfinished-method.md)|通知探查器类已经完成加载。|  
|[ClassLoadStarted 方法](icorprofilercallback-classloadstarted-method.md)|通知探查器正在加载某个类。|  
|[ClassUnloadFinished 方法](icorprofilercallback-classunloadfinished-method.md)|通知探查器类已经完成卸载。|  
|[ClassUnloadStarted 方法](icorprofilercallback-classunloadstarted-method.md)|通知探查器正在卸载某个类。|  
|[COMClassicVTableCreated 方法](icorprofilercallback-comclassicvtablecreated-method.md)|通知探查器，运行时可调用包装器 (为指定 IID 和类创建了 RCW) 。|  
|[COMClassicVTableDestroyed 方法](icorprofilercallback-comclassicvtabledestroyed-method.md)|通知探查器正在销毁某个 RCW。|  
|[ExceptionCatcherEnter 方法](icorprofilercallback-exceptioncatcherenter-method.md)|通知探查器控制正在传递到适当的 `catch` 块。|  
|[ExceptionCatcherLeave 方法](icorprofilercallback-exceptioncatcherleave-method.md)|通知探查器控制正在传递到适当的 `catch` 块。|  
|[ExceptionCLRCatcherExecute 方法](icorprofilercallback-exceptionclrcatcherexecute-method.md)|在 .NET Framework 版本2.0 中已过时。|  
|[ExceptionCLRCatcherFound 方法](icorprofilercallback-exceptionclrcatcherfound-method.md)|在 .NET Framework 2.0 中已过时。|  
|[ExceptionOSHandlerEnter 方法](icorprofilercallback-exceptionoshandlerenter-method.md)|未实现。 需要非托管异常信息的探查器必须通过其他方式获取此信息。|  
|[ExceptionOSHandlerLeave 方法](icorprofilercallback-exceptionoshandlerleave-method.md)|未实现。 需要非托管异常信息的探查器必须通过其他方式获取此信息。|  
|[ExceptionSearchCatcherFound 方法](icorprofilercallback-exceptionsearchcatcherfound-method.md)|通知探查器，异常处理的搜索阶段已找到引发的异常的处理程序。|  
|[ExceptionSearchFilterEnter 方法](icorprofilercallback-exceptionsearchfilterenter-method.md)|通知探查器正在执行用户筛选器。|  
|[ExceptionSearchFilterLeave 方法](icorprofilercallback-exceptionsearchfilterleave-method.md)|通知探查器用户筛选器刚刚执行完毕。|  
|[ExceptionSearchFunctionEnter 方法](icorprofilercallback-exceptionsearchfunctionenter-method.md)|通知探查器异常处理的搜索阶段已输入一个函数。|  
|[ExceptionSearchFunctionLeave 方法](icorprofilercallback-exceptionsearchfunctionleave-method.md)|通知探查器异常处理的搜索阶段已经完成了对函数的搜索。|  
|[ExceptionThrown 方法](icorprofilercallback-exceptionthrown-method.md)|通知探查器引发了异常。|  
|[ExceptionUnwindFinallyEnter 方法](icorprofilercallback-exceptionunwindfinallyenter-method.md)|通知探查器异常处理的展开阶段正在输入 `finally` 指定函数中包含的子句。|  
|[ExceptionUnwindFinallyLeave 方法](icorprofilercallback-exceptionunwindfinallyleave-method.md)|通知探查器异常处理的展开阶段已离开 `finally` 子句。|  
|[ExceptionUnwindFunctionEnter 方法](icorprofilercallback-exceptionunwindfunctionenter-method.md)|通知探查器异常处理的展开阶段已输入一个函数。|  
|[ExceptionUnwindFunctionLeave 方法](icorprofilercallback-exceptionunwindfunctionleave-method.md)|通知探查器异常处理的展开阶段已完成展开某个函数。|  
|[FunctionUnloadStarted 方法](icorprofilercallback-functionunloadstarted-method.md)|通知探查器运行时已开始卸载某个函数。|  
|[Initialize 方法](icorprofilercallback-initialize-method.md)|调用以在新的 CLR 应用程序启动时初始化探查器。|  
|[JITCachedFunctionSearchFinished 方法](icorprofilercallback-jitcachedfunctionsearchfinished-method.md)|通知探查器搜索已完成先前使用 NGen.exe 编译的函数。|  
|[JITCachedFunctionSearchStarted 方法](icorprofilercallback-jitcachedfunctionsearchstarted-method.md)|通知探查器搜索已开始使用 NGen.exe 以前编译过的函数。|  
|[JITCompilationFinished 方法](icorprofilercallback-jitcompilationfinished-method.md)|通知探查器 JIT 编译器已经完成了对函数的编译。|  
|[JITCompilationStarted 方法](icorprofilercallback-jitcompilationstarted-method.md)|通知探查器实时 (JIT) 编译器已开始编译函数。|  
|[JITFunctionPitched 方法](icorprofilercallback-jitfunctionpitched-method.md)|通知探查器已从内存中删除了已进行 JIT 编译的函数。|  
|[JITInlining 方法](icorprofilercallback-jitinlining-method.md)|通知探查器 JIT 编译器即将使用另一个函数插入一个函数。|  
|[ManagedToUnmanagedTransition 方法](icorprofilercallback-managedtounmanagedtransition-method.md)|通知探查器已发生从托管代码到非托管代码的转换。|  
|[ModuleAttachedToAssembly 方法](icorprofilercallback-moduleattachedtoassembly-method.md)|通知探查器模块正在附加到其父程序集。|  
|[ModuleLoadFinished 方法](icorprofilercallback-moduleloadfinished-method.md)|通知探查器模块已完成加载。|  
|[ModuleLoadStarted 方法](icorprofilercallback-moduleloadstarted-method.md)|通知探查器正在加载模块。|  
|[ModuleUnloadFinished 方法](icorprofilercallback-moduleunloadfinished-method.md)|通知探查器模块已完成卸载。|  
|[ModuleUnloadStarted 方法](icorprofilercallback-moduleunloadstarted-method.md)|通知探查器正在卸载模块。|  
|[MovedReferences 方法](icorprofilercallback-movedreferences-method.md)|通知探查器有关垃圾回收期间移动的对象引用。|  
|[ObjectAllocated 方法](icorprofilercallback-objectallocated-method.md)|通知探查器已为对象分配堆中的内存。|  
|[ObjectReferences 方法](icorprofilercallback-objectreferences-method.md)|通知探查器有关指定的对象引用的内存中的对象。|  
|[ObjectsAllocatedByClass 方法](icorprofilercallback-objectsallocatedbyclass-method.md)|通知探查器自上次垃圾回收后已创建的每个指定类的实例数。|  
|[RemotingClientInvocationFinished 方法](icorprofilercallback-remotingclientinvocationfinished-method.md)|通知探查器远程处理调用已在客户端上完成运行。|  
|[RemotingClientInvocationStarted 方法](icorprofilercallback-remotingclientinvocationstarted-method.md)|通知探查器已开始远程调用。|  
|[RemotingClientReceivingReply 方法](icorprofilercallback-remotingclientreceivingreply-method.md)|通知探查器远程处理调用的服务器端部分已完成，并且客户端现在正在接收，并即将处理答复。|  
|[RemotingClientSendingMessage 方法](icorprofilercallback-remotingclientsendingmessage-method.md)|通知探查器客户端正在向服务器发送请求。|  
|[RemotingServerInvocationReturned 方法](icorprofilercallback-remotingserverinvocationreturned-method.md)|通知探查器进程已完成调用方法以响应远程方法调用请求。|  
|[RemotingServerInvocationStarted 方法](icorprofilercallback-remotingserverinvocationstarted-method.md)|通知探查器进程正在调用方法以响应远程方法调用请求。|  
|[RemotingServerReceivingMessage 方法](icorprofilercallback-remotingserverreceivingmessage-method.md)|通知探查器进程正在接收远程方法调用或激活请求。|  
|[RemotingServerSendingReply 方法](icorprofilercallback-remotingserversendingreply-method.md)|通知探查器进程已处理完远程方法调用请求，即将通过通道传输答复。|  
|[RootReferences 方法](icorprofilercallback-rootreferences-method.md)|在垃圾回收后通知探查器有关根引用的信息。|  
|[RuntimeResumeFinished 方法](icorprofilercallback-runtimeresumefinished-method.md)|通知探查器运行时已恢复所有运行时线程并返回到正常操作。|  
|[RuntimeResumeStarted 方法](icorprofilercallback-runtimeresumestarted-method.md)|通知探查器运行时正在恢复所有运行时线程。|  
|[RuntimeSuspendAborted 方法](icorprofilercallback-runtimesuspendaborted-method.md)|通知探查器运行时已中止所发生的运行时挂起。|  
|[RuntimeSuspendFinished 方法](icorprofilercallback-runtimesuspendfinished-method.md)|通知探查器运行时已完成所有运行时线程的挂起。|  
|[RuntimeSuspendStarted 方法](icorprofilercallback-runtimesuspendstarted-method.md)|通知探查器运行时将要挂起所有运行时线程。|  
|[RuntimeThreadResumed 方法](icorprofilercallback-runtimethreadresumed-method.md)|通知探查器指定的线程在挂起后已恢复。|  
|[RuntimeThreadSuspended 方法](icorprofilercallback-runtimethreadsuspended-method.md)|通知探查器指定的线程已或即将挂起。|  
|[Shutdown 方法](icorprofilercallback-shutdown-method.md)|通知探查器应用程序正在关闭。|  
|[ThreadAssignedToOSThread 方法](icorprofilercallback-threadassignedtoosthread-method.md)|通知探查器使用特定操作系统 (OS) 线程实现了托管线程。|  
|[ThreadCreated 方法](icorprofilercallback-threadcreated-method.md)|通知探查器已创建一个线程。|  
|[ThreadDestroyed 方法](icorprofilercallback-threaddestroyed-method.md)|通知探查器线程已销毁。|  
|[UnmanagedToManagedTransition 方法](icorprofilercallback-unmanagedtomanagedtransition-method.md)|通知探查器已发生从非托管代码到托管代码的转换。|  
  
## <a name="remarks"></a>注解  

 CLR 在 `ICorProfilerCallback` (或 [ICorProfilerCallback2](icorprofilercallback2-interface.md)) 接口中调用方法，以便在探查器已订阅的事件发生时通知探查器。 这是 CLR 与代码探查器进行通信时所使用的主回调接口。  
  
 代码探查器必须实现接口的方法 `ICorProfilerCallback` 。 对于 .NET Framework 版本2.0 或更高版本，探查器还必须实现 `ICorProfilerCallback2` 方法。 每个方法实现都必须返回值为 "S_OK" 的 HRESULT，否则失败时 E_FAIL。 目前，CLR 将忽略每个回调返回的 HRESULT （ [ICorProfilerCallback：： ObjectReferences](icorprofilercallback-objectreferences-method.md)除外）。  
  
 在 Microsoft Windows 注册表中，代码探查器必须将其组件对象模型注册 (COM) 实现 `ICorProfilerCallback` 和接口的对象 `ICorProfilerCallback2` 。 代码探查器通过调用 [ICorProfilerInfo：： SetEventMask](icorprofilerinfo-seteventmask-method.md)订阅要接收通知的事件。 通常在探查器的 [ICorProfilerCallback：： Initialize](icorprofilercallback-initialize-method.md)实现中完成此操作。 然后，探查器可以在事件即将发生或刚刚发生在正在执行的运行时进程中时接收来自运行时的通知。  
  
> [!NOTE]
> 探查器将注册一个 COM 对象。 如果探查器面向 .NET Framework 版本1.0 或1.1，则该 COM 对象只需实现的方法 `ICorProfilerCallback` 。 如果目标 .NET Framework 版本2.0 或更高版本，则 COM 对象还必须实现的方法 `ICorProfilerCallback2` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [分析接口](profiling-interfaces.md)
- [ICorProfilerCallback2 接口](icorprofilercallback2-interface.md)
- [ICorProfilerCallback3 接口](icorprofilercallback3-interface.md)
- [ICorProfilerCallback4 接口](icorprofilercallback4-interface.md)
