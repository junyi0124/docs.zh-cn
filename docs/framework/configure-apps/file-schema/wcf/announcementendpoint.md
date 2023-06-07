---
title: <announcementEndpoint>
ms.date: 03/30/2017
ms.assetid: 034b7c69-a770-4502-8cef-38007bbcd025
ms.openlocfilehash: f68972cdf0e55f92fd4856aff912f00db7c62be4
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91201611"
---
# \<announcementEndpoint>

此配置元素定义具有固定公告协定的标准终结点。 当分别打开或关闭服务时，服务可以选择通过发送一条联机和脱机公告消息来公告其可用性。 Windows Communication Foundation (WCF) 服务指定元素中的公告终结点 [\<serviceDiscovery>](servicediscovery.md) ，并使用 AnnouncementClient 来执行公告。 希望侦听来自其他服务的公告的客户端实际充当 WCF 服务;因此，您必须在部分中配置该客户端的公告终结点 [\<services>](services.md) 。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<standardEndpoints>**](standardendpoints.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<announcementEndpoint>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<system.serviceModel>
  <standardEndpoints>
    <announcementEndpoint>
      <standardEndpoint discoveryVersion="WSDiscovery11/WSDiscoveryApril2005"
                        maxAnnouncementDelay="Timespan"
                        name="String" />
    </announcementEndpoint>
  </standardEndpoints>
</system.serviceModel>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|discoveryVersion|一个字符串，指定两个 WS-Discovery 协议版本中的其中一个版本。 有效值为 WSDiscovery11 和 WSDiscoveryApril2005。 此值的类型为 <xref:System.ServiceModel.Discovery.Configuration.AnnouncementEndpointElement.DiscoveryVersion>。|  
|maxAnnouncementDelay|一个 Timespan 值，指定 Discovery 协议在发送 Hello 消息之前等待的最大延迟值。 消息在发送之前将等待一个随机时间值（介于 0 到此特性值之间）。 此特性用于设置随机的短时间延迟，以防止在网络出现故障后所有服务同时重新联机所造成的网络风暴。|  
|name|一个字符串，指定标准终结点的配置的名称。 此名称在服务终结点的 `endpointConfiguration` 特性中用于将标准终结点链接到其配置。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<standardEndpoints>](standardendpoints.md)|具有一个或多个固定属性（地址、绑定和协定）的预定义终结点的标准终结点集合。|  
  
## <a name="example"></a>示例  

 下面的示例演示通过 http 和对等网络侦听公告消息的客户端。  
  
```xml  
<services>
  <service name="ServiceAnnouncementListener">
    <endpoint name="httpAnnouncementEndpoint"
              kind="announcementEndpoint"
              binding="basicHttpBinding"
              address="announcements" />
    <endpoint name="peerNetAnnouncementEndpoint"
              kind="announcementEndpoint"
              binding="peerTcpBinding"
              address="net.p2p://discoveryMesh/multicast"
              bindingConfiguration="discoveryPeerTcpBindingConfig" />
  ...
  </service>
</services>

<standardEndpoints>
  <announcementEndpoint>
    <standardEndpoint name="httpAnnouncementEndpoint"
                      version="WSDiscoveryApril2005" />
    <standardEndpoint name="peerNetAnnouncementEndpoint"
                      version="WSDiscoveryApril2005" />
  </announcementEndpoint>
</standardEndpoints>
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Discovery.AnnouncementEndpoint>
