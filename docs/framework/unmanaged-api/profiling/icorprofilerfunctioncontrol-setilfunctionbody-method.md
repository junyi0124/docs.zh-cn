---
title: ICorProfilerFunctionControl::SetILFunctionBody 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerFunctionControl.SetILFunctionBody
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerFunctionControl::SetILFunctionBody
helpviewer_keywords:
- ICorProfilerFunctionControl::SetILFunctionBody method [.NET Framework profiling]
- SetILFunctionBody method, ICorProfilerFunctionControl interface [.NET Framework profiling]
ms.assetid: 2c33f0f7-75b2-4c19-b2c7-c94b54997576
topic_type:
- apiref
ms.openlocfilehash: fa82cd1e646777c9841c1b3d653134aa7ba7ed7c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95712737"
---
# <a name="icorprofilerfunctioncontrolsetilfunctionbody-method"></a>ICorProfilerFunctionControl::SetILFunctionBody 方法

替换方法的公共中间语言 (CIL) 主体。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetILFunctionBody(  
    [in]  ULONG   cbNewILMethodHeader,  
    [in, size_is(cbNewILMethodHeader)] LPCBYTE pbNewILMethodHeader);  
```  
  
## <a name="parameters"></a>参数  

 `cbNewILMethodHeader`  
 [in] 新 CIL 的总体大小，包括标头和主体之后的任何结构。  
  
 `pbNewILMethodHeader`  
 [in] 一个指向新 CIL 的标头的指针。  
  
## <a name="return-value"></a>返回值  

 此方法会返回以下特定的 HRESULT。  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|替换成功。|  
  
## <a name="remarks"></a>注解  

 与 [ICorProfilerInfo：： SetILFunctionBody](icorprofilerinfo-setilfunctionbody-method.md) 方法不同， `SetILFunctionBody` 方法管理新 CIL 主体所需的内存。 这意味着，无需通过使用 [IMethodMalloc](imethodmalloc-interface.md) 接口来分配探查器提供的 CIL 主体，也不必在特定范围内进行分配。 它可在任何堆上进行分配。 探查器可以在返回后释放用于其 CIL 体的内存 `SetILFunctionBody` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerFunctionControl 接口](icorprofilerfunctioncontrol-interface.md)
