---
title: ICorProfilerInfo3::SetEnterLeaveFunctionHooks3 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo3.SetEnterLeaveFunctionHooks3 Method
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo3::SetEnterLeaveFunctionHooks3
helpviewer_keywords:
- SetEnterLeaveFunctionHooks3 method [.NET Framework profiling]
- ICorProfilerInfo3::SetEnterLeaveFunctionHooks3 method [.NET Framework profiling]
ms.assetid: f0621465-b84f-40ab-a4e5-56a7abc776a7
topic_type:
- apiref
ms.openlocfilehash: a7272d55771620db129125ce543d12d19a0b4dfb
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95697839"
---
# <a name="icorprofilerinfo3setenterleavefunctionhooks3-method"></a>ICorProfilerInfo3::SetEnterLeaveFunctionHooks3 方法

指定将在 [FunctionEnter3](functionenter3-function.md)、 [FunctionLeave3](functionleave3-function.md)和 [FunctionTailcall3](functiontailcall3-function.md) 函数上调用的探查器实现函数。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetEnterLeaveFunctionHooks3(  
            [in] FunctionEnter3    *pFuncEnter3,  
            [in] FunctionLeave3    *pFuncLeave3,  
            [in] FunctionTailcall3 *pFuncTailcall3);  
```  
  
## <a name="parameters"></a>参数  

 `pFuncEnter3`  
 中指向要用作回调的实现的指针 `FunctionEnter3` 。  
  
 `pFuncLeave3`  
 中指向要用作回调的实现的指针 `FunctionLeave3` 。  
  
 `pFuncTailcall3`  
 中指向要用作回调的实现的指针 `FunctionTailcall3` 。  
  
## <a name="remarks"></a>注解  

 [FunctionEnter3](functionenter3-function.md)、 [FunctionLeave3](functionleave3-function.md)和 [FunctionTailcall3](functiontailcall3-function.md) 挂钩不提供堆栈帧和参数检查。 若要访问该信息， `COR_PRF_ENABLE_FUNCTION_ARGS` `COR_PRF_ENABLE_FUNCTION_RETVAL` 必须设置、和/或  `COR_PRF_ENABLE_FRAME_INFO` 标志。 探查器可以使用 [ICorProfilerInfo：： SetEventMask](icorprofilerinfo-seteventmask-method.md) 方法来设置事件标志，然后使用 [ICorProfilerInfo3：： SetEnterLeaveFunctionHooks3WithInfo](icorprofilerinfo3-setenterleavefunctionhooks3withinfo-method.md) 方法来注册此函数的实现。  
  
 一次只能有一组回调处于活动状态，最新版本优先。 因此，如果探查器同时调用 [SetEnterLeaveFunctionHooks2 方法](icorprofilerinfo2-setenterleavefunctionhooks2-method.md) 和 `SetEnterLeaveFunctionHooks3` 方法， `SetEnterLeaveFunctionHooks3` 则使用。  
  
 `SetEnterLeaveFunctionHooks3`只能从探查器的[ICorProfilerCallback：： Initialize](icorprofilercallback-initialize-method.md)回调调用此方法。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [SetEnterLeaveFunctionHooks3WithInfo](icorprofilerinfo3-setenterleavefunctionhooks3withinfo-method.md)
- [FunctionEnter3](functionenter3-function.md)
- [FunctionLeave3](functionleave3-function.md)
- [FunctionTailcall3](functiontailcall3-function.md)
- [FunctionEnter3WithInfo](functionenter3withinfo-function.md)
- [FunctionLeave3WithInfo](functionleave3withinfo-function.md)
- [FunctionTailcall3WithInfo](functiontailcall3withinfo-function.md)
- [分析全局静态函数](profiling-global-static-functions.md)
- [ICorProfilerInfo3](icorprofilerinfo3-interface.md)
- [分析接口](profiling-interfaces.md)
- [分析](index.md)
