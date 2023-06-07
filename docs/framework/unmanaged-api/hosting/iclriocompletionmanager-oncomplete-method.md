---
title: ICLRIoCompletionManager::OnComplete 方法
ms.date: 03/30/2017
api_name:
- ICLRIoCompletionManager.OnComplete
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRIoCompletionManager::OnComplete
helpviewer_keywords:
- OnComplete method [.NET Framework hosting]
- ICLRIoCompletionManager::OnComplete method [.NET Framework hosting]
ms.assetid: 003f6974-9727-4322-bed5-e330d1224d0b
topic_type:
- apiref
ms.openlocfilehash: 15119974acf74b49669e5ffbee59fbff9e5c84c9
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95714089"
---
# <a name="iclriocompletionmanageroncomplete-method"></a>ICLRIoCompletionManager::OnComplete 方法

通知公共语言运行时 (CLR) 通过调用 [IHostIoCompletionManager：： Bind](ihostiocompletionmanager-bind-method.md) 方法发出的 i/o 请求的状态。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT OnComplete (  
    [in] DWORD dwErrorCode,  
    [in] DWORD NumberOfBytesTransferred,  
    [in] void* pvOverlapped  
);  
```  
  
## <a name="parameters"></a>参数  

 `dwErrorCode`  
 中一个 HRESULT 值，指示绑定操作的状态。  
  
- S_OK 指示操作已成功完成。  
  
- HOST_E_INTERRUPTED 指示调用在完成前终止。  
  
- E_FAIL 指示发生了未知、无法恢复的灾难性故障。  
  
 `NumberOfBytesTransferred`  
 中处理 i/o 请求过程中传输的字节数。  
  
 `pvOverlapped`  
 中指向 `OVERLAPPED` 传递给方法调用的结构的指针 `IHostIoCompletionManager::Bind` 。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`OnComplete` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE|CLR 未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 方法返回 E_FAIL 后，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
  
## <a name="remarks"></a>注解  

 如果主机实现 i/o 完成抽象，则 CLR 通过使用 [IHostIoCompletionManager](ihostiocompletionmanager-interface.md)的方法在主机上发出 i/o 请求。 然后，宿主调用 `OnComplete` 方法以通知运行时此类请求的结果。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRIoCompletionManager 接口](iclriocompletionmanager-interface.md)
- [IHostIoCompletionManager 接口](ihostiocompletionmanager-interface.md)
- [IHostThreadPoolManager 接口](ihostthreadpoolmanager-interface.md)
