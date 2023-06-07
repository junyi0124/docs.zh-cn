---
title: ICorProfilerInfo::GetILFunctionBodyAllocator 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.GetILFunctionBodyAllocator
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::GetILFunctionBodyAllocator
helpviewer_keywords:
- GetILFunctionBodyAllocator method [.NET Framework profiling]
- ICorProfilerInfo::GetILFunctionBodyAllocator method [.NET Framework profiling]
ms.assetid: 5da1bf3d-dddf-4892-b266-578ee54d570b
topic_type:
- apiref
ms.openlocfilehash: b18de87cf89985e0f7ec11edf58b43d67720251c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95718015"
---
# <a name="icorprofilerinfogetilfunctionbodyallocator-method"></a>ICorProfilerInfo::GetILFunctionBodyAllocator 方法

获取一个接口，该接口提供一种方法，用于分配内存，以用于在 Microsoft 中间语言 (MSIL) 代码中交换方法体。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetILFunctionBodyAllocator(  
    [in]  ModuleID      moduleId,  
    [out] IMethodMalloc **ppMalloc);  
```  
  
## <a name="parameters"></a>参数  

 `moduleId`  
 中方法所在的模块的 ID。  
  
 `ppMalloc`  
 弄指向 [IMethodMalloc](imethodmalloc-interface.md) 接口的指针，该接口提供分配内存的方法。  
  
## <a name="remarks"></a>注解  

 MSIL 代码中的方法体必须位于 RVA)  (RVA 上（相对于已加载的模块），这意味着它遵循 4 GB 内的模块。 为了使工具能够更轻松地调换方法的主体， `GetILFunctionBodyAllocator` 方法可确保在该范围内分配内存。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo 接口](icorprofilerinfo-interface.md)
