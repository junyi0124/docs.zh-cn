---
title: IHostSyncManager 接口
ms.date: 03/30/2017
api_name:
- IHostSyncManager
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostSyncManager
helpviewer_keywords:
- IHostSyncManager interface [.NET Framework hosting]
ms.assetid: 2e081a37-6a28-4c93-b7ab-1c96a464637c
topic_type:
- apiref
ms.openlocfilehash: 8a5fc42191634a2e5a441baecc4b78212ffad687
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720485"
---
# <a name="ihostsyncmanager-interface"></a>IHostSyncManager 接口

提供一些方法，使公共语言运行时 (CLR) 通过调用主机而不是使用 Win32 同步函数来创建同步基元。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[CreateAutoEvent 方法](ihostsyncmanager-createautoevent-method.md)|创建自动重置事件对象。|  
|[CreateCrst 方法](ihostsyncmanager-createcrst-method.md)|创建用于同步的临界区对象。|  
|[CreateCrstWithSpinCount 方法](ihostsyncmanager-createcrstwithspincount-method.md)|使用自旋计数为同步创建一个临界区对象。|  
|[CreateManualEvent 方法](ihostsyncmanager-createmanualevent-method.md)|创建手动重置的事件对象。|  
|[CreateMonitorEvent 方法](ihostsyncmanager-createmonitorevent-method.md)|创建监视的自动重置事件对象。|  
|[CreateRWLockReaderEvent 方法](ihostsyncmanager-createrwlockreaderevent-method.md)|创建手动重置的事件对象，以便实现读取器锁。|  
|[CreateRWLockWriterEvent 方法](ihostsyncmanager-createrwlockwriterevent-method.md)|创建自动重置事件对象，以便实现编写器锁。|  
|[CreateSemaphore 方法](ihostsyncmanager-createsemaphore-method.md)|为 CLR 创建一个 [IHostSemaphore](ihostsemaphore-interface.md) 对象，以用作等待事件的信号量。|  
|[SetCLRSyncManager 方法](ihostsyncmanager-setclrsyncmanager-method.md)|设置要与当前实例关联的 [ICLRSyncManager](iclrsyncmanager-interface.md) 实例 `IHostSyncManager` 。|  
  
## <a name="remarks"></a>注解  

 CLR `IHostSyncManager` 通过使用 IID_IHostSyncManager 的调用 [IHostControl：： GetHostManager](ihostcontrol-gethostmanager-method.md) 方法来发现主机的实现 `IID` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRSyncManager 接口](iclrsyncmanager-interface.md)
- [承载接口](hosting-interfaces.md)
