---
title: IHostSemaphore::Wait 方法
ms.date: 03/30/2017
api_name:
- IHostSemaphore.Wait
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostSemaphore::Wait
helpviewer_keywords:
- IHostSemaphore::Wait method [.NET Framework hosting]
- Wait method, IHostSemaphore interface [.NET Framework hosting]
ms.assetid: 0da962a3-ce55-44dd-ab7a-14ad7105af4a
topic_type:
- apiref
ms.openlocfilehash: 69b2338e6992c386a3cd34a632d69b73a67f14fa
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95682999"
---
# <a name="ihostsemaphorewait-method"></a>IHostSemaphore::Wait 方法

导致当前 [IHostSemaphore](ihostsemaphore-interface.md) 实例等待，直到其拥有或经过指定的时间量。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT Wait (  
    [in] DWORD dwMilliseconds,  
    [in] DWORD option  
);  
```  
  
## <a name="parameters"></a>参数  

 `dwMilliseconds`  
 中如果当前实例不属于，则返回前等待的毫秒数 `IHostSemaphore` 。  
  
 `option`  
 中 [WAIT_OPTION](wait-option-enumeration.md) 值之一，指定主机在此操作阻止的情况下应执行的操作。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`Wait` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE| (CLR) 的公共语言运行时未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
|HOST_E_DEADLOCK|主机在等待间隔期间检测到死锁，并将当前 `IHostSemaphore` 实例选择为死锁牺牲品。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRSyncManager 接口](iclrsyncmanager-interface.md)
- [IHostAutoEvent 接口](ihostautoevent-interface.md)
- [IHostManualEvent 接口](ihostmanualevent-interface.md)
- [IHostSemaphore 接口](ihostsemaphore-interface.md)
- [IHostSyncManager 接口](ihostsyncmanager-interface.md)
