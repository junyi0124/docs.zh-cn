---
title: IMetaDataImport::EnumFieldsWithName 方法
ms.date: 03/30/2017
api_name:
- IMetaDataImport.EnumFieldsWithName
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::EnumFieldsWithName
helpviewer_keywords:
- IMetaDataImport::EnumFieldsWithName method [.NET Framework metadata]
- EnumFieldsWithName method [.NET Framework metadata]
ms.assetid: 42145e8d-000f-4d0b-ae43-c08201190fa2
topic_type:
- apiref
ms.openlocfilehash: 0a254587282dea43a3507fbbeca35bd7aa9604f3
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95711566"
---
# <a name="imetadataimportenumfieldswithname-method"></a>IMetaDataImport::EnumFieldsWithName 方法

枚举具有指定名称的指定类型的 FieldDef 标记。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EnumFieldsWithName (  
   [in, out] HCORENUM    *phEnum,
   [in]  mdTypeDef       cl,
   [in]  LPCWSTR         szName,
   [out] mdFieldDef      rFields[],
   [in]  ULONG           cMax,
   [out] ULONG           *pcTokens
);  
```  
  
## <a name="parameters"></a>参数  

 `phEnum`  
 [in，out]指向枚举器的指针。  
  
 `cl`  
 中要枚举其字段的类型的标记。  
  
 `szName`  
 中用于限制枚举的作用域的字段名称。  
  
 `rFields`  
 弄用于存储 FieldDef 标记的数组。  
  
 `cMax`  
 [in] `rFields` 数组的最大大小。  
  
 `pcTokens`  
 弄中返回的 FieldDef 令牌的实际数量 `rFields` 。  
  
## <a name="remarks"></a>注解  

 与 [IMetaDataImport：： EnumFields](imetadataimport-enumfields-method.md)不同，会 `EnumFieldsWithName` 丢弃所有不具有指定名称的字段标记。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|`S_OK`|`EnumFieldsWithName` 已成功返回。|  
|`S_FALSE`|没有要枚举的字段。 在这种情况下， `pcTokens` 为零。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataImport 接口](imetadataimport-interface.md)
- [IMetaDataImport2 接口](imetadataimport2-interface.md)
