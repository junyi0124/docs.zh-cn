---
title: <reliableSession>
ms.date: 03/30/2017
ms.assetid: 129b4a59-37f0-4030-b664-03795d257d29
ms.openlocfilehash: ec69d9194d98302a4744e290f23fbb150b2e87cc
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91181306"
---
# \<reliableSession>

定义 WS-ReliableMessaging 的设置。 如果将该元素添加到自定义绑定，则生成的通道可支持一次性传递保证。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<customBinding>**](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<reliableSession>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<reliableSession acknowledgementInterval="TimeSpan"
                 flowControlEnabled="Boolean"
                 inactivityTimeout="TimeSpan"
                 maxPendingChannels="Integer"
                 maxRetryCount="Integer"
                 maxTransferWindowSize="Integer"
                 reliableMessagingVersion="Default/WSReliableMessagingFebruary2005/WSReliableMessaging11"
                 ordered="Boolean" />
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|acknowledgementInterval|一个 <xref:System.TimeSpan>，包含最大时间间隔，通道在等待此时间间隔后才会发送截至该时刻所收到的消息的确认。 默认值为 00:00:0.2。|  
|flowControlEnabled|一个布尔值，指示是否激活高级流控制（特定于 Microsoft 的 WS-ReliableMessaging 流控制实现）。 默认为 `true`。|  
|inactivityTimeout|一个 <xref:System.TimeSpan>，指定通道在出错之前允许其他通信方不发送任何消息的最大持续时间。 默认值为 00:10:00。<br /><br /> 通道上的活动被定义为接收应用程序消息或基础结构消息。 此属性控制保持非活动会话存在的最长时间。 如果经过此最长时间后该会话仍不活动，则基础结构会中止该会话，且通道会出错。 **注意：**  应用程序不需要定期发送消息以使连接保持活动状态。|  
|maxPendingChannels|一个整数，指定侦听器上可等待接受的最大通道数。 该值应介于 1 至 16384 之间（包括这两个值）。 默认值为 4。<br /><br /> 通道在等待被接受时处于挂起状态。 达到此限制后，将不创建任何通道。 确切地说，在达到此数值之前（通过接受挂起的通道来实现），通道处于挂起模式。 这是对每个工厂的限制。<br /><br /> 当达到此阈值时如果远程应用程序尝试建立新的可靠会话，则会拒绝请求且打开操作将提示此错误。 此限制不适用于挂起的传出通道数。|  
|maxRetryCount|一个整数，指定可靠通道未收到消息确认时，在其基础通道上调用 Send 尝试重新传输该消息的最大尝试次数。<br /><br /> 此值应大于零。 默认值为 8。<br /><br /> 此值应为大于零的整数。 如果在最后一次重新传输后未收到确认，则通道出错。<br /><br /> 如果接收方在接收时确认了消息的传递，则认为该消息已传输。<br /><br /> 如果在传输消息后的一段确定时间内未收到确认，则基础结构将自动重新传输该消息。 此基础结构尝试重新发送消息的次数最多为此属性指定的次数。 如果在最后一次重新传输后未收到确认，则通道出错。<br /><br /> 基础结构使用指数补偿算法根据计算的平均往返时间来确定何时重新传输。 在重新传输之前，此时间最初为 1 秒钟，每尝试一次，延迟时间便会加倍，因此在第一次尝试传输和最后一次尝试传输之间大约会经过 8.5 分钟。 可以根据计算的往返时间来调整第一次尝试重新传输的时间，因此这些尝试所经历的时间将会相应地发生变化。 这样，可以使重新传输时间动态地适应不断变化的网络条件。|  
|maxTransferWindowSize|一个指定缓冲区最大大小的整数。 有效值介于 1 和 4096 之间（包括这两个值）。<br /><br /> 在客户端上，该属性定义可靠通道在保存接收者尚未确认的消息时所使用的缓冲区的最大大小。 此配额的单位是消息。 如果缓冲区已满，将阻止后来的“发送”操作。<br /><br /> 在接收方上，该属性定义通道在存储尚未发送到应用程序的传入消息时所使用的缓冲区的最大大小。 如果缓冲区已满，则接收方将删除后来的消息而不会给出任何提示并要求客户端重新传输。|  
|ordered|一个布尔值，指定是否保证消息以其发送顺序抵达。 如果此设置为 `false`，则消息可以不按顺序抵达。 默认为 `true`。|  
|reliableMessagingVersion|<xref:System.ServiceModel.ReliableMessagingVersion> 中的一个有效值，指定要使用的 WS-ReliableMessaging 版本。|  
  
### <a name="child-elements"></a>子元素  

 无  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<binding>](bindings.md)|定义自定义绑定的所有绑定功能。|  
  
