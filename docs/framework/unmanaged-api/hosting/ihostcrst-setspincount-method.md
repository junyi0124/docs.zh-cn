---
title: IHostCrst::SetSpinCount 方法
ms.date: 03/30/2017
api_name:
- IHostCrst.SetSpinCount
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostCrst::SetSpinCount
helpviewer_keywords:
- IHostCrst::SetSpinCount method [.NET Framework hosting]
- SetSpinCount method [.NET Framework hosting]
ms.assetid: 863fc8ce-9b8a-477e-8dd8-75c8544bb43a
topic_type:
- apiref
ms.openlocfilehash: 22274759f931da614a234efe0a6f6eb3aade027c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95729559"
---
# <a name="ihostcrstsetspincount-method"></a>IHostCrst::SetSpinCount 方法

为当前的 [IHostCrst](ihostcrst-interface.md) 实例设置自旋计数。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetSpinCount (  
    [in] DWORD dwSpinCount  
);  
```  
  
## <a name="parameters"></a>参数  

 `dwSpinCount`  
 中当前实例的新自旋计数 `IHostCrst` 。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`SetSpinCount` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE| (CLR) 的公共语言运行时未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
  
## <a name="remarks"></a>注解  

 在多处理器系统上，如果当前实例所表示的临界区 `IHostCrst` 不可用，则调用线程将在 `dwSpinCount` 调用 [IHostSemaphore：： Wait](ihostsemaphore-wait-method.md) 与临界区关联的信号量之前，旋转时间。 如果在自旋操作期间关键部分变为免费，则调用线程将避免等待操作。  
  
 的用法与 `dwSpinCount` Win32 函数中具有相同名称的参数的用法相同 `InitializeCriticalSectionAndSpinCount` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRSyncManager 接口](iclrsyncmanager-interface.md)
- [IHostCrst 接口](ihostcrst-interface.md)
- [IHostSyncManager 接口](ihostsyncmanager-interface.md)
