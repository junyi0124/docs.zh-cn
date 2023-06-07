---
title: IMetaDataImport::EnumInterfaceImpls 方法
ms.date: 03/30/2017
api_name:
- IMetaDataImport.EnumInterfaceImpls
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::EnumInterfaceImpls
helpviewer_keywords:
- IMetaDataImport::EnumInterfaceImpls method [.NET Framework metadata]
- EnumInterfaceImpls method [.NET Framework metadata]
ms.assetid: ba6e178f-128b-4e47-a13c-b4be73eb106c
topic_type:
- apiref
ms.openlocfilehash: 0b040a2741a44b9d361dabc38c26b8934659003b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95711515"
---
# <a name="imetadataimportenuminterfaceimpls-method"></a>IMetaDataImport::EnumInterfaceImpls 方法

枚举由指定实现的所有接口 `TypeDef` 。
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EnumInterfaceImpls (  
   [in, out]  HCORENUM       *phEnum,
   [in]   mdTypeDef          td,  
   [out]  mdInterfaceImpl    rImpls[],
   [in]   ULONG              cMax,  
   [out]  ULONG*             pcImpls  
);  
```  
  
## <a name="parameters"></a>参数  

 `phEnum`  
 [in，out]指向枚举器的指针。  
  
 `td`  
 中要枚举其 MethodDef 标记表示接口实现的 TypeDef 的标记。  
  
 `rImpls`  
 弄用于存储 MethodDef 标记的数组。  
  
 `cMax`  
 中数组的最大长度 `rImpls` 。  
  
 `pcImpls`  
 弄返回的标记的实际数量 `rImpls` 。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|`S_OK`|`EnumInterfaceImpls` 已成功返回。|  
|`S_FALSE`|没有要枚举的 MethodDef 标记。 在这种情况下， `pcImpls` 设置为零。|  

## <a name="remarks"></a>注解

枚举返回 `mdInterfaceImpl` 由指定的实现的每个接口的令牌集合 `TypeDef` 。 通过或)  (指定接口的顺序来返回接口令牌 `DefineTypeDef` `SetTypeDefProps` 。 `mdInterfaceImpl`可以使用[GetInterfaceImplProps](imetadataimport-getinterfaceimplprops-method.md)查询返回的标记的属性。
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataImport 接口](imetadataimport-interface.md)
- [IMetaDataImport2 接口](imetadataimport2-interface.md)
