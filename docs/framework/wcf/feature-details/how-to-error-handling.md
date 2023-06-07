---
title: 如何：错误处理
ms.date: 03/30/2017
ms.assetid: de566e39-9358-44ff-8244-780f6b799966
ms.openlocfilehash: 3b8e48a74ff7671b942b5499fb3a0b5d0f389d61
ms.sourcegitcommit: 8a0fe8a2227af612f8b8941bdb8b19d6268748e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/03/2019
ms.locfileid: "71834701"
---
# <a name="how-to-error-handling"></a>如何：错误处理

本主题概述了创建采用错误处理的路由配置所需要的基本步骤。 在本示例中，消息路由到目标终结点。 如果消息因网络或通信相关故障 (<xref:System.ServiceModel.CommunicationException>) 而无法传递，则会将消息重新发送到备用终结点。

> [!NOTE]
> 为了模拟网络故障，本示例中使用的目标终结点包含一个错误地址。 由于没有任何服务正在侦听指定地址，因此路由到此终结点的消息将失败。

> [!NOTE]
> 虽然本主题中包含的示例并未明确讨论超时设置，但在使用错误处理时，您必须考虑这些设置。 如果遇到错误，在客户端收到响应之前，还会发生额外的延迟。 这是因为路由服务在收到此错误后，会尝试将消息发送到备份终结点。 如果与主目标终结点和备份目标终结点关联的超时值非常大，客户端可能会遇到较长时间的延迟，因为在成功发送消息之前，会将消息故障转移到备份列表中的多个终结点。
>
> 由于路由服务可能会遇到的最大延迟等于与筛选器关联的所有终结点的超时值之和，因此建议您在客户端相应地增大预期超时。

### <a name="implement-error-handling"></a>实现错误处理

1. 通过指定由服务公开的服务终结点，创建基本的路由服务配置。 下面的示例将定义一个用于接收消息的服务终结点， 还定义了用于发送消息的客户端终结点；即 deadDestination 和 realDestination。 deadDestination 终结点包含一个未引用正在运行的服务的地址，且该终结点用于模拟向此终结点发送消息时的网络故障。

    ```xml
    <services>
      <service behaviorConfiguration="routingConfiguration"
               name="System.ServiceModel.Routing.RoutingService">
        <host>
          <baseAddresses>
            <add baseAddress="http://localhost/routingservice/" />
          </baseAddresses>
        </host>
        <!-- Create the Routing Service endpoint -->
        <endpoint address="router"
                  binding="basicHttpBinding"
                  name="RoutingServiceEndpoint"
                  contract="System.ServiceModel.Routing.IRequestReplyRouter" />
      </service>
    </services>

    <!-- Create the destination endpoints that we want to send to -->
    <client>
      <!-- Create a dummy endpoint that we know will be down -->
      <endpoint name="deadDestination"
                address="net.tcp://localhost:9090/servicemodelsamples/fakeDestination"
                binding="netTcpBinding"
                contract="*" />

      <!-- Now create the real service endpoint -->
      <endpoint name="realDestination"
                address="net.tcp://localhost:8080/servicemodelsamples/service"
                binding="netTcpBinding"
                contract="*" />
    </client>
    ```

2. 定义用于将消息路由到目标终结点的筛选器。  对于本示例，MatchAll 筛选器用于匹配路由服务接收的所有消息。

    ```xml
    <filters>
      <!-- Create a MatchAll filter which will catch all messages -->
      <filter name="MatchAllFilter1" filterType="MatchAll" />
    </filters>
    ```

3. 定义备份终结点列表，该列表包含在向主目标终结点发送消息期间发生网络或通信故障时，消息发送到的目标终结点。 下面的示例定义的备份列表仅包含一个终结点；但是，可以在备份列表中指定多个终结点。

     如果备份列表包含多个终结点，则当发生网络或通信故障时，路由服务会尝试将消息发送到该列表中的第一个终结点。 如果在发送到此终结点时发生网络或通信故障，路由服务会尝试将消息发送到该列表包含的下一个终结点。 该服务将继续向备份终结点列表中的每个终结点发送消息，直到成功发送此消息、所有备份终结点均返回网络或通信相关错误，或者发送消息但终结点返回非网络和通信相关错误为止。

    ```xml
    <backupLists>
      <backupList name="backupEndpointList">
          <add endpointName="realDestination" />
      </backupList>
    </backupLists>
    ```

4. 定义筛选器表，该筛选器表将筛选器与 deadDestination 终结点和备份终结点列表相关联。  路由服务首先尝试向与筛选器关联的目标终结点发送消息。 由于 deadDestination 包含一个未引用正在运行的服务的地址，因此，这会导致网络错误。 然后，路由服务会尝试将消息发送到 backupEndpointList 中指定的终结点。

    ```xml
    <filterTables>
            <!-- Set up the Routing Service's Message Filter Table -->
            <filterTable name="filterTable1">
                <!-- Add an entity that maps the MatchAllMessageFilter to the dead destination -->
                <!-- If that endpoint is down, tell the Routing Service to try the endpoints -->
                <!-- Listed in the backupEndpointList -->
                <add filterName="MatchAllFilter1" endpointName="deadDestination" backupList="backupEndpointList"/>
            </filterTable>
          </filterTables>
    ```

5. 若要根据筛选器表中包含的筛选器对传入消息求值，必须使用路由行为将筛选器表与服务终结点相关联。  下面的示例演示如何将 "filterTable1" 与服务终结点相关联。

    ```xml
    <behaviors>
      <serviceBehaviors>
        <!-- Set up the Routing Behavior -->
        <behavior name="routingConfiguration">
          <routing filterTableName="filterTable1" />
        </behavior>
      </serviceBehaviors>
    </behaviors>
    ```

## <a name="example"></a>示例

下面是配置文件的完整列表：

```xml
<?xml version="1.0" encoding="utf-8" ?>
<!-- Copyright (c) Microsoft Corporation.  All Rights Reserved. -->
<configuration>
  <system.serviceModel>
    <services>
      <service behaviorConfiguration="routingConfiguration"
               name="System.ServiceModel.Routing.RoutingService">
        <host>
          <baseAddresses>
            <add baseAddress="http://localhost/routingservice/" />
          </baseAddresses>
        </host>
        <!-- Create the Routing Service endpoint -->
        <endpoint address="router"
                  binding="basicHttpBinding"
                  name="RoutingServiceEndpoint"
                  contract="System.ServiceModel.Routing.IRequestReplyRouter" />
      </service>
    </services>
    <behaviors>
      <serviceBehaviors>
        <!-- Set up the Routing Behavior -->
        <behavior name="routingConfiguration">
          <routing filterTableName="filterTable1" />
        </behavior>
      </serviceBehaviors>
    </behaviors>
    <!-- Create the destination endpoints that we want to send to -->
    <client>
      <!-- Create a dummy endpoint that we know will be down -->
      <endpoint name="deadDestination"
                address="net.tcp://localhost:9090/servicemodelsamples/fakeDestination"
                binding="netTcpBinding"
                contract="*" />

      <!-- Now create the real service endpoint -->
      <endpoint name="realDestination"
                address="net.tcp://localhost:8080/servicemodelsamples/service"
                binding="netTcpBinding"
                contract="*" />
    </client>
    <routing>
      <filters>
        <!-- Create a MatchAll filter which will catch all messages -->
        <filter name="MatchAllFilter1" filterType="MatchAll" />
      </filters>
      <filterTables>
        <!-- Set up the Routing Service's Message Filter Table -->
        <filterTable name="filterTable1">
            <!-- Add an entry that maps the MatchAllMessageFilter to the dead destination -->
            <!-- If that endpoint is down, tell the Routing Service to try the endpoints -->
            <!-- Listed in the backupEndpointList -->
            <add filterName="MatchAllFilter1" endpointName="deadDestination" backupList="backupEndpointList"/>
        </filterTable>
      </filterTables>
      <!-- Create the backup endpoint list -->
      <backupLists>
        <backupList name="backupEndpointList">
            <add endpointName="realDestination" />
        </backupList>
      </backupLists>
      </routing>
  </system.serviceModel>
</configuration>
```
