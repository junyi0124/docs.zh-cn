---
title: IHostPolicyManager::OnFailure 方法
ms.date: 03/30/2017
api_name:
- IHostPolicyManager.OnFailure
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostPolicyManager::OnFailure
helpviewer_keywords:
- OnFailure method [.NET Framework hosting]
- IHostPolicyManager::OnFailure method [.NET Framework hosting]
ms.assetid: 77d3f31e-9a53-4349-9c02-610a71736d42
topic_type:
- apiref
ms.openlocfilehash: efa7b9d49ea9807af2164bb6ee54422dd72b14e2
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95730417"
---
# <a name="ihostpolicymanageronfailure-method"></a>IHostPolicyManager::OnFailure 方法

向宿主通知公共语言运行时 (CLR) 将要使用 [ICLRPolicyManager：： SetActionOnFailure](iclrpolicymanager-setactiononfailure-method.md) 方法调用指定的操作，以响应资源分配或回收失败。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT OnFailure(  
    [in] EClrFailure   failure,  
    [in] EPolicyAction action  
);  
```  
  
## <a name="parameters"></a>参数  

 `failure`  
 中 [EClrFailure](eclrfailure-enumeration.md) 值之一，指示 CLR 响应的失败类型。  
  
 `action`  
 中 [EPolicyAction](epolicyaction-enumeration.md) 值之一，指示 CLR 响应的操作 `failure` 。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`OnFailure` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE|CLR 未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [EClrFailure 枚举](eclrfailure-enumeration.md)
- [EPolicyAction 枚举](epolicyaction-enumeration.md)
- [ICLRPolicyManager 接口](iclrpolicymanager-interface.md)
- [IHostPolicyManager 接口](ihostpolicymanager-interface.md)
