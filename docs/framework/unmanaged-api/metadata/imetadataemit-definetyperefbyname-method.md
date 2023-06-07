---
title: IMetaDataEmit::DefineTypeRefByName 方法
ms.date: 03/30/2017
api_name:
- IMetaDataEmit.DefineTypeRefByName
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit::DefineTypeRefByName
helpviewer_keywords:
- DefineTypeRefByName method [.NET Framework metadata]
- IMetaDataEmit::DefineTypeRefByName method [.NET Framework metadata]
ms.assetid: c30a4ce3-2d3e-411a-98df-e62ac4a5dd50
topic_type:
- apiref
ms.openlocfilehash: b83c868f1a804d4e6f32adffc190ae086aa5e0a0
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95719328"
---
# <a name="imetadataemitdefinetyperefbyname-method"></a>IMetaDataEmit::DefineTypeRefByName 方法

获取在指定范围内定义的类型（位于当前范围之外）的元数据标记。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT DefineTypeRefByName (
    [in]  mdToken     tkResolutionScope,
    [in]  LPCWSTR     szName,
    [out] mdTypeRef   *ptr
);  
```  
  
## <a name="parameters"></a>参数  

 `tkResolutionScope`  
 中指定解析范围的标记。 以下令牌类型有效：  
  
- `mdModuleRef`如果在定义调用方的同一程序集中定义了该类型，则为。  
  
- `mdAssemblyRef`如果类型是在定义调用方的程序集中定义的，则为。  
  
- `mdTypeRef`如果类型是嵌套类型，则为。  
  
- `mdModule`如果在定义调用方的同一模块中定义了该类型，则为。  
  
- 如果类型是全局定义的，则为 Null。  
  
 `szName`  
 中Unicode 中目标类型的名称。  
  
 `ptr`  
 弄指向分配给类型的标记的指针 `mdTypeRef` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MSCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataEmit 接口](imetadataemit-interface.md)
- [IMetaDataEmit2 接口](imetadataemit2-interface.md)
