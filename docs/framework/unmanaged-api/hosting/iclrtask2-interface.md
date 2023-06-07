---
title: ICLRTask2 接口
ms.date: 03/30/2017
api_name:
- ICLRTask2
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRTask2
helpviewer_keywords:
- ICLRTask2 interface [.NET Framework hosting]
ms.assetid: b5a22ebc-0582-49de-91f9-97a3d9789290
topic_type:
- apiref
ms.openlocfilehash: 9332b3462ba389783a113d173e32850d40427ce2
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720225"
---
# <a name="iclrtask2-interface"></a>ICLRTask2 接口

提供 [ICLRTask](iclrtask-interface.md) 接口的所有功能;此外，还提供了允许线程中止在当前线程上延迟的方法。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[BeginPreventAsyncAbort 方法](iclrtask2-beginpreventasyncabort-method.md)|延迟当前线程上的新线程中止请求。|  
|[EndPreventAsyncAbort 方法](iclrtask2-endpreventasyncabort-method.md)|允许新的或挂起的线程中止请求导致在当前线程上中止线程。|  
  
## <a name="remarks"></a>注解  

 `ICLRTask2`接口继承 `ICLRTask` 接口并添加允许主机延迟线程中止的方法，以保护不能失败的代码区域。 调用 `BeginPreventAsyncAbort` 会递增当前线程的延迟线程中止计数器，并调用 `EndPreventAsyncAbort` 减量。 对和的调用 `BeginPreventAsyncAbort` `EndPreventAsyncAbort` 可以嵌套。 只要计数器大于零，就会延迟当前线程的线程中止。  
  
 如果对 `BeginPreventAsyncAbort` 和 `EndPreventAsyncAbort` 的调用不成对，则可能会达到无法将线程中止传递到当前线程的状态。  
  
 此延迟不适用于自行中止的线程。  
  
 此功能公开的功能由虚拟机在内部使用 (VM) 。 滥用这些方法可能会导致 VM 中未指定的行为。 例如， `EndPreventAsyncAbort` 如果未通过第一次调用调用，则在 `BeginPreventAsyncAbort` VM 之前将计数器设置为零。 同样，不检查内部计数器是否溢出。 如果它超过其整数限制，原因是主机和 VM 均递增，则未指定产生的行为。  
  
 有关从和继承此接口的其他用法的成员的信息 `ICLRTask` ，请参阅 [ICLRTask](iclrtask-interface.md) 接口。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRTask 接口](iclrtask-interface.md)
- [ICLRTaskManager 接口](iclrtaskmanager-interface.md)
- [IHostTask 接口](ihosttask-interface.md)
- [IHostTaskManager 接口](ihosttaskmanager-interface.md)
- [承载接口](hosting-interfaces.md)
