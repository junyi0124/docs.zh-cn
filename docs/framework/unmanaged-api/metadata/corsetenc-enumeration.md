---
title: CorSetENC 枚举
ms.date: 03/30/2017
api_name:
- CorSetENC
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorSetENC
helpviewer_keywords:
- CorSetENC enumeration [.NET Framework metadata]
ms.assetid: fe4150e8-071d-43fb-8e06-c3c616dbeed2
topic_type:
- apiref
ms.openlocfilehash: df945803f2d56d04ccc68f314eb55665579ed7fd
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95705977"
---
# <a name="corsetenc-enumeration"></a>CorSetENC 枚举

包含一些值，用于在元数据生成期间影响行为。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum CorSetENC {  
  
    MDSetENCOn                  = 0x00000001,  
    MDSetENCOff                 = 0x00000002,  
  
    MDUpdateENC                 = 0x00000001,  
    MDUpdateFull                = 0x00000002,  
    MDUpdateExtension           = 0x00000003,  
    MDUpdateIncremental         = 0x00000004,  
    MDUpdateDelta               = 0x00000005,  
    MDUpdateMask                = 0x00000007  
  
} CorSetENC;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`MDSetENCOn`|已过时。|  
|`MDSetENCOff`|已过时。|  
|`MDUpdateENC`|指示可以更新元数据，而不能移动标记。|  
|`MDUpdateFull`|指示在更新过程中可以移动令牌。|  
|`MDUpdateExtension`|指示更新只能包含添加内容。 无法移动令牌。|  
|`MDUpdateIncremental`|指示编译是递增的。|  
|`MDUpdateDelta`|指示仅保存更改的元数据。|  
|`MDUpdateMask`|包括 `MDUpdateENC` 、 `MDUpdateFull` 和 `MDUpdateIncremental` 。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Corhdr。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据枚举](metadata-enumerations.md)
