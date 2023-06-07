---
title: IMetaDataEmit::DefineProperty 方法
ms.date: 03/30/2017
api_name:
- IMetaDataEmit.DefineProperty
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit::DefineProperty
helpviewer_keywords:
- IMetaDataEmit::DefineProperty method [.NET Framework metadata]
- DefineProperty method [.NET Framework metadata]
ms.assetid: 5c4c1dc2-d40d-4173-bbe6-7058fb21c98f
topic_type:
- apiref
ms.openlocfilehash: d2a4a15126f34666a58021a59e9e193685b15a49
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95719484"
---
# <a name="imetadataemitdefineproperty-method"></a>IMetaDataEmit::DefineProperty 方法

使用指定的和方法访问器创建指定类型的属性定义， `get` `set` 并获取该属性定义的标记。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT DefineProperty (
    [in]  mdTypeDef          td,
    [in]  LPCWSTR            szProperty,
    [in]  DWORD              dwPropFlags,
    [in]  PCCOR_SIGNATURE    pvSig,
    [in]  ULONG              cbSig,
    [in]  DWORD              dwCPlusTypeFlag,
    [in]  void const         *pValue,
    [in]  ULONG              cchValue,
    [in]  mdMethodDef        mdSetter,
    [in]  mdMethodDef        mdGetter,
    [in]  mdMethodDef        rmdOtherMethods[],
    [out] mdProperty         *pmdProp
);  
```  
  
## <a name="parameters"></a>参数  

 `td`  
 中正在为其定义属性的类或接口的标记。  
  
 `szProperty`  
 中属性的名称。  
  
 `dwPropFlags`  
 中属性标志。  
  
 `pvSig`  
 中属性签名。  
  
 `cbSig`  
 中中的字节数 `pvSig` 。  
  
 `dwCPlusTypeFlag`  
 中属性的默认值的类型。  
  
 `pValue`  
 中属性的默认值。  
  
 `cchValue`  
 中中 (Unicode) 字符的计数 `pValue` 。  
  
 `mdSetter`  
 中用于设置属性值的方法。  
  
 `mdGetter`  
 中获取属性值的方法。  
  
 `rmdOtherMethods[]`  
 中与属性关联的其他方法的数组。 使用终止数组 `mdTokenNil` 。  
  
 `pmdProp`  
 弄 `mdProperty` 分配的令牌。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MSCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataEmit 接口](imetadataemit-interface.md)
- [IMetaDataEmit2 接口](imetadataemit2-interface.md)
