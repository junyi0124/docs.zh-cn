---
title: ICLRDomainManager::SetAppDomainManagerType 方法
ms.date: 03/30/2017
api_name:
- ICLRDomainManager.SetAppDomainManagerType
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRDomainManager::SetAppDomainManagerType
helpviewer_keywords:
- SetAppDomainManagerType method, ICLRDomainManager interface [.NET Framework hosting]
- ICLRDomainManager::SetAppDomainManagerType method [.NET Framework hosting]
ms.assetid: ee91abb0-cb74-41dd-927b-e117fb8ffdf4
ms.openlocfilehash: 7c6b328793e6437682ad8d642e611be30e7b0fe6
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95702142"
---
# <a name="iclrdomainmanagersetappdomainmanagertype-method"></a>ICLRDomainManager::SetAppDomainManagerType 方法

指定从类派生的、 <xref:System.AppDomainManager?displayProperty=nameWithType> 将用于初始化默认应用程序域的应用程序域管理器的类型。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetAppDomainManagerType(  
    [in] LPCWSTR wszAppDomainManagerAssembly,  
    [in] LPCWSTR wszAppDomainManagerType,  
    [in] EInitializeNewDomainFlags dwInitializeDomainFlags  
);  
```  
  
## <a name="parameters"></a>参数  

 `wszAppDomainManagerAssembly`  
 中包含应用程序域管理器类型的程序集的显示名称;例如： "AdMgrExample，Version = 1.0.0.0，Culture = 中立，PublicKeyToken = 6856bccf150f00b3"。  
  
 `wszAppDomainManagerType`  
 中应用程序域管理器的类型名称，包括命名空间。  
  
 `dwInitializeDomainFlags`  
 中 [EInitializeNewDomainFlags](einitializenewdomainflags-enumeration.md) 枚举值的组合，提供有关应用程序域管理器的信息。  
  
## <a name="return-value"></a>返回值  

 此方法返回以下特定 HRESULT 以及表示方法失败的 HRESULT 错误。  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|该方法已成功完成。|  
|HOST_E_CLRNOTAVAILABLE| (CLR) 的公共语言运行时未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
  
## <a name="remarks"></a>注解  

 目前，的唯一定义的值 `dwInitializeDomainFlags` 为 `eInitializeNewDomainFlags_NoSecurityChanges` ，它指示公共语言运行时 (CLR) 应用程序域管理器在执行方法期间将不会修改安全设置 <xref:System.AppDomainManager.InitializeNewDomain%2A?displayProperty=nameWithType> 。 这允许 CLR 优化加载具有条件 <xref:System.Security.AllowPartiallyTrustedCallersAttribute> (APTCA) 特性的程序集。 如果这组程序集的可传递闭包很大，则这可能会显著提高启动时间。  
  
> [!IMPORTANT]
> 如果主机 `eInitializeNewDomainFlags_NoSecurityChanges` 为应用程序域管理器指定了，则 <xref:System.InvalidOperationException> 如果尝试修改应用程序域的安全性，则会引发。  
  
 调用 [ICLRControl：： SetAppDomainManagerType](iclrcontrol-setappdomainmanagertype-method.md)方法等效于调用 `ICLRDomainManager::SetAppDomainManagerType` `eInitializeNewDomainFlags_None` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [承载](index.md)
- [ICLRDomainManager 接口](iclrdomainmanager-interface.md)
- [EInitializeNewDomainFlags 枚举](einitializenewdomainflags-enumeration.md)
