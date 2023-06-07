---
title: IMetaDataImport::GetCustomAttributeProps 方法
ms.date: 03/30/2017
api_name:
- IMetaDataImport.GetCustomAttributeProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetCustomAttributeProps
helpviewer_keywords:
- GetCustomAttributeProps method [.NET Framework metadata]
- IMetaDataImport::GetCustomAttributeProps method [.NET Framework metadata]
ms.assetid: 6eefb243-a281-41c1-bcdc-7e17513bc446
topic_type:
- apiref
ms.openlocfilehash: b92e9ab714e2d8d2c66ed5546deba16352e8e390
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95711138"
---
# <a name="imetadataimportgetcustomattributeprops-method"></a>IMetaDataImport::GetCustomAttributeProps 方法

获取给定元数据标记的自定义属性的值。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetCustomAttributeProps (  
   [in]            mdCustomAttribute   cv,  
   [out, optional] mdToken             *ptkObj,  
   [out, optional] mdToken             *ptkType,  
   [out, optional] void const          **ppBlob,  
   [out, optional] ULONG               *pcbSize  
);  
```  
  
## <a name="parameters"></a>参数  

 `cv`  
 [in] 表示要检索的自定义属性的元数据标记。  
  
 `ptkObj`  
 [out, optional] 表示自定义属性修改的对象的元数据标记。 此值可为任何类型的元数据标记（`mdCustomAttribute` 除外）。  
  
 `ptkType`  
 [out, optional] 表示已返回自定义属性的 <xref:System.Type> 的 `mdMethodDef` 或 `mdMemberRef` 元数据标记。  
  
 `ppBlob`  
 [out, optional] 指向自定义属性的值的数据数组指针。  
  
 `pcbSize`  
 [out, optional] *`ppBlob` 中返回的数据大小（以字节为单位）。  
  
## <a name="remarks"></a>注解  

 自定义属性以元数据引擎理解的格式存储为数据数组。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataImport 接口](imetadataimport-interface.md)
- [IMetaDataImport2 接口](imetadataimport2-interface.md)
