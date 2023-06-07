---
title: IMetaDataImport::GetCustomAttributeByName 方法
ms.date: 03/30/2017
api_name:
- IMetaDataImport.GetCustomAttributeByName
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetCustomAttributeByName
helpviewer_keywords:
- IMetaDataImport::GetCustomAttributeByName method [.NET Framework metadata]
- GetCustomAttributeByName method [.NET Framework metadata]
ms.assetid: 909aa530-2e3b-4d0a-a38a-a2750e535d7d
topic_type:
- apiref
ms.openlocfilehash: 3eb894aaf8ccdc99ea23ddf946f39f3ec71773d1
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95711203"
---
# <a name="imetadataimportgetcustomattributebyname-method"></a>IMetaDataImport::GetCustomAttributeByName 方法

获取自定义特性（给定其名称和所有者）。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetCustomAttributeByName (  
   [in]  mdToken          tkObj,  
   [in]  LPCWSTR          szName,  
   [out] const void       **ppData,  
   [out] ULONG            *pcbData  
);  
```  
  
## <a name="parameters"></a>参数  

 `tkObj`  
 中一个元数据标记，它表示拥有自定义特性的对象。  
  
 `szName`  
 中自定义属性的名称。  
  
 `ppData`  
 弄一个指针，它指向作为自定义属性的值的数据数组。  
  
 `pcbData`  
 弄在 * 中返回的数据的大小（以字节为单位） `ppData` 。  
  
## <a name="remarks"></a>注解  

 为同一个所有者定义多个自定义属性是合法的;它们甚至可能具有相同的名称。 但是， `GetCustomAttributeByName` 只返回一个实例。  (`GetCustomAttributeByName` 返回它遇到的第一个实例。 ) 若要查找自定义属性的所有实例，请调用 [IMetaDataImport：： EnumCustomAttributes](imetadataimport-enumcustomattributes-method.md) 方法。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataImport 接口](imetadataimport-interface.md)
- [IMetaDataImport2 接口](imetadataimport2-interface.md)
