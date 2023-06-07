---
title: <udpDiscoveryEndpoint>
ms.date: 03/30/2017
ms.assetid: 1f485329-2771-43bc-88de-df8f2faa3bb7
ms.openlocfilehash: f02cbddcdd4390d754f93e6f6d9aae6646cb137b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91183619"
---
# \<udpDiscoveryEndpoint>

此配置元素定义一个通过 UDP 多播绑定为发现操作预先配置的标准终结点。 此终结点具有固定协定并支持两个 WS-Discovery 协议版本。 此外，根据 WS-Discovery 规范（WS-Discovery 2005 年 4 月版或 WS-Discovery 1.1 版）中的规定，它还具有固定 UDP 绑定和默认地址。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<standardEndpoints>**](standardendpoints.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<udpDiscoveryEndpoint>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<system.serviceModel>
  <standardEndpoints>
    <udpDiscoveryEndpoint>
      <standardEndpoint discoveryMode="Adhoc/Managed"
                        discoveryVersion="WSDiscovery11/WSDiscoveryApril2005"
                        maxResponseDelay="Timespan"
                        multicastAddress="Uri"
                        name="String" />
    </udpDiscoveryEndpoint>
  </standardEndpoints>
</system.serviceModel>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|discoveryMode|一个字符串，指定发现协议的模式。 有效值为 "即席" 和 "Managed"。 在托管模式下，协议依靠发现代理，此代理用作可检测服务的存储库。 即席模式要求协议使用 UDP 多播机制查找可用的服务。 此值的类型为 <xref:System.ServiceModel.Discovery.ServiceDiscoveryMode>。|  
|discoveryVersion|一个字符串，指定两个 WS-Discovery 协议版本中的其中一个版本。 有效值为 WSDiscovery11 和 WSDiscoveryApril2005。 此值的类型为 <xref:System.ServiceModel.Discovery.DiscoveryVersion>。|  
|maxResponseDelay|一个 Timespan 值，指定 Discovery 协议在发送 Probe Match、Resolve Match 这类消息之前等待的最大延迟值。<br /><br /> 如果同时发送所有 ProbeMatch，则可能发生网络风暴。 若要防止发生这种情况，可在发送 ProbeMatch 时在各 ProbeMatch 之间应用随机延迟。 随机延迟的范围是从 0 到此特性所设置的值之间。 如果将此特性设置为 0，则在不使用任何延迟的紧凑循环中发送 ProbeMatch 消息。 否则，在发送 ProbeMatch 消息时将应用一定的随机延迟，以使发送所有 ProbeMatch 消息所用的总时间不会超过 maxResponseDelay。 此值只与服务相关，不可用于客户端。|  
|multicastAddress|一个 URI，指定用于发送和接收发现消息的多播地址。 默认值是符合协议规范的多播地址。|  
|`name`|一个字符串，指定标准终结点的配置的名称。 此名称在服务终结点的 `endpointConfiguration` 特性中用于将标准终结点链接到其配置。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<udpTransportSettings>](udptransportsettings.md)|一个设置集合，用于为 UDP 终结点配置 UDP 传输。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<standardEndpoints>](standardendpoints.md)|具有一个或多个固定属性（地址、绑定和协定）的预定义终结点的标准终结点集合。|  
  
## <a name="example"></a>示例  

 下面的示例演示通过 UDP 多播传输侦听发现消息的服务。  
  
```xml  
<services>
  <service name="CalculatorService"
           behaviorConfiguration="CalculatorServiceBehavior">
    <endpoint binding="basicHttpBinding"
              address="calculator"
              contract="ICalculatorService" />
    <endpoint name="DiscoveryEndpoint"
              kind="udpDiscoveryEndpoint" />
  </service>
  <standardEndpoints>
    <udpDiscoveryEndpoint>
      <standardEndpoint name="DiscoveryEndpoint"
                        version="WSDiscoveryApril2005" />
    </udpDiscoveryEndpoint>
  </standardEndpoints>
</services>
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Discovery.DiscoveryEndpoint>
