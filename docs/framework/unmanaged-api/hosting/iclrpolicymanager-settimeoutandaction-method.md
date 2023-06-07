---
title: ICLRPolicyManager::SetTimeoutAndAction 方法
ms.date: 03/30/2017
api_name:
- ICLRPolicyManager.SetTimeoutAndAction
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRPolicyManager::SetTimeoutAndAction
helpviewer_keywords:
- ICLRPolicyManager::SetTimeoutAndAction method [.NET Framework hosting]
- SetTimeoutAndAction method [.NET Framework hosting]
ms.assetid: 60454f91-d855-4ddf-bb6d-60a02f5eabab
topic_type:
- apiref
ms.openlocfilehash: 41e13e20a1cf5a7000907b1cc7d8d2af5174ceba
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95728963"
---
# <a name="iclrpolicymanagersettimeoutandaction-method"></a>ICLRPolicyManager::SetTimeoutAndAction 方法

设置指定操作的超时值，并指定操作发生时公共语言运行时 (CLR) 应执行的策略操作。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetTimeoutAndAction (  
    [in] EClrOperation operation,  
    [in] DWORD dwMilliseconds,  
    [in] EPolicyAction action  
);  
```  
  
## <a name="parameters"></a>参数  

 `operation`  
 中 [EClrOperation](eclroperation-enumeration.md) 值之一，指示要设置超时和策略的操作 `action` 。 支持以下值：  
  
- OPR_AppDomainUnload  
  
- OPR_ProcessExit  
  
- OPR_ThreadRudeAbortInCriticalRegion  
  
- OPR_ThreadRudeAbortInNonCriticalRegion  
  
 `dwMilliseconds`  
 中新的超时值（以毫秒为单位）。 如果值为 "无限" `operation` ，则永远不会超时。  
  
 `action`  
 中 [EPolicyAction](epolicyaction-enumeration.md) 值之一，指示发生时 CLR 应执行的策略操作 `operation` 。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`SetTimeoutAndAction` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE|CLR 未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 方法返回 E_FAIL 后，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
|E_INVALIDARG|无法为指定的设置超时 `operation` ，或者为提供的值无效 `action` 。|  
  
## <a name="remarks"></a>注解  

 `SetTimeoutAndAction` 封装了 [ICLRPolicyManager：： SetTimeout](iclrpolicymanager-settimeout-method.md) 和 [ICLRPolicyManager：： SetActionOnTimeout](iclrpolicymanager-setactionontimeout-method.md) 方法的功能，可调用这些方法来代替对这两个方法的顺序调用。  
  
> [!IMPORTANT]
> 并非所有策略操作值都可以指定为 CLR 操作的超时行为。 有关有效值，请参阅主题的 "备注" 部分。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [EClrOperation 枚举](eclroperation-enumeration.md)
- [EPolicyAction 枚举](epolicyaction-enumeration.md)
- [ICLRPolicyManager 接口](iclrpolicymanager-interface.md)
- [SetActionOnTimeout 方法](iclrpolicymanager-setactionontimeout-method.md)
- [ICLRPolicyManager：： SetTimeoutAndAction](iclrpolicymanager-settimeoutandaction-method.md)
