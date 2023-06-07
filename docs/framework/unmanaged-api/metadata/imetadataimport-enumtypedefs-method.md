---
title: IMetaDataImport::EnumTypeDefs 方法
ms.date: 03/30/2017
api_name:
- IMetaDataImport.EnumTypeDefs
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::EnumTypeDefs
helpviewer_keywords:
- EnumTypeDefs method [.NET Framework metadata]
- IMetaDataImport::EnumTypeDefs method [.NET Framework metadata]
ms.assetid: 4e508711-da92-4381-aaf8-6803075cdaa2
topic_type:
- apiref
ms.openlocfilehash: 4545f5f8d78e588c655a72340210a785b0feb619
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720407"
---
# <a name="imetadataimportenumtypedefs-method"></a>IMetaDataImport::EnumTypeDefs 方法

枚举表示当前范围内的所有类型的 TypeDef 标记。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EnumTypeDefs (  
   [out] HCORENUM   *phEnum,
   [in]  mdTypeDef  rTypeDefs[],  
   [in]  ULONG      cMax,
   [out] ULONG      *pcTypeDefs  
);  
```  
  
## <a name="parameters"></a>参数  

 `phEnum`  
 弄指向新枚举器的指针。 第一次调用此方法时，此值必须为 NULL。  
  
 `rTypeDefs`  
 中用于存储 TypeDef 标记的数组。  
  
 `cMax`  
 [in] `rTypeDefs` 数组的最大大小。  
  
 `pcTypeDefs`  
 弄中返回的 TypeDef 标记的数目 `rTypeDefs` 。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|`S_OK`|`EnumTypeDefs` 已成功返回。|  
|`S_FALSE`|没有要枚举的令牌。 在这种情况下， `pcTypeDefs` 为零。|  
  
## <a name="remarks"></a>注解  

 TypeDef 标记表示一种类型，如类或接口，以及通过扩展性机制添加的任何类型。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataImport 接口](imetadataimport-interface.md)
- [IMetaDataImport2 接口](imetadataimport2-interface.md)
