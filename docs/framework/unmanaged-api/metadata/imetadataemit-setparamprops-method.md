---
title: IMetaDataEmit::SetParamProps 方法
ms.date: 03/30/2017
api_name:
- IMetaDataEmit.SetParamProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit::SetParamProps
helpviewer_keywords:
- IMetaDataEmit::SetParamProps method [.NET Framework metadata]
- SetParamProps method [.NET Framework metadata]
ms.assetid: a95a3908-9f87-4084-937e-8e01ef03ad63
topic_type:
- apiref
ms.openlocfilehash: b0cc28807938bcfb9b2465093ff4cfb94066ee98
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95675056"
---
# <a name="imetadataemitsetparamprops-method"></a>IMetaDataEmit::SetParamProps 方法

设置或更改之前调用 [IMetaDataEmit：:D efineparam](imetadataemit-defineparam-method.md)时定义的方法参数的功能。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetParamProps (
    [in]  mdParamDef  pd,
    [in]  LPCWSTR     szName,
    [in]  DWORD       dwParamFlags,
    [in]  DWORD       dwCPlusTypeFlag,
    [in]  void const  *pValue,
    [in]  ULONG       cchValue
);  
```  
  
## <a name="parameters"></a>参数  

 `pd`  
 中目标参数的标记。  
  
 `szName`  
 中Unicode 中参数的名称。  
  
 `dwParamFlags`  
 中参数的标志。  
  
 `dwCPlusTypeFlag`  
 中常量值的 ELEMENT_TYPE_ *。  
  
 `pValue`  
 中参数的常数值。  
  
 `cchValue`  
 中 (Unicode) 字符的大小 `pValue` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MSCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataEmit 接口](imetadataemit-interface.md)
- [IMetaDataEmit2 接口](imetadataemit2-interface.md)
