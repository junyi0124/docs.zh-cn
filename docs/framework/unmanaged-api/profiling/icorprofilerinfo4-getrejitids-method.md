---
title: ICorProfilerInfo4::GetReJITIDs 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo4.GetReJITIDs
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo4::GetReJITIDs
helpviewer_keywords:
- GetReJITIDs method, ICorProfilerInfo4 interface [.NET Framework profiling]
- ICorProfilerInfo4::GetReJITIDs method [.NET Framework profiling]
ms.assetid: 634ac28c-a5b7-4fc3-af84-256c24ca8177
topic_type:
- apiref
ms.openlocfilehash: 2ac645cbaaa45554568874653be016e4691bbd2f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95707472"
---
# <a name="icorprofilerinfo4getrejitids-method"></a>ICorProfilerInfo4::GetReJITIDs 方法

返回 Id 的数组，这些 Id 标识仍分配的指定函数的所有 JIT 重新编译版本。 这包括 JIT 重新编译的函数版本，这些版本已恢复但尚未释放 (例如，当包含还原函数的应用程序域仍在使用时) 。  
  
## <a name="syntax"></a>语法  
  
```cpp
HRESULT GetReJITIDs (  
     [in]  FunctionID          functionId,  
     [in]  ULONG               cReJitIds,  
     [out] ULONG *             pcReJitIds,  
     [out, size_is(cReJitIds), length_is(*pcReJitIds)]   ReJITID        reJitIds[]);  
```  
  
## <a name="parameters"></a>参数  

 `functionId`  
 中 `FunctionID` 要枚举其版本的函数实例的。  
  
 `cReJitIds`  
 中在数组中分配的 JIT 重新编译的 Id 的数目 `reJitIds` 。  
  
 `pcReJitIds`  
 弄JIT 重新编译的 Id 的实际数目。  
  
 `reJitIds`  
 弄调用方分配的数组，其中包含指定函数的 JIT 重新编译的 Id。  
  
## <a name="remarks"></a>注解  

 `GetReJITIDs` 枚举给定函数实例的活动 JIT 重新编译的 Id。 它遵循与 `ICorProfilerInfo` 接受调用方分配的缓冲区的其他函数相同的使用模式。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo4 接口](icorprofilerinfo4-interface.md)
- [分析接口](profiling-interfaces.md)
- [分析](index.md)
