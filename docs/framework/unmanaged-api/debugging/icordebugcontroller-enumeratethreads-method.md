---
title: ICorDebugController::EnumerateThreads 方法
ms.date: 03/30/2017
api_name:
- ICorDebugController.EnumerateThreads
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugController::EnumerateThreads
helpviewer_keywords:
- ICorDebugController::EnumerateThreads method [.NET Framework debugging]
- EnumerateThreads method [.NET Framework debugging]
ms.assetid: 73f536f6-4668-4a4a-b3e4-ac7df862d5be
topic_type:
- apiref
ms.openlocfilehash: f98118f9206d9ccd7dc9dc9a943500c7b4cd676a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732653"
---
# <a name="icordebugcontrollerenumeratethreads-method"></a>ICorDebugController::EnumerateThreads 方法

获取进程中活动托管线程的枚举器。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EnumerateThreads (  
    [out] ICorDebugThreadEnum **ppThreads  
);  
```  
  
## <a name="parameters"></a>参数  

 `ppThreads`  
 弄一个指向 "ICorDebugThreadEnum" 对象地址的指针，该对象表示在进程中处于活动状态的所有托管线程的枚举器。  
  
## <a name="remarks"></a>注解  

 在调度了 [ICorDebugManagedCallback：： CreateThread](icordebugmanagedcallback-createthread-method.md) 回调之后以及在调度 [ICorDebugManagedCallback：： ExitThread](icordebugmanagedcallback-exitthread-method.md) 回调之前，线程被视为处于活动状态。 托管线程在其堆栈上可能不一定有任何托管帧。 甚至可以在 [ICorDebugManagedCallback：： CreateProcess](icordebugmanagedcallback-createprocess-method.md) 回调之前枚举线程。 枚举自然将为空。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅
