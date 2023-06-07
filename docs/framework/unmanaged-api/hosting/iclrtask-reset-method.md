---
title: ICLRTask::Reset 方法
ms.date: 03/30/2017
api_name:
- ICLRTask.Reset
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRTask::Reset
helpviewer_keywords:
- ICLRTask::Reset method [.NET Framework hosting]
- Reset method, ICLRTask interface [.NET Framework hosting]
ms.assetid: 1bfb5d3a-0ffd-4bb4-9bf6-aec00cb675b7
topic_type:
- apiref
ms.openlocfilehash: b87bc026a2cac2d0b913128c43142d56aee03025
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95725191"
---
# <a name="iclrtaskreset-method"></a>ICLRTask::Reset 方法

通知公共语言运行时 (CLR) 宿主已经完成了一个任务，并使 CLR 能够重复使用当前 [ICLRTask](iclrtask-interface.md) 实例来表示其他任务。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT Reset (  
    [in] BOOL fFull  
);  
```  
  
## <a name="parameters"></a>参数  

 `fFull`  
 [in] `true` 如果除了与当前实例相关的安全性和区域设置信息，运行时还应重置与线程相关的所有静态值， `ICLRTask` 则为; 否则为 `false` 。  
  
 如果该值为 `true` ，则运行时将重置使用或存储的数据 <xref:System.Threading.Thread.AllocateDataSlot%2A> <xref:System.Threading.Thread.AllocateNamedDataSlot%2A> 。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`Reset` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE|CLR 未加载到进程中，或 CLR 处于无法运行托管代码或处理调用的状态。 顺利|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
  
## <a name="remarks"></a>注解  

 CLR 可以回收以前创建的 `ICLRTask` 实例，以避免在每次需要全新任务时重复创建新实例的系统开销。 主机完成任务后，将通过调用 `ICLRTask::Reset` 而不是 [ICLRTask：： ExitTask](iclrtask-exittask-method.md) 来启用此功能。 下面的列表总结了实例的正常生命周期 `ICLRTask` ：  
  
1. 运行时创建一个新的 `ICLRTask` 实例。  
  
2. 运行时调用 [IHostTaskManager：： GetCurrentTask](ihosttaskmanager-getcurrenttask-method.md) 以获取对当前宿主任务的引用。  
  
3. 运行时调用 [IHostTask：： SetCLRTask](ihosttask-setclrtask-method.md) 以将新实例与宿主任务相关联。  
  
4. 任务执行并完成。  
  
5. 宿主通过调用销毁任务 `ICLRTask::ExitTask` 。  
  
 `Reset` 通过两种方式改变此方案。 在上面的步骤5中，主机将调用 `Reset` 以将任务重置为干净状态，然后将该 `ICLRTask` 实例从其关联的 [IHostTask](ihosttask-interface.md) 实例中分离。 如果需要，主机还可以缓存 `IHostTask` 实例以供重复使用。 在上面的步骤1中，运行时 `ICLRTask` 从缓存中提取回收，而不是创建新的实例。  
  
 当主机还具有可重复使用的辅助角色任务池时，此方法非常有效。 当主机销毁其某个实例时 `IHostTask` ，它会 `ICLRTask` 通过调用销毁相应的 `ExitTask` 。  
  
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
