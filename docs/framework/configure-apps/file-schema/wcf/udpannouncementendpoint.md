---
title: <udpAnnouncementEndpoint>
ms.date: 03/30/2017
ms.assetid: 5b3fa9c5-f372-4df9-a9d6-1e426063b721
ms.openlocfilehash: 67503b1bc3c6282ff5018adc20acbb89de49ba50
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91173752"
---
# \<udpAnnouncementEndpoint>

此配置元素定义服务通过 UDP 绑定发送公告消息所使用的标准终结点。 它具有固定协定并支持两个发现版本。 此外，根据 WS-Discovery 规范（WS-Discovery 2005 年 4 月版或 WS-Discovery 1.1 版）中的规定，它还具有固定 UDP 绑定和默认地址值。 您可以指定用于发送和接收公告消息的多播地址。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<standardEndpoints>**](standardendpoints.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<udpAnnouncementEndpoint>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<system.serviceModel>
  <standardEndpoints>
    <announcementEndpoint>
      <standardEndpoint discoveryVersion="WSDiscovery11/WSDiscoveryApril2005"
                        maxAnnouncementDelay="Timespan"
                        multicastAddress="Uri"
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
|multicastAddress|一个 URI，指定用于发送和接收发现消息的多播地址。 默认值是符合协议规范的多播地址。|  
|name|一个字符串，指定标准终结点的配置的名称。 此名称在服务终结点的 `endpointConfiguration` 特性中用于将标准终结点链接到其配置。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<udpTransportSettings>](udptransportsettings.md)|一个设置集合，用于为 UDP 终结点配置 UDP 传输。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<standardEndpoints>](standardendpoints.md)|具有一个或多个固定属性（地址、绑定和协定）的预定义终结点的标准终结点集合。|  
  
## <a name="example"></a>示例  

 下面的示例演示一个客户端，该客户端分别通过采用默认多播地址和所指定多播地址的 UDP 多播传输侦听公告。  
  
```xml  
<services>
  <service name="ServiceAnnouncementListener">
    <endpoint name="udpAnnouncementEndpointStandard"
              kind="udpAnnouncementEndpoint"
              bindingConfiguration="..." />
    <endpoint name="udpAnnouncementEndpoint2"
              kind="udpAnnouncementEndpoint"
              endpointConfiguration="AnnouncementConfiguration3702"
              bindingConfiguration="..." />
    ...
  </service>
</services>
<standardEndpoints>
  <udpAnnouncementEndpoint>
    <standardEndpoint name="AnnouncementConfiguration2"
                      version="WSDiscoveryApril2005"
                      multicastAddress="soap.udp://239.255.255.250:3703"/>
  </udpAnnouncementEndpoint>
</standardEndpoints>
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Discovery.UdpAnnouncementEndpoint>
