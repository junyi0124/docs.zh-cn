---
title: ICLRPolicyManager::SetActionOnTimeout 方法
ms.date: 03/30/2017
api_name:
- ICLRPolicyManager.SetActionOnTimeout
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRPolicyManager::SetActionOnTimeout
helpviewer_keywords:
- SetActionOnTimeout method [.NET Framework hosting]
- ICLRPolicyManager::SetActionOnTimeout method [.NET Framework hosting]
ms.assetid: 38439fa1-2b99-4fa8-a6ec-08afc0f83b9c
topic_type:
- apiref
ms.openlocfilehash: 3ddd78ea35d5709abb30af085b2212a09b28c2ef
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95725555"
---
# <a name="iclrpolicymanagersetactionontimeout-method"></a>ICLRPolicyManager::SetActionOnTimeout 方法

指定在指定操作超时时公共语言运行时 (CLR) 应执行的策略操作。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetActionOnTimeout (  
    [in] EClrOperation operation,  
    [in] EPolicyAction action  
);  
```  
  
## <a name="parameters"></a>参数  

 `operation`  
 中 [EClrOperation](eclroperation-enumeration.md) 值之一，指示要为其指定超时操作的操作。 支持以下值：  
  
- OPR_AppDomainUnload  
  
- OPR_ProcessExit  
  
- OPR_ThreadRudeAbortInCriticalRegion  
  
- OPR_ThreadRudeAbortInNonCriticalRegion  
  
 `action`  
 中 [EPolicyAction](epolicyaction-enumeration.md) 值之一，指示操作超时时要执行的策略操作。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`SetActionOnTimeout` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE|CLR 未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 方法返回 E_FAIL 后，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
|E_INVALIDARG|无法为指定的设置超时 `operation` ，或者为提供的值无效 `operation` 。|  
  
## <a name="remarks"></a>注解  

 超时值可以是 CLR 设置的默认超时值，也可以是主机在 [ICLRPolicyManager：： SetTimeout](iclrpolicymanager-settimeout-method.md) 方法的调用中指定的值。  
  
 并非所有策略操作值都可以指定为 CLR 操作的超时行为。 `SetActionOnTimeout` 通常仅用于升级行为。 例如，主机可以指定线程中止进入强制线程中止，但不能指定相反的。 下表描述了有效值的有效值 `action` `operation` 。  
  
|值 `operation`|`action` 的有效值|  
|---------------------------|-------------------------------|  
|OPR_ThreadRudeAbortInNonCriticalRegion<br /><br /> OPR_ThreadRudeAbortInCriticalRegion|- eRudeAbortThread<br />- eUnloadAppDomain<br />- eRudeUnloadAppDomain<br />- eExitProcess<br />- eFastExitProcess<br />- eRudeExitProcess<br />- eDisableRuntime|  
|OPR_AppDomainUnload|- eUnloadAppDomain<br />- eRudeUnloadAppDomain<br />- eExitProcess<br />- eFastExitProcess<br />- eRudeExitProcess<br />- eDisableRuntime|  
|OPR_ProcessExit|- eExitProcess<br />- eFastExitProcess<br />- eRudeExitProcess<br />- eDisableRuntime|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [EClrOperation 枚举](eclroperation-enumeration.md)
- [EPolicyAction 枚举](epolicyaction-enumeration.md)
- [ICLRControl 接口](iclrcontrol-interface.md)
- [ICLRPolicyManager 接口](iclrpolicymanager-interface.md)
