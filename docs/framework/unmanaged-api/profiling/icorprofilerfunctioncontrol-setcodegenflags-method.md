---
title: ICorProfilerFunctionControl::SetCodegenFlags 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerFunctionControl.SetCodegenFlags
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerFunctionControl::SetCodegenFlags
helpviewer_keywords:
- ICorProfilerFunctionControl::SetCodegenFlags method [.NET Framework profiling]
- SetCodegenFlags method, ICorProfilerFunctionControl interface [.NET Framework profiling]
ms.assetid: a2d5daa5-b990-4ae5-bf2a-c0862fe58bd7
topic_type:
- apiref
ms.openlocfilehash: 3593b07759b4d6feee239042e5aabaf0876fdd1c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95716299"
---
# <a name="icorprofilerfunctioncontrolsetcodegenflags-method"></a>ICorProfilerFunctionControl::SetCodegenFlags 方法

设置 [COR_PRF_CODEGEN_FLAGS](cor-prf-codegen-flags-enumeration.md) 枚举中的一个或多个标志，以控制实时 (JIT) 重新编译函数的代码生成。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetCodegenFlags(  
    [in] DWORD flags);  
```  
  
## <a name="parameters"></a>参数  

 `flags`  
 中 [COR_PRF_CODEGEN_FLAGS](cor-prf-codegen-flags-enumeration.md) 枚举中的一个或多个标志。  
  
## <a name="remarks"></a>注解  

 探查器通过 [ICorProfilerCallback4：： GetReJITParameters](icorprofilercallback4-getrejitparameters-method.md) 回调获取此接口的实例。 `SetCodegenFlags` 允许探查器控制重新编译函数的代码生成。 与所有其他 JIT 重新编译参数一样，代码生成标志适用于函数的所有实例。  
  
 编译函数时，JIT 编译器会将这些编译标志连同其他源指定的其他标志一起考虑。  其他源包括调试器、启动时探查器设置的全局标志（使用 [ICorProfilerInfo：： SetEventMask](icorprofilerinfo-seteventmask-method.md) 方法 (与值 `COR_PRF_DISABLE_INLINING` 和 `COR_PRF_DISABLE_OPTIMIZATIONS`) 以及探查器的 [ICorProfilerCallback：： JITInlining](icorprofilercallback-jitinlining-method.md) 回调。  JIT 编译器为请求最小优化量的源提供优先级别。  例如，如果探查器 `COR_PRF_DISABLE_INLINING` 在启动时指定，但未 `COR_PRF_CODEGEN_DISABLE_INLINING` 在 [ICorProfilerFunctionControl：： SetCodegenFlags](icorprofilerfunctioncontrol-setcodegenflags-method.md) 回调中指定，内联仍处于禁用状态。  同样，如果探查器未 `COR_PRF_CODEGEN_DISABLE_INLINING` 在中指定 `SetCodegenFlags` ，但随后通过使用 [ICorProfilerCallback：： JITInlining](icorprofilercallback-jitinlining-method.md) 回调禁用内联，则禁用内联。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerFunctionControl 接口](icorprofilerfunctioncontrol-interface.md)
