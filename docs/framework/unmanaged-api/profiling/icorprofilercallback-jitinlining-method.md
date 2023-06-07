---
title: ICorProfilerCallback::JITInlining 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.JITInlining
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::JITInlining
helpviewer_keywords:
- JITInlining method [.NET Framework profiling]
- ICorProfilerCallback::JITInlining method [.NET Framework profiling]
ms.assetid: c2f45801-dd38-4b78-b6b7-64397dc73f83
topic_type:
- apiref
ms.openlocfilehash: cf68594620b24f2a5823aa423c5911f2d9d6b328
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95725490"
---
# <a name="icorprofilercallbackjitinlining-method"></a>ICorProfilerCallback::JITInlining 方法

通知探查器，实时 (JIT) 编译器将与另一个函数插入一个函数。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT JITInlining(  
    [in]  FunctionID callerId,  
    [in]  FunctionID calleeId,  
    [out] BOOL      *pfShouldInline);  
```  
  
## <a name="parameters"></a>参数  

 `callerId`  
 中将向其中插入函数的函数的 ID `calleeId` 。  
  
 `calleeId`  
 中要插入的函数的 ID。  
  
 `pfShouldInline`  
 [out] `true` 如果允许执行插入，则为;否则为 `false` 。  
  
## <a name="remarks"></a>注解  

 探查器可以将设置 `pfShouldInline` 为 `false` ，以防止将 `calleeId` 函数插入 `callerId` 函数。 此外，探查器可以通过使用 [COR_PRF_MONITOR](cor-prf-monitor-enumeration.md) 枚举的 COR_PRF_DISABLE_INLINING 值全局禁用内联插入。  
  
 以内联方式插入的函数不会引发用于进入或离开的事件。 因此，探查器必须设置 `pfShouldInline` 为才能 `false` 生成准确的调用图。 设置 `pfShouldInline` 为 `false` 会影响性能，因为内联插入通常会提高速度，并减少插入方法的单独 JIT 编译事件数。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerCallback 接口](icorprofilercallback-interface.md)
