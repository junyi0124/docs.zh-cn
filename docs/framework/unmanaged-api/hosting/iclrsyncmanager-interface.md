---
title: ICLRSyncManager 接口
ms.date: 03/30/2017
api_name:
- ICLRSyncManager
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRSyncManager
helpviewer_keywords:
- ICLRSyncManager interface [.NET Framework hosting]
ms.assetid: a49f9d80-1c76-4ddd-8c49-34f913a5c596
topic_type:
- apiref
ms.openlocfilehash: 5bfab21a36becf943b1813f266cf70c4b5e5b1d2
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95690988"
---
# <a name="iclrsyncmanager-interface"></a>ICLRSyncManager 接口

定义一些方法，这些方法允许宿主获取有关所请求任务的信息，并检测其同步实现中的死锁。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[CreateRWLockOwnerIterator 方法](iclrsyncmanager-createrwlockowneriterator-method.md)|请求公共语言运行时 (CLR) 为主机创建迭代器，以用于确定等待读取器-编写器锁的任务集。|  
|[DeleteRWLockOwnerIterator 方法](iclrsyncmanager-deleterwlockowneriterator-method.md)|请求 CLR 销毁通过调用创建的迭代器 `CreateRWLockOwnerIterator` 。|  
|[GetMonitorOwner 方法](iclrsyncmanager-getmonitorowner-method.md)|获取拥有指定监视器的任务。|  
|[GetRWLockOwnerNext 方法](iclrsyncmanager-getrwlockownernext-method.md)|获取正在等待当前读取器-编写器锁的下一个任务。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Threading.Thread>
- [IHostSyncManager 接口](ihostsyncmanager-interface.md)
- [托管和非托管线程处理](/previous-versions/dotnet/netframework-4.0/5s8ee185(v=vs.100))
- [承载接口](hosting-interfaces.md)
