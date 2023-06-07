---
title: 配置示例
ms.date: 03/30/2017
ms.assetid: 75515b4a-8d70-44c8-99e0-7423df41380e
ms.openlocfilehash: 927aab30ab122d281b4f3577ec05a2035c08f2fc
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96264330"
---
# <a name="configuration-sample"></a>配置示例

此示例演示如何使用配置文件使服务成为可发现的服务。  
  
> [!NOTE]
> 此示例将在配置中实现发现。 有关在代码中实现发现的示例，请参阅 " [基本](basic-sample.md)"。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Discovery\Configuration`  
  
## <a name="service-configuration"></a>服务配置  

 此示例中的配置文件演示两个功能：  
  
- 使服务在标准 <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> 上成为可发现的服务。  
  
- 调整该服务的应用程序终结点的发现相关信息，并在标准终结点上调整某些与发现相关的设置。  
  
 若要启用发现，必须在该服务的应用程序配置文件中进行几个更改：  
  
- 必须将一个发现终结点添加到 `<service>` 元素。 这是一个标准 <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> 终结点， 这是一个运行时与发现服务相关联的系统终结点。 发现服务侦听此终结点上的消息。  
  
- `<serviceDiscovery>` 行为将添加到 `<serviceBehaviors>` 节。 这使该服务在运行时可被发现，并使用前面提到的发现终结点来侦听发现 `Probe` 和 `Resolve` 消息。 通过这两个添加操作，可在指定的发现终结点处发现服务。  
  
 下面的配置代码段演示定义了一个应用程序终结点和一个发现终结点的服务：  
  
```xml
<services>  
        <service name="Microsoft.Samples.Discovery.CalculatorService"  
                 behaviorConfiguration="calculatorServiceBehavior">  
          <endpoint address=""  
                    binding="wsHttpBinding"  
                    contract="Microsoft.Samples.Discovery.ICalculatorService"  
                    behaviorConfiguration="endpointBehaviorConfiguration" />  
          <endpoint name="udpDiscovery"
                    kind="udpDiscoveryEndpoint"
                endpointConfiguration="adhocDiscoveryEndpointConfiguration"/>        </service>  
      </services>  
```  
  
 若要利用公告，需要添加公告终结点。 为此，请修改配置文件，如下面的代码所示。  
  
```xml  
<serviceDiscovery>  
            <announcementEndpoints>  
              <endpoint kind="udpAnnouncementEndpoint"/>  
            </announcementEndpoints>  
          </serviceDiscovery>  
```  
  
 向发现服务行为添加公告终结点会为服务创建默认公告客户端。 这可保证在打开和关闭服务时，服务会分别发送联机和脱机公告。  
  
 通过修改其他行为，此配置文件还可以执行这些简单步骤之外的步骤。 通过使用特定终结点，可以控制与发现相关的信息。 也就是说，用户可以控制一个终结点是否可被发现，还可以使用 <xref:System.ServiceModel.Discovery.EndpointDiscoveryBehavior.Scopes%2A> 和自定义 XML 元数据来标记该终结点。 为此，用户必须将一个 `behaviorConfiguration` 属性添加到应用程序终结点。 在这种情况下，以下属性将添加到该应用程序终结点。  
  
`behaviorConfiguration="endpointBehaviorConfiguration"`  
  
 现在，通过该行为配置元素，可以控制与发现相关的特性。 在这种情况下，两个范围将添加到该应用程序终结点。  
  
```xml  
<endpointBehaviors>  
          <behavior name="endpointBehaviorConfiguration">  
            <endpointDiscovery>  
              <scopes>  
                <add scope="http://www.example.com/calculator"/>  
                <add scope="ldap:///ou=engineering,o=examplecom,c=us"/>  
              </scopes>  
            </endpointDiscovery>  
  
          </behavior>
        </endpointBehaviors>  
