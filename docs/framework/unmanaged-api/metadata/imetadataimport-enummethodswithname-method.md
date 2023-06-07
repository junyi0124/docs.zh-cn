---
title: IMetaDataImport::EnumMethodsWithName 方法
ms.date: 03/30/2017
api_name:
- IMetaDataImport.EnumMethodsWithName
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::EnumMethodsWithName
helpviewer_keywords:
- IMetaDataImport::EnumMethodsWithName method [.NET Framework metadata]
- EnumMethodsWithName method [.NET Framework metadata]
ms.assetid: a8624913-2e23-46ad-a0c1-bb8eccbbf20f
topic_type:
- apiref
ms.openlocfilehash: 5b5345fc4819716dc6c2a00323f94546cfc67f32
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720927"
---
# <a name="imetadataimportenummethodswithname-method"></a>IMetaDataImport::EnumMethodsWithName 方法

枚举具有指定名称并且由指定的 TypeDef 标记所引用的类型定义的方法。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EnumMethodsWithName (  
   [in, out] HCORENUM    *phEnum,  
   [in]  mdTypeDef       cl,  
   [in]  LPCWSTR         szName,  
   [out] mdMethodDef     rMethods[],  
   [in]  ULONG           cMax,  
   [out] ULONG           *pcTokens  
);  
```  
  
## <a name="parameters"></a>参数  

 `phEnum`  
 [in，out]指向枚举器的指针。 第一次调用此方法时，此值必须为 NULL。  
  
 `cl`  
 中一个 TypeDef 标记，表示其方法要枚举的类型。  
  
 `szName`  
 中限制枚举的作用域的名称。  
  
 `rMethods`  
 弄用于存储 MethodDef 标记的数组。  
  
 `cMax`  
 [in] `rMethods` 数组的最大大小。  
  
 `pcTokens`  
 弄中返回的 MethodDef 标记的数目 `rMethods` 。  
  
## <a name="remarks"></a>注解  

 此方法枚举字段和方法，而不是属性或事件。 与 [IMetaDataImport：： EnumMethods](imetadataimport-enummethods-method.md)不同，会 `EnumMethodsWithName` 丢弃所有不具有指定名称的方法标记。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|`S_OK`|`EnumMethodsWithName` 已成功返回。|  
|`S_FALSE`|没有要枚举的令牌。 在这种情况下， `pcTokens` 为零。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataImport 接口](imetadataimport-interface.md)
- [IMetaDataImport2 接口](imetadataimport2-interface.md)
