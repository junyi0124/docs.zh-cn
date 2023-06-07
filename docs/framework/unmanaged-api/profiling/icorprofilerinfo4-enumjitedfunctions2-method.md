---
title: ICorProfilerInfo4::EnumJITedFunctions2 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo4.EnumJITedFunctions2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo4::EnumJITedFunctions2
helpviewer_keywords:
- EnumJITedFunctions2 method, ICorProfilerInfo4 interface [.NET Framework profiling]
- ICorProfilerInfo4::EnumJITedFunctions2 method [.NET Framework profiling]
ms.assetid: 40e9a1be-9bd2-4fad-9921-34a84b61c1e3
topic_type:
- apiref
ms.openlocfilehash: 2a6ddaef7b64427f8349abedbbd85b4d82a0b88c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95697787"
---
# <a name="icorprofilerinfo4enumjitedfunctions2-method"></a>ICorProfilerInfo4::EnumJITedFunctions2 方法

返回之前 JIT 编译和 JIT 重新编译的所有函数的枚举器。 此方法将替换 [ICorProfilerInfo3：： EnumJITedFunctions](icorprofilerinfo3-enumjitedfunctions-method.md) 方法，该方法不枚举 JIT 重新编译的 id。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EnumJITedFunctions([out] ICorProfilerFunctionEnum** ppEnum);  
```  
  
## <a name="parameters"></a>参数  

 `ppEnum`  
 弄指向 [ICorProfilerFunctionEnum](icorprofilerfunctionenum-interface.md) 枚举器的指针。  
  
## <a name="remarks"></a>注解  

 此方法可能与 `JITCompilation` 回调（如 [ICorProfilerCallback：： JITCompilationStarted](icorprofilercallback-jitcompilationstarted-method.md) 方法）重叠。 返回的枚举包含字段的值 `COR_PRF_FUNCTION::reJitId` 。 此方法替换的 [ICorProfilerInfo3：： EnumJITedFunctions](icorprofilerinfo3-enumjitedfunctions-method.md) 方法不枚举 JIT 重新编译的 id，因为 `COR_PRF_FUNCTION::reJitId` 字段始终设置为0。 `ICorProfilerInfo4::EnumJITedFunctions`方法会枚举 JIT 重新编译的 id，因为 `COR_PRF_FUNCTION::reJitId` 字段设置正确。 请注意， [ICorProfilerInfo4：： EnumJITedFunctions2](icorprofilerinfo4-enumjitedfunctions2-method.md) 方法可以触发垃圾回收，而 [ICorProfilerInfo3：： EnumJITedFunctions 方法](icorprofilerinfo3-enumjitedfunctions-method.md) 将不会触发垃圾回收。  有关详细信息，请参阅 [CORPROF_E_UNSUPPORTED_CALL_SEQUENCE HRESULT](corprof-e-unsupported-call-sequence-hresult.md)。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [EnumJITedFunctions 方法](icorprofilerinfo3-enumjitedfunctions-method.md)
- [ICorProfilerInfo4 接口](icorprofilerinfo4-interface.md)
- [分析接口](profiling-interfaces.md)
- [分析](index.md)
