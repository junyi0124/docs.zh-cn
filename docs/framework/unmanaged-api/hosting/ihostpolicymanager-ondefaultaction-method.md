---
title: IHostPolicyManager::OnDefaultAction 方法
ms.date: 03/30/2017
api_name:
- IHostPolicyManager.OnDefaultAction
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostPolicyManager::OnDefaultAction
helpviewer_keywords:
- OnDefaultAction method [.NET Framework hosting]
- IHostPolicyManager::OnDefaultAction method [.NET Framework hosting]
ms.assetid: 071e73bd-4795-470f-9373-cfaef553b7f2
topic_type:
- apiref
ms.openlocfilehash: a22f16c14514b90ce888fafc0ea554bd9f90cb11
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95684078"
---
# <a name="ihostpolicymanagerondefaultaction-method"></a>IHostPolicyManager::OnDefaultAction 方法

向宿主通知公共语言运行时 (CLR) 将要使用 [ICLRPolicyManager：： SetDefaultAction](iclrpolicymanager-setdefaultaction-method.md) 方法调用设置的默认操作，以响应线程中止或 <xref:System.AppDomain> 卸载。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT OnDefaultAction (  
    [in] EClrOperation  operation,
    [in] EPolicyAction  action  
);  
```  
  
## <a name="parameters"></a>参数  

 `operation`  
 中 [EClrOperation](eclroperation-enumeration.md) 值之一，指示 CLR 响应的事件类型。  
  
 `action`  
 中 [EPolicyAction](epolicyaction-enumeration.md) 值之一，指示 CLR 响应事件所采取的操作。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`OnDefaultAction` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE|CLR 未加载到进程中，或 CLR 处于无法运行托管代码或处理调用的状态。 顺利|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [EClrOperation 枚举](eclroperation-enumeration.md)
- [EPolicyAction 枚举](epolicyaction-enumeration.md)
- [ICLRPolicyManager 接口](iclrpolicymanager-interface.md)
- [IHostPolicyManager 接口](ihostpolicymanager-interface.md)
