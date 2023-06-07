---
title: ICLRDebugManager::EndConnection 方法
ms.date: 03/30/2017
api_name:
- ICLRDebugManager.EndConnection
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRDebugManager::EndConnection
helpviewer_keywords:
- ICLRDebugManager::EndConnection method [.NET Framework hosting]
- EndConnection method [.NET Framework hosting]
ms.assetid: 89dc7363-2f29-4eb2-8f23-fccdda6a76a6
topic_type:
- apiref
ms.openlocfilehash: d6f22d6185f4063078463043a6ffd46e56289267
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95719848"
---
# <a name="iclrdebugmanagerendconnection-method"></a>ICLRDebugManager::EndConnection 方法

删除任务列表与标识符和友好名称之间的关联。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EndConnection (  
    [in] CONNID dwConnectionId  
);  
```  
  
## <a name="parameters"></a>参数  

 `dwConnectionId`  
 中连接的特定于宿主的标识符，以及 (CLR) 任务的公共语言运行时关联列表。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`EndConnection` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE|CLR 未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 方法返回 E_FAIL 后，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
|E_INVALIDARG|[BeginConnection](iclrdebugmanager-beginconnection-method.md) 从不使用调用 `dwConnectionId` ，或为 `dwConnectionId` 零。|  
  
## <a name="remarks"></a>注解  

 [ICLRDebugManager](iclrdebugmanager-interface.md) 提供了三种方法：、 `BeginConnection` [SetConnectionTasks](iclrdebugmanager-setconnectiontasks-method.md)和 `EndConnection` ，以将任务列表与标识符和友好名称关联起来。  
  
> [!IMPORTANT]
> 对于每组任务，这三种方法都必须按特定的顺序进行调用。 `BeginConnection` 首先调用以建立新连接。 `SetConnectionTasks` 在旁边调用，提供要与该连接相关联的一组任务。 `EndConnection` 最后调用，以删除任务列表与标识符和友好名称之间的关联。但是，可以嵌套不同连接的调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRControl 接口](iclrcontrol-interface.md)
- [ICLRDebugManager 接口](iclrdebugmanager-interface.md)
- [BeginConnection 方法](iclrdebugmanager-beginconnection-method.md)
- [SetConnectionTasks 方法](iclrdebugmanager-setconnectiontasks-method.md)
- [IHostControl 接口](ihostcontrol-interface.md)
