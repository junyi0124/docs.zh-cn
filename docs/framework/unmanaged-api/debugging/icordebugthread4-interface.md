---
title: ICorDebugThread4 接口
ms.date: 03/30/2017
api_name:
- ICorDebugThread4
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugThread4
helpviewer_keywords:
- ICorDebugThread4 interface [.NET Framework debugging]
ms.assetid: a8c7719a-322b-4133-8566-4c27218dc104
topic_type:
- apiref
ms.openlocfilehash: 5c108420772be9e6d0932f3759f18da9446f99d6
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95727934"
---
# <a name="icordebugthread4-interface"></a>ICorDebugThread4 接口

提供线程阻塞信息。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetBlockingObjects 方法](icordebugthread4-getblockingobjects-method.md)|提供 [CorDebugBlockingObject](cordebugblockingobject-structure.md) 结构的有序枚举，这些结构提供线程阻塞信息。|  
|[HadUnhandledException 方法](icordebugthread4-hadunhandledexception-method.md)|指示线程是否曾经出现过未经处理的异常。|  
|[GetCurrentCustomDebuggerNotification 方法](icordebugthread4-getcurrentcustomdebuggernotification-method.md)|获取当前线程上的当前 [ICorDebugManagedCallback3：： CustomNotification](icordebugmanagedcallback3-customnotification-method.md) 对象。|  
  
## <a name="remarks"></a>注解  

 此接口是 ICorDebugThread、ICorDebugThread2 和 [ICorDebugThread3](icordebugthread3-interface.md) 接口的逻辑扩展。  
  
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
