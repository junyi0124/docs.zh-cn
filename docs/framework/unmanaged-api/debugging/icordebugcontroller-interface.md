---
title: ICorDebugController 接口
ms.date: 03/30/2017
api_name:
- ICorDebugController
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugController
helpviewer_keywords:
- ICorDebugController interface [.NET Framework debugging]
ms.assetid: dbb1c4dc-269a-459b-ab1d-6c70788782ce
topic_type:
- apiref
ms.openlocfilehash: 1ca9e55a2183ca4293d30607496b588cbf21d6dd
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95679941"
---
# <a name="icordebugcontroller-interface"></a>ICorDebugController 接口

表示可以控制代码执行上下文的 <xref:System.Diagnostics.Process> 或 <xref:System.AppDomain> 范围。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|`ICorDebugController::CanCommitChanges`|此方法已过时。|  
|`ICorDebugController::CommitChanges`|此方法已过时。|  
|[Continue 方法](icordebugcontroller-continue-method.md)|调用 [ICorDebugController：： Stop](icordebugcontroller-stop-method.md)后，继续执行托管线程。|  
|[Detach 方法](icordebugcontroller-detach-method.md)|将调试器与进程或应用程序域分离。|  
|[EnumerateThreads 方法](icordebugcontroller-enumeratethreads-method.md)|获取进程中活动托管线程的枚举器。|  
|[HasQueuedCallbacks 方法](icordebugcontroller-hasqueuedcallbacks-method.md)|获取一个值，该值指示当前是否为指定线程排队任何托管回调。|  
|[IsRunning 方法](icordebugcontroller-isrunning-method.md)|获取一个值，该值指示进程中的线程当前是否可以自由运行。|  
|[SetAllThreadsDebugState 方法](icordebugcontroller-setallthreadsdebugstate-method.md)|设置进程中所有托管线程的调试状态。|  
|[Stop 方法](icordebugcontroller-stop-method.md)|在进程中运行托管代码的所有线程上执行协作停止。|  
|[Terminate 方法](icordebugcontroller-terminate-method.md)|用指定的退出代码终止进程。|  
  
## <a name="remarks"></a>注解  

 如果 `ICorDebugController` 正在控制某个进程，则该范围将包括该进程的所有线程。 如果 `ICorDebugController` 正在控制某个应用程序域，则该作用域只包含该特定应用程序域的线程。  
  
> [!NOTE]
> 此接口不支持跨计算机或跨进程远程调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
