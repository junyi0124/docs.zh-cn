---
title: IMetaDataImport::EnumSignatures 方法
ms.date: 03/30/2017
api_name:
- IMetaDataImport.EnumSignatures
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::EnumSignatures
helpviewer_keywords:
- EnumSignatures method [.NET Framework metadata]
- IMetaDataImport::EnumSignatures method [.NET Framework metadata]
ms.assetid: d0d65060-6f90-42a2-95cf-6ffb04352996
topic_type:
- apiref
ms.openlocfilehash: 3021124184ab0491337a07144e6f77b5bfea3681
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95721967"
---
# <a name="imetadataimportenumsignatures-method"></a>IMetaDataImport::EnumSignatures 方法

枚举当前范围内表示独立签名的 Signature 标记。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EnumSignatures (  
   [in, out] HCORENUM     *phEnum,  
   [out]     mdSignature  rSignatures[],  
   [in]      ULONG        cMax,  
   [out]     ULONG        *pcSignatures  
);  
```  
  
## <a name="parameters"></a>参数  

 `phEnum`  
 [in，out]指向枚举器的指针。 第一次调用此方法时，此值必须为 NULL。  
  
 `rSignatures`  
 弄用于存储签名令牌的数组。  
  
 `cMax`  
 [in] `rSignatures` 数组的最大大小。  
  
 `pcSignatures`  
 弄中返回的签名令牌数 `rSignatures` 。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|`S_OK`|`EnumSignatures` 已成功返回。|  
|`S_FALSE`|没有要枚举的令牌。 在这种情况下， `pcSignatures` 为零。|  
  
## <a name="remarks"></a>注解  

 签名令牌由 [IMetaDataEmit：： GetTokenFromSig](imetadataemit-gettokenfromsig-method.md) 方法创建。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataImport 接口](imetadataimport-interface.md)
- [IMetaDataImport2 接口](imetadataimport2-interface.md)
