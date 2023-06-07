---
title: IHostSecurityManager::GetSecurityContext 方法
ms.date: 03/30/2017
api_name:
- IHostSecurityManager.GetSecurityContext
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostSecurityManager::GetSecurityContext
helpviewer_keywords:
- GetSecurityContext method [.NET Framework hosting]
- IHostSecurityManager::GetSecurityContext method [.NET Framework hosting]
ms.assetid: 958970d6-f6a2-4b84-b32a-f555cbaf8f61
topic_type:
- apiref
ms.openlocfilehash: dfb96de02549e6d0f178c099793741f7fbd61d55
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95724788"
---
# <a name="ihostsecuritymanagergetsecuritycontext-method"></a>IHostSecurityManager::GetSecurityContext 方法

从主机获取请求的 [IHostSecurityContext](ihostsecuritycontext-interface.md) 。  
  
## <a name="syntax"></a>语法  
  
```cpp
HRESULT GetSecurityContext (  
    [in]  EContextType eContextType,
    [out] IHostSecurityContext** ppSecurityContext  
);  
```  
  
## <a name="parameters"></a>参数  

 `eContextType`  
 中 [EContextType](econtexttype-enumeration.md) 值之一，指示要返回的安全上下文的类型。  
  
 `ppSecurityContext`  
 弄的接口指针的地址 `IHostSecurityContext` `eContextType` 。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`GetSecurityContext` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE| (CLR) 的公共语言运行时未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
  
## <a name="remarks"></a>注解  

 宿主可以通过 CLR 和用户代码控制对线程标记的所有代码访问。 它还可以确保在异步操作或代码点之间跨受限制的代码访问传递完整的安全上下文信息。 `IHostSecurityContext` 封装此安全上下文信息，这对于 CLR 是不透明的。 CLR 捕获此信息，并在线程池辅助角色项调度、终结器执行以及模块和类构造之间移动。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [EContextType 枚举](econtexttype-enumeration.md)
- [IHostSecurityContext 接口](ihostsecuritycontext-interface.md)
- [IHostSecurityManager 接口](ihostsecuritymanager-interface.md)
