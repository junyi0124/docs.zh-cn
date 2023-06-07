---
title: IMetaDataDispenserEx::GetOption 方法
ms.date: 03/30/2017
api_name:
- IMetaDataDispenserEx.GetOption
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataDispenserEx::GetOption
helpviewer_keywords:
- GetOption method [.NET Framework metadata]
- IMetaDataDispenserEx::GetOption method [.NET Framework metadata]
ms.assetid: d7f794e5-8e25-4d65-850a-7c34fbfce87d
topic_type:
- apiref
ms.openlocfilehash: 0ceadf42ac49fd3fc89c78a6a26b2f529afeeaf0
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95700556"
---
# <a name="imetadatadispenserexgetoption-method"></a>IMetaDataDispenserEx::GetOption 方法

为当前元数据范围获取指定选项的值。 选项控制如何处理对当前元数据范围的调用。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetOption (  
    [in]  REFGUID         optionId,
    [out] const VARIANT   *pValue  
);  
```  
  
## <a name="parameters"></a>参数  

 `optionId`  
 中指向 GUID 的指针，该 GUID 指定要检索的选项。 有关支持的 Guid 的列表，请参阅 "备注" 部分。  
  
 `pValue`  
 弄返回的选项的值。 此值的类型将是指定选项的类型的变体。  
  
## <a name="remarks"></a>注解  

 以下列表显示了此方法支持的 Guid。 有关说明，请参阅 [IMetaDataDispenserEx：： SetOption](imetadatadispenserex-setoption-method.md) 方法。 如果不 `optionId` 在此列表中，则此方法返回 HRESULT `E_INVALIDARG` ，指示参数不正确。  
  
- MetaDataCheckDuplicatesFor  
  
- MetaDataRefToDefCheck  
  
- MetaDataNotificationForTokenMovement  
  
- MetaDataSetENC  
  
- MetaDataErrorIfEmitOutOfOrder  
  
- MetaDataGenerateTCEAdapters  
  
- MetaDataLinkerOptions  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MsCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataDispenserEx 接口](imetadatadispenserex-interface.md)
- [IMetaDataDispenser 接口](imetadatadispenser-interface.md)
