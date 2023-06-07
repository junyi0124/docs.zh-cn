---
title: IMetaDataEmit::SetEventProps 方法
ms.date: 03/30/2017
api_name:
- IMetaDataEmit.SetEventProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit::SetEventProps
helpviewer_keywords:
- IMetaDataEmit::SetEventProps method [.NET Framework metadata]
- SetEventProps method [.NET Framework metadata]
ms.assetid: 3b039e50-63ec-4730-99ff-2327408de477
topic_type:
- apiref
ms.openlocfilehash: 5c2880ac07f0317bc36ff4bbde68cd3a25febf52
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95721980"
---
# <a name="imetadataemitseteventprops-method"></a>IMetaDataEmit::SetEventProps 方法

设置或更新通过之前对 IMetaDataEmit 的调用定义的事件的指定功能 [：:D efineevent](imetadataemit-defineevent-method.md)。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetEventProps (  
    [in]  mdEvent     ev,
    [in]  DWORD       dwEventFlags,
    [in]  mdToken     tkEventType,
    [in]  mdMethodDef mdAddOn,
    [in]  mdMethodDef mdRemoveOn,
    [in]  mdMethodDef mdFire,
    [in]  mdMethodDef rmdOtherMethods[]
);  
```  
  
## <a name="parameters"></a>参数  

 `ev`  
 中事件标记。  
  
 `dwEventFlags`  
 中事件标志。 这是一个值的位掩码 `CorEventAttr` 。  
  
 `tkEventType`  
 中事件类的标记。 这是 `mdTypeDef` 或 `mdTypeRef` 令牌。  
  
 `mdAddOn`  
 中用于订阅事件的方法，或为 null。  
  
 `mdRemoveOn`  
 中用于取消订阅事件的方法，或为 null。  
  
 `mdFire`  
 中派生类 (用来引发事件) 的方法。  
  
 `rmdOtherMethods[]`  
 中与事件关联的其他方法的标记数组。 数组的最后一个元素必须是 `mdMethodDefNil` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MSCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataEmit 接口](imetadataemit-interface.md)
- [IMetaDataEmit2 接口](imetadataemit2-interface.md)
