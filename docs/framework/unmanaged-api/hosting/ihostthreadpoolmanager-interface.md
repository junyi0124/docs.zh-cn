---
title: IHostThreadPoolManager 接口
ms.date: 03/30/2017
api_name:
- IHostThreadPoolManager
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostThreadPoolManager
helpviewer_keywords:
- IHostThreadPoolManager interface [.NET Framework hosting]
ms.assetid: c3a2cd90-7c4e-4374-bb87-b41befb8344f
topic_type:
- apiref
ms.openlocfilehash: b6625b0ef4dc3de4067514a0b39849c7a958d5c4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95730755"
---
# <a name="ihostthreadpoolmanager-interface"></a>IHostThreadPoolManager 接口

提供一些方法，这些方法使公共语言运行时 (CLR) 配置线程池并将工作项排队到线程池。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetAvailableThreads 方法](ihostthreadpoolmanager-getavailablethreads-method.md)|获取线程池中当前未处理工作项的线程的数目。|  
|[GetMaxThreads 方法](ihostthreadpoolmanager-getmaxthreads-method.md)|获取主机在线程池中并发维护的最大线程数。|  
|[GetMinThreads 方法](ihostthreadpoolmanager-getminthreads-method.md)|获取主机为获得请求而保留的空闲线程的最小数目。|  
|[QueueUserWorkItem 方法](ihostthreadpoolmanager-queueuserworkitem-method.md)|将函数排队以便执行，并提供包含函数要使用的数据的对象。|  
|[SetMaxThreads 方法](ihostthreadpoolmanager-setmaxthreads-method.md)|设置宿主可在线程池中维护的最大线程数。|  
|[SetMinThreads 方法](ihostthreadpoolmanager-setminthreads-method.md)|设置主机在预期请求中必须保持的空闲线程的最小数目。|  
  
## <a name="remarks"></a>注解  

 主机不需要使用对和方法的调用中指定的值来配置线程池 `SetMaxThreads` `SetMinThreads` 。 在这种情况下，宿主应从这些方法返回 E_NOTIMPL 的 HRESULT 值。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Threading>
- <xref:System.Threading.ThreadPool>
- [承载接口](hosting-interfaces.md)
