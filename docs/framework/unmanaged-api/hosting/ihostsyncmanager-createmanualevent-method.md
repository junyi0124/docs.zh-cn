---
title: IHostSyncManager::CreateManualEvent 方法
ms.date: 03/30/2017
api_name:
- IHostSyncManager.CreateManualEvent
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostSyncManager::CreateManualEvent
helpviewer_keywords:
- CreateManualEvent method [.NET Framework hosting]
- IHostSyncManager::CreateManualEvent method [.NET Framework hosting]
ms.assetid: 68661fbd-09cf-46dc-890b-e694f8a3880a
topic_type:
- apiref
ms.openlocfilehash: 67af8f125b2be39138bac5d51148215f3a3acf86
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723865"
---
# <a name="ihostsyncmanagercreatemanualevent-method"></a>IHostSyncManager::CreateManualEvent 方法

创建手动重置的事件对象。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CreateManualEvent (  
    [in]  BOOL bInitialState,  
    [out] IHostManualEvent **ppEvent  
);  
```  
  
## <a name="parameters"></a>参数  

 `bInitialState`  
 [in] `true` ，如果对象已终止，则为; 否则为 `false` 。  
  
 `ppEvent`  
 弄指向 [IHostManualEvent](ihostmanualevent-interface.md) 实例的地址的指针; 如果未能创建该事件，则为 null。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`CreateManualEvent` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE| (CLR) 的公共语言运行时未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
|E_OUTOFMEMORY|没有足够的内存可用于创建请求的事件对象。|  
  
## <a name="remarks"></a>注解  

 `CreateManualEvent` 创建一个 `IHostManualEvent` 手动重置事件对象，该对象需要调用 [IHostManualEvent：： reset](ihostmanualevent-reset-method.md) 方法，以将其设置为非终止状态。 `CreateManualEvent``CreateEvent`使用 `true` 为参数指定的值镜像 Win32 函数 `bManualReset` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRSyncManager 接口](iclrsyncmanager-interface.md)
- [IHostManualEvent 接口](ihostmanualevent-interface.md)
- [IHostSyncManager 接口](ihostsyncmanager-interface.md)
