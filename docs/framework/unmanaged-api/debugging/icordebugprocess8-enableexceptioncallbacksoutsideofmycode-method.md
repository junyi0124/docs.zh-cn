---
title: ICorDebugProcess8::EnableExceptionCallbacksOutsideOfMyCode 方法
ms.date: 03/30/2017
dev_langs:
- cpp
ms.assetid: b3af44ec-7d41-425b-aed9-0c4379e5cbe9
ms.openlocfilehash: 750d2a2d69c74e147c34c9c496079ee48ac04b42
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732536"
---
# <a name="icordebugprocess8enableexceptioncallbacksoutsideofmycode-method"></a>ICorDebugProcess8::EnableExceptionCallbacksOutsideOfMyCode 方法

[.NET Framework 4.6 及更高版本中支持]  
  
 启用或禁用某些类型的 [ICorDebugManagedCallback2](icordebugmanagedcallback2-interface.md) 异常回调。  
  
## <a name="syntax"></a>语法  
  
```cpp
HRESULT EnableExceptionCallbacksOutsideOfMyCode(  
   [in] BOOL enableExceptionsOutsideOfJMC  
);  
```  
  
## <a name="parameters"></a>参数  

 `enableExceptionsOutsideOfJMC`  
 [in]  
  
## <a name="remarks"></a>注解  

 如果 `enableExceptionsOutsideOfJMC` 的值是 `false`：  
  
- DEBUG_EXCEPTION_FIRST_CHANCE 异常将不会导致回调到调试器。  
  
- 如果异常不会转义到用户代码（即从异常源到异常处理程序的路径没有被标记为 JustMyCode 或 JMC 的方法），则 DEBUG_EXCEPTION_CATCH_HANDLER_FOUND 异常不会导致回调到调试器。  
  
 `enableExceptionsOutsideOfJMC` 的默认值为 `true`。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v46plus](../../../../includes/net-current-v46plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugProcess8 接口](icordebugprocess8-interface.md)
- [调试接口](debugging-interfaces.md)
