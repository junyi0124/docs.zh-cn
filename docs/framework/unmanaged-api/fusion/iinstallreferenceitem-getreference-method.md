---
title: IInstallReferenceItem::GetReference 方法
ms.date: 03/30/2017
api_name:
- IInstallReferenceItem.GetReference
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IInstallReferenceItem::GetReference
helpviewer_keywords:
- IInstallReferenceItem::GetReference method [.NET Framework fusion]
- GetReference method [.NET Framework fusion]
ms.assetid: 8960332f-c98a-405a-ba92-7003de0c1187
topic_type:
- apiref
ms.openlocfilehash: 14286970a4f7093d72b47b780ea880f5ccb1bca5
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95721067"
---
# <a name="iinstallreferenceitemgetreference-method"></a>IInstallReferenceItem::GetReference 方法

获取一个指针，该指针指向此[IInstallReferenceItem](iinstallreferenceitem-interface.md)对象表示的[FUSION_INSTALL_REFERENCE](fusion-install-reference-structure.md)结构。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetReference (  
    [out] LPFUSION_INSTALL_REFERENCE *ppRefData,  
    [in]  DWORD dwFlags,  
    [in]  LPVOID pvReserved  
);  
```  
  
## <a name="parameters"></a>参数  

 `ppRefData`  
 弄返回的 `FUSION_INSTALL_REFERENCE` 指针。  
  
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
- [FUSION_INSTALL_REFERENCE 结构](fusion-install-reference-structure.md)
