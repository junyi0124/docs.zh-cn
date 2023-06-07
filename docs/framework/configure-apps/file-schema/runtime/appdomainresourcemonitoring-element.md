---
title: <appDomainResourceMonitoring> 元素
ms.date: 03/30/2017
helpviewer_keywords:
- appDomainResourceMonitoring element
- <appDomainResourceMonitoring> element
ms.assetid: 02119ab6-1e91-448e-97ad-e7b2e5c4bbbd
ms.openlocfilehash: 9ecf2e382b5d483377df871835793219b3f74760
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91170267"
---
# <a name="appdomainresourcemonitoring-element"></a>\<appDomainResourceMonitoring> 元素

指示运行时在过程的生命周期过程中收集所有应用程序域的统计数据。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<appDomainResourceMonitoring>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<appDomainResourceMonitoring
   enabled="true|false"/>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`enabled`|必需的特性。<br /><br /> 指定运行时是否收集应用程序域资源监视的统计信息。|  
  
## <a name="enabled-attribute"></a>enabled 特性  
  
|值|描述|  
|-----------|-----------------|  
|`true`|收集应用程序域资源监视的统计信息。|  
|`false`|不收集应用程序域资源监视的统计信息。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|`configuration`|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|  
|`runtime`|包含有关程序集绑定和垃圾回收的信息。|  
  
## <a name="remarks"></a>备注  

 应用程序域资源监视可通过托管应用程序域类、托管 [ICLRAppDomainResourceMonitor](../../../unmanaged-api/hosting/iclrappdomainresourcemonitor-interface.md) 接口和 WINDOWS (ETW) 的事件跟踪获得。 启用监视后，会在进程的生存期内收集进程中所有应用程序域的统计信息。  
  
 若要从托管代码启用监视，请使用 <xref:System.AppDomain.MonitoringIsEnabled%2A> 属性。  
  
 此配置元素仅在 .NET Framework 4 及更高版本中可用。  
  
## <a name="example"></a>示例  

 下面的示例演示如何启用应用程序域资源监视。  
  
```xml  
<configuration>  
   <runtime>  
      <appDomainResourceMonitoring enabled="true"/>  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.AppDomain.MonitoringIsEnabled%2A?displayProperty=nameWithType>
- [运行时设置架构](index.md)
- [配置文件架构](../index.md)
