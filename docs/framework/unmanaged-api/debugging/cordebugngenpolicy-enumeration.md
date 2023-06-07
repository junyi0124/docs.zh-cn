---
title: CorDebugNGenPolicy 枚举
ms.date: 03/30/2017
dev_langs:
- cpp
api_name:
- CorDebugNGenPolicy
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- CorDebugNGenPolicy
helpviewer_keywords:
- CorDebugNgenPolicy enumeration [.NET Framework debugging]
ms.assetid: edb4e4d2-3166-44d4-8b17-bf302f7ea093
topic_type:
- apiref
ms.openlocfilehash: b64de0fa2ecbddd2decf69a4099d9897ec42a563
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95696422"
---
# <a name="cordebugngenpolicy-enumeration"></a>CorDebugNGenPolicy 枚举

提供用于确定调试器是否从本机映像缓存中加载本机 (NGen) 映像的值。  
  
## <a name="syntax"></a>语法  
  
```cpp
enum CorDebugNGENPolicy {  
    DISABLE_LOCAL_NIC = 1  
} CorDebugNGENPolicy;  
```  
  
## <a name="members"></a>成员  
  
|成员名称|说明|  
|-----------------|-----------------|  
|`DISABLE_LOCAL_NIC`|在 Windows 8.x 应用商店应用中，禁用了本地本机映像缓存中的映像。 在桌面应用中，此设置不起作用。|  
  
## <a name="remarks"></a>注解  

 `CorDebugNGENPolicy`枚举由[ICorDebugProcess5：： EnableNGENPolicy](icordebugprocess5-enablengenpolicy-method.md)方法使用。 禁用本地本机映像缓存中的映像可提供一致的调试体验，方法是确保调试器加载可调试 JIT 编译的映像，而不是优化的本机映像。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试枚举](debugging-enumerations.md)
