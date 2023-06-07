---
title: IHostTaskManager::ReverseLeaveRuntime 方法
ms.date: 03/30/2017
api_name:
- IHostTaskManager.ReverseLeaveRuntime
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostTaskManager::ReverseLeaveRuntime
helpviewer_keywords:
- IHostTaskManager::ReverseLeaveRuntime method [.NET Framework hosting]
- ReverseLeaveRuntime method [.NET Framework hosting]
ms.assetid: 4837d398-16a1-4e32-902c-022cd1aad3ca
topic_type:
- apiref
ms.openlocfilehash: 8e0981415c03120cc30e6349daced51e79216938
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95669960"
---
# <a name="ihosttaskmanagerreverseleaveruntime-method"></a>IHostTaskManager::ReverseLeaveRuntime 方法

向宿主通知控制是否正在离开公共语言运行时 (CLR) 并输入从托管代码调用的非托管函数。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT ReverseLeaveRuntime ();  
```  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`ReverseLeaveRuntime` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE|CLR 未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
|E_OUTOFMEMORY|没有足够的内存可用来完成请求的资源分配。|  
  
## <a name="remarks"></a>注解  

 CLR 调用 `ReverseLeaveRuntime` 以通知宿主当前正在执行的任务正在将控制权返回给非托管函数，而该函数又通过平台调用从托管代码调用。 对的每个调用都 `ReverseLeaveRuntime` 匹配对 [ReverseEnterRuntime](ihosttaskmanager-reverseenterruntime-method.md)的相应调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [CallNeedsHostHook 方法](ihosttaskmanager-callneedshosthook-method.md)
- [EnterRuntime 方法](ihosttaskmanager-enterruntime-method.md)
- [ICLRTask 接口](iclrtask-interface.md)
- [ICLRTaskManager 接口](iclrtaskmanager-interface.md)
- [IHostTask 接口](ihosttask-interface.md)
- [IHostTaskManager 接口](ihosttaskmanager-interface.md)
- [LeaveRuntime 方法](ihosttaskmanager-leaveruntime-method.md)
- [平台调用详解](/previous-versions/dotnet/netframework-4.0/0h9e9t7d(v=vs.100))
