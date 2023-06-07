---
title: <namedCaches> 元素（缓存设置）
ms.date: 03/30/2017
helpviewer_keywords:
- namedCaches element
- caching [.NET Framework], configuration
- <namedCaches> element
ms.assetid: 6bd4fbc5-55a6-4dc4-998b-cdcc7e023330
ms.openlocfilehash: ad76c01bba859934be399d73262bd974309efe98
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91192394"
---
# <a name="namedcaches-element-cache-settings"></a>\<namedCaches> 元素（缓存设置）

指定命名实例的配置设置的集合 <xref:System.Runtime.Caching.MemoryCache> 。 <xref:System.Runtime.Caching.Configuration.MemoryCacheSection.NamedCaches%2A>属性从配置文件的一个或多个元素中引用配置设置的集合 `namedCaches` 。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.runtime.caching>**](system-runtime-caching-element-cache-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<memoryCache>**](memorycache-element-cache-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<namedCaches>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<namedCaches>  
  <add name="default"/>
</namedCaches>  
```  
  
## <a name="type"></a>类型  

 `None`  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`cacheMemoryLimitMegabytes`|一个整数值，指定的实例可以增长到的最大允许大小（以 mb <xref:System.Runtime.Caching.MemoryCache> 为单位）。 默认值为0，这意味着 <xref:System.Runtime.Caching.MemoryCache> 默认情况下使用类的自动调整试探法。|  
|`name`|缓存的名称。|  
|`physicalMemoryLimitPercentage`|一个介于0到100之间的整数值，用于指定缓存可使用的实际安装计算机内存的最大百分比。 默认值为0，这意味着 <xref:System.Runtime.Caching.MemoryCache> 默认情况下使用类的自动调整试探法。|  
|`pollingInterval`|一个时间间隔的值，在该时间间隔之后，缓存实现会将当前内存负载与为缓存实例设置的基于绝对值和百分比的内存限制进行比较。 此值以 "HH： MM： SS" 格式输入。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<add>](add-element-for-namedcaches.md)|向内存缓存的 `namedCaches` 集合添加一个命名的缓存。|  
|[\<clear>](clear-element-for-namedcaches.md)|清除内存缓存的 `namedCaches` 集合。|  
|[\<remove>](remove-element-for-namedcaches.md)|从内存缓存的 `namedCaches` 集合中删除一个命名的缓存条目。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<configuration>](../configuration-element.md)|指定公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|  
|[\<memoryCache>](memorycache-element-cache-settings.md)|定义一个用于配置基于 <xref:System.Runtime.Caching.MemoryCache> 类的缓存的元素。|  
|[\<system.runtime.caching>](system-runtime-caching-element-cache-settings.md)|包含使你可以在 .NET Framework 中内置的应用程序中实现输出缓存的类型。|  
  
## <a name="remarks"></a>备注  

 Web.config 文件的内存缓存配置部分可以包含 `add` `remove` 该集合的、和 `clear` 属性 `namedCaches` 。 每个 `namedCaches` 项都由特性唯一标识 `name` 。  
  
 可以通过引用应用程序配置文件中的信息来检索内存缓存项的实例。 默认情况下，只有默认的缓存实例在配置文件中有一个条目。 默认缓存实例是从属性返回的实例 <xref:System.Runtime.Caching.MemoryCache.Default%2A> 。  
  
 如果将 name 属性设置为 "default"，则元素将使用默认内存缓存实例。  
  
## <a name="example"></a>示例  

 下面的示例演示如何通过将 `name` 属性设置为 "default"，将缓存的名称设置为默认缓存项名称。  
  
 将 `cacheMemoryLimitMegabytes` 属性和 `physicalMemoryPercentage` 属性设置为零。 将这些特性设置为零意味着使用类的自动调整试探法 <xref:System.Runtime.Caching.MemoryCache> 。 缓存实现将当前内存负载与基于百分比的绝对内存和每两分钟的内存限制进行比较。  
  
```xml  
<configuration>  
  
  <system.runtime.caching>  
    <memoryCache>  
      <namedCaches>  
          <add name="default"
               cacheMemoryLimitMegabytes="0"
               physicalMemoryLimitPercentage="0"  
               pollingInterval="00:02:00" />  
      </namedCaches>  
    </memoryCache>  
  </system.runtime.caching>  
  
</configuration>  
```  
  
## <a name="see-also"></a>请参阅

- [\<memoryCache> 元素 (缓存设置) ](memorycache-element-cache-settings.md)
