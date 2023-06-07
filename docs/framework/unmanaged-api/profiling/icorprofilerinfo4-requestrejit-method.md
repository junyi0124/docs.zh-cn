---
title: ICorProfilerInfo4::RequestReJIT 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo4.RequestReJIT
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo4::RequestReJIT
helpviewer_keywords:
- RequestReJIT method, ICorProfilerInfo4 interface [.NET Framework profiling]
- ICorProfilerInfo4::RequestReJIT method [.NET Framework profiling]
ms.assetid: 781ed736-f30c-4816-920e-3552e36542c6
topic_type:
- apiref
ms.openlocfilehash: 92e2901b2c228e4b774fbb1dbcc66cd0b4f82087
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95733849"
---
# <a name="icorprofilerinfo4requestrejit-method"></a>ICorProfilerInfo4::RequestReJIT 方法

请求 JIT 重新编译指定函数的所有实例。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT RequestReJIT (  
   [in] ULONG    cFunctions,  
   [in, size_is(cFunctions)]  ModuleID    moduleIds[],  
   [in, size_is(cFunctions)]  mdMethodDef methodIds[]);  
```  
  
## <a name="parameters"></a>参数  

 `cFunctions`  
 [in] 要重新编译的函数数目。  
  
 `moduleIds`  
 [in] 指定（`module`、`methodDef`）对的 `moduleId` 部分，它标识要重新编译的函数。  
  
 `methodIds`  
 [in] 指定（`module`、`methodDef`）对的 `methodId` 部分，它标识要重新编译的函数。  
  
## <a name="return-value"></a>返回值  

 此方法返回以下特定 HRESULT 以及表示方法失败的 HRESULT 错误。  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|尝试将所有方法标记为 JIT 重新编译。 探查器必须实现 [ICorProfilerCallback4：： ReJITError](icorprofilercallback4-rejiterror-method.md) 方法来确定哪些方法已成功标记为 JIT 重新编译。|  
|CORPROF_E_CALLBACK4_REQUIRED|探查器必须实现 [ICorProfilerCallback4](icorprofilercallback4-interface.md) 接口，以便支持此调用。|  
|CORPROF_E_REJIT_NOT_ENABLED|尚未启用 JIT 重新编译。 必须通过使用 [ICorProfilerInfo：： SetEventMask](icorprofilerinfo-seteventmask-method.md) 方法设置标志来在初始化期间启用 JIT 重新编译 `COR_PRF_ENABLE_REJIT` 。|  
|E_INVALIDARG|`cFunctions` 为 0，或者 `moduleIds` 或 `methodIds` 为 `NULL`。|  
|||  
|E_OUTOFMEMORY|CLR 无法完成请求，因为它已耗尽内存。|  
  
## <a name="remarks"></a>注解  

 调用 `RequestReJIT` 以使运行时重新编译一组指定的函数。 然后，代码探查器可以使用 [ICorProfilerFunctionControl](icorprofilerfunctioncontrol-interface.md) 接口来调整重新编译函数时生成的代码。 这不会影响当前正在执行的函数，仅影响将来的函数调用。 如果此前已 JIT 重新编译任意指定函数，请求重新编译是指还原和重新编译函数。 若要保留可还原性，当 JIT 编译器编译函数的原始版本时，将仅考虑被调用方用于内联决定的原始版本。 JIT 编译器重新编译函数时，将考虑被调用方用于内联的当前版本（重新编译版本或原始版本）。  
  
 探查器通常会调用 `RequestReJIT` 以响应用户输入对探查器检测一个或多个方法的请求。 `RequestReJIT` 通常将运行时挂起以执行其中部分工作，这有可能触发垃圾回收。 因此，探查器应从其先前创建的线程调用 `RequestReJIT`，而不从 CLR 创建的且当前正在执行探查器回调的线程调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo4 接口](icorprofilerinfo4-interface.md)
- [分析接口](profiling-interfaces.md)
- [分析](index.md)
