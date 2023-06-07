---
title: ICLRSyncManager::GetMonitorOwner 方法
ms.date: 03/30/2017
api_name:
- ICLRSyncManager.GetMonitorOwner
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRSyncManager::GetMonitorOwner
helpviewer_keywords:
- ICLRSyncManager::GetMonitorOwner method [.NET Framework hosting]
- GetMonitorOwner method [.NET Framework hosting]
ms.assetid: 840983a4-396d-47b4-86a0-d35f9b437cdb
topic_type:
- apiref
ms.openlocfilehash: a2cb82d8071518af4d4bc3276871f3846a5a5693
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95687062"
---
# <a name="iclrsyncmanagergetmonitorowner-method"></a>ICLRSyncManager::GetMonitorOwner 方法

获取拥有由指定 cookie 标识的监视器的 [IHostTask](ihosttask-interface.md) 实例。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetMonitorOwner (  
    [in]  SIZE_T     cookie,  
    [out] IHostTask *ppOwnerHostTask  
);  
```  
  
## <a name="parameters"></a>参数  

 `cookie`  
 中与监视器关联的 cookie。  
  
 `ppOwnerHostTask`  
 弄指向 `IHostTask` 当前拥有监视器的的指针; 如果没有任务具有所有权，则为 null。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`GetMonitorOwner` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE|CLR 未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
  
## <a name="remarks"></a>注解  

 宿主通常 `GetMonitorOwner` 作为死锁检测机制的一部分来调用。 当使用 [IHostSyncManager：： CreateMonitorEvent](ihostsyncmanager-createmonitorevent-method.md)调用创建该 cookie 时，该 cookie 与监视器相关联。  
  
> [!NOTE]
> 如果调用此方法的调用当前对与该监视器关联的 cookie 有效，则调用会阻止该调用，但不会死锁。 其他任务也可能会在尝试获取此监视器时阻止。  
  
 `GetMonitorOwner` 始终立即返回，并且可以在调用后随时调用 `CreateMonitorEvent` 。 宿主无需等待，直到任务等待事件。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRSyncManager 接口](iclrsyncmanager-interface.md)
- [IHostSyncManager 接口](ihostsyncmanager-interface.md)
