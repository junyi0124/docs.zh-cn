---
title: CorDebugMappingResult 枚举
ms.date: 03/30/2017
api_name:
- CorDebugMappingResult
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- CorDebugMappingResult
helpviewer_keywords:
- CorDebugMappingResult enumeration [.NET Framework debugging]
ms.assetid: 701281dd-2936-45c8-a1f0-3bf7332b093b
topic_type:
- apiref
ms.openlocfilehash: 0e7afa386af1bd2eebc2b58592d01b764660248f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95704690"
---
# <a name="cordebugmappingresult-enumeration"></a>CorDebugMappingResult 枚举

提供如何获取指令指针 (IP) 的值的详细信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum CorDebugMappingResult {  
    MAPPING_PROLOG              = 0x1,  
    MAPPING_EPILOG              = 0x2,  
    MAPPING_NO_INFO             = 0x4,  
    MAPPING_UNMAPPED_ADDRESS    = 0x8,  
    MAPPING_EXACT               = 0x10,  
    MAPPING_APPROXIMATE         = 0x20,  
} CorDebugMappingResult;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`MAPPING_PROLOG`|本机代码在序言中，因此 IP 的值为0。|  
|`MAPPING_EPILOG`|本机代码在 epilog 中，因此 IP 的值是方法的最后一条指令的地址。|  
|`MAPPING_NO_INFO`|此方法没有可用的映射信息，因此 IP 的值为0。|  
|`MAPPING_UNMAPPED_ADDRESS`|尽管方法存在映射信息，但当前地址无法映射到 Microsoft 中间语言 (MSIL) 代码。 IP 的值为0。|  
|`MAPPING_EXACT`|方法完全映射到 MSIL 代码或已解释帧，因此 IP 的值是准确的。|  
|`MAPPING_APPROXIMATE`|已成功映射方法，但 IP 的值可能是近似的。|  
  
## <a name="remarks"></a>注解  

 可以使用 [ICorDebugILFrame：： GetIP](icordebugilframe-getip-method.md) 方法获取指令指针的值。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试枚举](debugging-enumerations.md)
