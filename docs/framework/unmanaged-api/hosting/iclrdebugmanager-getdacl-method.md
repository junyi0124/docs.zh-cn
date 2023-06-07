---
title: ICLRDebugManager::GetDacl 方法
ms.date: 03/30/2017
api_name:
- ICLRDebugManager.GetDacl
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRDebugManager::GetDacl
helpviewer_keywords:
- GetDacl method [.NET Framework hosting]
- ICLRDebugManager::GetDacl method [.NET Framework hosting]
ms.assetid: 7115e920-aaff-440a-824e-39497139c6f6
topic_type:
- apiref
ms.openlocfilehash: 8a7de5a900bc1af219924b6a83f83cf7e2ef6150
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95726140"
---
# <a name="iclrdebugmanagergetdacl-method"></a>ICLRDebugManager::GetDacl 方法

未实现此方法。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetDacl (  
    [out] PACL* ppacl  
);  
```  
  
## <a name="parameters"></a>参数  

 `ppacl`  
 弄指向访问控制列表 (ACL) 的接口指针。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|E_NOTIMPL|该方法未实现。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRControl 接口](iclrcontrol-interface.md)
- [ICLRDebugManager 接口](iclrdebugmanager-interface.md)
- [SetDacl 方法](iclrdebugmanager-setdacl-method.md)
- [IHostControl 接口](ihostcontrol-interface.md)
