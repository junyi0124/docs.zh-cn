---
title: ICorRuntimeHost::CreateDomain 方法
ms.date: 03/30/2017
api_name:
- ICorRuntimeHost.CreateDomain
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICorRuntimeHost::CreateDomain
helpviewer_keywords:
- CreateDomain method [.NET Framework hosting]
- ICorRuntimeHost::CreateDomain method [.NET Framework hosting]
ms.assetid: b96c5ef3-a9df-4c7c-9952-432d3272cb5c
topic_type:
- apiref
ms.openlocfilehash: c495ce47b699d2e32d1f02e4afcf0444a9930c34
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723904"
---
# <a name="icorruntimehostcreatedomain-method"></a>ICorRuntimeHost::CreateDomain 方法

创建应用程序域。 调用方接收类型为的实例的接口指针 <xref:System._AppDomain> <xref:System.AppDomain?displayProperty=nameWithType> 。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CreateDomain (  
    [in] LPWSTR    pwzFriendlyName,  
    [in] IUnknown* pIdentityArray,  
    [out] void   **pAppDomain  
);  
```  
  
## <a name="parameters"></a>参数  

 `pwzFriendlyName`  
 中用于给域指定友好名称的可选参数。 此友好名称可在用户界面（例如调试器）中显示，以标识域。  
  
 `pIdentityArray`  
 中指向实例的可选指针数组， `IIdentity` 这些实例表示通过安全策略映射以建立权限集的证据。 `IIdentity`可以通过调用[CreateEvidence](icorruntimehost-createevidence-method.md)方法来获取对象。  
  
 `pAppDomain`  
 弄类型为的接口指针 <xref:System._AppDomain> <xref:System.AppDomain?displayProperty=nameWithType> ，可用于进一步控制域。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|操作成功。|  
|S_FALSE|操作未能完成。|  
|E_FAIL|发生了未知的灾难性故障。 如果某个方法返回 E_FAIL，则公共语言运行时 (CLR) 在该进程中不再可用。 对任何宿主 Api 的后续调用都会返回 HOST_E_CLRNOTAVAILABLE。|  
|HOST_E_CLRNOTAVAILABLE|CLR 未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：** 1.0、1。1  
  
## <a name="see-also"></a>另请参阅

- <xref:System._AppDomain>
- <xref:System.AppDomain>
- [ICorRuntimeHost 接口](icorruntimehost-interface.md)
