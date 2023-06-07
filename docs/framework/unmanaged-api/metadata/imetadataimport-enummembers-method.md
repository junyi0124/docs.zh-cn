---
title: IMetaDataImport::EnumMembers 方法
ms.date: 03/30/2017
api_name:
- IMetaDataImport.EnumMembers
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::EnumMembers
helpviewer_keywords:
- IMetaDataImport::EnumMembers method [.NET Framework metadata]
- EnumMembers method [.NET Framework metadata]
ms.assetid: 3fb8e178-342b-4c89-9bcf-f7f834e6cb77
topic_type:
- apiref
ms.openlocfilehash: 92503df60ae44dfd44819fe3eda8e6a0549b2b66
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720980"
---
# <a name="imetadataimportenummembers-method"></a>IMetaDataImport::EnumMembers 方法

枚举表示指定类型的成员的 MemberDef 标记。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EnumMembers (
   [in, out]  HCORENUM    *phEnum,
   [in]  mdTypeDef   cl,
   [out] mdToken     rMembers[],
   [in]  ULONG       cMax,
   [out] ULONG       *pcTokens  
);  
```  
  
## <a name="parameters"></a>参数  

 `phEnum`  
 [in，out]指向枚举器的指针。  
  
 `cl`  
 中一个 TypeDef 标记，表示要枚举其成员的类型。  
  
 `rMembers`  
 弄用于保存 MemberDef 标记的数组。  
  
 `cMax`  
 [in] `rMembers` 数组的最大大小。  
  
 `pcTokens`  
 弄中返回的 MemberDef 令牌的实际数量 `rMembers` 。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|`S_OK`|`EnumMembers` 已成功返回。|  
|`S_FALSE`|没有要枚举的 MemberDef 令牌。 在这种情况下， `pcTokens` 为零。|  
  
## <a name="remarks"></a>注解  

 枚举类的成员集合时， `EnumMembers` 只会返回 (字段和方法的成员，但 **不会** 返回直接在类上定义的属性或事件) 。 即使该类为这些继承成员提供了实现，它也不会返回类继承的任何成员。 若要枚举继承成员，调用方必须显式遍历继承链。 请注意，根据发出原始元数据的语言或编译器，继承链的规则可能会有所不同。

 属性和事件不是由枚举的 `EnumMembers` 。 若要枚举这些枚举，请使用 [EnumProperties](imetadataimport-enumproperties-method.md) 或 [EnumEvents](imetadataimport-enumevents-method.md)。
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataImport 接口](imetadataimport-interface.md)
- [IMetaDataImport2 接口](imetadataimport2-interface.md)
