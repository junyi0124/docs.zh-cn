---
title: CorDebugThreadState 枚举
ms.date: 03/30/2017
api_name:
- CorDebugThreadState
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- CorDebugThreadState
helpviewer_keywords:
- CorDebugThreadState enumeration [.NET Framework debugging]
ms.assetid: a3ccdf18-4ec6-494d-9024-48e5c8c724f5
topic_type:
- apiref
ms.openlocfilehash: 5eee2aee5873fe512136bc5407e395acdc31af29
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95722604"
---
# <a name="cordebugthreadstate-enumeration"></a>CorDebugThreadState 枚举

指定用于调试的线程的状态。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum CorDebugThreadState {  
    THREAD_RUN,  
    THREAD_SUSPEND  
} CorDebugThreadState;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`THREAD_RUN`|线程自由运行，除非调试事件发生。|  
|`THREAD_SUSPEND`|线程无法运行。|  
  
## <a name="remarks"></a>注解  

 调试器使用 `CorDebugThreadState` 枚举来控制线程的执行。 可以使用 [ICorDebugThread：： SetDebugState](icordebugthread-setdebugstate-method.md) 或 [ICorDebugController：： SetAllThreadsDebugState](icordebugcontroller-setallthreadsdebugstate-method.md) 方法设置线程的状态。  
  
 向 [宿主 API](../hosting/index.md) 提供的回调启用消息泵，因此不需要中断的状态。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试枚举](debugging-enumerations.md)
