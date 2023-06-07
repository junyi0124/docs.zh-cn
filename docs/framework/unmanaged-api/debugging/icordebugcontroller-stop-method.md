---
title: ICorDebugController::Stop 方法
ms.date: 03/30/2017
api_name:
- ICorDebugController.Stop
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugController::Stop
helpviewer_keywords:
- Stop method, ICorDebugController interface [.NET Framework debugging]
- ICorDebugController::Stop method [.NET Framework debugging]
ms.assetid: c34e79be-a7fb-479e-8dec-d126a4c330e5
topic_type:
- apiref
ms.openlocfilehash: 11cc6e4108a2064a8a9fcefa760bf3c3411d63fb
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95679853"
---
# <a name="icordebugcontrollerstop-method"></a>ICorDebugController::Stop 方法

在进程中运行托管代码的所有线程上执行协作停止。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT Stop (  
    [in] DWORD dwTimeoutIgnored  
);  
```  
  
## <a name="parameters"></a>参数  

 `dwTimeoutIgnored`  
 未使用。  
  
## <a name="remarks"></a>注解  

 `Stop` 在进程中运行托管代码的所有线程上执行协作停止。 在仅限托管的调试会话期间，非托管线程可能会继续运行 (但在尝试调用托管代码) 时将被阻止。 在互操作调试会话期间，也将停止非托管线程。 `dwTimeoutIgnored`该值当前被忽略，并被视为无限 (-1) 。 如果协作式停止由于死锁而失败，则所有线程都将被挂起并返回 E_TIMEOUT。  
  
> [!NOTE]
> `Stop` 是调试 API 中唯一的同步方法。 如果 `Stop` 返回 S_OK，则停止该进程。 不会提供回调以通知侦听器停止。 调试器必须调用 [ICorDebugController：： Continue](icordebugcontroller-continue-method.md) 以允许进程恢复。  
  
 调试器维护一个停止计数器。 当计数器变为零时，控制器会恢复。 每次调用 `Stop` 或每个调度回调都会递增计数器。 每次调用都会 `ICorDebugController::Continue` 递减计数器。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅
