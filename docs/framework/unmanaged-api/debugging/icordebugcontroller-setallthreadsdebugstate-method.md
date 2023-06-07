---
title: ICorDebugController::SetAllThreadsDebugState 方法
ms.date: 03/30/2017
api_name:
- ICorDebugController.SetAllThreadsDebugState
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugController::SetAllThreadsDebugState
helpviewer_keywords:
- SetAllThreadsDebugState method [.NET Framework debugging]
- ICorDebugController::SetAllThreadsDebugState method [.NET Framework debugging]
ms.assetid: bdda4bd7-4743-4d58-a22b-8067e967db95
topic_type:
- apiref
ms.openlocfilehash: d8375948be5820aaf6e879b82bcfde6471cccf3f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95679892"
---
# <a name="icordebugcontrollersetallthreadsdebugstate-method"></a>ICorDebugController::SetAllThreadsDebugState 方法

设置进程中所有托管线程的调试状态。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetAllThreadsDebugState (  
    [in] CorDebugThreadState  state,  
    [in] ICorDebugThread      *pExceptThisThread  
);  
```  
  
## <a name="parameters"></a>参数  

 `state`  
 中一个 "CorDebugThreadState" 枚举的值，该值指定线程的状态以进行调试。  
  
 `pExceptThisThread`  
 中一个指向 "ICorDebugThread" 对象的指针，该对象表示要从调试状态设置中免除的线程。 如果此值为 null，则不免除任何线程。  
  
## <a name="remarks"></a>注解  

 此 `SetAllThreadsDebugState` 方法可能会影响通过 [EnumerateThreads 方法](icordebugcontroller-enumeratethreads-method.md)不可见的线程，因此，通过方法挂起的线程 `SetAllThreadsDebugState` 需要通过 `SetAllThreadsDebugState` 方法恢复。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅
