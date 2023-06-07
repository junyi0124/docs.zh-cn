---
title: <behavior> 的 <serviceBehaviors>
ms.date: 03/30/2017
ms.assetid: 78fc0a08-55de-416a-ac12-a5e6ffc9a987
ms.openlocfilehash: 739f95f527fd73062c8cec43efc6777efeb077f3
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91195150"
---
# <a name="behavior-of-servicebehaviors"></a>\<behavior> 的 \<serviceBehaviors>

`behavior` 元素包含服务行为的设置集合。 每个行为都按其 `name` 进行索引。 服务可以使用元素的特性通过此名称链接到每个行为 `behaviorConfiguration` [\<endpoint>](endpoint-element.md) 。 这样，终结点可以共享公共行为配置而不用重新定义设置。 从 .NET Framework 4 开始，绑定和行为不需要具有名称。 有关默认配置和无值绑定和行为的详细信息，请参阅[WCF 服务的](../../../wcf/samples/simplified-configuration-for-wcf-services.md)[简化配置](../../../wcf/simplified-configuration.md)和简化配置。  
  
> [!NOTE]
> 特定于 Windows 工作流活动的行为元素（例如 [\<sendMessageChannelCache>](../windows-workflow-foundation/sendmessagechannelcache.md) 元素）记录在页的[ \<behavior> \<serviceBehaviors> ](../windows-workflow-foundation/behavior-of-servicebehaviors-of-workflow.md)中。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<behavior>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<system.ServiceModel>
  <behaviors>
    <serviceBehaviors>
       <behavior name="String" />
    </serviceBehaviors>
  </behaviors>
</system.ServiceModel>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|说明|  
|---------------|-----------------|  
|name|一个包含行为的配置名称的唯一字符串。 此值是用户定义的一个字符串，该字符串必须是唯一的，因为它将充当元素的标识字符串。 从 .NET Framework 4 开始，绑定和行为不需要具有名称。 有关默认配置和无值绑定和行为的详细信息，请参阅[WCF 服务的](../../../wcf/samples/simplified-configuration-for-wcf-services.md)[简化配置](../../../wcf/simplified-configuration.md)和简化配置。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<dataContractSerializer>](datacontractserializer-element.md)|包含 DataContractSerializer 的配置数据。|  
|[\<persistenceProvider>](persistenceprovider.md)|指定要使用的持久性提供程序实现的类型以及用于持久性操作的超时值。|  
|[\<routing>](routing-of-servicebehavior.md)|提供对路由服务的运行时访问以允许对路由配置进行动态修改。|  
|[\<serviceAuthenticationManager>](serviceauthenticationmanager.md)|提供一个工作流配置元素，该元素在服务级别建立传输、消息或发起方的有效性。|  
|[\<serviceAuthorization>](serviceauthorization-element.md)|指定用于授予服务操作访问权限的设置。|  
|[\<serviceCredentials>](servicecredentials.md)|指定要用于对服务进行身份验证的凭据以及与客户端凭据验证相关的设置。|  
|[\<serviceDebug>](servicedebug.md)|指定 Windows Communication Foundation (WCF) 服务的调试和帮助信息功能。|  
|[\<serviceDiscovery>](servicediscovery.md)|指定服务终结点的可发现性。|  
|[\<serviceMetadata>](servicemetadata.md)|指定服务元数据的发布和相关信息。|  
|[\<serviceSecurityAudit>](servicesecurityaudit.md)|指定用于在服务操作过程中启用安全事件审核的设置。|  
|[\<serviceThrottling>](servicethrottling.md)|指定 WCF 服务的限制机制。|  
|[\<serviceTimeouts>](servicetimeouts.md)|指定服务的超时。|  
|[\<workflowRuntime>](workflowruntime.md)|指定用于承载基于工作流的 WCF 服务的 WorkflowRuntime 实例的设置。|  
|[\<useRequestHeadersForMetadataAddress>](userequestheadersformetadataaddress.md)|允许从请求消息头中检索元数据地址信息。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<serviceBehaviors>](servicebehaviors.md)|服务行为元素的集合。|
