---
title: IHostTaskManager::CreateTask 方法
ms.date: 03/30/2017
api_name:
- IHostTaskManager.CreateTask
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostTaskManager::CreateTask
helpviewer_keywords:
- CreateTask method, IHostTaskManager interface [.NET Framework hosting]
- IHostTaskManager::CreateTask method [.NET Framework hosting]
ms.assetid: a6f8ad36-61e1-42b0-9db2-add575646d18
topic_type:
- apiref
ms.openlocfilehash: 7fdf25d44bdf630e306cf0f5dcb3387a3b0f7c76
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95731678"
---
# <a name="ihosttaskmanagercreatetask-method"></a>IHostTaskManager::CreateTask 方法

请求宿主创建新任务。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CreateTask (  
    [in]  DWORD stacksize,
    [in]  LPTHREAD_START_ROUTINE pStartAddress,  
    [in]  PVOID pParameter,  
    [out] IHostTask **ppTask  
);  
```  
  
## <a name="parameters"></a>参数  

 `stacksize`  
 中请求的堆栈的请求大小（以字节为单位），或默认大小为 0 (零) 。  
  
 `pStartAddress`  
 中指向任务要执行的函数的指针。  
  
 `pParameter`  
 中指向要传递到函数的用户数据的指针; 如果函数不采用任何参数，则为 null。  
  
 `ppTask`  
 弄指向主机创建的 [IHostTask](ihosttask-interface.md) 实例的地址的指针; 如果无法创建任务，则为 null。 在通过调用 [IHostTask：： Start](ihosttask-start-method.md)显式启动任务之前，任务将保持挂起状态。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`CreateTask` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE| (CLR) 的公共语言运行时未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
|E_OUTOFMEMORY|没有足够的内存可用于创建请求的任务。|  
  
## <a name="remarks"></a>注解  

 CLR 调用 `CreateTask` 来请求宿主创建新任务。 主机返回指向实例的接口指针 `IHostTask` 。 返回的任务必须保持挂起状态，直到通过调用显式启动它 `IHostTask::Start` 。  
  
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
