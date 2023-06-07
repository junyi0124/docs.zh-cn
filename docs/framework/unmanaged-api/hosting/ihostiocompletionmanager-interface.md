---
title: IHostIoCompletionManager 接口
ms.date: 03/30/2017
api_name:
- IHostIoCompletionManager
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostIoCompletionManager
helpviewer_keywords:
- IHostIoCompletionManager interface [.NET Framework hosting]
ms.assetid: c28d1983-83f7-46e2-990f-dbb9dc07c818
topic_type:
- apiref
ms.openlocfilehash: 75ad8670008242008aa344835143ff9b2add0a6c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95719588"
---
# <a name="ihostiocompletionmanager-interface"></a>IHostIoCompletionManager 接口

提供一些方法，这些方法允许公共语言运行时 (CLR) 与主机提供的 i/o 完成端口交互。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[Bind 方法](ihostiocompletionmanager-bind-method.md)|将句柄绑定到 i/o 完成端口。|  
|[CloseIoCompletionPort 方法](ihostiocompletionmanager-closeiocompletionport-method.md)|关闭通过先前对的调用创建的端口 `CreateIoCompletionPort` 。|  
|[CreateIoCompletionPort 方法](ihostiocompletionmanager-createiocompletionport-method.md)|请求宿主创建新的 i/o 完成端口。|  
|[GetAvailableThreads 方法](ihostiocompletionmanager-getavailablethreads-method.md)|获取当前未处理请求的 i/o 完成线程数。|  
|[GetHostOverlappedSize 方法](ihostiocompletionmanager-gethostoverlappedsize-method.md)|获取宿主打算追加到 i/o 请求的任何自定义数据的大小。|  
|[GetMaxThreads 方法](ihostiocompletionmanager-getmaxthreads-method.md)|获取主机可为服务 i/o 请求分配的最大线程数。|  
|[GetMinThreads 方法](ihostiocompletionmanager-getminthreads-method.md)|获取主机提供的用于处理 i/o 请求的最小线程数。|  
|[InitializeHostOverlapped 方法](ihostiocompletionmanager-initializehostoverlapped-method.md)|向宿主提供初始化有关 i/o 请求的任何自定义数据的机会。|  
|[SetCLRIoCompletionManager 方法](ihostiocompletionmanager-setclriocompletionmanager-method.md)|向宿主提供一个接口指针，该指针指向 CLR 实现的 [ICLRIoCompletionManager](iclriocompletionmanager-interface.md) 实例。|  
|[SetMaxThreads 方法](ihostiocompletionmanager-setmaxthreads-method.md)|设置主机 allots i/o 请求的最大线程数。|  
|[SetMinThreads 方法](ihostiocompletionmanager-setminthreads-method.md)|设置主机应为 i/o 完成分配的最小线程数。|  
  
## <a name="remarks"></a>注解  

 `IHostIoCompletionManager` 对应于 `ICLRIoCompletionManager` CLR 实现的接口。 CLR 调用的方法将 `IHostIoCompletionManager` 句柄绑定到主机提供的端口，并且宿主将调用的方法 `ICLRIoCompletionManager` 来报告 i/o 请求的完成。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [承载接口](hosting-interfaces.md)
