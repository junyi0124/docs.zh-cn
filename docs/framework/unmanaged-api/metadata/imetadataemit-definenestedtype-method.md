---
title: IMetaDataEmit::DefineNestedType 方法
ms.date: 03/30/2017
api_name:
- IMetaDataEmit.DefineNestedType
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit::DefineNestedType
helpviewer_keywords:
- IMetaDataEmit::DefineNestedType method [.NET Framework metadata]
- DefineNestedType method [.NET Framework metadata]
ms.assetid: 1e994de6-4628-459c-b967-b34be1e9fe4f
topic_type:
- apiref
ms.openlocfilehash: 99dc141cca0f911c8dd65645f6c22d950cc678d4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95719536"
---
# <a name="imetadataemitdefinenestedtype-method"></a>IMetaDataEmit::DefineNestedType 方法

创建类型定义的元数据签名，返回 `mdTypeDef` 该类型的标记，并指定该定义类型是参数所引用的类型的成员 `tdEncloser` 。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT DefineNestedType (
    [in]  LPCWSTR     szTypeDef,  
    [in]  DWORD       dwTypeDefFlags,
    [in]  mdToken     tkExtends,
    [in]  mdToken     rtkImplements[],
    [in]  mdTypeDef   tdEncloser,
    [out] mdTypeDef   *ptd  
);  
```  
  
## <a name="parameters"></a>参数  

 `szTypeDef`  
 中Unicode 中类型的名称。  
  
 `dwTypeDefFlags`  
 [in] `TypeDef` 属性. 这是一个值的位掩码 `CorTypeAttr` 。  
  
 `tkExtends`  
 中基类的标记。 这是 `mdTypeDef` 或 `mdTypeRef` 令牌。  
  
 `rtkImplements`[]  
 中标记的数组，该数组指定此类或接口实现的接口。  
  
 `tdEncloser`  
 中封闭类型的标记。 数组的最后一个元素必须是 `mdTokenNil` 。  
  
 `ptd`  
 弄 `mdTypeDef` 分配的令牌。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MSCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataEmit 接口](imetadataemit-interface.md)
- [IMetaDataEmit2 接口](imetadataemit2-interface.md)
