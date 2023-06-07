---
title: COR_ARRAY_LAYOUT 结构
ms.date: 03/30/2017
api_name:
- COR_ARRAY_LAYOUT
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- COR_ARRAY_LAYOUT
helpviewer_keywords:
- COR_DEBUG_IL_TO_NATIVE_MAP structure [.NET Framework debugging]
ms.assetid: aa20ac3d-6f60-4aa2-91c5-f3a86f82eba8
topic_type:
- apiref
ms.openlocfilehash: 2ca6c89a671c4d7882e7cefdb820d07ac5636530
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95727401"
---
# <a name="cor_array_layout-structure"></a>COR_ARRAY_LAYOUT 结构

提供有关内存中数组对象的布局的信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef struct COR_ARRAY_LAYOUT {  
    COR_TYPEID componentID;  
    CorElementType componentType;  
    ULONG32 firstElementOffset;  
    ULONG32 elementSize;  
    ULONG32 countOffset;
    ULONG32 rankSize;
    ULONG32 numRanks;
    ULONG32 rankOffset;
} COR_ARRAY_LAYOUT;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`componentID`|数组包含的对象类型的标识符。|  
|`componentType`|一个 CorElementType 枚举值，该值指示组件是垃圾回收引用、值类还是基元。|  
|`firstElementOffset`|数组中第一个元素的偏移量。|  
|`elementSize`|每个元素的大小。|  
|`countOffset`|数组中元素数的偏移量。|  
|`rankSize`|排名的大小（以字节为单位）。|  
|`numRanks`|数组中的通道数。|  
|`rankOffset`|排名开始处的偏移量。|  
  
## <a name="remarks"></a>注解  

 `rankSize`字段指定多维数组中排名的大小。 它对于一维数组也是准确的。  
  
 `numRanks`对于一维数组和维度的多维数组，值为 1 `N` `N` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试结构](debugging-structures.md)
- [调试](index.md)
