---
title: ICorProfilerInfo2::GetRVAStaticAddress 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo2.GetRVAStaticAddress
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo2::GetRVAStaticAddress
helpviewer_keywords:
- GetRVAStaticAddress method [.NET Framework profiling]
- ICorProfilerInfo2::GetRVAStaticAddress method [.NET Framework profiling]
ms.assetid: a25a8f8b-5cfa-440d-9376-a1a1c3a9fc11
topic_type:
- apiref
ms.openlocfilehash: ea4f6f129cf2919124b1bef1fd837f2b1e13760e
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95727050"
---
# <a name="icorprofilerinfo2getrvastaticaddress-method"></a>ICorProfilerInfo2::GetRVAStaticAddress 方法

获取 (RVA) 静态字段的指定相对虚拟地址的地址。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetRVAStaticAddress(  
    [in] ClassID classId,  
    [in] mdFieldDef fieldToken,  
    [out] void **ppAddress);  
```  
  
## <a name="parameters"></a>参数  

 `classId`  
 中包含请求的 RVA 静态字段的类的 ID。  
  
 `fieldToken`  
 中请求的 RVA 静态字段的元数据标记。  
  
 `ppAddress`  
 弄指向 RVA 静态字段的地址的指针。  
  
## <a name="remarks"></a>注解  

 此 `GetRVAStaticAddress` 方法可能会返回以下内容之一：  
  
- 如果未在指定的上下文中为给定的静态字段分配地址，则 CORPROF_E_DATAINCOMPLETE HRESULT。  
  
- 可能位于垃圾回收堆中的对象的地址。 这些地址可能会在垃圾回收后失效，因此，在垃圾回收后，探查器不应假定它们是有效的。  
  
 在类的类构造函数完成之前， `GetRVAStaticAddress` 将为其所有静态字段返回 CORPROF_E_DATAINCOMPLETE，不过，某些静态字段可能已经初始化并且可能是对垃圾回收对象的根。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo 接口](icorprofilerinfo-interface.md)
- [ICorProfilerInfo2 接口](icorprofilerinfo2-interface.md)
