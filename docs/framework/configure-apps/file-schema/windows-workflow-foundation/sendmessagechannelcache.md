---
title: <sendMessageChannelCache>
ms.date: 03/30/2017
ms.topic: reference
ms.assetid: 241e428e-5030-4b13-8a0a-69f05288d3d9
ms.openlocfilehash: c1775ddabffda58c7529a64b89c9c53ff3da7b99
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91164917"
---
# \<sendMessageChannelCache>

一种服务行为，该行为支持缓存共享级别的自定义、通道工厂缓存的设置，以及使用 Send 消息传递活动将消息发送给服务终结点的工作流通道缓存的设置。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.ServiceModel>**](system-servicemodel-of-workflow.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors-of-workflow.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors-of-workflow.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-servicebehaviors-of-workflow.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<sendMessageChannelCache>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<behaviors>
  <serviceBehaviors>
    <behavior name="String">
      <sendMessageChannelCache allowUnsafeCaching="Boolean">
        <channelSettings idleTimeout="TimeSpan"
                         leaseTimeout="TimeSpan"
                         maxItemsInCache="Integer" />
        <factorySettings idleTimeout="TimeSpan"
                         leaseTimeout="TimeSpan"
                         maxItemsInCache="Integer" />
      </sendMessageChannelCache>
    </behavior>
  </serviceBehaviors>
</behaviors>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|allowUnsafeCaching|一个布尔值，该值指示是否启用缓存。 如果工作流服务具有自定义绑定或自定义行为，缓存会变得不安全，因此默认情况下禁用缓存。 但是，如果要启用缓存，请将此属性设置为 **true**。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<channelSettings>](channelsettings.md)|指定通道缓存的设置。|  
|[\<factorySettings>](factorysettings.md)|指定通道工厂缓存的设置。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<serviceBehaviors> 的 \<behavior>](behavior-of-servicebehaviors-of-workflow.md)|指定行为元素。|  
  
## <a name="remarks"></a>备注  

 此服务行为适用于将消息发送给服务终结点的工作流。 这些工作流通常是客户端工作流，但也可以是在 <xref:System.ServiceModel.WorkflowServiceHost> 中承载的工作流服务。  
  
 默认情况下，在 <xref:System.ServiceModel.WorkflowServiceHost> 承载的工作流中，由 <xref:System.ServiceModel.Activities.Send> 消息传递活动使用的缓存可在 <xref:System.ServiceModel.WorkflowServiceHost> 中的所有工作流实例中共享（主机级缓存）。 对于未由 <xref:System.ServiceModel.WorkflowServiceHost> 承载的客户端工作流，缓存仅对该工作流实例可用（实例级缓存）。 对于已在配置中定义了终结点的工作流中的所有 Send 活动，默认情况下为禁用缓存。  
  
 有关如何更改通道工厂和通道缓存的默认缓存共享级别和缓存设置的详细信息，请参阅 [更改发送活动的缓存共享级别](../../../wcf/feature-details/changing-the-cache-sharing-levels-for-send-activities.md)。  
  
## <a name="example"></a>示例  

 在承载的工作流服务中，可以在应用程序配置文件中指定工厂缓存和通道缓存设置。 为此，应添加一个包含工厂和通道缓存的缓存设置的服务行为，并将此服务行为添加到您的服务中。 下面的示例显示了配置文件的内容，其中包含 `MyChannelCacheBehavior`  具有自定义工厂缓存和通道缓存设置的服务行为。 此服务行为通过属性添加到服务 `behaviorConfiguration` 。  
  
```xml  
<configuration>
  <system.serviceModel>  
    <!-- List of other config sections here -->
    <behaviors>  
      <serviceBehaviors>  
        <behavior name="MyChannelCacheBehavior">  
          <sendMessageChannelCache allowUnsafeCaching ="false" >  
            <!-- Control only the host level settings -->
            <factorySettings maxItemsInCache = "8" idleTimeout = "00:05:00" leaseTimeout="10:00:00" />  
            <channelSettings maxItemsInCache = "32" idleTimeout = "00:05:00" leaseTimeout="00:06:00" />  
          </sendMessageChannelCache>  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
    <services>  
      <service name="MyService" behaviorConfiguration="MyChannelCacheBehavior" />  
    </services>  
  </system.serviceModel>  
</configuration>  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Activities.SendMessageChannelCache>
- <xref:System.ServiceModel.Activities.Configuration.SendMessageChannelCacheElement>
- <xref:System.ServiceModel.Activities.Send>
- [更改发送活动的缓存共享级别](../../../wcf/feature-details/changing-the-cache-sharing-levels-for-send-activities.md)
