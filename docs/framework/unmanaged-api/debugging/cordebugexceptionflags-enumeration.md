---
title: CorDebugExceptionFlags 枚举
ms.date: 03/30/2017
api_name:
- CorDebugExceptionFlags
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- CorDebugExceptionFlags
helpviewer_keywords:
- CorDebugExceptionFlags enumeration [.NET Framework debugging]
ms.assetid: b22687a8-e9cf-4e65-a1b0-f92a81bc524e
topic_type:
- apiref
ms.openlocfilehash: a50272bce2e27963a1d684fef40bac30cf44e1f0
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95712711"
---
# <a name="cordebugexceptionflags-enumeration"></a>CorDebugExceptionFlags 枚举

提供有关异常的附加信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum CorDebugExceptionFlags {  
    DEBUG_EXCEPTION_NONE = 0,  
    DEBUG_EXCEPTION_CAN_BE_INTERCEPTED = 0x0001  
} CorDebugExceptionFlags;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`DEBUG_EXCEPTION_NONE`|没有异常。|  
|`DEBUG_EXCEPTION_CAN_BE_INTERCEPTED`|异常是可截获的。<br /><br /> 异常的发生时间可能仍然会使调试器无法截获异常。 例如，如果当前回调或实时 (JIT) 连接引起的异常事件下没有托管代码，则无法截获异常。|  
  
## <a name="remarks"></a>注解  

 以后的版本可能会向此枚举中添加新值，因此你应针对意外值准备使用 `CorDebugExceptionFlags` 的代码。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试枚举](debugging-enumerations.md)
