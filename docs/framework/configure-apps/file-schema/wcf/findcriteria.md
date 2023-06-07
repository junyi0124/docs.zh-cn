---
title: <findCriteria>
ms.date: 03/30/2017
ms.assetid: 5454cd19-6bf5-4ba8-94d1-f58d10dc1917
ms.openlocfilehash: ce2b1fdd85e0454f901bac393e2f44ae0c6da43f
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91178142"
---
# \<findCriteria>

一个配置元素，提供客户端应用程序用于搜索发现服务的一组条件。 可将这些条件划分为搜索条件（指定要查找的服务）和查找终止条件（搜索应持续的时长）。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<standardEndpoints>**](standardendpoints.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<dynamicEndpoint>**](dynamicendpoint.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<standardEndpoint>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<discoveryClientSettings>**](discoveryclientsettings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<findCriteria>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<system.serviceModel>
  <standardEndpoints>
    <dynamicEndpoint>
      <standardEndpoint>
        <discoveryClientSettings discoveryEndpoint="String">
          <findCriteria duration="TimeSpan"
                        maxResults="Integer"
                        scopeMatchBy="Uri">
            <contractTypeNames>
              <add name="String"
                   namespace="String" />
            </contractTypeNames>
            <extensions />
            <scopes>
              <add scope="URI" />
            </scopes>
          </findCriteria>
        </discoveryClientSettings>
      </standardEndpoint>
    </dynamicEndpoint>
  </standardEndpoints>
</system.serviceModel>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|duration|一个 Timespan 值，指定等待网络上的服务所发送的答复的最长时间。 默认持续时间为 20 秒。|  
|maxResults|一个整数，指定等待网络或 Internet 上的服务所发送的最大答复数。 如果在经过 `duration` 特性指定的值之前收到的答复达到了最大数目，则查找操作将结束。|  
|scopeMatchBy|一个 URI，指定当对 Probe 消息中的范围和终结点中的范围进行匹配时采用的匹配算法。<br /><br /> 支持五种范围匹配规则。 如果未指定范围匹配规则，则使用 `ScopeMatchByPrefix`。 有关这方面的更多信息，请参见 <xref:System.ServiceModel.Discovery.FindCriteria>。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<contractTypeNames>](contracttypenames.md)|一个配置元素的集合，这些元素包含工作流服务协定类型的名称。|  
|\<extensions> 的 \<findCriteria>|一个 XML 元素对象集合，这些对象提供扩展。|  
|[\<scopes>](scopes.md)|一个对象集合，这些对象包含在查找操作过程中用于查找一个或多个特定服务的绝对 URI。<br /><br /> 如果找到特定服务，则表示已在服务 URI 和范围 URI 之间进行了成功的匹配，此匹配操作有时是借助处理匹配复杂性的范围规则完成的。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<standardEndpoints>](standardendpoints.md)|包含应用程序以客户端形式参与服务发现过程所需的设置。|  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Discovery.FindCriteria>
- <xref:System.ServiceModel.Discovery.Configuration.FindCriteriaElement>
