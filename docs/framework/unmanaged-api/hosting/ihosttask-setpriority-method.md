---
title: IHostTask::SetPriority 方法
ms.date: 03/30/2017
api_name:
- IHostTask.SetPriority
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostTask::SetPriority
helpviewer_keywords:
- IHostTask::SetPriority method [.NET Framework hosting]
- SetPriority method [.NET Framework hosting]
ms.assetid: cd8c379b-c7a0-434f-8e23-899bd26be75d
topic_type:
- apiref
ms.openlocfilehash: 80b4bb2f6a547250acbc16a89e7396c60cc50d87
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720446"
---
# <a name="ihosttasksetpriority-method"></a>IHostTask::SetPriority 方法

请求宿主调整当前 [IHostTask](ihosttask-interface.md) 实例所表示的任务的线程优先级别。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetPriority (  
    [in] int newPriority  
);  
```  
  
## <a name="parameters"></a>参数  

 `newPriority`  
 中一个整数，表示当前实例表示的任务所请求的线程优先级值 `IHostTask` 。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`SetPriority` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE| (CLR) 的公共语言运行时未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
  
## <a name="remarks"></a>注解  

 将使用部分基于线程优先级别的轮循机制为线程授予处理时间。 `SetPriority` 允许 CLR 为当前任务设置该线程优先级别。 支持以下 `newPriority` 值。  
  
- THREAD_PRIORITY_ABOVE_NORMAL  
  
- THREAD_PRIORITY_BELOW_NORMAL  
  
- THREAD_PRIORITY_HIGHEST  
  
- THREAD_PRIORITY_IDLE  
  
- THREAD_PRIORITY_LOWEST  
  
- THREAD_PRIORITY_NORMAL  
  
- THREAD_PRIORITY_TIME_CRITICAL  
  
 `SetPriority`当用户代码修改了的值时，CLR 将调用 <xref:System.Threading.Thread.Priority%2A?displayProperty=nameWithType> 。 宿主可以定义自己的线程优先级分配算法，并可以随意忽略此请求。  
  
> [!NOTE]
> `SetPriority` 不会报告线程优先级别是否已更改。 调用 [IHostTask：： GetPriority](ihosttask-getpriority-method.md) 以确定任务的线程优先级别的值。  
  
 线程优先级别值由 Win32 `SetThreadPriority` 函数定义。 有关线程优先级的详细信息，请参阅 Windows 平台文档。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Threading.Thread>
- [ICLRTask 接口](iclrtask-interface.md)
- [ICLRTaskManager 接口](iclrtaskmanager-interface.md)
- [IHostTask 接口](ihosttask-interface.md)
- [GetPriority 方法](ihosttask-getpriority-method.md)
- [IHostTaskManager 接口](ihosttaskmanager-interface.md)
