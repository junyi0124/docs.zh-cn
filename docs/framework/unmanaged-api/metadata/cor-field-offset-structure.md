---
title: COR_FIELD_OFFSET 结构
ms.date: 03/30/2017
api_name:
- COR_FIELD_OFFSET
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- COR_FIELD_OFFSET
helpviewer_keywords:
- COR_FIELD_OFFSET structure [.NET Framework metadata]
ms.assetid: cced5298-277f-4a5a-8ecf-a0050c1096ea
topic_type:
- apiref
ms.openlocfilehash: 1a8ab5aa5909af60089d5e4cc8092e15bc75e8cc
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95724177"
---
# <a name="cor_field_offset-structure"></a>COR_FIELD_OFFSET 结构

存储一个类中的指定字段的偏移量。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef struct COR_FIELD_OFFSET {  
    mdFieldDef  ridOfField;  
    ULONG       ulOffset;  
} COR_FIELD_OFFSET;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`ridOfField`|`mdFieldDef`表示字段的元数据标记。|  
|`ulOffset`|字段在其类中的偏移量。|  
  
## <a name="remarks"></a>注解  

 [IMetaDataImport：： GetClassLayout](imetadataimport-getclasslayout-method.md) 和 [IMetaDataEmit：： SetClassLayout](imetadataemit-setclasslayout-method.md) 方法采用类型的参数 `COR_FIELD_OFFSET` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Corhdr.h，Corprof.idl  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据结构](metadata-structures.md)
- [IMetaDataEmit 接口](imetadataemit-interface.md)
- [IMetaDataImport 接口](imetadataimport-interface.md)
