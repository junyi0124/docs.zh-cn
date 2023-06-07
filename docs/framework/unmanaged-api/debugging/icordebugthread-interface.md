---
title: ICorDebugThread 接口
ms.date: 03/30/2017
api_name:
- ICorDebugThread
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugThread
helpviewer_keywords:
- ICorDebugThread interface [.NET Framework debugging]
ms.assetid: 3930fd9b-2bc3-4b72-80a0-b6eeb94d60c6
topic_type:
- apiref
ms.openlocfilehash: 5165ef081aad849c11747807d8cc76b2df0a6c74
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95729312"
---
# <a name="icordebugthread-interface"></a>ICorDebugThread 接口

表示进程中的线程。 `ICorDebugThread` 实例的生存期与它表示的线程的生存期相同。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[ClearCurrentException 方法](icordebugthread-clearcurrentexception-method.md)|未实现此方法。 请勿使用。|  
|[CreateEval 方法](icordebugthread-createeval-method.md)|创建一个对此运行的 ICorDebugEval 对象 `ICorDebugThread` 。|  
|[CreateStepper 方法](icordebugthread-createstepper-method.md)|创建一个 ICorDebugStepper 对象，该对象允许单步执行此中的活动框架 `ICorDebugThread` 。|  
|[EnumerateChains 方法](icordebugthread-enumeratechains-method.md)|获取一个接口指针，该指针指向包含此中所有堆栈链的 ICorDebugChainEnum 枚举器 `ICorDebugThread` 。|  
|[GetActiveChain 方法](icordebugthread-getactivechain-method.md)|获取此上的活动 ICorDebugChain 的接口指针 `ICorDebugThread` 。|  
|[GetActiveFrame 方法](icordebugthread-getactiveframe-method.md)|获取此上的活动 ICorDebugFrame 的接口指针 `ICorDebugThread` 。|  
|[GetAppDomain 方法](icordebugthread-getappdomain-method.md)|获取一个接口指针，该指针指向此当前正在执行的应用程序域 `ICorDebugThread` 。|  
|[GetCurrentException 方法](icordebugthread-getcurrentexception-method.md)|获取一个指向 ICorDebugValue 对象的接口指针，该对象表示当前由托管代码引发的异常。|  
|[GetDebugState 方法](icordebugthread-getdebugstate-method.md)|获取描述此的当前调试状态的 CorDebugThreadState 值 `ICorDebugThread` 。|  
|[GetHandle 方法](icordebugthread-gethandle-method.md)|获取此的活动部分的当前句柄 `ICorDebugThread` 。|  
|[GetID 方法](icordebugthread-getid-method.md)|获取此的活动部分的当前操作系统标识符 `ICorDebugThread` 。|  
|[GetObject 方法](icordebugthread-getobject-method.md)| (CLR) 线程获取公共语言运行时的接口指针。|  
|[GetProcess 方法](icordebugthread-getprocess-method.md)|获取一个接口指针，该指针指向此 `ICorDebugThread` 构成部分的进程。|  
|[GetRegisterSet 方法](icordebugthread-getregisterset-method.md)|获取一个接口指针，该指针指向与此关联的寄存器集 `ICorDebugThread` 。|  
|[GetUserState 方法](icordebugthread-getuserstate-method.md)|获取描述此的当前状态的 CorDebugUserState 值的按位组合 `ICorDebugThread` 。|  
|[SetDebugState 方法](icordebugthread-setdebugstate-method.md)|设置 `CorDebugThreadState` 描述此的调试状态的值的按位组合 `ICorDebugThread` 。|  
  
## <a name="remarks"></a>注解  
  
> [!NOTE]
> 此接口不支持跨计算机或跨进程远程调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
