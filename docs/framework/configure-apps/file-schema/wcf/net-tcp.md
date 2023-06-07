---
title: <net.tcp>
ms.date: 03/30/2017
ms.assetid: 8bc2f2be-11c1-4bab-9018-1d21ae568d94
ms.openlocfilehash: 12709d58d9192825598b15a50baa10a54450226e
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91178068"
---
# \<net.tcp>

指定允许多个进程共享同一 TCP 端口的 NET.TCP 端口共享服务的配置设置。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel.activation>**](system-servicemodel-activation.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<net.tcp>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<configuration>
  <system.serviceModel.activation>
    <net.tcp listenBacklog="Integer"
             maxPendingAccepts="Integer"
             maxPendingConnections="Integer"
             receiveTimeout="TimeSpan"
             teredoEnabled="Boolean">
      <allowAccounts>
        <!-- LocalSystem account -->
        <add securityIdentifier="S-1-5-18"/>
        <!-- LocalService account -->
        <add securityIdentifier="S-1-5-19"/>
        <!-- Administrators account -->
        <add securityIdentifier="S-1-5-20"/>
        <!-- Network Service account -->
        <add securityIdentifier="S-1-5-32-544" />
        <!-- IIS_IUSRS account (Vista only)-->
        <add securityIdentifier="S-1-5-32-568"/>
      </allowAccounts>
    </net.tcp>
  </system.serviceModel.activation>
</configuration>
```  
  
## <a name="type"></a>类型  

 `Type`  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`listenBacklog`|一个整数，指定已从共享连接接受但尚未调度到 Windows Communication Foundation (WCF) 服务的最大未处理连接数。 默认值为 10。|  
|`maxPendingAccepts`|一个整数，指定共享服务侦听终结点上的最大未完成并发接受线程数。 默认值为 2。|  
|`MaxPendingConnections`|侦听器可以拥有的正在等待应用程序接受的最大连接数。 超出此配额值时，新的传入连接会被丢弃而不是等待接受。 连接功能（如消息安全）可能会使客户端打开多个连接。 在设置此配额值时，服务管理员应该考虑这些额外的连接。 默认值为 10。|  
|`receiveTimeout`|<xref:System.TimeSpan>，它将为读取组帧数据并执行来自基础连接的连接调度指定超时值。 默认值为“00:00:10”。|  
|`teredoEnabled`|一个布尔值，指示端口共享服务是否使用 Microsoft Teredo 服务代表 WCF 服务在 TCP 端口上进行侦听。 默认为 `false`。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<allowAccounts>](allowaccounts.md)|一个配置元素的集合，这些元素包含一个 `securityIdentifier` 属性，用于为承载 WCF 服务并被授予对共享服务的连接访问权限的进程指定用户帐户。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<system.serviceModel.activation>](system-servicemodel-activation.md)|包含侦听器进程 SMSvcHost.exe 的配置设置。|  
  
## <a name="remarks"></a>备注  

 有关端口共享的详细信息，请参阅 [Net.tcp 端口共享](../../../wcf/feature-details/net-tcp-port-sharing.md)。 若要了解如何配置端口共享服务，请参阅 [配置 Net.tcp 端口共享服务](../../../wcf/feature-details/configuring-the-net-tcp-port-sharing-service.md)。  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Activation.Configuration.NetTcpSection>
- [Net.TCP 端口共享](../../../wcf/feature-details/net-tcp-port-sharing.md)
- [配置 Net.TCP 端口共享服务](../../../wcf/feature-details/configuring-the-net-tcp-port-sharing-service.md)
