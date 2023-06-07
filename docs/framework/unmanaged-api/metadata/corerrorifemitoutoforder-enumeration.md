---
title: CorErrorIfEmitOutOfOrder 枚举
ms.date: 03/30/2017
api_name:
- CorErrorIfEmitOutOfOrder
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorErrorIfEmitOutOfOrder
helpviewer_keywords:
- CorErrorIfEmitOutOfOrder enumeration [.NET Framework metadata]
ms.assetid: 6d758aad-29a7-44fe-9481-bbff5b799a32
topic_type:
- apiref
ms.openlocfilehash: 1d1b0cbb8be33f285b63e7353da973455e0fd752
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95718847"
---
# <a name="corerrorifemitoutoforder-enumeration"></a>CorErrorIfEmitOutOfOrder 枚举

包含一些标志值，用于指示无序发出元数据时应生成错误消息的条件。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum CorErrorIfEmitOutOfOrder {  
  
    MDErrorOutOfOrderDefault    = 0x00000000,  
    MDErrorOutOfOrderNone       = 0x00000000,  
    MDErrorOutOfOrderAll        = 0xffffffff,  
    MDMethodOutOfOrder          = 0x00000001,  
    MDFieldOutOfOrder           = 0x00000002,  
    MDParamOutOfOrder           = 0x00000004,  
    MDPropertyOutOfOrder        = 0x00000008,  
    MDEventOutOfOrder           = 0x00000010  
  
} CorErrorIfEmitOutOfOrder;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`MDErrorOutOfOrderDefault`|指示默认行为，不会生成错误消息。|  
|`MDErrorOutOfOrderNone`|指示编译器不应生成错误消息。|  
|`MDErrorOutOfOrderAll`|指示当字段、属性、事件、方法或参数的顺序发出时，编译器应生成错误消息。|  
|`MDMethodOutOfOrder`|指示当方法无序发出时，编译器应生成错误消息。|  
|`MDFieldOutOfOrder`|指示当字段按顺序发出时，编译器应生成错误消息。|  
|`MDParamOutOfOrder`|指示当参数不按顺序发出时，编译器应生成错误消息。|  
|`MDPropertyOutOfOrder`|指示当某个属性未按顺序发出时，编译器应生成错误消息。|  
|`MDEventOutOfOrder`|指示当事件顺序发出时，编译器应生成错误消息。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Corhdr。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据枚举](metadata-enumerations.md)
