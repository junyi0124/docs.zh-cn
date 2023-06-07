---
title: ICorProfilerInfo 接口
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo
helpviewer_keywords:
- ICorProfilerInfo interface [.NET Framework profiling]
ms.assetid: eb4e4ce0-06e7-4469-bbc4-edc2eb5da4b1
topic_type:
- apiref
ms.openlocfilehash: a029784a28036e531670ad373893b4256c5864c4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95671182"
---
# <a name="icorprofilerinfo-interface"></a>ICorProfilerInfo 接口

提供一些方法，代码探查器可以使用这些方法与公共语言运行时 (CLR) 进行通信，以控制事件监视和请求信息。  
  
> [!NOTE]
> 接口中的每个方法都 `ICorProfilerInfo` 返回 HRESULT，以指示成功或失败。 有关可能的返回代码的列表，请参阅 CorError。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[BeginInprocDebugging 方法](icorprofilerinfo-begininprocdebugging-method.md)|初始化进程内调试支持。 此方法在 .NET Framework 版本2.0 中已过时。|  
|[EndInprocDebugging 方法](icorprofilerinfo-endinprocdebugging-method.md)|关闭进程内调试会话。 此方法在 .NET Framework 版本2.0 中已过时。|  
|[ForceGC 方法](icorprofilerinfo-forcegc-method.md)|强制在运行时内发生垃圾回收。|  
|[GetAppDomainInfo 方法](icorprofilerinfo-getappdomaininfo-method.md)|获取有关指定应用程序域的信息。|  
|[GetAssemblyInfo 方法](icorprofilerinfo-getassemblyinfo-method.md)|获取有关指定程序集的信息。|  
|[GetClassFromObject 方法](icorprofilerinfo-getclassfromobject-method.md)|获取 `ClassID` 的<br /><br /> 对象 `ObjectID` 。|  
|[GetClassFromToken 方法](icorprofilerinfo-getclassfromtoken-method.md)|给定元数据标记，获取类的 ID。 此方法在 .NET Framework 版本2.0 中已过时。 改为使用 [ICorProfilerInfo2：： GetClassFromTokenAndTypeArgs](icorprofilerinfo2-getclassfromtokenandtypeargs-method.md) 方法。|  
|[GetClassIDInfo 方法](icorprofilerinfo-getclassidinfo-method.md)|获取指定类的父模块和元数据标记。|  
|[GetCodeInfo 方法](icorprofilerinfo-getcodeinfo-method.md)|获取与指定函数 ID 关联的本机代码的范围。 此方法已过时。 改为使用 [ICorProfilerInfo2：： GetCodeInfo2](icorprofilerinfo2-getcodeinfo2-method.md) 方法。|  
|[GetCurrentThreadID 方法](icorprofilerinfo-getcurrentthreadid-method.md)|获取当前线程的 ID （如果它是托管线程）。|  
|[GetEventMask 方法](icorprofilerinfo-geteventmask-method.md)|获取探查器要从 CLR 接收事件通知的当前事件类别。|  
|[GetFunctionFromIP 方法](icorprofilerinfo-getfunctionfromip-method.md)|将托管代码指令指针映射到 `FunctionID` 。|  
|[GetFunctionFromToken 方法](icorprofilerinfo-getfunctionfromtoken-method.md)|获取函数的 ID。 此方法在 .NET Framework 版本2.0 中已过时。 改为使用 [ICorProfilerInfo2：： GetFunctionFromTokenAndTypeArgs](icorprofilerinfo2-getfunctionfromtokenandtypeargs-method.md) 方法。|  
|[GetFunctionInfo 方法](icorprofilerinfo-getfunctioninfo-method.md)|获取指定函数的父类和元数据标记。|  
|[GetHandleFromThread 方法](icorprofilerinfo-gethandlefromthread-method.md)|将线程的 ID 映射到 Win32 线程句柄。|  
|[GetILFunctionBody 方法](icorprofilerinfo-getilfunctionbody-method.md)|获取一个指针，该指针指向 Microsoft 中间语言 (MSIL) 代码的方法体，从其标头开始。|  
|[GetILFunctionBodyAllocator 方法](icorprofilerinfo-getilfunctionbodyallocator-method.md)|获取一个接口，该接口提供一个方法，用于分配用于在 MSIL 代码中交换方法的主体的内存。|  
|[GetILToNativeMapping 方法](icorprofilerinfo-getiltonativemapping-method.md)|获取指定函数中包含的代码的从 MSIL 偏移量到本机偏移量的映射。|  
|[GetInprocInspectionInterface 方法](icorprofilerinfo-getinprocinspectioninterface-method.md)|获取一个对象，该对象可用于查询 ICorDebugProcess 接口。 此方法在 .NET Framework 版本2.0 中已过时。|  
|[GetInprocInspectionIThisThread 方法](icorprofilerinfo-getinprocinspectionithisthread-method.md)|获取一个对象，该对象可查询 ICorDebugThread 接口。 此方法在 .NET Framework 版本2.0 中已过时。|  
|[GetModuleInfo 方法](icorprofilerinfo-getmoduleinfo-method.md)|给定模块 ID 后，将返回模块的文件名和模块的父程序集的 ID。|  
|[GetModuleMetaData 方法](icorprofilerinfo-getmodulemetadata-method.md)|获取映射到指定模块的元数据接口实例。|  
|[GetObjectSize 方法](icorprofilerinfo-getobjectsize-method.md)|获取指定对象的大小。|  
|[GetThreadContext 方法](icorprofilerinfo-getthreadcontext-method.md)|获取当前与指定线程关联的上下文标识。|  
|[GetThreadInfo 方法](icorprofilerinfo-getthreadinfo-method.md)|获取指定线程的当前 Win32 线程标识。|  
|[GetTokenAndMetadataFromFunction 方法](icorprofilerinfo-gettokenandmetadatafromfunction-method.md)|获取可用于指定函数的标记的元数据标记和元数据接口的实例。|  
|[IsArrayClass 方法](icorprofilerinfo-isarrayclass-method.md)|确定指定的类是否为数组类。|  
|[SetEnterLeaveFunctionHooks 方法](icorprofilerinfo-setenterleavefunctionhooks-method.md)|指定在托管函数的 "enter"、"leave" 和 "tailcall" 挂钩上调用的探查器实现函数。|  
|[SetEventMask 方法](icorprofilerinfo-seteventmask-method.md)|设置一个值，该值指定探查器要从 CLR 接收通知的事件类型。|  
|[SetFunctionIDMapper 方法](icorprofilerinfo-setfunctionidmapper-method.md)|指定将调用以将 `FunctionID` 值映射至替换值（传递至探查器的输入/退出挂钩）的探查器实现函数。|  
|[SetFunctionReJIT 方法](icorprofilerinfo-setfunctionrejit-method.md)|未实现。 请勿使用。|  
|[SetILFunctionBody 方法](icorprofilerinfo-setilfunctionbody-method.md)|替换指定模块中指定函数的主体。|  
|[SetILInstrumentedCodeMap 方法](icorprofilerinfo-setilinstrumentedcodemap-method.md)|指定指定函数的原始 MSIL 的偏移量如何映射到该函数的探查器修改后的新偏移量。|  
  
## <a name="remarks"></a>注解  

 探查器调用接口中的方法 `ICorProfilerInfo` ，以便与 CLR 通信以控制事件监视和请求信息。  
  
 接口的方法 `ICorProfilerInfo` 由 CLR 使用自由线程模型实现。 每个方法均返回一个 HRESULT，指示成功或失败。 有关可能的返回代码的列表，请参阅 CorError。  
  
 CLR 通过探查器的 [ICorProfilerCallback：： Initialize](icorprofilercallback-initialize-method.md)实现， `ICorProfilerInfo` 在初始化期间每个代码探查器的接口传递。 然后，代码探查器可以调用接口的方法 `ICorProfilerInfo` ，以获取有关在 CLR 控制下执行的托管代码的信息。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [分析接口](profiling-interfaces.md)
- [ICorProfilerInfo2 接口](icorprofilerinfo2-interface.md)
