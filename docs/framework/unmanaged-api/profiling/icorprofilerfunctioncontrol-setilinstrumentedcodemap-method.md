---
title: ICorProfilerFunctionControl::SetILInstrumentedCodeMap 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerFunctionControl.SetILInstrumentedCodeMap
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerFunctionControl::SetILInstrumentedCodeMap
helpviewer_keywords:
- ICorProfilerFunctionControl::SetILInstrumentedCodeMap method [.NET Framework profiling]
- SetIILInstrumentedCodeMap method, ICorProfilerFunctionControl interface [.NET Framework profiling]
ms.assetid: ecf56646-7e5f-46c4-8340-f3a04e88920f
topic_type:
- apiref
ms.openlocfilehash: d22d789724a62cebb0136b9b01be22d6825384ac
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95714531"
---
# <a name="icorprofilerfunctioncontrolsetilinstrumentedcodemap-method"></a>ICorProfilerFunctionControl::SetILInstrumentedCodeMap 方法

使用指定的公共中间语言 (CIL) 映射项为指定的函数设置代码图。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetILInstrumentedCodeMap(  
    [in]   ULONG      cILMapEntries,  
    [in, size_is(cILMapEntries)] COR_IL_MAP rgILMapEntries[]);  
```  
  
## <a name="parameters"></a>参数  

 `cILMapEntries`  
 [in] 映射中的项数。  
  
 `rgILMapEntries`  
 [in] 调用方分配的 COR_IL_MAP 项的数组。 对于 [ICorProfilerInfo：： SetILInstrumentedCodeMap](icorprofilerinfo-setilinstrumentedcodemap-method.md) 方法，这些项的解释是相同的。  
  
## <a name="remarks"></a>注解  

 通过调用此方法设置映射，使调试器可以通过调用 [ICorDebugILCode2：： GetInstrumentedILMap](../debugging/icordebugilcode2-getinstrumentedilmap-method.md)来检索映射。 在计算堆栈跟踪和变量生存期的 IL 偏移量时，它还允许调试器在内部使用该映射。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo 接口](icorprofilerinfo-interface.md)