## <a name="remarks"></a>备注  

 可靠会话提供可靠的消息和会话功能。 可靠消息在失败时重新尝试通信并允许指定传递保证（如消息按顺序抵达）。 会话在调用之间将保持客户端的状态。 此元素还可以选择提供有序消息传递。 这个已实现的会话可通过 SOAP 和传输中介。  
  
 发送或接收消息时，每个绑定元素都表示一个处理步骤。 在运行时，绑定元素会创建必要的通道工厂和侦听器，用以生成发送和接收消息所需的传出和传入通道堆栈。 `reliableSession` 会在堆栈中提供一个可选层，该可选层可在终结点之间建立可靠会话并配置此会话的行为。  
  
 有关详细信息，请参阅 [可靠会话](../../../wcf/feature-details/reliable-sessions.md)。  
  
## <a name="example"></a>示例  

 下面的示例演示如何用各种传输和消息编码元素配置自定义绑定，并特别启用可靠会话，这将保持客户端状态并指定按顺序传递保证。 此功能是在客户端和服务的应用程序配置文件中配置的。 此示例显示了服务配置。  
  
```xml  
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceModel.Samples.CalculatorService"
               behaviorConfiguration="CalculatorServiceBehavior">
        <!-- This endpoint is exposed at the base address provided by host: http://localhost/servicemodelsamples/service.svc -->
        <!-- specify customBinding binding and a binding configuration to use -->
        <endpoint address=""
                  binding="customBinding"
                  bindingConfiguration="Binding1"
                  contract="Microsoft.ServiceModel.Samples.ICalculator" />
        <!-- The mex endpoint is exposed at http://localhost/servicemodelsamples/service.svc/mex -->
        <endpoint address="mex"
                  binding="mexHttpBinding"
                  contract="IMetadataExchange" />
      </service>
    </services>
    <!-- custom binding configuration - configures HTTP transport, reliable sessions -->
    <bindings>
      <customBinding>
        <binding name="Binding1">
          <reliableSession />
          <security authenticationMode="SecureConversation"
                    requireSecurityContextCancellation="true">
          </security>
          <compositeDuplex />
          <oneWay />
          <textMessageEncoding messageVersion="Soap12WSAddressing10"
                               writeEncoding="utf-8" />
          <httpTransport authenticationScheme="Anonymous"
                         bypassProxyOnLocal="false"
                         hostNameComparisonMode="StrongWildcard"
                         proxyAuthenticationScheme="Anonymous"
                         realm=""
                         useDefaultWebProxy="true" />
        </binding>
      </customBinding>
    </bindings>
    <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->
    <behaviors>
      <serviceBehaviors>
        <behavior name="CalculatorServiceBehavior">
          <serviceMetadata httpGetEnabled="True" />
          <serviceDebug includeExceptionDetailInFaults="False" />
        </behavior>
      </serviceBehaviors>
    </behaviors>
  </system.serviceModel>
</configuration>
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.ReliableSessionElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- <xref:System.ServiceModel.Channels.ReliableSessionBindingElement>
- [可靠会话](../../../wcf/feature-details/reliable-sessions.md)
- [绑定](../../../wcf/bindings.md)
- [扩展绑定](../../../wcf/extending/extending-bindings.md)
- [自定义绑定](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
