---
title: ICorProfilerInfo3::EnumJITedFunctions 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo3.EnumJITedFunctions Method
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo3::EnumJITedFunctions
helpviewer_keywords:
- ICorProfilerInfo3::EnumJITedFunctions method [.NET Framework profiling]
- EnumJITedFunctions method [.NET Framework profiling]
ms.assetid: e2847a36-f460-45e2-9b6c-b33b008f40d9
topic_type:
- apiref
ms.openlocfilehash: 6227baaead518eae2de5913369b72de1072ac052
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95681491"
---
# <a name="icorprofilerinfo3enumjitedfunctions-method"></a>ICorProfilerInfo3::EnumJITedFunctions 方法

返回之前 JIT 编译的所有函数的枚举器。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EnumJITedFunctions([out] ICorProfilerFunctionEnum** ppEnum);  
```  
  
## <a name="parameters"></a>参数  

 `ppEnum`  
 弄指向 [ICorProfilerFunctionEnum](icorprofilerfunctionenum-interface.md) 枚举器的指针。  
  
## <a name="remarks"></a>注解  

 此方法可能与 `JITCompilation` 回调（如 [ICorProfilerCallback：： JITCompilationStarted](icorprofilercallback-jitcompilationstarted-method.md) 方法）重叠。 此方法返回的枚举器不包含从使用 Ngen.exe 生成的本机映像加载的函数。  
  
> [!NOTE]
> 对于字段的值，返回的枚举只包含 "0" `COR_PRF_FUNCTION::reJitId` 。  如果需要有效值 `COR_PRF_FUNCTION::reJitId` ，请使用 [ICorProfilerInfo4：： EnumJITedFunctions2](icorprofilerinfo4-enumjitedfunctions2-method.md) 方法。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo3 接口](icorprofilerinfo3-interface.md)
- [分析接口](profiling-interfaces.md)
- [分析](index.md)