```  
  
 有关范围的详细信息，请参阅 [发现 Find 和 s](../feature-details/discovery-find-and-findcriteria.md)。  
  
 还可以控制发现终结点的特定详细信息。 这是通过 <xref:System.ServiceModel.Configuration.StandardEndpointsSection> 完成的。 在此示例中，将修改所用协议的版本并添加一个 `maxResponseDelay` 特性，如以下代码示例所示。  
  
```xml  
<standardEndpoints>  
   <udpDiscoveryEndpoint>  
      <standardEndpoint name="adhocDiscoveryEndpointConfiguration" discoveryVersion="WSDiscovery11" maxResponseDelay="00:00:00.600" />
   </udpDiscoveryEndpoint>  
</standardEndpoints>  
```  
  
 下面是此示例中使用的完整配置文件：  
  
```xml  
<configuration>  
    <system.serviceModel>  
  
      <services>  
        <service name="Microsoft.Samples.Discovery.CalculatorService"  
                 behaviorConfiguration="calculatorServiceBehavior">  
          <endpoint address=""  
                    binding="wsHttpBinding"  
                    contract="Microsoft.Samples.Discovery.ICalculatorService"  
                    behaviorConfiguration="endpointBehaviorConfiguration" />  
         <!-- Define the discovery endpoint -->
<endpoint name="udpDiscovery" kind="udpDiscoveryEndpoint" endpointConfiguration="adhocDiscoveryEndpointConfiguration"/>        </service>  
      </services>  
  
      <behaviors>  
  
        <serviceBehaviors>  
          <behavior name="calculatorServiceBehavior">  
  
            <!-- Add an announcement endpoint -->  
            <serviceDiscovery>  
              <announcementEndpoints>  
                <endpoint kind="udpAnnouncementEndpoint"/>  
              </announcementEndpoints>  
            </serviceDiscovery>  
          </behavior>  
        </serviceBehaviors>  
  
        <endpointBehaviors>  
          <behavior name="endpointBehaviorConfiguration">  
            <!-- Add scopes used to identify the service -->  
            <endpointDiscovery>  
              <scopes>  
                <add scope="http://www.example.com/calculator"/>  
                <add scope="ldap:///ou=engineering,o=examplecom,c=us"/>  
              </scopes>  
            </endpointDiscovery>  
  
          </behavior>
        </endpointBehaviors>  
  
      </behaviors>  
  
      <standardEndpoints>  
        <udpDiscoveryEndpoint>  
         <!-- Configure the UDP discovery endpoint -->  
          <standardEndpoint name="adhocDiscoveryEndpointConfiguration" discoveryVersion="WSDiscovery11" maxResponseDelay="00:00:00.600" />
        </udpDiscoveryEndpoint>  
      </standardEndpoints>  
  
    </system.serviceModel>  
</configuration>  
```  
  
## <a name="client-configuration"></a>客户端配置  

 在客户端的应用程序配置文件中，一个类型为 `standardEndpoint` 的 `dynamicEndpoint` 用于使用发现，如下面的配置代码段所示。  
  
```xml  
<client>  
   <!--  Create an endpoint, make kind="dynamicEndpoint" and use the endpointConfiguration to change settings of DynamicEndpoint -->  
   <endpoint name="calculatorEndpoint"  
             binding="wsHttpBinding"  
             contract="ICalculatorService"  
             kind ="dynamicEndpoint"  
             endpointConfiguration="dynamicEndpointConfiguration">  
   </endpoint>  
</client>  
```  
  
 客户端使用 `dynamicEndpoint` 时，运行时将自动执行发现。 会发现过程中会使用各种设置，如 `discoveryClientSettings` 节（该节指定要使用的发现终结点类型）中定义的设置：  
  
```xml  
<endpoint kind="udpDiscoveryEndpoint" endpointConfiguration="adhocDiscoveryEndpointConfiguration" />  
```  
  
 用于搜索服务的查找条件：  
  
```xml  
<!-- Add Scopes, ScopeMatchBy, Extensions and termination criteria in FindCriteria -->  
<findCriteria scopeMatchBy="http://schemas.microsoft.com/ws/2008/06/discovery/rfc" duration="00:00:10" maxResults="1">  
   <scopes>  
      <add scope="http://www.microsoft.com/building42/floor1"/>  
   </scopes>  
   <!-- These extensions are sent from the client to the service as part of the probe message -->  
   <extensions>  
      <CustomMetadata>This is custom metadata that is sent to the service along with the client's find request.</CustomMetadata>  
   </extensions>  
