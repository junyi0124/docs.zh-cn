---
title: IHostSecurityManager::SetSecurityContext 方法
ms.date: 03/30/2017
api_name:
- IHostSecurityManager.SetSecurityContext
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostSecurityManager::SetSecurityContext
helpviewer_keywords:
- SetSecurityContext method [.NET Framework hosting]
- IHostSecurityManager::SetSecurityContext method [.NET Framework hosting]
ms.assetid: e4372384-ee69-48d7-97e0-8fab7866597a
topic_type:
- apiref
ms.openlocfilehash: dadacaea2b8741afc7b8c51404e2604dc759a629
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95680373"
---
# <a name="ihostsecuritymanagersetsecuritycontext-method"></a>IHostSecurityManager::SetSecurityContext 方法

设置当前正在执行的线程的安全上下文。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetSecurityContext (  
    [in]  EContextType eContextType,  
    [out] IHostSecurityContext** ppSecurityContext  
);  
```  
  
## <a name="parameters"></a>参数  

 `eContextType`  
 中 [EContextType](econtexttype-enumeration.md) 值之一，指示公共语言运行时 (CLR) 在主机上放置的上下文的类型。  
  
 `ppSecurityContext`  
 弄指向新 [IHostSecurityContext](ihostsecuritycontext-interface.md) 对象地址的指针。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`SetSecurityContext` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE|CLR 未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
  
## <a name="remarks"></a>注解  

 CLR `SetSecurityContext` 在多个方案中调用。 在执行类和模块构造函数和终结器之前，CLR 将调用 `SetSecurityContext` 以防止执行失败。 然后，在执行构造函数或终结器后，通过使用对的另一调用，将安全上下文重置为其原始状态 `SetSecurityContext` 。 I/o 完成时，会出现类似的模式。 如果主机实现 [IHostIoCompletionManager](ihostiocompletionmanager-interface.md)，则 CLR 会在 `SetSecurityContext` 主机调用 [ICLRIoCompletionManager：： OnComplete](iclriocompletionmanager-oncomplete-method.md)后调用。  
  
 在工作线程中的异步点，CLR 在 `SetSecurityContext` <xref:System.Threading.ThreadPool.QueueUserWorkItem%2A?displayProperty=nameWithType> [IHostThreadPoolManager：： QueueUserWorkItem](ihostthreadpoolmanager-queueuserworkitem-method.md)内或内调用，具体取决于宿主或 CLR 是否实现线程池。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Threading.ThreadPool?displayProperty=nameWithType>
- [EContextType 枚举](econtexttype-enumeration.md)
- [ICLRIoCompletionManager 接口](iclriocompletionmanager-interface.md)
- [IHostIoCompletionManager 接口](ihostiocompletionmanager-interface.md)
- [IHostSecurityContext 接口](ihostsecuritycontext-interface.md)
- [IHostSecurityManager 接口](ihostsecuritymanager-interface.md)
- [IHostThreadPoolManager 接口](ihostthreadpoolmanager-interface.md)
