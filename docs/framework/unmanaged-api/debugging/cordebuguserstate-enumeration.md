---
title: CorDebugUserState 枚举
ms.date: 03/30/2017
api_name:
- CorDebugUserState
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- CorDebugUserState
helpviewer_keywords:
- CorDebugUserState enumeration [.NET Framework debugging]
ms.assetid: 5f6c2bcd-8102-4e3b-abc5-86ab0bd62def
topic_type:
- apiref
ms.openlocfilehash: 968874a46279b7eac651d45c3890429a326651b2
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95726946"
---
# <a name="cordebuguserstate-enumeration"></a>CorDebugUserState 枚举

指示线程的用户状态。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum CorDebugUserState {  
    USER_STOP_REQUESTED     =  0x01,  
    USER_SUSPEND_REQUESTED  =  0x02,  
    USER_BACKGROUND         =  0x04,  
    USER_UNSTARTED          =  0x08,  
    USER_STOPPED            =  0x10,  
    USER_WAIT_SLEEP_JOIN    =  0x20,  
    USER_SUSPENDED          =  0x40,  
    USER_UNSAFE_POINT       =  0x80,  
    USER_THREADPOOL         = 0x100  
} CorDebugUserState;  
```  
  
## <a name="members"></a>成员  
  
|Value|说明|  
|-----------|-----------------|  
|`USER_STOP_REQUESTED`|已请求终止线程。|  
|`USER_SUSPEND_REQUESTED`|已请求挂起线程。|  
|`USER_BACKGROUND`|线程在后台运行。|  
|`USER_UNSTARTED`|线程尚未开始执行。|  
|`USER_STOPPED`|线程已终止。|  
|`USER_WAIT_SLEEP_JOIN`|线程正在等待另一个线程完成任务。|  
|`USER_SUSPENDED`|线程已挂起。|  
|`USER_UNSAFE_POINT`|线程在不安全的点上。 也就是说，线程在执行时可能会阻止垃圾回收。<br /><br /> 调试事件可以从不安全的点进行调度，但在不安全点挂起线程很有可能会导致死锁，直到线程恢复。 安全点和不安全点由实时 (JIT) 和垃圾回收实现确定。|  
|`USER_THREADPOOL`|线程来自线程池。|  
  
## <a name="remarks"></a>注解  

 线程的用户状态是调试器在检查线程时具有的状态。 一个线程可能会组合用户状态。  
  
 使用 [ICorDebugThread：： GetUserState](icordebugthread-getuserstate-method.md) 方法检索线程的用户状态。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试枚举](debugging-enumerations.md)
