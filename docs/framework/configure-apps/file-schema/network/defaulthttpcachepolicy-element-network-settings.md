---
title: <defaultHttpCachePolicy> 元素（网络设置）
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.net/requestCaching/defaultHttpCachePolicy
- http://schemas.microsoft.com/.NetConfiguration/v2.0#defaultHttpCachePolicy
helpviewer_keywords:
- defaultHttpCachePolicy element
- <defaultHttpCachePolicy> element
ms.assetid: 2c1247d0-39b0-4c12-919a-a925ce075c79
ms.openlocfilehash: 4120c57fbb65da1c124414cbe9cfba7ae64388f8
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91190314"
---
# <a name="defaulthttpcachepolicy-element-network-settings"></a>\<defaultHttpCachePolicy> 元素（网络设置）

描述 HTTP 缓存是否处于活动状态，并描述默认缓存策略。  

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.net>**](system-net-element-network-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<requestCaching>**](requestcaching-element-network-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<defaultHttpCachePolicy>**

## <a name="syntax"></a>语法  
  
```xml  
<defaultHttpCachePolicy  
  policyLevel="BypassCache|Default"  
  minimumFresh="d.hh:mm:ss|minValue|maxValue"  
  maximumAge="d.hh:mm:ss|minValue|maxValue"  
  maximumStale="d.hh:mm:ss|minValue|maxValue"  
/>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`maximumAge`|指定将缓存的对象标记为过期之前的最大时间间隔。|  
|`maximumStale`|指定在将缓存对象标记为过期之前计算的新鲜度时间之后的最长时间。|  
|`minimumFresh`|指定将缓存的对象视为最新的最短时间。|  
|`policyLevel`|指定缓存策略是否为自动，或是否绕过缓存。 默认值是 `BypassCache`。|  
  
### <a name="child-elements"></a>子元素  

 无  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[requestCaching](requestcaching-element-network-settings.md)|控制网络请求的缓存机制。|  
  
## <a name="remarks"></a>备注  

 特性的值 `policyLevel` 为 `BypassCache` 或 `Default` 。  
  
 `maximumAge`、 `maximumStale` 和元素的值 `minimumFresh` 是格式为*d*的显式时间间隔。*hh*：*mm*：*ss* (天、小时、分钟和秒) ，或相应的常量 `minValue` 或 `maxValue` 。  
  
## <a name="configuration-files"></a>配置文件  

 此元素可在应用程序配置文件或计算机配置文件 (Machine.config) 中使用。  
  
## <a name="example"></a>示例  

 下面的示例演示如何指定最小的最短时间为6小时，最长生存期为两天，最大陈旧时间为四小时。  
  
```xml  
<configuration>  
  <system.net>  
    <requestCaching>  
      <defaultHttpCachePolicy  
        minimumFresh="0.06:00:00"  
        maximumAge="2.00:00:00"  
        maximumStale="0.04:00:00"
      />  
    </requestCaching>  
  </system.net>  
</configuration>  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Net.Cache>
- <xref:System.Net.WebRequest>
- <xref:System.Net.Cache.RequestCacheLevel>
- [网络设置架构](index.md)
