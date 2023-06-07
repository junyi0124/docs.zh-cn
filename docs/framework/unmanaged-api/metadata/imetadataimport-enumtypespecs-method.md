---
title: IMetaDataImport::EnumTypeSpecs 方法
ms.date: 03/30/2017
api_name:
- IMetaDataImport.EnumTypeSpecs
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::EnumTypeSpecs
helpviewer_keywords:
- EnumTypeSpecs method [.NET Framework metadata]
- IMetaDataImport::EnumTypeSpecs method [.NET Framework metadata]
ms.assetid: 75331c7b-988b-436c-9eb9-a270d37b4f06
topic_type:
- apiref
ms.openlocfilehash: 38c9f8df12b0fc83a236d2cb7c32d1198be7096d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95719809"
---
# <a name="imetadataimportenumtypespecs-method"></a>IMetaDataImport::EnumTypeSpecs 方法

枚举当前元数据范围内定义的 TypeSpec 标记。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EnumTypeSpecs (  
   [in, out] HCORENUM    *phEnum,  
   [out] mdTypeSpec      rTypeSpecs[],  
   [in]  ULONG           cMax,  
   [out] ULONG           *pcTypeSpecs  
);  
```  
  
## <a name="parameters"></a>参数  

 `phEnum`  
 [in，out]指向枚举器的指针。 此方法的第一次调用时，此值必须为 NULL。  
  
 `rTypeSpecs`  
 弄用于存储 TypeSpec 标记的数组。  
  
 `cMax`  
 [in] `rTypeSpecs` 数组的最大大小。  
  
 `pcTypeSpecs`  
 弄中返回的 TypeSpec 标记的数目 `rTypeSpecs` 。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|`S_OK`|`EnumTypeSpecs` 已成功返回。|  
|`S_FALSE`|没有要枚举的令牌。 在这种情况下， `pcTypeSpecs` 为零。|  
  
## <a name="remarks"></a>注解  

 TypeSpec 标记由 [IMetaDataEmit：： GetTokenFromTypeSpec](imetadataemit-gettokenfromtypespec-method.md) 方法创建。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataImport 接口](imetadataimport-interface.md)
- [IMetaDataImport2 接口](imetadataimport2-interface.md)
