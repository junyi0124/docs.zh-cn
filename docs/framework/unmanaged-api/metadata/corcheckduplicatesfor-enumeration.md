---
title: CorCheckDuplicatesFor 枚举
ms.date: 03/30/2017
api_name:
- CorCheckDuplicatesFor
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorCheckDuplicatesFor
helpviewer_keywords:
- CorCheckDuplicatesFor enumeration [.NET Framework metadata]
ms.assetid: d8ec8d3c-70f7-4cc6-9957-68068fd8f49c
topic_type:
- apiref
ms.openlocfilehash: 4acdfd6df410f229a002fa191ef24766748a1262
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95672352"
---
# <a name="corcheckduplicatesfor-enumeration"></a>CorCheckDuplicatesFor 枚举

指定将检查重复项的元数据标记。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum CorCheckDuplicatesFor {  
  
    MDDupAll                    = 0xffffffff,  
    MDDupENC                    = MDDupAll,  
    MDNoDupChecks               = 0x00000000,  
    MDDupTypeDef                = 0x00000001,  
    MDDupInterfaceImpl          = 0x00000002,  
    MDDupMethodDef              = 0x00000004,  
    MDDupTypeRef                = 0x00000008,  
    MDDupMemberRef              = 0x00000010,  
    MDDupCustomAttribute        = 0x00000020,  
    MDDupParamDef               = 0x00000040,  
    MDDupPermission             = 0x00000080,  
    MDDupProperty               = 0x00000100,  
    MDDupEvent                  = 0x00000200,  
    MDDupFieldDef               = 0x00000400,  
    MDDupSignature              = 0x00000800,  
    MDDupModuleRef              = 0x00001000,  
    MDDupTypeSpec               = 0x00002000,  
    MDDupImplMap                = 0x00004000,  
    MDDupAssemblyRef            = 0x00008000,  
    MDDupFile                   = 0x00010000,  
    MDDupExportedType           = 0x00020000,  
    MDDupManifestResource       = 0x00040000,  
    MDDupGenericParam           = 0x00080000,  
    MDDupMethodSpec             = 0x00100000,  
    MDDupGenericParamConstraint = 0x00200000,  
  
    MDDupAssembly               = 0x10000000,  
  
    MDDupDefault =
        MDNoDupChecks | MDDupTypeRef | MDDupMemberRef |
        MDDupSignature | MDDupTypeSpec | MDDupMethodSpec  
  
} CorCheckDuplicatesFor;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`MDDupAll`|检查所有元数据标记的重复项。|  
|`MDDupENC`|未使用。|  
|`MDNoDupChecks`|请勿检查重复项的元数据标记。|  
|`MDDupTypeDef`|检查标记的重复项 `mdTypeDef` 。|  
|`MDDupInterfaceImpl`|检查标记的重复项 `mdInterfaceImpl` 。|  
|`MDDupMethodDef`|检查标记的重复项 `mdMethodDef` 。|  
|`MDDupTypeRef`|检查标记的重复项 `mdTypeRef` 。|  
|`MDDupMemberRef`|检查标记的重复项 `mdMemberRef` 。|  
|`MDDupCustomAttribute`|检查标记的重复项 `mdCustomAttribute` 。|  
|`MDDupParamDef`|检查标记的重复项 `mdParamDef` 。|  
|`MDDupPermission`|检查标记的重复项 `mdPermission` 。|  
|`MDDupProperty`|检查标记的重复项 `mdProperty` 。|  
|`MDDupEvent`|检查标记的重复项 `mdEvent` 。|  
|`MDDupFieldDef`|检查标记的重复项 `mdFieldDef` 。|  
|`MDDupSignature`|检查标记的重复项 `mdSignature` 。|  
|`MDDupModuleRef`|检查标记的重复项 `mdModuleRef` 。|  
|`MDDupTypeSpec`|检查标记的重复项 `mdTypeSpec` 。|  
|`MDDupImplMap`|检查标记的重复项 `mdImplMap` 。|  
|`MDDupAssemblyRef`|检查标记的重复项 `mdAssemblyRef` 。|  
|`MDDupFile`|检查标记的重复项 `mdFile` 。|  
|`MDDupExportedType`|检查标记的重复项 `mdExportedType` 。|  
|`MDDupManifestResource`|检查标记的重复项 `mdManifestResource` 。|  
|`MDDupGenericParam`|检查标记的重复项 `mdGenericParam` 。|  
|`MDDupMethodSpec`|检查标记的重复项 `mdMethodSpec` 。|  
|`MDDupGenericParamConstraint`|检查标记的重复项 `mdGenericParamConstraint` 。|  
|`MDDupAssembly`|检查标记的重复项 `mdAssembly` 。|  
|`MDDupDefault`|检查、、、 `mdMemberRef` `mdTypeRef` `mdSignature` `mdTypeSpec` 和标记的重复项 `mdMethodSpec` 。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Corhdr。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据枚举](metadata-enumerations.md)
