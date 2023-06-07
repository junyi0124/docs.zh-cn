---
title: IHostGCManager 接口
ms.date: 03/30/2017
api_name:
- IHostGCManager
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostGCManager
helpviewer_keywords:
- IHostGCManager interface [.NET Framework hosting]
ms.assetid: 820330a4-244c-4f67-ab5e-f24b0b3c2080
topic_type:
- apiref
ms.openlocfilehash: eb7e52b5237d4341c27b8c167249dc2614168679
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95729514"
---
# <a name="ihostgcmanager-interface"></a>IHostGCManager 接口

提供一些方法，这些方法用来通知由公共语言运行时 (CLR) 实现的垃圾回收机制中的事件宿主。  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|[SuspensionEnding 方法](ihostgcmanager-suspensionending-method.md)|通知宿主 CLR 正在继续执行已挂起垃圾回收的线程上的任务。|  
|[SuspensionStarting 方法](ihostgcmanager-suspensionstarting-method.md)|通知宿主 CLR 正在暂停执行任务，以执行垃圾回收。|  
|[ThreadIsBlockingForSuspension 方法](ihostgcmanager-threadisblockingforsuspension-method.md)|通知宿主从中发出方法调用的线程将要阻止垃圾回收。|  
  
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
- [承载接口](hosting-interfaces.md)
