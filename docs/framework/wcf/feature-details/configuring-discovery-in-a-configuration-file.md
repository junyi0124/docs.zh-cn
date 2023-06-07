---
title: 在配置文件中配置发现
ms.date: 03/30/2017
ms.assetid: b9884c11-8011-4763-bc2c-c526b80175d0
ms.openlocfilehash: 1ffd5cb2e884b6eeae292326cb0dc1586995ba38
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96284181"
---
# <a name="configuring-discovery-in-a-configuration-file"></a>在配置文件中配置发现

发现功能主要采用四组配置设置。 本主题将简要介绍其中的每组设置，并用示例演示如何配置这些设置。 本主题每一部分后面都将提供一个链接，以便于您获取有关各个方面的更详尽的文档。  
  
## <a name="behavior-configuration"></a>行为配置  

 发现功能采用服务行为和终结点行为。 <xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior> 行为能够发现服务的所有终结点，并允许您指定公告终结点。  下面的示例演示如何添加 <xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior> 以及指定公告终结点。  
  
```xml  
<behaviors>  
      <serviceBehaviors>  
        <behavior name="helloWorldServiceBehavior">  
          <serviceDiscovery>  
            <announcementEndpoints>  
              <endpoint kind="udpAnnouncementEndpoint"/>  
            </announcementEndpoints>  
          </serviceDiscovery>  
        </behavior>  
      </serviceBehaviors>  
</behaviors>  
```  
  
 指定行为后，请从 <的> 元素进行引用， `service` 如下面的示例中所示。  
  
```xml  
<system.serviceModel>  
   <services>  
      <service name="HelloWorldService" behaviorConfiguration="helloWorldServiceBehavior">  
         <!-- Application Endpoint -->  
         <endpoint address="endpoint0"  
                   binding="basicHttpBinding"  
                   contract="IHelloWorldService" />  
         <!-- Discovery Endpoints -->  
         <endpoint kind="udpDiscoveryEndpoint" />  
        </service>  
    </services>  
</system.serviceModel>  
```  
  
 为使服务可供检测，还必须添加发现终结点，上面的示例添加了一个 <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> 标准终结点。  
  
 添加公告终结点时，还必须向 <> 元素添加公告侦听器服务， `services` 如以下示例中所示。  
  
```xml  
<services>  
   <service name="HelloWorldService" behaviorConfiguration="helloWorldServiceBehavior">  
      <!-- Application Endpoint -->  
      <endpoint address="endpoint0"  
                binding="basicHttpBinding"  
                contract="IHelloWorldService" />  
      <!-- Discovery Endpoints -->  
      <endpoint kind="udpDiscoveryEndpoint" />  
   </service>  
   <!-- Announcement Listener Configuration -->  
   <service name="AnnouncementListener">  
      <endpoint kind="udpAnnouncementEndpoint" />  
   </service>  
</services>
```  
  
 <xref:System.ServiceModel.Discovery.EndpointDiscoveryBehavior> 行为用于允许或禁止发现特定终结点。  下面的示例配置一个具有两个应用程序终结点的服务，其中一个终结点允许发现，另一个终结点禁止发现。 为每个终结点添加一个 <xref:System.ServiceModel.Discovery.EndpointDiscoveryBehavior> 行为。  
  
```xml  
<system.serviceModel>  
   <services>  
      <service name="HelloWorldService"  
               behaviorConfiguration="helloWorldServiceBehavior">  
  
        <!-- Application Endpoints -->  
        <endpoint address="endpoint0"  
                 binding="basicHttpBinding"  
                 contract="IHelloWorldService"
                 behaviorConfiguration="ep0Behavior" />  
  
        <endpoint address="endpoint1"  
                  binding="basicHttpBinding"  
                  contract="IHelloWorldService"  
                  behaviorConfiguration="ep1Behavior" />  
  
        <!-- Discovery Endpoints -->  
        <endpoint kind="udpDiscoveryEndpoint" />  
      </service>  
   </services>  
   <behaviors>  
      <serviceBehaviors>  
        <behavior name="helloWorldServiceBehavior">  
          <serviceDiscovery />  
        </behavior>  
      </serviceBehaviors>  
      <endpointBehaviors>  
        <behavior name="ep0Behavior">  
          <endpointDiscovery enabled="true"/>  
        </behavior>  
        <behavior name="ep1Behavior">  
          <endpointDiscovery enabled="false"/>  
        </behavior>  
     </endpointBehaviors>  
   </behaviors>  
</system.serviceModel>  
```  
  
 <xref:System.ServiceModel.Discovery.EndpointDiscoveryBehavior> 行为还可用于向服务返回的终结点元数据添加自定义元数据。 以下示例演示如何执行此操作。  
  
