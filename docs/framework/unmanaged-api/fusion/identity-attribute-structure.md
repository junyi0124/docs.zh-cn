---
title: IDENTITY_ATTRIBUTE 结构
ms.date: 03/30/2017
api_name:
- IDENTITY_ATTRIBUTE
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IDENTITY_ATTRIBUTE
helpviewer_keywords:
- IDENTITY_ATTRIBUTE structure [.NET Framework fusion]
ms.assetid: 1ee7c434-9681-4fa8-badd-652cb1a9742b
topic_type:
- apiref
ms.openlocfilehash: da4b1d6f2a7079ef33859fce29c9555ac06fcfc2
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95725646"
---
# <a name="identity_attribute-structure"></a>IDENTITY_ATTRIBUTE 结构

包含有关 [IDefinitionIdentity](idefinitionidentity-interface.md) 实例的元数据属性信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef struct _IDENTITY_ATTRIBUTE {  
    LPCWSTR  pszNamespace;  
    LPCWSTR  pszName;  
    LPCWSTR  pszValue;  
} IDENTITY_ATTRIBUTE;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`pszNamespace`|指向以 null 结尾的字符串的指针，该字符串包含特性所在的命名空间。|  
|`pszName`|指向以 null 结尾的字符串的指针，该字符串包含属性的名称。|  
|`pszValue`|指向以 null 结尾的字符串的指针，该字符串包含属性的值。|  
  
## <a name="remarks"></a>注解  

 该 `IDENTITY_ATTRIBUTE` 结构包含三个指向以 null 结尾的字符串的指针。 这三个字符串描述了一个属性。  
  
 结构的实例 `IDENTITY_ATTRIBUTE` 与 [IDENTITY_ATTRIBUTE_BLOB](identity-attribute-blob-structure.md) 结构的实例相关联。 `IDENTITY_ATTRIBUTE`结构包含实际的字符串，相应的 `IDENTITY_ATTRIBUTE_BLOB` 结构列出了结构中列出的三个字符串的偏移量 `IDENTITY_ATTRIBUTE` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** 隔离。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IDefinitionIdentity 接口](idefinitionidentity-interface.md)
- [IDENTITY_ATTRIBUTE_BLOB 结构](identity-attribute-blob-structure.md)
- [合成结构](fusion-structures.md)
