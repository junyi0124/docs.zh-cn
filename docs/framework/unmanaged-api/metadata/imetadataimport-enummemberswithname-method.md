---
title: IMetaDataImport::EnumMembersWithName 方法
ms.date: 03/30/2017
api_name:
- IMetaDataImport.EnumMembersWithName
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::EnumMembersWithName
helpviewer_keywords:
- IMetaDataImport::EnumMembersWithName method [.NET Framework metadata]
- EnumMembersWithName method [.NET Framework metadata]
ms.assetid: 7c9e9120-3104-42f0-86ce-19a025f20dcc
topic_type:
- apiref
ms.openlocfilehash: 35b82c33e54619eb9bebd5e5749ae202e905357a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720981"
---
# <a name="imetadataimportenummemberswithname-method"></a>IMetaDataImport::EnumMembersWithName 方法

枚举表示具有指定名称的指定类型的成员的 MemberDef 标记。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EnumMembersWithName (  
   [in, out] HCORENUM    *phEnum,
   [in]      mdTypeDef   cl,
   [in]      LPCWSTR     szName,
   [out]     mdToken     rMembers[],
   [in]      ULONG       cMax,
   [out]     ULONG       *pcTokens  
);  
```  
  
## <a name="parameters"></a>参数  

 `phEnum`  
 [in，out]指向枚举器的指针。  
  
 `cl`  
 中一个 TypeDef 标记，它表示具有要枚举的成员的类型。  
  
 `szName`  
 中限制枚举器范围的成员名称。  
  
 `rMembers`  
 弄用于存储 MemberDef 标记的数组。  
  
 `cMax`  
 [in] `rMembers` 数组的最大大小。  
  
 `pcTokens`  
 弄中返回的 MemberDef 令牌的实际数量 `rMembers` 。  
  
## <a name="remarks"></a>注解  

 此方法枚举字段和方法，而不是属性或事件。 与 [IMetaDataImport：： EnumMembers](imetadataimport-enummembers-method.md)不同，会 `EnumMembersWithName` 丢弃所有不具有指定名称的字段和成员标记。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|`S_OK`|`EnumTypeDefs` 已成功返回。|  
|`S_FALSE`|没有要枚举的 MemberDef 令牌。 在这种情况下， `pcTokens` 为零。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataImport 接口](imetadataimport-interface.md)
- [IMetaDataImport2 接口](imetadataimport2-interface.md)
