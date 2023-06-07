---
title: ICLRTask::SwitchIn 方法
ms.date: 03/30/2017
api_name:
- ICLRTask.SwitchIn
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRTask::SwitchIn
helpviewer_keywords:
- ICLRTask::SwitchIn method [.NET Framework hosting]
- SwitchIn method [.NET Framework hosting]
ms.assetid: 3d37ce20-aa65-4043-8f13-7c728b5d8a52
topic_type:
- apiref
ms.openlocfilehash: e98ae17d55c74d32844da96137c258d076ebc2db
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95691048"
---
# <a name="iclrtaskswitchin-method"></a>ICLRTask::SwitchIn 方法

通知公共语言运行时 (CLR) 当前 [ICLRTask](iclrtask-interface.md) 实例表示的任务现在处于可运行状态。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SwitchIn (  
    [in] HANDLE threadHandle  
);  
```  
  
## <a name="parameters"></a>参数  

 `threadHandle`  
 中当前实例所表示的任务正在执行的物理线程的句柄 `ICLRTask` 。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`SwitchIn` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE|CLR 未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
|HOST_E_INVALIDOPERATION|`SwitchIn` 调用 [SwitchOut 方法](iclrtask-switchout-method.md)之前调用了。|  
  
## <a name="remarks"></a>注解  

 `threadHandle`参数表示操作系统线程的句柄，该句柄由当前实例表示的任务已 `ICLRTask` 计划。 如果此线程上发生模拟，则必须先调用 [IHostSecurityManager：： RevertToSelf](ihostsecuritymanager-reverttoself-method.md) ，然后才能在该任务中切换。  
  
> [!NOTE]
> 对不使用之前调用的的调用将 `SwitchIn` `SwitchOut` 失败，HRESULT 值为 HOST_E_INVALIDOPERATION。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRTask 接口](iclrtask-interface.md)
- [ICLRTaskManager 接口](iclrtaskmanager-interface.md)
- [IHostTask 接口](ihosttask-interface.md)
- [IHostTaskManager 接口](ihosttaskmanager-interface.md)
