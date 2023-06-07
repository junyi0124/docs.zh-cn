---
title: VariableLocationType 枚举
ms.date: 03/30/2017
api_name:
- VariableLocationType
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- VariableLocationType
helpviewer_keywords:
- VariableLocationType enumeration [.NET Framework debugging]
ms.assetid: 8635ee3a-c84b-4626-876c-416bee54f787
topic_type:
- apiref
ms.openlocfilehash: 1c65efa006a8b2f4fb4db257b4ad2cde99c4e75e
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95725256"
---
# <a name="variablelocationtype-enumeration"></a>VariableLocationType 枚举

指示变量的本机位置类型。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum VariableLocationType  
{  
    VLT_REGISTER,
    VLT_REGISTER_RELATIVE,
    VLT_INVALID  
} VariableLocationType;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`VLT_REGISTER`|变量在寄存器中。|  
|`VLT_REGISTER_RELATIVE`|变量在寄存器相对内存位置。|  
|`VLT_INVALID`|变量不存储在寄存器或寄存器相对内存位置中。|  
  
## <a name="remarks"></a>注解  

 枚举的成员 `VariableLocationType` 由 [ICorDebugVariableHome：： GetLocationType](icordebugvariablehome-getlocationtype-method.md) 方法返回。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v462plus](../../../../includes/net-current-v462plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试枚举](debugging-enumerations.md)
