---
title: ICorDebugProcess8 接口
ms.date: 03/30/2017
ms.assetid: 7ab1a70f-ec11-46ff-8869-cd8ca679cc51
ms.openlocfilehash: 00c432374eeb82692492b8e6b10472f13bc44d6e
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732510"
---
# <a name="icordebugprocess8-interface"></a>ICorDebugProcess8 接口

[.NET Framework 4.6 及更高版本中支持]  
  
 对 ICorDebugProcess 接口进行逻辑扩展，以启用或禁用某些类型的 [ICorDebugManagedCallback2](icordebugmanagedcallback2-interface.md) 异常回调。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[EnableExceptionCallbacksOutsideOfMyCode 方法](icordebugprocess8-enableexceptioncallbacksoutsideofmycode-method.md)|启用或禁用某些类型的 [ICorDebugManagedCallback2](icordebugmanagedcallback2-interface.md) 异常回调。|  
  
## <a name="remarks"></a>备注  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v46plus](../../../../includes/net-current-v46plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
- [调试](index.md)
