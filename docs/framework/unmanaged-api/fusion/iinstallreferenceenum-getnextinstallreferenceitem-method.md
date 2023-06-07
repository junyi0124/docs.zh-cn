---
title: IInstallReferenceEnum::GetNextInstallReferenceItem 方法
ms.date: 03/30/2017
api_name:
- IInstallReferenceEnum.GetNextInstallReferenceItem
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IInstallReferenceEnum::GetNextInstallReferenceItem
helpviewer_keywords:
- IInstallReferenceEnum::GetNextInstallReferenceItem method [.NET Framework fusion]
- GetNextInstallReferenceItem method [.NET Framework fusion]
ms.assetid: ce969c9d-6538-4c34-8784-148ffd99fe7a
topic_type:
- apiref
ms.openlocfilehash: e093de7d2c2388274cbe9ebbe46084ee6ae3ff8c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95721096"
---
# <a name="iinstallreferenceenumgetnextinstallreferenceitem-method"></a>IInstallReferenceEnum::GetNextInstallReferenceItem 方法

获取一个指针，该指针指向此[IInstallReferenceEnum](iinstallreferenceenum-interface.md)对象中包含的下一个[IInstallReferenceItem](iinstallreferenceitem-interface.md)对象。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetNextInstallReferenceItem (  
    [out] IInstallReferenceItem **ppRefItem,  
    [in]  DWORD dwFlags,  
    [in]  LPVOID pvReserved  
);  
```  
  
## <a name="parameters"></a>参数  

 `ppRefItem`  
 弄返回的 `IInstallReferenceItem` 指针。  
  
 `dwFlags`  
 中保留以供将来进行扩展。 `dwFlags` 必须为 0 (零) 。  
  
 `pvReserved`  
 中保留以供将来进行扩展。 `pvReserved` 必须为空引用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** 合成。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IInstallReferenceItem 接口](iinstallreferenceitem-interface.md)
- [IInstallReferenceEnum 接口](iinstallreferenceenum-interface.md)
