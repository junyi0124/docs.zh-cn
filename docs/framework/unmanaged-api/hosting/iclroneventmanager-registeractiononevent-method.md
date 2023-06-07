---
title: ICLROnEventManager::RegisterActionOnEvent 方法
ms.date: 03/30/2017
api_name:
- ICLROnEventManager.RegisterActionOnEvent
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLROnEventManager::RegisterActionOnEvent
helpviewer_keywords:
- ICLROnEventManager::RegisterActionOnEvent method [.NET Framework hosting]
- RegisterActionOnEvent method [.NET Framework hosting]
ms.assetid: b944cf49-918d-4c4e-993b-77d097a52550
topic_type:
- apiref
ms.openlocfilehash: 7f0770585e977f5299a40517c28dfb776b2ab898
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95725607"
---
# <a name="iclroneventmanagerregisteractiononevent-method"></a>ICLROnEventManager::RegisterActionOnEvent 方法

为指定事件注册回调指针。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT RegisterActionOnEvent (  
    [in] EClrEvent event,  
    [in] IActionOnCLREvent *pAction  
);  
```  
  
## <a name="parameters"></a>参数  

 `event`  
 中 [EClrEvent](eclrevent-enumeration.md) 值之一，指示要为其注册所描述的回调指针的事件 `pAction` 。  
  
 `pAction`  
 中指向 [IActionOnCLREvent](iactiononclrevent-interface.md) 对象的指针，当注册的事件激发时，将调用该对象。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`RegisterActionOnEvent` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE| (CLR) 的公共语言运行时未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 方法返回 E_FAIL 后，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
  
## <a name="remarks"></a>注解  

 宿主可以为描述的两个事件类型中的一个或两个注册回调 `EClrEvent` 。 宿主 `ICLROnEventManager` 通过调用 [ICLRControl：： GetCLRManager](iclrcontrol-getclrmanager-method.md) 方法获取接口。  
  
> [!NOTE]
> 注册的事件 `RegisterActionOnEvent` 可以从不同的线程激发一次，以发出卸载或禁用 CLR 的信号。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [EClrEvent 枚举](eclrevent-enumeration.md)
- [IActionOnCLREvent 接口](iactiononclrevent-interface.md)
- [ICLRControl 接口](iclrcontrol-interface.md)
- [ICLROnEventManager 接口](iclroneventmanager-interface.md)
