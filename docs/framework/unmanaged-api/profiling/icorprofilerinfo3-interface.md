---
title: ICorProfilerInfo3 接口
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo3
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo3
helpviewer_keywords:
- ICorProfilerInfo3 interface [.NET Framework profiling]
ms.assetid: 044a262f-0fa7-485d-b0c1-64cdc359c654
topic_type:
- apiref
ms.openlocfilehash: 9944234da1677608aec10066b61bfc6a6cb72bcb
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95697852"
---
# <a name="icorprofilerinfo3-interface"></a>ICorProfilerInfo3 接口

提供了一些方法，可由代码探查器用于与公共语言运行时 (CLR) 通信，从而控制事件监视并请求信息。 `ICorProfilerInfo3`接口是[ICorProfilerInfo2](icorprofilerinfo2-interface.md)接口的扩展。 它提供 .NET Framework 4 及更高版本中支持的新方法。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[EnumJITedFunctions 方法](icorprofilerinfo3-enumjitedfunctions-method.md)|返回所有先前 JIT 编译的函数的枚举器。|  
|[EnumModules 方法](icorprofilerinfo3-enummodules-method.md)|返回一个枚举器，此枚举器提供方法以按顺序循环访问加载到应用程序的托管模块集合。|  
|[GetAppDomainsContainingModule 方法](icorprofilerinfo3-getappdomainscontainingmodule-method.md)|获取其中已加载给定模块的应用程序域的标识符。|  
|[GetFunctionEnter3Info 方法](icorprofilerinfo3-getfunctionenter3info-method.md)|提供由 [FunctionEnter3WithInfo](functionenter3withinfo-function.md) 函数向探查器报告的函数的堆栈帧和参数信息;只能在回调过程中调用 `FunctionEnter3WithInfo` 。|  
|[GetFunctionLeave3Info 方法](icorprofilerinfo3-getfunctionleave3info-method.md)|提供由 [FunctionLeave3WithInfo 函数](functionleave3withinfo-function.md) 函数向探查器报告的函数的堆栈帧和返回值;只能在回调过程中调用 `FunctionLeave3WithInfo` 。|  
|[GetFunctionTailcall3Info 方法](icorprofilerinfo3-getfunctiontailcall3info-method.md)|提供由 [FunctionTailcall3WithInfo](functiontailcall3withinfo-function.md) 函数向探查器报告的函数的堆栈帧;只能在回调过程中调用 `FunctionTailcall3WithInfo` 。|  
|[GetModuleInfo2 方法](icorprofilerinfo3-getmoduleinfo2-method.md)|若给定模块 ID，返回模块的文件名、模块父程序集的 ID 以及描述模块属性的位掩码。|  
|[GetRuntimeInformation 方法](icorprofilerinfo3-getruntimeinformation-method.md)|提供有关正在探查的运行时的版本信息。|  
|[GetStringLayout2 方法](icorprofilerinfo3-getstringlayout2-method.md)|获取有关字符串对象布局的信息。|  
|[GetThreadStaticAddress2 方法](icorprofilerinfo3-getthreadstaticaddress2-method.md)|获取指定线程和应用程序域范围内的指定线程静态字段的地址。|  
|[RequestProfilerDetach 方法](icorprofilerinfo3-requestprofilerdetach-method.md)|指示运行时分离探查器。|  
|[SetEnterLeaveFunctionHooks3 方法](icorprofilerinfo3-setenterleavefunctionhooks3-method.md)|指定将在 [FunctionEnter3](functionenter3-function.md)、 [FunctionLeave3](functionleave3-function.md)和 [FunctionTailcall3](functiontailcall3-function.md) 函数上调用的探查器实现函数。|  
|[SetEnterLeaveFunctionHooks3WithInfo 方法](icorprofilerinfo3-setenterleavefunctionhooks3withinfo-method.md)|指定将在托管函数的 [FunctionEnter3WithInfo](functionenter3withinfo-function.md)、 [FunctionLeave3WithInfo](functionleave3withinfo-function.md)和 [FunctionTailcall3WithInfo](functiontailcall3withinfo-function.md) 挂钩上调用的探查器实现函数。|  
|[SetFunctionIDMapper2 方法](icorprofilerinfo3-setfunctionidmapper2-method.md)|指定将调用以将 `FunctionID` 值映射至替换值（传递至探查器的输入/退出挂钩）的探查器实现函数。 此方法将 [ICorProfilerInfo：： SetFunctionIDMapper](icorprofilerinfo-setfunctionidmapper-method.md) 与探查器可能用于消除运行时之间的歧义的参数进行扩展。|  
  
## <a name="remarks"></a>注解  

 CLR 通过使用自由线程模型实现 `ICorProfilerInfo3` 接口的方法。 每个方法均返回一个 HRESULT，指示成功或失败。 有关可能的返回代码的列表，请参阅 CorError.h 文件。  
  
 在 `ICorProfilerInfo3` 初始化期间，使用探查器的 [ICorProfilerCallback：： Initialize](icorprofilercallback-initialize-method.md) 或 [ICorProfilerCallback3：： InitializeForAttach](icorprofilercallback3-initializeforattach-method.md) 方法的实现将一个接口传递到每个代码探查器。 然后，代码探查器可调用 `ICorProfilerInfo3` 方法获取有关正在 CLR 控件下执行的托管代码的信息。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [分析接口](profiling-interfaces.md)
- [ICorProfilerInfo 接口](icorprofilerinfo-interface.md)
