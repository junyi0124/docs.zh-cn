---
title: CorDebugChainReason 枚举
ms.date: 03/30/2017
api_name:
- CorDebugChainReason
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- CorDebugChainReason
helpviewer_keywords:
- CorDebugChainReason enumeration [.NET Framework debugging]
ms.assetid: c915da51-50b2-41df-841a-2b971f4d0975
topic_type:
- apiref
ms.openlocfilehash: 6185c5dda69a0cf7e9847ddc021448612a426b19
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95716052"
---
# <a name="cordebugchainreason-enumeration"></a>CorDebugChainReason 枚举

指示启动调用链的一个或多个原因。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum CorDebugChainReason {  
    CHAIN_NONE              = 0x000,  
    CHAIN_CLASS_INIT        = 0x001,  
    CHAIN_EXCEPTION_FILTER  = 0x002,  
    CHAIN_SECURITY          = 0x004,  
    CHAIN_CONTEXT_POLICY    = 0x008,  
    CHAIN_INTERCEPTION      = 0x010,  
    CHAIN_PROCESS_START     = 0x020,  
    CHAIN_THREAD_START      = 0x040,  
    CHAIN_ENTER_MANAGED     = 0x080,  
    CHAIN_ENTER_UNMANAGED   = 0x100,  
    CHAIN_DEBUGGER_EVAL     = 0x200,  
    CHAIN_CONTEXT_SWITCH    = 0x400,  
    CHAIN_FUNC_EVAL         = 0x800  
} CorDebugChainReason;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`CHAIN_NONE`|尚未启动任何调用链。|  
|`CHAIN_CLASS_INIT`|由构造函数启动该链。|  
|`CHAIN_EXCEPTION_FILTER`|由异常筛选器启动该链。|  
|`CHAIN_SECURITY`|由强制实施安全的代码启动该链。|  
|`CHAIN_CONTEXT_POLICY`|由上下文策略启动该链。|  
|`CHAIN_INTERCEPTION`|未使用。|  
|`CHAIN_PROCESS_START`|未使用。|  
|`CHAIN_THREAD_START`|由线程执行开始启动该链。|  
|`CHAIN_ENTER_MANAGED`|由托管代码中的条目启动该链。|  
|`CHAIN_ENTER_UNMANAGED`|由非托管代码中的条目启动该链。|  
|`CHAIN_DEBUGGER_EVAL`|未使用。|  
|`CHAIN_CONTEXT_SWITCH`|未使用。|  
|`CHAIN_FUNC_EVAL`|由函数求值启动该链。|  
  
## <a name="remarks"></a>注解  

 使用 [ICorDebugChain：： GetReason](icordebugchain-getreason-method.md) 方法来确定启动调用链的原因。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试枚举](debugging-enumerations.md)