```xml  
<behavior name="ep4Behavior">  
   <endpointDiscovery enabled="true">  
      <extensions>  
         <CustomMetadata>This is custom metadata.</CustomMetadata>  
         <d:Types xmlns:d="http://schemas.xmlsoap.org/ws/2005/04/discovery" xmlns:i="http://printer.example.org/2003/imaging">i:PrintBasic</d:Types>  
         <CustomMetadata netsted="true">  
            <NestedMetadata>This is a nested custom metadata.</NestedMetadata>  
         </CustomMetadata>  
      </extensions>  
   </endpointDiscovery>  
</behavior>  
```  
  
 <xref:System.ServiceModel.Discovery.EndpointDiscoveryBehavior> 行为还可用于添加客户端搜索服务时采用的范围和类型。 下面的示例演示如何在客户端配置文件中执行此操作。  
  
```xml  
<behavior name="ep2Behavior">  
   <endpointDiscovery enabled="true">  
      <scopes>  
         <add scope="http://www.microsoft.com/building42/floor1"/>  
         <add scope="ldap:///ou=engineeringo=examplecomc=us"/>  
      </scopes>  
      <types>  
         <add name="test" namespace="http://example.microsoft.com/" />  
         <add name="additionalContract" namespace="http://example.microsoft.com/" />  
      </types>  
   </endpointDiscovery>  
</behavior>  
```  
  
 有关的详细信息 <xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior> ， <xref:System.ServiceModel.Discovery.EndpointDiscoveryBehavior> 请参阅 [WCF 发现概述](wcf-discovery-overview.md)。  
  
## <a name="binding-element-configuration"></a>绑定元素配置  

 绑定元素配置在客户端最有意义。 您可以使用配置指定从 WCF 客户端应用程序发现服务时采用的查找条件。  下面的示例使用 <xref:System.ServiceModel.Discovery.DiscoveryClient> 通道创建自定义绑定，并指定包含类型和范围的查找条件。 此外，本示例还指定了 <xref:System.ServiceModel.Discovery.FindCriteria.Duration%2A> 和 <xref:System.ServiceModel.Discovery.FindCriteria.MaxResults%2A> 属性的值。  
  
```xml  
<bindings>  
   <customBinding>  
      <!-- Binding Configuration for the Application Endpoint -->  
      <binding name="discoBindingConfiguration">  
         <discoveryClient>  
            <endpoint kind="discoveryEndpoint"  
                      address="http://localhost:8000/ConfigTest/Discovery"  
                      binding="customBinding"  
                      bindingConfiguration="httpSoap12WSAddressing10"/>  
            <findCriteria duration="00:00:10" maxResults="2">  
              <types>  
                <add name="IHelloWorldService"/>  
              </types>  
              <scopes>  
                <add scope="http://www.microsoft.com/building42/floor1"/>  
              </scopes>
            </findCriteria>  
          </discoveryClient>  
          <textMessageEncoding messageVersion="Soap11"/>  
          <httpTransport />  
      </binding>
   </customBinding>
</bindings>  
```  
  
 客户端终结点必须引用此自定义绑定配置：  
  
```xml  
<client>  
      <endpoint address="http://schemas.microsoft.com/discovery/dynamic"  
                binding="customBinding"  
                bindingConfiguration="discoBindingConfiguration"  
                contract="IHelloWorldService" />  
</client>  
```  
  
 有关查找条件的详细信息，请参阅 [发现查找和 s](discovery-find-and-findcriteria.md)。 有关发现和绑定元素的详细信息，请参阅 [WCF 发现概述](wcf-discovery-overview.md)  
  
