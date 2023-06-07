---
title: IHostIoCompletionManager::Bind 方法
ms.date: 03/30/2017
api_name:
- IHostIoCompletionManager.Bind
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostIoCompletionManager::Bind
helpviewer_keywords:
- IHostIoCompletionManager::Bind method [.NET Framework hosting]
- Bind method [.NET Framework hosting]
ms.assetid: acd74cb5-7e22-4a07-83c3-82288e1abd9f
topic_type:
- apiref
ms.openlocfilehash: 5231db8de6129ed593e4e0d508b312b7034c01f0
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95733901"
---
# <a name="ihostiocompletionmanagerbind-method"></a>IHostIoCompletionManager::Bind 方法

将指定的句柄绑定到由之前对 [CreateIoCompletionPort](ihostiocompletionmanager-createiocompletionport-method.md)的调用创建的 i/o 完成端口。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT Bind (  
    [in] HANDLE hPort,  
    [in] HANDLE hHandle  
);  
```  
  
## <a name="parameters"></a>参数  

 `hPort`  
 中要绑定到的 i/o 完成端口 `hHandle` 。 如果的值 `hPort` 为 null， `hHandle` 则绑定到默认 i/o 完成端口。  
  
 `hHandle`  
 中要绑定到的操作系统句柄 `hPort` 。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`Bind` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE| (CLR) 的公共语言运行时未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
  
## <a name="remarks"></a>注解  

 I/o 完成端口是使用对的调用创建的 `CreateIoCompletionPort` 。 CLR 调用 `Bind` 将句柄绑定到该端口。  
  
> [!NOTE]
> 当 i/o 请求完成时，主机必须调用 [ICLRIoCompletionManager：： OnComplete](iclriocompletionmanager-oncomplete-method.md) 方法。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRIoCompletionManager 接口](iclriocompletionmanager-interface.md)
