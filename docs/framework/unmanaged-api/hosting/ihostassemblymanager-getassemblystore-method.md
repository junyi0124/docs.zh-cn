---
title: IHostAssemblyManager::GetAssemblyStore 方法
ms.date: 03/30/2017
api_name:
- IHostAssemblyManager.GetAssemblyStore
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostAssemblyManager::GetAssemblyStore
helpviewer_keywords:
- IHostAssemblyManager::GetAssemblyStore method [.NET Framework hosting]
- GetAssemblyStore method [.NET Framework hosting]
ms.assetid: d0f74593-9bb1-4a11-8096-e29734b20698
topic_type:
- apiref
ms.openlocfilehash: 936ea068f3cc5567a00af5f2bdd5f3d9cd52bc81
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95681179"
---
# <a name="ihostassemblymanagergetassemblystore-method"></a>IHostAssemblyManager::GetAssemblyStore 方法

获取一个接口指针，该指针指向表示宿主加载的程序集列表的 [IHostAssemblyStore](ihostassemblystore-interface.md) 。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetAssemblyStore (  
    [out] IHostAssemblyStore **ppAssemblyStore  
);  
```  
  
## <a name="parameters"></a>参数  

 `ppAssemblyStore`  
 弄指向实例的函数指针 `IHostAssemblyStore` ，如果宿主未实现，则为 null `IHostAssemblyStore` 。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`GetAssemblyStore` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE| (CLR) 的公共语言运行时未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
|E_NOINTERFACE|宿主不提供的实现 `IHostAssemblyStore` 。|  
  
## <a name="remarks"></a>注解  

 `IHostAssemblyStore` 提供允许主机独立于 CLR 绑定到程序集和模块的方法。 主机通常提供程序集存储区，以允许从文件系统以外的其他格式加载程序集。  
  
> [!NOTE]
> 如果主机未实现，则 `IHostAssemblyStore` `GetAssemblyStore` 应将 HRESULT 值返回 E_NOINTERFACE，并将设置 `ppAssemblyStore` 为 null。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IHostAssemblyManager 接口](ihostassemblymanager-interface.md)
- [IHostAssemblyStore 接口](ihostassemblystore-interface.md)
