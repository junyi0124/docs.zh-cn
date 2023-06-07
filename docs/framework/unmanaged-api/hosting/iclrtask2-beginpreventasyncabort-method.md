---
title: ICLRTask2::BeginPreventAsyncAbort 方法
ms.date: 03/30/2017
api_name:
- ICLRTask2.BeginPreventAsyncAbort
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRTask2::BeginPreventAsyncAbort
helpviewer_keywords:
- ICLRTask2::BeginPreventAsyncAbort method [.NET Framework hosting]
- BeginPreventAsyncAbort method [.NET Framework hosting]
ms.assetid: 75754c2f-38c7-4707-85fe-559db4542729
topic_type:
- apiref
ms.openlocfilehash: daf211fcc496f63ef71575abf6a28655004db264
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720238"
---
# <a name="iclrtask2beginpreventasyncabort-method"></a>ICLRTask2::BeginPreventAsyncAbort 方法

使新线程中止请求延迟，导致线程在当前线程上中止。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT BeginPreventAsyncAbort();  
```  
  
## <a name="return-value"></a>返回值  

 此方法返回以下特定 HRESULT 以及表示方法失败的 HRESULT 错误。  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|该方法已成功完成。|  
|HOST_E_INVALIDOPERATION|在不是当前线程的线程上调用了方法。|  
  
## <a name="remarks"></a>注解  

 调用此方法会将当前线程的延迟线程中止计数器增加一个。  
  
 调用 `BeginPreventAsyncAbort` 和 [ICLRTask2：： EndPreventAsyncAbort](iclrtask2-endpreventasyncabort-method.md) 可以嵌套。 只要计数器大于零，就会延迟当前线程的线程中止。 如果此调用不与方法的调用配对，则 `EndPreventAsyncAbort` 可能会达到无法将线程中止传递到当前线程的状态。  
  
 此延迟不适用于自行中止的线程。  
  
 此功能公开的功能由虚拟机在内部使用 (VM) 。 滥用这些方法可能会导致 VM 中未指定的行为。 例如， `EndPreventAsyncAbort` 如果未通过第一次调用调用，则在 `BeginPreventAsyncAbort` VM 之前将计数器设置为零。 同样，不检查内部计数器是否溢出。 如果它超过其整数限制，原因是主机和 VM 均递增，则未指定产生的行为。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [EndPreventAsyncAbort 方法](iclrtask2-endpreventasyncabort-method.md)
- [ICLRTask2 接口](iclrtask2-interface.md)
- [ICLRTaskManager 接口](iclrtaskmanager-interface.md)
- [IHostTask 接口](ihosttask-interface.md)
- [IHostTaskManager 接口](ihosttaskmanager-interface.md)
- [承载接口](hosting-interfaces.md)
