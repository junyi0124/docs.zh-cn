---
title: IMetaDataEmit2::DefineGenericParam 方法
ms.date: 03/30/2017
api_name:
- IMetaDataEmit2.DefineGenericParam
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit2::DefineGenericParam
helpviewer_keywords:
- IMetaDataEmit2::DefineGenericParam method [.NET Framework metadata]
- DefineGenericParam method [.NET Framework metadata]
ms.assetid: 47b2a3b6-907d-43dc-858d-1ae7dca1316a
topic_type:
- apiref
ms.openlocfilehash: c9ff918121e7bb4ee972e674207810358b3f36f2
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95712906"
---
# <a name="imetadataemit2definegenericparam-method"></a>IMetaDataEmit2::DefineGenericParam 方法

创建泛型类型参数的定义，并获取该泛型类型参数的令牌。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT DefineGenericParam (
    [in]  mdToken         tk,
    [in]  ULONG           ulParamSeq,
    [in]  DWORD           dwParamFlags,
    [in]  LPCWSTR         szname,
    [in]  DWORD           reserved,
    [in]  mdToken         rtkConstraints[],
    [out] mdGenericParam  *pgp  
);  
```  
  
## <a name="parameters"></a>参数  

 `tk`  
 中 `mdTypeDef` 或 `mdMethodDef` 标记，它表示要为其定义泛型参数的方法或构造函数。  
  
 `ulParamSeq`  
 中泛型参数的索引。  
  
 `dwParamFlags`  
 中描述泛型参数类型的 [CorGenericParamAttr](corgenericparamattr-enumeration.md) 枚举的值。  
  
 `szname`  
 中参数的名称。  
  
 `reserved`  
 中此参数保留供将来进行扩展。  
  
 `rtkConstraints`  
 中类型约束的零终止数组。 数组成员必须是 `mdTypeDef` 、 `mdTypeRef` 或 `mdTypeSpec` 元数据标记。  
  
 `pgp`  
 弄表示泛型参数的标记。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MsCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataEmit2 接口](imetadataemit2-interface.md)
- [IMetaDataEmit 接口](imetadataemit-interface.md)
