---
title: ICLRAppDomainResourceMonitor 接口
ms.date: 03/30/2017
api_name:
- ICLRAppDomainResourceMonitor
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRAppDomainResourceMonitor
helpviewer_keywords:
- ICLRAppDomainResourceMonitor interface [.NET Framework hosting]
ms.assetid: 72fa83a1-8997-41d7-b355-ab177a24a303
topic_type:
- apiref
ms.openlocfilehash: 84c53f0666d0e04b898e28c1d8e146eab566ca1b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95674692"
---
# <a name="iclrappdomainresourcemonitor-interface"></a>ICLRAppDomainResourceMonitor 接口

提供检查应用程序域的内存和 CPU 使用情况的方法。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetCurrentAllocated 方法](iclrappdomainresourcemonitor-getcurrentallocated-method.md)|获取应用程序域创建后所做的所有内存分配的总大小（以字节为单位），而不会减去已进行垃圾回收的内存。|  
|[GetCurrentSurvived 方法](iclrappdomainresourcemonitor-getcurrentsurvived-method.md)|获取当前应用程序域引用的最后一个完整的、阻止的垃圾回收和所引用的字节数。|  
|[GetCurrentCpuTime 方法](iclrappdomainresourcemonitor-getcurrentcputime-method.md)|获取自应用程序域创建以来在当前应用程序域中执行时所有线程已使用的总处理器时间。|  
  
## <a name="remarks"></a>注解  

 `ICLRAppDomainResourceMonitor`接口提供类似于以下托管属性的功能：  
  
- <xref:System.AppDomain.MonitoringIsEnabled%2A?displayProperty=nameWithType>  
  
- <xref:System.AppDomain.MonitoringTotalProcessorTime%2A?displayProperty=nameWithType>  
  
- <xref:System.AppDomain.MonitoringTotalAllocatedMemorySize%2A?displayProperty=nameWithType>  
  
- <xref:System.AppDomain.MonitoringSurvivedProcessMemorySize%2A?displayProperty=nameWithType>  
  
- <xref:System.AppDomain.MonitoringSurvivedMemorySize%2A?displayProperty=nameWithType>  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>请参阅

- [\<appDomainResourceMonitoring> 元素](../../configure-apps/file-schema/runtime/appdomainresourcemonitoring-element.md)
- [应用程序域资源监视](../../../standard/garbage-collection/app-domain-resource-monitoring.md)
- [承载接口](hosting-interfaces.md)
- [承载](index.md)
