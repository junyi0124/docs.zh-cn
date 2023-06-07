---
title: ICLRHostProtectionManager::SetProtectedCategories 方法
ms.date: 03/30/2017
api_name:
- ICLRHostProtectionManager.SetProtectedCategories
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRHostProtectionManager::SetProtectedCategories
helpviewer_keywords:
- SetProtectedCategories method [.NET Framework hosting]
- ICLRHostProtectionManager::SetProtectedCategories method [.NET Framework hosting]
ms.assetid: fa21dc7b-5da7-440b-b59e-9180e5181f9d
topic_type:
- apiref
ms.openlocfilehash: 0557a8f1c7c495950933a44cacd23ada8e84964e
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95730495"
---
# <a name="iclrhostprotectionmanagersetprotectedcategories-method"></a>ICLRHostProtectionManager::SetProtectedCategories 方法

指定应阻止在部分受信任代码中运行的托管类型和成员的类别。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetProtectedCategories (  
    [in] EApiCategories  categories  
);  
```  
  
## <a name="parameters"></a>参数  

 `categories`  
 中 [EApiCategories](eapicategories-enumeration.md) 值的组合，指示应阻止托管类型和成员的哪些类别在部分受信任的代码中运行。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`SetProtectedCategories` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE| (CLR) 的公共语言运行时未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 方法返回 E_FAIL 后，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
  
## <a name="remarks"></a>注解  

 每个 `EApiCategories` 值都是指托管类型和成员的列表。 `EApiCategories`枚举和方法与 `SetProtectedCategories` 托管类直接相关 <xref:System.Security.Permissions.HostProtectionAttribute> ，后者用于标记公开与所述的类别对应的功能的托管类型和成员 `EApiCategories` 。 有关详细信息，请参阅 <xref:System.Security.Permissions.HostProtectionAttribute> 和 <xref:System.Security.Permissions.HostProtectionResource> 枚举，后者直接对应于 `EApiCategories` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Security.Permissions.HostProtectionAttribute>
- <xref:System.Security.Permissions.HostProtectionResource>
- [EApiCategories 枚举](eapicategories-enumeration.md)
- [ICLRControl 接口](iclrcontrol-interface.md)
- [ICLRHostProtectionManager 接口](iclrhostprotectionmanager-interface.md)
