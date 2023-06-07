---
title: ICLRControl::GetCLRManager 方法
ms.date: 03/30/2017
api_name:
- ICLRControl.GetCLRManager
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRControl::GetCLRManager
helpviewer_keywords:
- GetCLRManager method [.NET Framework hosting]
- ICLRControl::GetCLRManager method [.NET Framework hosting]
ms.assetid: 8a11bfa4-cbb0-4082-82b5-f9fba66c93f5
topic_type:
- apiref
ms.openlocfilehash: d18b3a5c06ac0d3a86f7823f3b140c76c6c9a746
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95728350"
---
# <a name="iclrcontrolgetclrmanager-method"></a>ICLRControl::GetCLRManager 方法

获取一个接口指针，该指针指向主机可用于配置公共语言运行时 (CLR) 的任何管理器类型的实例。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetCLRManager (  
    [in]  REFIID  riid,  
    [out] void  **ppObject  
);  
```  
  
## <a name="parameters"></a>参数  

 `riid`  
 中 `IID` 要返回的管理器类型的。 支持以下 `IID` 值。  
  
- IID_ICLRDebugManager：指定 `ppObject` 将为 [ICLRDebugManager](iclrdebugmanager-interface.md)类型。  
  
- IID_ICLRErrorReportingManager：指定 `ppObject` 将为 [ICLRErrorReportingManager](iclrerrorreportingmanager-interface.md)类型。  
  
- IID_ICLRGCManager：指定 `ppObject` 将为 [ICLRGCManager](iclrgcmanager-interface.md)类型。  
  
- IID_ICLRHostProtectionManager：指定 `ppObject` 将为 [ICLRHostProtectionManager](iclrhostprotectionmanager-interface.md)类型。  
  
- IID_ICLROnEventManager：指定 `ppObject` 将为 [ICLROnEventManager](iclroneventmanager-interface.md)类型。  
  
- IID_ICLRPolicyManager：指定 `ppObject` 将为 [ICLRPolicyManager](iclrpolicymanager-interface.md)类型。  
  
- IID_ICLRTaskManager：指定 `ppObject` 将为 [ICLRTaskManager](iclrtaskmanager-interface.md)类型。  
  
 `ppObject`  
 弄指向请求的管理器的接口指针; 如果请求的管理器类型无效，则为 null。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|该方法已成功返回。|  
|HOST_E_CLRNOTAVAILABLE|CLR 未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 方法返回 E_FAIL 后，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
|E_NOINTERFACE|接口类型不受支持。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRControl 接口](iclrcontrol-interface.md)
- [IHostControl 接口](ihostcontrol-interface.md)
