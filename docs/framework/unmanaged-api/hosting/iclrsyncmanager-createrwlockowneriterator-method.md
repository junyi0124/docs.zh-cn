---
title: ICLRSyncManager::CreateRWLockOwnerIterator 方法
ms.date: 03/30/2017
api_name:
- ICLRSyncManager.CreateRWLockOwnerIterator
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRSyncManager::CreateRWLockOwnerIterator
helpviewer_keywords:
- ICLRSyncManager::CreateRWLockOwnerIterator method [.NET Framework hosting]
- CreateRWLockOwnerIterator method [.NET Framework hosting]
ms.assetid: b5535b87-9439-424e-b9b3-7d6fafb9819e
topic_type:
- apiref
ms.openlocfilehash: 2b6a2082d27fca4c78dcb15a13cfd87e8066e388
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95687211"
---
# <a name="iclrsyncmanagercreaterwlockowneriterator-method"></a>ICLRSyncManager::CreateRWLockOwnerIterator 方法

请求公共语言运行时 (CLR) 为主机创建迭代器，以用于确定等待读取器-编写器锁的任务集。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CreateRWLockOwnerIterator (  
    [in]  SIZE_T    cookie,  
    [out] SIZE_T   *pIterator  
);  
```  
  
## <a name="parameters"></a>参数  

 `cookie`  
 中与所需的读取器锁关联的 cookie。  
  
 `pIterator`  
 弄指向可传递给 [GetRWLockOwnerNext](iclrsyncmanager-getrwlockownernext-method.md) 和 [DeleteRWLockOwnerIterator](iclrsyncmanager-deleterwlockowneriterator-method.md) 方法的迭代器的指针。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`CreateRWLockOwnerIterator` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE|CLR 未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
|HOST_E_INVALIDOPERATION|`CreateRWLockOwnerIterator` 在当前正在运行托管代码的线程上调用了。|  
  
## <a name="remarks"></a>注解  

 `CreateRWLockOwnerIterator` `DeleteRWLockOwnerIterator` 在死锁检测期间，主机通常会调用、和 `GetRWLockOwnerNext` 方法。 宿主负责确保读取器-编写器锁仍有效，因为 CLR 不会尝试使读取器-编写器锁保持活动状态。 主机可以使用多种策略来确保锁的有效性：  
  
- 宿主可以在读取器-编写器锁 (（例如 [IHostSemaphore：： ReleaseSemaphore](ihostsemaphore-releasesemaphore-method.md)) ）上阻止释放调用，同时确保此块不会导致死锁。  
  
- 宿主可以阻止退出等待与读取器-编写器锁关联的事件对象，并再次确保此块不会导致死锁。  
  
> [!NOTE]
> `CreateRWLockOwnerIterator` 只能在当前正在执行非托管代码的线程上调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRSyncManager 接口](iclrsyncmanager-interface.md)
- [IHostSyncManager 接口](ihostsyncmanager-interface.md)
