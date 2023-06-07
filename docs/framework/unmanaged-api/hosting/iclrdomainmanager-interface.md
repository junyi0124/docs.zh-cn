---
title: ICLRDomainManager 接口
ms.date: 03/30/2017
api_name:
- ICLRDomainManager
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRDomainManager
helpviewer_keywords:
- ICLRDomainManager interface [.NET Framework hosting]
ms.assetid: f08b2390-d872-4521-a815-e9c237c4c45d
ms.openlocfilehash: a5abb601fe795a0c615403eec69f68ad9f66f00f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95681166"
---
# <a name="iclrdomainmanager-interface"></a>ICLRDomainManager 接口

使宿主能够指定将用于初始化默认应用程序域的应用程序域管理器，并指定初始化属性。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[SetAppDomainManagerType 方法](iclrdomainmanager-setappdomainmanagertype-method.md)|指定从类派生的、 <xref:System.AppDomainManager?displayProperty=nameWithType> 将用于初始化默认应用程序域的应用程序域管理器的类型。|  
|[SetPropertiesForDefaultAppDomain 方法](iclrdomainmanager-setpropertiesfordefaultappdomain-method.md)|设置将用于初始化默认应用程序域的属性。|  
  
## <a name="remarks"></a>注解  

 若要获取此接口的实例，请使用管理器类型 IID 调用 [ICLRControl：： GetCLRManager](iclrcontrol-getclrmanager-method.md) 方法 `IID_ICLRDomainManager` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [承载接口](hosting-interfaces.md)
- [承载](index.md)
