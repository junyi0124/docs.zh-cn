---
title: IHostTask 接口
ms.date: 03/30/2017
api_name:
- IHostTask
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostTask
helpviewer_keywords:
- IHostTask interface [.NET Framework hosting]
ms.assetid: a71dbbd5-64b8-47eb-9f03-8e8c85fbe2bc
topic_type:
- apiref
ms.openlocfilehash: 10efe853c9a7ad7676058bc01b07063c557623d8
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95699217"
---
# <a name="ihosttask-interface"></a>IHostTask 接口

提供允许公共语言运行时 (CLR) 与宿主通信以管理任务的方法。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[Alert 方法](ihosttask-alert-method.md)|请求宿主唤醒当前实例表示的任务 `IHostTask` ，以便可以中止任务。|  
|[GetPriority 方法](ihosttask-getpriority-method.md)|获取当前实例表示的任务的线程优先级别 `IHostTask` 。|  
|[Join 方法](ihosttask-join-method.md)|阻止调用任务，直到当前实例表示的任务 `IHostTask` 完成、指定的时间间隔结束或调用 [IHostTask：： Alert](ihosttask-alert-method.md) 。|  
|[SetCLRTask 方法](ihosttask-setclrtask-method.md)|将 [ICLRTask 接口](iclrtask-interface.md) 实例与当前实例相关联 `IHostTask` 。|  
|[SetPriority 方法](ihosttask-setpriority-method.md)|请求宿主调整当前实例所表示的任务的线程优先级别 `IHostTask` 。|  
|[Start 方法](ihosttask-start-method.md)|请求宿主将当前实例表示的任务 `IHostTask` 从挂起状态移动到实时状态，在此状态下可以执行代码。|  
  
## <a name="remarks"></a>注解  

 CLR 调用定义的方法 `IHostTask` 来启动任务、设置其线程优先级别，等等。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRTask 接口](iclrtask-interface.md)
- [ICLRTaskManager 接口](iclrtaskmanager-interface.md)
- [IHostTaskManager 接口](ihosttaskmanager-interface.md)
- [承载接口](hosting-interfaces.md)
