---
title: <settings> 元素（网络设置）
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#settings
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.net/settings
helpviewer_keywords:
- settings element
- <settings> element
ms.assetid: 189ce989-c39b-427d-b004-6b82a668b931
ms.openlocfilehash: c5fe0b9eccd1c429c0041fcfab06b0cc20a20aa2
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91167270"
---
# <a name="settings-element-network-settings"></a>\<settings> 元素（网络设置）

配置 <xref:System.Net?displayProperty=nameWithType> 命名空间的基本网络选项。  

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.net>**](system-net-element-network-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<settings>**

## <a name="syntax"></a>语法  
  
```xml  
<settings>  
  <httpListener>...</httpListener>  
  <httpWebRequest>...</httpWebRequest>  
  <ipv6>...</ipv6>  
  <performanceCounters>...</performanceCounters>  
  <servicePointManager>...</servicePointManager>  
  <socket>...</socket>  
  <webProxyScript>...</webProxyScript>  
</settings>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  

 无。  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[httpListener](httplistener-element-network-settings.md)|自定义类使用的参数 <xref:System.Net.HttpListener> 。|  
|[httpWebRequest](httpwebrequest-element-network-settings.md)|自定义 Web 请求参数。|  
|[ipv6](ipv6-element-network-settings.md)|启用 Internet 协议版本 6 (IPv6) 支持。|  
|[\<performanceCounter> 元素（网络设置）](performancecounter-element-network-settings.md)|启用网络性能计数器。|  
|[servicePointManager](servicepointmanager-element-network-settings.md)|配置与网络资源的连接。|  
|[片](socket-element-network-settings.md)|指定套接字操作是否使用完成端口。|  
|[\<webProxyScript> 元素（网络设置）](webproxyscript-element-network-settings.md)|配置用于发现 Web 代理的脚本的特征。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[system.net](system-net-element-network-settings.md)|包含指定 .NET Framework 如何连接到网络的设置。|  
  
## <a name="remarks"></a>备注  
  
## <a name="configuration-files"></a>配置文件  

 此元素可在应用程序配置文件或计算机配置文件 (Machine.config) 中使用。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Net?displayProperty=nameWithType>
- [网络设置架构](index.md)
