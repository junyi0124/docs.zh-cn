---
title: CorDebugBlockingReason 枚举
ms.date: 03/30/2017
api_name:
- CorDebugBlockingReason
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- CorDebugBlockingReason
helpviewer_keywords:
- CorDebugBlockingReason enumeration [.NET Framework debugging]
ms.assetid: a6ac2531-ddfe-46fd-88fe-8b1eabe0b255
topic_type:
- apiref
ms.openlocfilehash: ddd03d70656ad52fd9d577beedc60b51c7b305d5
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95672846"
---
# <a name="cordebugblockingreason-enumeration"></a>CorDebugBlockingReason 枚举

指定线程可能在给定对象上受到阻塞的原因。  
  
## <a name="syntax"></a>语法  
  
```cpp  
Typedef enum CorDebugBlockingReason  
{  
   BLOCKING_NONE = 0  
   BLOCKING_MONITOR_CRITICAL_SECTION = 1  
   BLOCKING_MONITOR_EVENT = 2  
}  CorDebugBlockingReason;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`BLOCKING_NONE`|仅限内部使用。|  
|`BLOCKING_MONITOR_CRITICAL_SECTION`|线程尝试获取与对象上的监视器锁关联的临界区。 通常，在调用或方法之一时，会发生这种情况 <xref:System.Threading.Monitor.Enter%2A?displayProperty=nameWithType> <xref:System.Threading.Monitor.TryEnter%2A?displayProperty=nameWithType> 。|  
|`BLOCKING_MONITOR_EVENT`|线程正在等待与对象的监视器锁关联的事件。 通常情况下，当调用其中一个方法时，会发生这种情况 <xref:System.Threading.Monitor?displayProperty=nameWithType> `Wait` 。|  
  
## <a name="remarks"></a>注解  

 当 `BLOCKING_MONITOR_CRITICAL_SECTION` `BLOCKING_MONITOR_EVENT` 在 [CorDebugBlockingObject](cordebugblockingobject-structure.md) 结构中使用或成员时，结构的 `pBlockingObject` 成员指向一个 "ICorDebugValue" 接口，该接口表示正在输入的对象。 还保证实现 [ICorDebugHeapValue3](icordebugheapvalue3-interface.md) 接口。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试枚举](debugging-enumerations.md)
- [调试](index.md)
