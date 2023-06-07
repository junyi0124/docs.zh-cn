---
title: ICorRuntimeHost::CreateDomainEx 方法
ms.date: 03/30/2017
api_name:
- ICorRuntimeHost.CreateDomainEx
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICorRuntimeHost::CreateDomainEx
helpviewer_keywords:
- ICorRuntimeHost::CreateDomainEx method [.NET Framework hosting]
- CreateDomainEx method [.NET Framework hosting]
ms.assetid: 1bdde382-f8ba-4cc8-94b2-d1ac919c585e
topic_type:
- apiref
ms.openlocfilehash: d6d9e06b6ed40bb0e5a65fd64f8bca7abe3afa84
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95715675"
---
# <a name="icorruntimehostcreatedomainex-method"></a>ICorRuntimeHost::CreateDomainEx 方法

创建应用程序域。 调用方将类型的接口指针接收 <xref:System._AppDomain> 到类型的实例 <xref:System.AppDomain?displayProperty=nameWithType> 。 此方法允许调用方传递 IAppDomainSetup 实例，以配置返回的实例的附加功能 <xref:System._AppDomain> 。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CreateDomainEx (  
    [in] LPCWSTR     pwzFriendlyName,  
    [in] IUnknown*   pSetup,  
    [in] IUnknown*   pIdentityArray,  
    [out] IUnknown** pAppDomain  
);  
```  
  
## <a name="parameters"></a>参数  

 `pwzFriendlyName`  
 中用于给域指定友好名称的可选参数。 此友好名称可在用户界面（例如调试器）中显示，以标识域。  
  
 `pSetup`  
 中类型的可选接口指针 `IAppDomainSetup` ，由对 [ICorRuntimeHost：： CreateDomainSetup](icorruntimehost-createdomainsetup-method.md) 方法的调用获得。  
  
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
  
## <a name="remarks"></a>注解  

 `CreateDomainEx`通过允许调用方[CreateDomain](icorruntimehost-createdomain-method.md)传入 `IAppDomainSetup` 具有属性值的实例来配置应用程序域，从而扩展了 CreateDomain 的功能。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：** 1.0、1。1  
  
## <a name="see-also"></a>另请参阅

- <xref:System._AppDomain>
- <xref:System.AppDomain>
- <xref:System.IAppDomainSetup?displayProperty=nameWithType>
- [CreateDomain 方法](icorruntimehost-createdomain-method.md)
- [ICorRuntimeHost 接口](icorruntimehost-interface.md)
