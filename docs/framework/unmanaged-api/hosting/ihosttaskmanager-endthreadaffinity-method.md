---
title: IHostTaskManager::EndThreadAffinity 方法
ms.date: 03/30/2017
api_name:
- IHostTaskManager.EndThreadAffinity
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostTaskManager::EndThreadAffinity
helpviewer_keywords:
- EndThreadAffinity method [.NET Framework hosting]
- IHostTaskManager::EndThreadAffinity method [.NET Framework hosting]
ms.assetid: 7738a904-0cd7-4fde-a3eb-2323a5533157
topic_type:
- apiref
ms.openlocfilehash: c662e242cf6745223b1e87716ce4f64971347d2a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95731652"
---
# <a name="ihosttaskmanagerendthreadaffinity-method"></a>IHostTaskManager::EndThreadAffinity 方法

在之前调用 [IHostTaskManager：： BeginThreadAffinity](ihosttaskmanager-beginthreadaffinity-method.md)时，通知宿主托管代码正在退出不能将当前任务移动到另一个操作系统线程的时间段。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EndThreadAffinity ();  
```  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`EndThreadAffinity` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE| (CLR) 的公共语言运行时未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
|E_UNEXPECTED|`EndThreadAffinity` 在没有对的早期调用的情况下调用 `BeginThreadAffinity` 。|  
  
## <a name="remarks"></a>注解  

 在调用之前，CLR 对当前任务进行相应的调用 `BeginThreadAffinity` `EndThreadAffinity` 。 如果没有此类对应的调用，主机的 [IHostTaskManager](ihosttaskmanager-interface.md) 实现应返回 E_UNEXPECTED，而不执行任何操作。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Threading>
- [ICLRTask 接口](iclrtask-interface.md)
- [ICLRTaskManager 接口](iclrtaskmanager-interface.md)
- [IHostTask 接口](ihosttask-interface.md)
- [IHostTaskManager 接口](ihosttaskmanager-interface.md)