</findCriteria>  
```  
  
 此示例将此功能加以扩展，并修改客户端使用的 <xref:System.ServiceModel.Discovery.FindCriteria> 以及用于发现的标准 `updDiscoveryEndpoint` 的某些属性。 将修改 <xref:System.ServiceModel.Discovery.FindCriteria> 以使用一个范围和一个特定 `scopeMatchBy` 算法以及自定义终止条件。 而且，该示例还演示客户端如何使用 `Probe` 消息来发送 XML 元素。 最后，对 <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> 进行了一些更改，如所用协议的版本以及 UDP 特定设置，如以下配置文件所示。  
  
```xml  
<udpDiscoveryEndpoint>
        <!-- Specify the discovery protocol version and UDP transport settings. -->
        <standardEndpoint name="adhocDiscoveryEndpointConfiguration" discoveryVersion="WSDiscovery11">  
          <transportSettings duplicateMessageHistoryLength="2048"  
                             maxPendingMessageCount="5"  
                             maxReceivedMessageSize="8192"  
                             maxBufferPoolSize="262144"/>  
        </standardEndpoint>
      </udpDiscoveryEndpoint>  
```  
  
 下面是示例中使用的完整客户端配置。  
  
```xml  
<configuration>  
  <system.serviceModel>  
  
    <client>  
      <!--  Create an endpoint, make kind="dynamicEndpoint" and use the endpointConfiguration to change settings of DynamicEndpoint -->  
      <endpoint name="calculatorEndpoint"  
                binding="wsHttpBinding"  
                contract="ICalculatorService"  
                kind ="dynamicEndpoint"  
                endpointConfiguration="dynamicEndpointConfiguration">  
      </endpoint>  
    </client>  
  
    <standardEndpoints>  
  
      <dynamicEndpoint>
        <standardEndpoint name="dynamicEndpointConfiguration">  
          <discoveryClientSettings>  
            <!-- Controls where the discovery happens. In this case, Probe message is sent over UdpDiscoveryEndpoint. -->  
            <endpoint kind="udpDiscoveryEndpoint" endpointConfiguration="adhocDiscoveryEndpointConfiguration" />  
  
            <!-- Add Scopes, ScopeMatchBy, Extensions and termination criteria in FindCriteria -->  
            <findCriteria scopeMatchBy="http://schemas.microsoft.com/ws/2008/06/discovery/rfc" duration="00:00:10" maxResults="1">  
              <scopes>  
                <add scope="http://www.microsoft.com/building42/floor1"/>  
              </scopes>  
              <!-- These extensions are sent from the client to the service as part of the probe message -->  
              <extensions>  
                <CustomMetadata>This is custom metadata that is sent to the service along with the client's find request.</CustomMetadata>  
              </extensions>  
            </findCriteria>  
          </discoveryClientSettings>  
        </standardEndpoint>
      </dynamicEndpoint>  
  
      <udpDiscoveryEndpoint>
        <!-- Specify the discovery protocol version and UDP transport settings. -->
        <standardEndpoint name="adhocDiscoveryEndpointConfiguration" discoveryVersion="WSDiscovery11">  
          <transportSettings duplicateMessageHistoryLength="2048"  
                             maxPendingMessageCount="5"  
                             maxReceivedMessageSize="8192"  
                             maxBufferPoolSize="262144"/>  
        </standardEndpoint>
      </udpDiscoveryEndpoint>  
  
    </standardEndpoints>  
  
  </system.serviceModel>
</configuration>
```  
  
#### <a name="to-use-this-sample"></a>使用此示例  
  
1. 此示例使用 HTTP 终结点，若要运行此示例，必须添加正确的 URL ACL。 有关详细信息，请参阅 [配置 HTTP 和 HTTPS](../feature-details/configuring-http-and-https.md)。 使用提升的特权执行下面的命令应添加相应的 ACL。 如果该命令无效，则可能需要使用你的域和用户名替换以下自变量。 `netsh http add urlacl url=http://+:8000/ user=%DOMAIN%\%UserName%`  
  
2. 生成解决方案。  
  
3. 从生成目录运行服务可执行文件。  
  
4. 运行客户端可执行文件。 请注意，客户端能够查找该服务。  
