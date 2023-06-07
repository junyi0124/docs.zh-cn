---
title: CorDebugUnmappedStop 枚举
ms.date: 03/30/2017
api_name:
- CorDebugUnmappedStop
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- CorDebugUnmappedStop
helpviewer_keywords:
- CorDebugUnmappedStop enumeration [.NET Framework debugging]
ms.assetid: a684f7d7-d0c2-4690-b721-639e613f11f8
topic_type:
- apiref
ms.openlocfilehash: e251bf67adcaf2bbd6565eda068d487eb0d70efd
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95725763"
---
# <a name="cordebugunmappedstop-enumeration"></a>CorDebugUnmappedStop 枚举

指定未映射代码的类型，这些代码可以中断分档器代码执行。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum CorDebugUnmappedStop {  
    STOP_NONE               = 0x0,  
    STOP_PROLOG             = 0x01,  
    STOP_EPILOG             = 0x02,  
    STOP_NO_MAPPING_INFO    = 0x04,  
    STOP_OTHER_UNMAPPED     = 0x08,  
    STOP_UNMANAGED          = 0x10,  
    STOP_ALL                = 0xffff,  
} CorDebugUnmappedStop;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`STOP_NONE`|请勿在任何类型的未映射代码中停止。|  
|`STOP_PROLOG`|在 prolog 代码中停止。|  
|`STOP_EPILOG`|在 epilog 代码中停止。|  
|`STOP_NO_MAPPING_INFO`|在没有映射信息的代码中停止。|  
|`STOP_OTHER_UNMAPPED`|在未映射的代码中停止，不适合 prolog、epilog、无映射-信息或非托管类别。|  
|`STOP_UNMANAGED`|在非托管代码中停止。 此值仅适用于互操作调试。|  
|`STOP_ALL`|在所有类型的未映射代码中停止。|  
  
## <a name="remarks"></a>注解  

 使用 [ICorDebugStepper：： SetUnmappedStopMask](icordebugstepper-setunmappedstopmask-method.md) 方法设置标志，这些标志指定分档器将停止的未映射代码。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试枚举](debugging-enumerations.md)
