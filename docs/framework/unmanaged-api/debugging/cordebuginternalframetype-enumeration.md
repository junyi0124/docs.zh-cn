---
title: CorDebugInternalFrameType 枚举
ms.date: 03/30/2017
api_name:
- CorDebugInternalFrameType
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- CorDebugInternalFrameType
helpviewer_keywords:
- CorDebugInternalFrameType enumeration [.NET Framework debugging]
ms.assetid: e4412dc2-c338-4cfb-94d8-f682095dd2b1
topic_type:
- apiref
ms.openlocfilehash: 1c94e03aa088d8f48eb7f7a418cebd0492319513
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95696578"
---
# <a name="cordebuginternalframetype-enumeration"></a>CorDebugInternalFrameType 枚举

标识堆栈帧的类型。 此枚举由 [ICorDebugInternalFrame：： GetFrameType](icordebuginternalframe-getframetype-method.md) 方法使用。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum CorDebugInternalFrameType {  
  
    STUBFRAME_NONE                 = 0x00000000,  
    STUBFRAME_M2U                  = 0x00000001,  
    STUBFRAME_U2M                  = 0x00000002,  
    STUBFRAME_APPDOMAIN_TRANSITION = 0x00000003,  
    STUBFRAME_LIGHTWEIGHT_FUNCTION = 0x00000004,  
    STUBFRAME_FUNC_EVAL            = 0x00000005,  
    STUBFRAME_INTERNALCALL         = 0x00000006,  
    STUBFRAME_CLASS_INIT           = 0x00000007,  
    STUBFRAME_EXCEPTION            = 0x00000008,  
    STUBFRAME_SECURITY             = 0x00000009,  
    STUBFRAME_JIT_COMPILATION     = 0x0000000a,  
} CorDebugInternalFrameType;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`STUBFRAME_NONE`|Null 值。 `ICorDebugInternalFrame::GetFrameType`方法从不返回此值。|  
|`STUBFRAME_M2U`|托管到非托管的存根帧。|  
|`STUBFRAME_U2M`|非托管到托管的存根帧。|  
|`STUBFRAME_APPDOMAIN_TRANSITION`|应用程序域之间的转换。|  
|`STUBFRAME_LIGHTWEIGHT_FUNCTION`|轻量方法调用。|  
|`STUBFRAME_FUNC_EVAL`|函数计算的开头。|  
|`STUBFRAME_INTERNALCALL`|公共语言运行时的内部调用。|  
|`STUBFRAME_CLASS_INIT`|类初始化的开头。|  
|`STUBFRAME_EXCEPTION`|引发的异常。|  
|`STUBFRAME_SECURITY`|用于代码访问安全性的帧。|  
|`STUBFRAME_JIT_COMPILATION`|运行时对方法进行 JIT 编译。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试枚举](debugging-enumerations.md)
