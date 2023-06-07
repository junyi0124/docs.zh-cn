---
title: IHostMemoryManager::RegisterMemoryNotificationCallback 方法
ms.date: 03/30/2017
api_name:
- IHostMemoryManager.RegisterMemoryNotificationCallback
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostMemoryManager::RegisterMemoryNotificationCallback
helpviewer_keywords:
- IHostMemoryManager::RegisterMemoryNotificationCallback method [.NET Framework hosting]
- RegisterMemoryNotificationCallback method [.NET Framework hosting]
ms.assetid: 65d301f6-4dbb-4b5f-8eff-82540e2b6465
topic_type:
- apiref
ms.openlocfilehash: edb29378412583d7cdec804b08f8f622d642b02f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95731301"
---
# <a name="ihostmemorymanagerregistermemorynotificationcallback-method"></a>IHostMemoryManager::RegisterMemoryNotificationCallback 方法

注册指向主机调用的回调函数的指针，以通知公共语言运行时 (计算机上当前内存负载的 CLR) 。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT RegisterMemoryNotificationCallback (  
    [in] ICLRMemoryNotificationCallback* pCallback  
);  
```  
  
## <a name="parameters"></a>参数  

 `pCallback`  
 中一个接口指针，该指针指向由 CLR 实现的 [ICLRMemoryNotificationCallback](iclrmemorynotificationcallback-interface.md) 实例。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`RegisterMemoryNotificationCallback` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE|CLR 未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
  
## <a name="remarks"></a>注解  

 由于 `ICLRMemoryNotificationCallback` 接口仅定义了一个 ([ICLRMemoryNotificationCallback：： OnMemoryNotification](iclrmemorynotificationcallback-onmemorynotification-method.md)) 的方法，并且由于 `pCallback` 是一个指向 `ICLRMemoryNotificationCallback` CLR 提供的实例的指针，因此注册对于回调函数本身是有效的。 宿主调用 `OnMemoryNotification` 以报告内存压力条件，而不是使用标准的 Win32 `CreateMemoryResourceNotification` 函数。 有关详细信息，请参阅 Windows 平台文档。  
  
> [!NOTE]
> 对 `OnMemoryNotification` 从不阻止的的调用。 它们始终立即返回。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRMemoryNotificationCallback 接口](iclrmemorynotificationcallback-interface.md)
- [IHostMemoryManager 接口](ihostmemorymanager-interface.md)
