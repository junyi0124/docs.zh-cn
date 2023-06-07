---
title: IHostManualEvent 接口
ms.date: 03/30/2017
api_name:
- IHostManualEvent
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostManualEvent
helpviewer_keywords:
- IHostManualEvent interface [.NET Framework hosting]
ms.assetid: 300c2661-b7d1-4c39-b080-9ebdef0fd523
topic_type:
- apiref
ms.openlocfilehash: 50e37b770e3ee6e0a5cdfca61fc5b09749e5735f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95673176"
---
# <a name="ihostmanualevent-interface"></a>IHostManualEvent 接口

提供宿主手动重置事件表示形式的实现。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[Reset 方法](ihostmanualevent-reset-method.md)|将当前实例重置 `IHostManualEvent` 为非终止状态。|  
|[Set 方法](ihostmanualevent-set-method.md)|将当前 `IHostManualEvent` 实例设置为终止状态。|  
|[Wait 方法](ihostmanualevent-wait-method.md)|导致当前 `IHostManualEvent` 实例等待，直到其拥有或经过指定的时间量。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRSyncManager 接口](iclrsyncmanager-interface.md)
- [IHostAutoEvent 接口](ihostautoevent-interface.md)
- [IHostSemaphore 接口](ihostsemaphore-interface.md)
- [IHostSyncManager 接口](ihostsyncmanager-interface.md)
- [承载接口](hosting-interfaces.md)
