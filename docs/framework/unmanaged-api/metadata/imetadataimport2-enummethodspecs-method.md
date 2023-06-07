---
title: IMetaDataImport2::EnumMethodSpecs 方法
ms.date: 03/30/2017
api_name:
- IMetaDataImport2.EnumMethodSpecs
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport2::EnumMethodSpecs
helpviewer_keywords:
- IMetaDataImport2::EnumMethodSpecs method [.NET Framework metadata]
- EnumMethodSpecs method [.NET Framework metadata]
ms.assetid: b3fc1e6c-bcb6-4915-baf8-7dc0a31b8724
topic_type:
- apiref
ms.openlocfilehash: 26b345567699c5780827ed835cff13069ea8f609
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95702740"
---
# <a name="imetadataimport2enummethodspecs-method"></a>IMetaDataImport2::EnumMethodSpecs 方法

获取与指定的 MethodDef 或 MemberRef 标记相关联的 MethodSpec 标记数组的枚举器。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EnumMethodSpecs (  
    [in, out] HCORENUM      *phEnum,
    [in]      mdToken       tk,  
    [out]     mdMethodSpec  rMethodSpecs[],  
    [in]      ULONG         cMax,  
    [out]     ULONG         *pcMethodSpecs  
);
```  
  
## <a name="parameters"></a>参数  

 `phEnum`  
 [in，out]指向的枚举器的指针 `rMethodSpecs` 。  
  
 `tk`  
 中MemberRef 或 MethodDef 标记，表示要枚举其 MethodSpec 标记的方法。 如果的值 `tk` 为 0 (零) ，则将枚举范围内的所有 MethodSpec 标记。  
  
 `rMethodSpecs`  
 弄要枚举的 MethodSpec 标记的数组。  
  
 `cMax`  
 中要放入的请求的最大数量 `rMethodSpecs` 。  
  
 `pcMethodSpecs`  
 弄放入的标记数 `rMethodSpecs` 。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|`S_OK`|`EnumMethodSpecs` 已成功返回。|  
|`S_FALSE`|`phEnum` 没有成员元素。 在这种情况下， `pcMethodSpecs` 设置为 0 (零) 。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MsCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataImport2 接口](imetadataimport2-interface.md)
- [IMetaDataImport 接口](imetadataimport-interface.md)
