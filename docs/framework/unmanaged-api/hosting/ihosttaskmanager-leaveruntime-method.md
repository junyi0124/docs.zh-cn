---
title: IHostTaskManager::LeaveRuntime 方法
ms.date: 03/30/2017
api_name:
- IHostTaskManager.LeaveRuntime
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostTaskManager::LeaveRuntime
helpviewer_keywords:
- IHostTaskManager::LeaveRuntime method [.NET Framework hosting]
- LeaveRuntime method [.NET Framework hosting]
ms.assetid: 43689cc4-e48e-46e5-a22d-bafd768b8759
topic_type:
- apiref
ms.openlocfilehash: 855f8a5d3582bbad59301a344d8a51198c40a051
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95673041"
---
# <a name="ihosttaskmanagerleaveruntime-method"></a>IHostTaskManager::LeaveRuntime 方法

通知宿主当前正在执行的任务即将使公共语言运行时 (CLR) 并输入非托管代码。  
  
> [!IMPORTANT]
> 对应的对 [IHostTaskManager：： EnterRuntime](ihosttaskmanager-enterruntime-method.md) 的调用会通知宿主当前正在执行的任务是重新进入托管代码。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT LeaveRuntime (  
    [in] SIZE_T target  
);  
```  
  
## <a name="parameters"></a>参数  

 `target`  
 中要调用的非托管函数的映射可移植可执行文件中的地址。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`LeaveRuntime` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE|CLR 未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
|E_OUTOFMEMORY|没有足够的内存可用来完成请求的分配。|  
  
## <a name="remarks"></a>注解  

 与非托管代码之间的调用序列可以嵌套。 例如，下面的列表描述了一种假设的情况，在这种情况下 `LeaveRuntime` ，对、 [IHostTaskManager：： ReverseEnterRuntime](ihosttaskmanager-reverseenterruntime-method.md)、 [IHostTaskManager：： ReverseLeaveRuntime](ihosttaskmanager-reverseleaveruntime-method.md)的调用序列，并 `IHostTaskManager::EnterRuntime` 允许主机标识嵌套层。  
  
|操作|对应的方法调用|  
|------------|-------------------------------|  
|托管 Visual Basic 可执行文件使用平台调用来调用以 C 编写的非托管函数。|`IHostTaskManager::LeaveRuntime`|  
|非托管 C 函数在用 c # 编写的托管 DLL 中调用方法。|`IHostTaskManager::ReverseEnterRuntime`|  
|托管 c # 函数调用另一个用 C 编写的非托管函数，同时也使用平台调用。|`IHostTaskManager::LeaveRuntime`|  
|第二个非托管函数向 c # 函数返回执行。|`IHostTaskManager::EnterRuntime`|  
|C # 函数返回第一个非托管函数的执行。|`IHostTaskManager::ReverseLeaveRuntime`|  
|第一个非托管函数会将执行返回到 Visual Basic 程序。|`IHostTaskManager::EnterRuntime`|  
  
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
