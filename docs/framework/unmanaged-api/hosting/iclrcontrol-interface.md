---
title: ICLRControl 接口
ms.date: 03/30/2017
api_name:
- ICLRControl
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRControl
helpviewer_keywords:
- ICLRControl interface [.NET Framework hosting]
ms.assetid: a24fd905-1fa6-45a0-ad65-e9e2ee58861e
topic_type:
- apiref
ms.openlocfilehash: acf449014f5bf5683d5488f8ed2ead963676fe6b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95728324"
---
# <a name="iclrcontrol-interface"></a>ICLRControl 接口

提供一些方法，这些方法允许宿主获取公共语言运行时 (CLR) 的引用并配置这些方面。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetCLRManager 方法](iclrcontrol-getclrmanager-method.md)|获取一个接口指针，该指针指向主机可用于配置 CLR 的任何管理器类型的实例。|  
|[SetAppDomainManagerType 方法](iclrcontrol-setappdomainmanagertype-method.md)|将派生自的类型设置 <xref:System.AppDomainManager> 为应用程序域管理器的类型。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRAssemblyIdentityManager 接口](iclrassemblyidentitymanager-interface.md)
- [ICLRDebugManager 接口](iclrdebugmanager-interface.md)
- [ICLRGCManager 接口](iclrgcmanager-interface.md)
- [ICLRHostBindingPolicyManager 接口](iclrhostbindingpolicymanager-interface.md)
- [ICLRHostProtectionManager 接口](iclrhostprotectionmanager-interface.md)
- [ICLROnEventManager 接口](iclroneventmanager-interface.md)
- [ICLRPolicyManager 接口](iclrpolicymanager-interface.md)
- [IHostControl 接口](ihostcontrol-interface.md)
- [承载接口](hosting-interfaces.md)