## <a name="standard-endpoint-configuration"></a>标准终结点配置  

 标准终结点是预定义的终结点，这样的终结点的一个或多个属性（地址、绑定或协定）采用默认值，或者具有一个或多个无法更改的属性值。 .NET 4 随附了 3 个与发现相关的标准终结点：<xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint>、<xref:System.ServiceModel.Discovery.UdpAnnouncementEndpoint> 和 <xref:System.ServiceModel.Discovery.DynamicEndpoint>。  <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> 是通过 UDP 多播绑定为发现操作预先配置的标准终结点。 <xref:System.ServiceModel.Discovery.UdpAnnouncementEndpoint> 是预先配置的通过 UDP 绑定发送公告消息的标准终结点。 <xref:System.ServiceModel.Discovery.DynamicEndpoint> 是使用发现功能在运行时动态查找已发现服务的终结点地址的标准终结点。  标准绑定是使用一个包含 kind 特性的 <> 元素指定的，该 `endpoint` 元素包含指定要添加的标准终结点的类型。 下面的示例演示如何添加 <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> 和 <xref:System.ServiceModel.Discovery.UdpAnnouncementEndpoint>。  
  
```xml  
<services>  
   <service name="HelloWorldService">  
      <!-- ...  -->
      <endpoint kind="udpDiscoveryEndpoint" />  
   </service>  
   <service name="AnnouncementListener">  
      <endpoint kind="udpAnnouncementEndpoint" />  
   </service>  
</services>  
```  
  
 标准终结点在 <> 元素中进行配置 `standardEndpoints` 。 下面的示例演示如何配置 <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> 和 <xref:System.ServiceModel.Discovery.UdpAnnouncementEndpoint>。  
  
```xml  
<standardEndpoints>  
      <udpAnnouncementEndpoint>  
        <standardEndpoint
            name="udpAnnouncementEndpointSettings"
            multicastAddress="soap.udp://239.255.255.250:3703"
            maxAnnouncementDelay="00:00:00.800">  
          <transportSettings  
            duplicateMessageHistoryLength="1028"  
            maxPendingMessageCount="10"  
            maxMulticastRetransmitCount="3"  
            maxUnicastRetransmitCount="2"  
            socketReceiveBufferSize="131072"  
            timeToLive="2" />  
        </standardEndpoint>  
      </udpAnnouncementEndpoint>  
      <udpDiscoveryEndpoint>  
        <standardEndpoint  
            name="udpDiscoveryEndpointSettings"  
            multicastAddress="soap.udp://239.255.255.252:3704"  
            maxResponseDelay="00:00:00.800">  
          <transportSettings  
            duplicateMessageHistoryLength="2048"  
            maxPendingMessageCount="5"  
            maxReceivedMessageSize="8192"  
            maxBufferPoolSize="262144"/>  
        </standardEndpoint>  
      </udpDiscoveryEndpoint>
</standardEndpoints>
```  
  
 添加标准终结点配置后，请在每个终结点的 <> 元素中引用该配置， `endpoint` 如下面的示例中所示。  
  
```xml  
<services>  
   <service name="HelloWorldService">  
      <!-- ...  -->
      <endpoint kind="udpDiscoveryEndpoint" endpointConfiguration="udpDiscoveryEndpointSettings"/>  
   </service>  
   <service name="AnnouncementListener">  
      <endpoint kind="udpAnnouncementEndpoint" endpointConfiguration="udpAnnouncementEndpointSettings" >  
   </service>  
</services>  
```  
  
 与发现功能使用的其他标准终结点不同，您可以为 <xref:System.ServiceModel.Discovery.DynamicEndpoint> 指定绑定和协定。 下面的示例演示如何添加和配置 <xref:System.ServiceModel.Discovery.DynamicEndpoint>。  
  
```xml  
<system.serviceModel>  
    <client>  
      <endpoint kind="dynamicEndpoint" binding="basicHttpBinding" contract="IHelloWorldService" endpointConfiguration="dynamicEndpointConfiguration" />  
    </client>
   <standardEndpoints>  
      <dynamicEndpoint>  
         <standardEndpoint name="dynamicEndpointConfiguration">  
             <discoveryClientSettings>  
              <findCriteria scopeMatchBy="http://schemas.microsoft.com/ws/2008/06/discovery/rfc" duration="00:00:10" maxResults="2">  
                 <types>  
                   <add name="IHelloWorldService"/>  
                 </types>  
                 <scopes>  
                   <add scope="http://www.microsoft.com/building42/floor1"/>  
                 </scopes>  
                 <extensions>  
                   <CustomMetadata>This is custom metadata.</CustomMetadata>
                 </extensions>  
               </findCriteria>  
             </discoveryClientSettings>  
           </standardEndpoint>  
         </dynamicEndpoint>  
   </standardEndpoints>  
</system.ServiceModel>  
```  
  
 有关标准终结点的详细信息，请参阅 [标准终结点](standard-endpoints.md)。
