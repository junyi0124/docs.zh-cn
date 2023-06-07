---
title: IHostThreadPoolManager::GetMaxThreads 方法
ms.date: 03/30/2017
api_name:
- IHostThreadPoolManager.GetMaxThreads
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostThreadPoolManager::GetMaxThreads
helpviewer_keywords:
- IHostThreadPoolManager::GetMaxThreads method [.NET Framework hosting]
- GetMaxThreads method, IHostThreadPoolManager interface [.NET Framework hosting]
ms.assetid: db268876-6178-4a81-aca3-318ee7f96001
topic_type:
- apiref
ms.openlocfilehash: 3aecebe2803d3a795db801491d0f60a5eb7c00ce
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95730781"
---
# <a name="ihostthreadpoolmanagergetmaxthreads-method"></a>IHostThreadPoolManager::GetMaxThreads 方法

获取主机在线程池中并发维护的最大线程数。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetMaxThreads (  
    [out] DWORD *pdwMaxWorkerThreads  
);  
```  
  
## <a name="parameters"></a>参数  

 `pdwMaxWorkerThreads`  
 弄一个指针，它指向主机在线程池中维护的最大线程数。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`GetMaxThreads` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE| (CLR ( 的公共语言运行时未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
|E_NOTIMPL|宿主不提供的实现 `GetMaxThreads` 。|  
  
## <a name="remarks"></a>注解  

 CLR 调用 `GetMaxThreads` 来确定线程池中线程的总数。 [GetAvailableThreads](ihostthreadpoolmanager-getavailablethreads-method.md)方法获取当前未处理工作项的线程数。 所有超出参数返回值的请求将 `pdwMaxWorkerThreads` 保持排队状态，直到线程变为可用。  
  
 如果主机未提供的实现 `GetMaxThreads` ，它应返回 E_NOTIMPL 的 HRESULT 值。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Threading.ThreadPool.GetMaxThreads%2A>
- <xref:System.Threading.ThreadPool>
- [GetMinThreads 方法](ihostthreadpoolmanager-getminthreads-method.md)
- [SetMaxThreads 方法](ihostthreadpoolmanager-setmaxthreads-method.md)
- [IHostThreadPoolManager 接口](ihostthreadpoolmanager-interface.md)
