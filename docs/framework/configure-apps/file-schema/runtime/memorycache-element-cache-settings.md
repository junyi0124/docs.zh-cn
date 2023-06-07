---
title: <memoryCache> 元素（缓存设置）
ms.date: 03/30/2017
helpviewer_keywords:
- <memoryCache> element
- caching [.NET Framework], configuration
- memoryCache element
ms.assetid: 182a622f-f7cf-472d-9d0b-451d2fd94525
ms.openlocfilehash: 14480682c5d221216df5da3844897855d1d92a0d
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91192420"
---
# <a name="memorycache-element-cache-settings"></a>\<memoryCache> 元素（缓存设置）

定义一个用于配置基于 <xref:System.Runtime.Caching.MemoryCache> 类的缓存的元素。 <xref:System.Runtime.Caching.Configuration.MemoryCacheElement> 类定义可以用于配置缓存的 [memoryCache](memorycache-element-cache-settings.md) 元素。 可以在单个应用程序中使用 <xref:System.Runtime.Caching.MemoryCache> 类的多个实例。 配置文件中的每个 `memoryCache` 元素可以包含一个命名 <xref:System.Runtime.Caching.MemoryCache> 实例的设置。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.runtime.caching>**](system-runtime-caching-element-cache-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<memoryCache>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<memoryCache>
    <namedCaches>  
        <!-- child elements -->  
    </namedCaches>
</memoryCache>  
```  
  
## <a name="type"></a>类型  

 <xref:System.Runtime.Caching.MemoryCache> 类。  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`CacheMemoryLimitMegabytes`|<xref:System.Runtime.Caching.MemoryCache> 对象的实例可以增长到的最大内存大小（以兆字节为单位）。 默认值为 0，这意味着默认情况下使用 <xref:System.Runtime.Caching.MemoryCache> 类的自动调整大小启发。|  
|`Name`|缓存配置的名称。|  
|`PhysicalMemoryLimitPercentage`|缓存可以使用的物理内存的百分比。 默认值为 0，这意味着默认情况下使用 <xref:System.Runtime.Caching.MemoryCache> 类的自动调整大小启发。|  
|`PollingInterval`|一个时间间隔的值，在该时间间隔之后，缓存实现会将当前内存负载与为缓存实例设置的基于绝对值和百分比的内存限制进行比较。 该值以“HH:MM:SS”格式输入。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<namedCaches>](namedcaches-element-cache-settings.md)|包含 `namedCache` 实例的配置设置的集合。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<configuration>](../configuration-element.md)|指定公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|  
|[\<system.runtime.caching>](system-runtime-caching-element-cache-settings.md)|包含使你可以在 .NET Framework 中内置的应用程序中实现输出缓存的类型。|  
  
## <a name="remarks"></a>备注  

 <xref:System.Runtime.Caching.MemoryCache> 类是抽象 <xref:System.Runtime.Caching.ObjectCache> 类的具体实现。 <xref:System.Runtime.Caching.MemoryCache> 类的实例可以随来自应用程序配置文件的配置信息一起提供。 [memoryCache](memorycache-element-cache-settings.md) 配置节包含 `namedCaches` 配置集合。  
  
 基于内存的缓存对象进行初始化时，它首先尝试查找与传递给内存缓存构造函数的参数中的名称进行匹配的 `namedCaches` 项。 如果找到 `namedCaches` 项，则从配置文件检索轮询和内存管理信息。  
  
 初始化过程随后使用构造函数中配置信息的名称/值对的可选集合来确定是否重写了任何配置项。 如果在名称/值对集合中传递以下值之一，则这些值会重写从配置文件获取的信息：  
  
- <xref:System.Runtime.Caching.Configuration.MemoryCacheElement.CacheMemoryLimitMegabytes%2A>  
  
- <xref:System.Runtime.Caching.Configuration.MemoryCacheElement.PhysicalMemoryLimitPercentage%2A>  
  
- <xref:System.Runtime.Caching.MemoryCache.PollingInterval%2A>  
  
## <a name="example"></a>示例  

 下面的示例演示如何 <xref:System.Runtime.Caching.MemoryCache> 通过将 `name` 属性设置为 "default"，将对象的名称设置为默认缓存对象名称。  
  
 将 `cacheMemoryLimitMegabytes` 属性和 `physicalMemoryLimitPercentage` 属性设置为零。 将这些特性设置为零意味着默认情况下使用 <xref:System.Runtime.Caching.MemoryCache> 自动调整大小试探法。 每隔两分钟，缓存实现应对当前内存负载和基于百分比的绝对内存限制进行比较。  
  
```xml  
<configuration>  
  <system.runtime.caching>  
    <memoryCache>  
      <namedCaches>  
          <add name="Default"
               cacheMemoryLimitMegabytes="0"
               physicalMemoryLimitPercentage="0"  
               pollingInterval="00:02:00" />  
      </namedCaches>  
    </memoryCache>  
  </system.runtime.caching>  
</configuration>  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Runtime.Caching.MemoryCache>
- [\<system.runtime.caching> 元素 (缓存设置) ](system-runtime-caching-element-cache-settings.md)
- [\<namedCaches> 元素 (缓存设置) ](namedcaches-element-cache-settings.md)
