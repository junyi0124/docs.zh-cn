---
title: <standardEndpoints>
ms.date: 03/30/2017
ms.assetid: d62153d7-a6e6-462a-a784-cca61e9c2ba1
ms.openlocfilehash: 5d0607b5c541fc5ae402a50a0f8c464355b6b246
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91157195"
---
# \<standardEndpoints>

此配置节用于定义一个标准终结点集合，这些终结点是预配置的可重用终结点。 标准终结点具有一个或多个设置为固定值的地址、绑定和协定特性。 例如，发现终结点具有固定的协定。 此外，还可以使用标准终结点用新属性扩展服务终结点，这与定义自定义绑定相似。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<standardEndpoints>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<system.serviceModel>
  <standardEndpoints>
  </standardEndpoints>
</system.serviceModel>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  

 无。  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<announcementEndpoint>](announcementendpoint.md)|定义具有固定公告协定的标准终结点。 当分别打开或关闭服务时，服务可以选择通过发送一条联机和脱机公告消息来公告其可用性。 Windows Communication Foundation (WCF) 服务指定元素中的公告终结点 [\<serviceDiscovery>](servicediscovery.md) ，并使用 AnnouncementClient 来执行公告。 希望侦听来自其他服务的公告的客户端实际充当 WCF 服务;因此，您必须在部分中配置该客户端的公告终结点 [\<services>](services.md) 。|  
|[\<discoveryEndpoint>](discoveryendpoint.md)|定义具有固定发现协定的标准终结点。 将此元素添加到服务配置后，该元素将指定侦听发现消息的位置。 将此元素添加到客户端配置后，该元素将指定发送发现查询的位置。|  
|[\<dynamicEndpoint>](dynamicendpoint.md)|此配置元素定义一个标准终结点，应用程序通过利用该终结点包含的信息，能够充当可在运行时动态查找终结点地址的客户端程序。|  
|[\<mexEndpoint>](mexendpoint.md)|定义具有固定 IMetadataExchange 协定的标准终结点。 由于所有元数据交换终结点都指定 IMetadataExchange 作为其协定，因此可以使用此标准终结点，而不必定义您自己的终结点。|  
|[\<udpAnnouncementEndpoint>](udpannouncementendpoint.md)|定义由服务用于通过 UDP 绑定发送公告消息的标准终结点。 它具有固定协定并支持两个发现版本。 此外，根据 WS-Discovery 规范（WS-Discovery 2005 年 4 月版或 WS-Discovery 1.1 版）中的规定，它还具有固定 UDP 绑定和默认地址值。 您可以指定用于发送和接收公告消息的多播地址。|  
|[\<udpDiscoveryEndpoint>](udpdiscoveryendpoint.md)|定义通过 UDP 多播绑定为发现操作预先配置的标准终结点。 此终结点具有固定协定并支持两个 WS-Discovery 协议版本。 此外，根据 WS-Discovery 规范（WS-Discovery 2005 年 4 月版或 WS-Discovery 1.1 版）中的规定，它还具有固定 UDP 绑定和默认地址。|  
|[\<webHttpEndpoint>](webhttpendpoint.md)|定义具有 [\<webHttpBinding>](webhttpbinding.md) 自动添加行为的固定绑定的标准终结点 [\<webHttp>](webhttp.md) 。 在编写 REST 服务时，请使用此终结点。|  
|[\<webScriptEndpoint>](webscriptendpoint.md)|定义具有 [\<webHttpBinding>](webhttpbinding.md) 自动添加行为的固定绑定的标准终结点 [\<enableWebScript>](enablewebscript.md) 。 在编写从 ASP.NET AJAX 应用程序中调用的服务时，请使用此终结点。|  
|[\<workflowControlEndpoint>](workflowcontrolendpoint.md)|定义用于控制工作流实例的执行（创建、运行、挂起、终止等）的标准终结点。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|\<system.ServiceModel>|所有 WCF 配置元素的根元素。|  
  
## <a name="see-also"></a>请参阅

- [标准终结点](../../../wcf/feature-details/standard-endpoints.md)
