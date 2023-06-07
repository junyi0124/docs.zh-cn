---
title: ICorDebugHeapValue3 接口
ms.date: 03/30/2017
api_name:
- ICorDebugHeapValue3
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugHeapValue3
helpviewer_keywords:
- ICorDebugHeapValue3 interface [.NET Framework debugging]
ms.assetid: 9c421bb0-e647-4b2d-a986-f3d578cc7f20
topic_type:
- apiref
ms.openlocfilehash: a1f4964c89aa3b658c57946d4b5a0327797118f1
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95728701"
---
# <a name="icordebugheapvalue3-interface"></a>ICorDebugHeapValue3 接口

公开对象的监视器锁属性。 此接口扩展 ICorDebugHeapValue 和 ICorDebugHeapValue2 接口。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetThreadOwningMonitorLock 方法](icordebugheapvalue3-getthreadowningmonitorlock-method.md)|返回拥有此对象的监视器锁的托管线程。|  
|[GetMonitorEventWaitList 方法](icordebugheapvalue3-getmonitoreventwaitlist-method.md)|提供在与监视器锁关联的事件上排队的线程的排序列表。|  
  
## <a name="remarks"></a>注解  
  
> [!NOTE]
> 此接口不支持跨计算机或跨进程远程调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
- [调试](index.md)
