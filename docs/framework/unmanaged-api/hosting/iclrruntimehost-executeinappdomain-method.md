---
title: ICLRRuntimeHost::ExecuteInAppDomain 方法
ms.date: 03/30/2017
api_name:
- ICLRRuntimeHost.ExecuteInAppDomain
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRRuntimeHost::ExecuteInAppDomain
helpviewer_keywords:
- ICLRRuntimeHost::ExecuteInAppDomain method [.NET Framework hosting]
- ExecuteInAppDomain method [.NET Framework hosting]
ms.assetid: e2b0e2db-3fae-4b56-844e-d30a125a660c
topic_type:
- apiref
ms.openlocfilehash: 4c28c4a5cc64b20c9ac9c57e1aef5e7b90a20e01
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95728883"
---
# <a name="iclrruntimehostexecuteinappdomain-method"></a>ICLRRuntimeHost::ExecuteInAppDomain 方法

指定 <xref:System.AppDomain> 要在其中执行指定的托管代码的。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT ExecuteInAppDomain(  
    [in] DWORD AppDomainId,
    [in] FExecuteInDomainCallback pCallback,
    [in] void* cookie  
);  
```  
  
## <a name="parameters"></a>参数  

 `AppDomainId`  
 中 <xref:System.AppDomain> 要在其中执行指定方法的的数值 ID。  
  
 `pCallback`  
 中指向函数的指针，该函数在指定的中执行 <xref:System.AppDomain> 。  
  
 `cookie`  
 中指向不透明调用方分配的内存的指针。 此参数由公共语言运行时 (CLR) 传递到域回调。 它不是运行时托管堆内存;此内存的分配和生存期均由调用方控制。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`ExecuteInAppDomain` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE|CLR 未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 如果方法返回 E_FAIL，则 CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
  
## <a name="remarks"></a>注解  

 `ExecuteInAppDomain` 允许主机控制 <xref:System.AppDomain> 应在其上执行指定托管方法的管理。 可以 <xref:System.AppDomain.Id%2A> 通过调用 [GetCurrentAppDomainId 方法](iclrruntimehost-getcurrentappdomainid-method.md)，获取与属性的值相对应的应用程序域标识符的值。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRRuntimeHost 接口](iclrruntimehost-interface.md)
