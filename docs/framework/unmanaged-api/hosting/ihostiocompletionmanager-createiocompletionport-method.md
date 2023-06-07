---
title: IHostIoCompletionManager::CreateIoCompletionPort 方法
ms.date: 03/30/2017
api_name:
- IHostIoCompletionManager.CreateIoCompletionPort
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostIoCompletionManager::CreateIoCompletionPort
helpviewer_keywords:
- IHostIoCompletionManager::CreateIoCompletionPort method [.NET Framework hosting]
- CreateIoCompletionPort method [.NET Framework hosting]
ms.assetid: 907a2b43-68db-44a7-acac-89e792e7bb3c
topic_type:
- apiref
ms.openlocfilehash: 0c74e073d55ab7dc98620052a0cfd68c294f7a1c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95724268"
---
# <a name="ihostiocompletionmanagercreateiocompletionport-method"></a>IHostIoCompletionManager::CreateIoCompletionPort 方法

请求宿主创建新的 i/o 完成端口。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CreateIoCompletionPort (  
    [out] HANDLE *phPort  
);  
```  
  
## <a name="parameters"></a>参数  

 `phPort`  
 弄指向新创建的 i/o 完成端口的句柄的指针; 如果无法创建端口，则为 0 (零) 。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`CreateIoCompletionPort` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE| (CLR) 的公共语言运行时未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
|E_OUTOFMEMORY|没有足够的内存可用于分配请求的资源。|  
  
## <a name="remarks"></a>注解  

 CLR 调用 `CreateIoCompletionPort` 方法来请求宿主创建新的 i/o 完成端口。 它通过调用 [IHostIoCompletionManager：： Bind](ihostiocompletionmanager-bind-method.md) 方法将 i/o 操作绑定到此端口。 主机通过调用 [ICLRIoCompletionManager：： OnComplete](iclriocompletionmanager-oncomplete-method.md)将状态报告回 CLR。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRIoCompletionManager 接口](iclriocompletionmanager-interface.md)
- [IHostIoCompletionManager 接口](ihostiocompletionmanager-interface.md)
