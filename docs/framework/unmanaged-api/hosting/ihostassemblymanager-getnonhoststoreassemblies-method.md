---
title: IHostAssemblyManager::GetNonHostStoreAssemblies 方法
ms.date: 03/30/2017
api_name:
- IHostAssemblyManager.GetNonHostStoreAssemblies
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostAssemblyManager::GetNonHostStoreAssemblies
helpviewer_keywords:
- IHostAssemblyManager::GetNonHostStoreAssemblies method [.NET Framework hosting]
- GetNonHostStoreAssemblies method [.NET Framework hosting]
ms.assetid: d2250b38-c76a-40ce-80c8-ba45149886e8
topic_type:
- apiref
ms.openlocfilehash: a34b907514376927d8a1aa66b136916108b704d8
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95681140"
---
# <a name="ihostassemblymanagergetnonhoststoreassemblies-method"></a>IHostAssemblyManager::GetNonHostStoreAssemblies 方法

获取一个指向 [ICLRAssemblyReferenceList](iclrassemblyreferencelist-interface.md) 的接口指针，该指针表示宿主需要公共语言运行时 (CLR) 加载的程序集的列表。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetNonHostStoreAssemblies (  
    [out] ICLRAssemblyReferenceList **ppReferenceList  
);  
```  
  
## <a name="parameters"></a>参数  

 `ppReferenceList`  
 弄指向的地址的指针 `ICLRAssemblyReferenceList` ，该地址包含对宿主预期 CLR 加载的程序集的引用的列表。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`GetNonHostStoreAssemblies` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE|CLR 未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
|E_OUTOFMEMORY|没有足够的内存可用于创建请求的引用列表 `ICLRAssemblyReferenceList` 。|  
  
## <a name="remarks"></a>注解  

 CLR 使用以下一组准则来解析引用：  
  
- 首先，它会咨询返回的程序集引用的列表 `GetNonHostStoreAssemblies` 。  
  
- 如果该程序集出现在列表中，则 CLR 正常绑定到该程序集。  
  
- 如果程序集未出现在列表中，并且宿主已提供 [IHostAssemblyStore](ihostassemblystore-interface.md)的实现，则 CLR 将调用 [IHostAssemblyStore：:P rovideassembly](ihostassemblystore-provideassembly-method.md) ，以允许主机提供要绑定到的程序集。  
  
- 否则，CLR 无法绑定到程序集。  
  
 如果主机设置 `ppReferenceList` 为 null，则 CLR 首先探测全局程序集缓存，调用 `ProvideAssembly` ，然后探测应用程序基以解析程序集引用。  
  
> [!NOTE]
> 初始化后，CLR 只调用 `GetNonHostStoreAssemblies` 一次。 不会再次调用方法。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRAssemblyReferenceList 接口](iclrassemblyreferencelist-interface.md)
- [IHostAssemblyManager 接口](ihostassemblymanager-interface.md)
- [IHostAssemblyStore 接口](ihostassemblystore-interface.md)
