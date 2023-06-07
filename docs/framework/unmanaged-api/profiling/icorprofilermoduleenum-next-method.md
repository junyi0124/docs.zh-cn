---
title: ICorProfilerModuleEnum::Next 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerModuleEnum.Next Method
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerModuleEnum::Next
helpviewer_keywords:
- ICorProfilerModuleEnum::Next method [.NET Framework profiling]
- Next method, ICorProfilerModuleEnum interface [.NET Framework profiling]
ms.assetid: a3cea59d-7622-4323-897a-0a464c40f77f
topic_type:
- apiref
ms.openlocfilehash: 94c2c159cf386e00dfc0d1df97536d7ade53407e
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732939"
---
# <a name="icorprofilermoduleenumnext-method"></a>ICorProfilerModuleEnum::Next 方法

从模块的序列集合中获取指定的连续模块数，从枚举器在该序列的当前位置开始。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT Next([in]  ULONG      celt,  
             [out, size_is(celt), length_is(*pceltFetched)]  
                    ModuleID ids[],  
             [out] ULONG *   pceltFetched);  
```  
  
## <a name="parameters"></a>参数  

 `celt`  
 [in] 要检索的模块的数量。  
  
 `ids`  
 [out] `ModuleID` 值的数组，其中每个表示检索的模块。  
  
 `pceltFetched`  
 [out] 指向 `ids` 数组中实际返回的元素数目的指针。  
  
## <a name="return-value"></a>返回值  

 此方法返回以下特定 HRESULT 以及表示方法失败的 HRESULT 错误。  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|已返回 `celt` 元素。|  
|S_FALSE|返回的元素少于 `celt` 个，表示枚举已完成。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerModuleEnum 接口](icorprofilermoduleenum-interface.md)
- [分析接口](profiling-interfaces.md)
