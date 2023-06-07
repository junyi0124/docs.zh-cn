---
title: <mexTcpBinding>
ms.date: 03/30/2017
ms.assetid: 01baba8d-d784-4255-9ea2-7afff1482bf0
ms.openlocfilehash: 0d12f886eaee6283ee686209dfc129e397a8e1fe
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91204692"
---
# \<mexTcpBinding>

指定用于通过 TCP 进行的 WS-MetadataExchange (WS-MEX) 消息交换的绑定的设置。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<mexTcpBinding>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<mexTcpBinding>
  <binding closeTimeout="TimeSpan"
           name="string"
           openTimeout="TimeSpan"
           receiveTimeout="TimeSpan"
           sendTimeout="TimeSpan">
  </binding>
</mexTcpBinding>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`closeTimeout`|一个 <xref:System.TimeSpan> 值，指定为完成关闭操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
|`name`|一个包含绑定的配置名称的字符串。 因为此值用作绑定的标识，所以它应该是唯一的。 从 .NET Framework 4 开始，绑定和行为不需要具有名称。 有关默认配置和无值绑定和行为的详细信息，请参阅[WCF 服务的](../../../wcf/samples/simplified-configuration-for-wcf-services.md)[简化配置](../../../wcf/simplified-configuration.md)和简化配置。|  
|`openTimeout`|一个 <xref:System.TimeSpan> 值，指定为完成打开操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
|`receiveTimeout`|一个 <xref:System.TimeSpan> 值，指定为完成接收操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:10:00。|  
|`sendTimeout`|一个 <xref:System.TimeSpan> 值，指定为完成发送操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<bindings>](bindings.md)|此元素包含标准绑定和自定义绑定的集合。|  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.MexTcpBindingElement>
- <xref:System.ServiceModel.Description.MetadataExchangeBindings.CreateMexTcpBinding%2A>
- [如何：使用配置文件发布服务的元数据](../../../wcf/feature-details/how-to-publish-metadata-for-a-service-using-a-configuration-file.md)
- [通过自定义绑定发布和检索元数据](../../../wcf/extending/publishing-and-retrieving-metadata-over-a-custom-binding.md)
- Metadata 
- [绑定](../../../wcf/bindings.md)
- [配置系统提供的绑定](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [使用绑定配置服务和客户端](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
