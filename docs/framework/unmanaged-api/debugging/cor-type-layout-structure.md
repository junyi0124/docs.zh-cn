---
title: COR_TYPE_LAYOUT 结构
ms.date: 03/30/2017
api_name:
- COR_TYPE_LAYOUT
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- COR_TYPE_LAYOUT
helpviewer_keywords:
- COR_TYPE_LAYOUT structure [.NET Framework debugging]
ms.assetid: 43a7addd-f25a-4049-9907-abec3eb17af2
topic_type:
- apiref
ms.openlocfilehash: f33c8f5cf218979404063342d9b1cc5123839f83
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95726322"
---
# <a name="cor_type_layout-structure"></a>COR_TYPE_LAYOUT 结构

提供有关内存中某个对象的布局的信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef struct COR_TYPE_LAYOUT {  
    COR_TYPEID parentID;  
    ULONG32 objectSize;  
    ULONG32 numFields;  
    ULONG32 boxOffset;  
    CorElementType type;  
} COR_TYPE_LAYOUT;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`parentID`|此类型的父类型的标识符。 如果类型 id 与相对应，则此值将为 NULL 类型 id (token1 = 0、token2 = 0) <xref:System.Object?displayProperty=nameWithType> 。|  
|`objectSize`|此类型的对象的基大小。 这是非变量大小对象的总大小。|  
|`numFields`|此类型的对象中包含的字段数。|  
|`boxOffset`|如果此类型为装箱，则为对象字段的开始偏移量。 此字段仅对基元和结构等值类型有效。|  
|`type`|此类型所属的 CorElementType。|  
  
## <a name="remarks"></a>注解  

 如果 `numFields` 大于零，则可以调用 [ICorDebugProcess5：： GetTypeFields](icordebugprocess5-gettypefields-method.md) 方法来获取有关此类型中的字段的信息。 如果 `type` 为 `ELEMENT_TYPE_STRING` 、 `ELEMENT_TYPE_ARRAY` 或 `ELEMENT_TYPE_SZARRAY` ，则此类型的对象的大小是可变的，你可以将 [COR_TYPEID](cor-typeid-structure.md) 结构传递给 [ICorDebugProcess5：： GetArrayLayout](icordebugprocess5-getarraylayout-method.md) 方法。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试结构](debugging-structures.md)
- [调试](index.md)
