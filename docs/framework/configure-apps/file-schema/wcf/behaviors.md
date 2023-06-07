---
title: <behaviors>
ms.date: 03/30/2017
ms.assetid: 0e5da4e6-1aa5-466c-924e-f10efee57f0b
ms.openlocfilehash: 914fa04c9aff0c287913104cd9bedc570c473330
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91201481"
---
# \<behaviors>

此元素定义名为 `endpointBehaviors` 和 `serviceBehaviors` 的两个子集合。  每个集合分别定义终结点和服务所使用的行为元素。 每个行为元素由其唯一的 `name` 属性标识。 从 .NET Framework 4 开始，绑定和行为不需要具有名称。 有关默认配置和无值绑定和行为的详细信息，请参阅[WCF 服务的](../../../wcf/samples/simplified-configuration-for-wcf-services.md)[简化配置](../../../wcf/simplified-configuration.md)和简化配置。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<behaviors>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<behaviors>
  <serviceBehaviors>
  </serviceBehaviors>
  <endpointBehaviors>
  </endpointBehaviors>
</behaviors>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  

 无  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<endpointBehaviors>](endpointbehaviors.md)|此配置节描述为特定终结点定义的所有行为。|  
|[\<serviceBehaviors>](servicebehaviors.md)|此配置节描述为特定服务定义的所有行为。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<system.serviceModel>](system-servicemodel.md)|所有 Windows Communication Foundation (WCF) 配置元素的根元素。|  
  
## <a name="remarks"></a>备注  

 使用 `<remove>` 元素可以从集合中移除特定行为。 为此，您只需在 `name` 元素的 `<remove>` 特性中提供要移除的行为的名称。  还可以使用 `<clear>` 元素清除集合中的所有内容，来确保行为集合最初为空。  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.BehaviorsSection>
- <xref:System.ServiceModel.Configuration.EndpointBehaviorElementCollection>
- <xref:System.ServiceModel.Configuration.EndpointBehaviorElement>
- <xref:System.ServiceModel.Configuration.ServiceBehaviorElementCollection>
- <xref:System.ServiceModel.Configuration.ServiceBehaviorElement>
- [使用行为配置和扩展运行时](../../../wcf/extending/configuring-and-extending-the-runtime-with-behaviors.md)
- [配置客户端行为](../../../wcf/configuring-client-behaviors.md)
- [指定客户端运行时行为](../../../wcf/specifying-client-run-time-behavior.md)
- [指定服务运行时行为](../../../wcf/specifying-service-run-time-behavior.md)
- [安全行为](../../../wcf/feature-details/security-behaviors-in-wcf.md)
