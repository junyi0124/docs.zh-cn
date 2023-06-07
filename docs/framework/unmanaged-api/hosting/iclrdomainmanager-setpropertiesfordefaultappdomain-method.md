---
title: ICLRDomainManager::SetPropertiesForDefaultAppDomain 方法
ms.date: 03/30/2017
api_name:
- ICLRDomainManager.SetPropertiesForDefaultAppDomain
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRDomainManager::SetPropertiesForDefaultAppDomain
helpviewer_keywords:
- ICLRDomainManager::SetPropertiesForDefaultAppDomain method [.NET Framework hosting]
- SetPropertiesForDefaultAppDomain method [.NET Framework hosting]
ms.assetid: 43e61c4b-c435-45ec-9ef6-c68403aa4200
ms.openlocfilehash: b5577d0444caf14fb47d9d7e2de60a8399378db7
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95702129"
---
# <a name="iclrdomainmanagersetpropertiesfordefaultappdomain-method"></a>ICLRDomainManager::SetPropertiesForDefaultAppDomain 方法

设置将用于初始化默认应用程序域的属性。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetPropertiesForDefaultAppDomain(  
    [in] DWORD nProperties,  
    [in] LPCWSTR *pwszPropertyNames,  
    [in] LPCWSTR *pwszPropertyValues  
);  
```  
  
## <a name="parameters"></a>参数  

 `nProperties`  
 中和中的项数 `pwszPropertyNames` `pwszPropertyValues` 。  
  
 `pwszPropertyNames`  
 中属性名称的数组; 如果没有属性，则为 null。 目前，此方法识别的唯一属性名称为 "PARTIAL_TRUST_VISIBLE_ASSEMBLIES"。  
  
 `pwszPropertyValues`  
 中属性值的数组; 如果没有属性，则为 null。  
  
## <a name="return-value"></a>返回值  

 此方法返回以下特定 HRESULT 以及表示方法失败的 HRESULT 错误。  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|该方法已成功完成。|  
|HRESULT_FROM_WIN32 (ERROR_UNKNOWN_PROPERTY) |`pwszPropertyNames` 包含此方法无法识别的属性名称。|  
  
## <a name="remarks"></a>注解  

 "PARTIAL_TRUST_VISIBLE_ASSEMBLIES" 的属性值是一个程序集的列表，这些程序集具有 <xref:System.Security.AllowPartiallyTrustedCallersAttribute> 带有标志的条件性 (APTCA) 特性 <xref:System.Security.PartialTrustVisibilityLevel.NotVisibleByDefault?displayProperty=nameWithType> ，在默认应用程序域中对部分受信任的调用方可见。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [承载](index.md)
- [ICLRDomainManager 接口](iclrdomainmanager-interface.md)
