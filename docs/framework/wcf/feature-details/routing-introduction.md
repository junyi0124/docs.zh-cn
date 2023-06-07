---
title: 路由简介
ms.date: 03/30/2017
ms.assetid: bf6ceb38-6622-433b-9ee7-f79bc93497a1
ms.openlocfilehash: 8ce98aab2ed14401fa7c2cbf43eb92a633fa96b0
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746464"
---
# <a name="routing-introduction"></a>路由简介

路由服务提供的泛型可插入 SOAP 中介能够根据消息内容路由消息。 使用路由服务，您可以创建复杂的路由逻辑，以便实现服务聚合、服务版本管理、优先级路由和多播路由等方案。 路由服务还提供了错误处理功能，使您可以设置备份终结点的列表。如果将消息发送到主目标终结点时失败，则会发送到这些备份终结点。

本主题面向不熟悉路由服务的人员，其中涵盖了路由服务的基本配置和承载等内容。

## <a name="configuration"></a>配置

路由服务作为公开一个或多个服务终结点的 WCF 服务实现，这些服务终结点接收来自客户端应用程序的消息，并将消息路由到一个或多个目标终结点。 该服务提供应用于服务公开的服务终结点的 <xref:System.ServiceModel.Routing.RoutingBehavior>。 此行为用于配置服务运行方式的各个方面。 为了便于在使用配置文件时进行配置，请在**RoutingBehavior**上指定参数。 在基于代码的方案中，这些参数将被指定为 <xref:System.ServiceModel.Routing.RoutingConfiguration> 对象的一部分，然后可以将该对象传递给**RoutingBehavior**。

在启动时，该行为向客户端终结点添加 <xref:System.ServiceModel.Routing.SoapProcessingBehavior>，用于对消息执行 SOAP 处理。 这允许路由服务将消息传输到需要的**MessageVersion**不同于接收消息的终结点的终结点。 **RoutingBehavior**还会注册服务扩展，<xref:System.ServiceModel.Routing.RoutingExtension>，它提供了在运行时修改路由服务配置的辅助功能点。

**RoutingConfiguration**类提供了一种一致的方式来配置和更新路由服务的配置。  它包含一些参数，这些参数用作路由服务的设置，并用于在服务启动时配置**RoutingBehavior** ，或将其传递给**RoutingExtension**以在运行时修改路由配置。

通过将多个 <xref:System.ServiceModel.Dispatcher.MessageFilter> 对象全部组合到筛选器表（<xref:System.ServiceModel.Dispatcher.MessageFilterTable%601> 对象）中，可以定义用于对消息执行基于内容的路由的路由逻辑。 将针对筛选器表中包含的消息筛选器以及与消息匹配的每个**MessageFilter**来计算传入消息，并将其转发到目标终结点。 用于路由消息的筛选器表是通过在配置中使用**RoutingBehavior**或使用**RoutingConfiguration**对象通过代码指定的。

### <a name="defining-endpoints"></a>定义终结点

在开始配置时，您似乎应先定义将要使用的路由逻辑，但是，首要步骤实际应是：确定消息将路由至的目标终结点的形状。 路由服务采用协定来定义用于收发消息的通道的形状，因此，输入通道的形状必须与输出通道的形状相匹配。  例如，如果要路由至的目标终结点采用请求-答复通道形状，您必须在入站终结点上使用兼容协定，例如，<xref:System.ServiceModel.Routing.IRequestReplyRouter>。

这意味着，如果目标终结点使用具有多种通信模式（例如单向和双向混合运行模式）的协定，您无法创建单一服务终结点来接收消息并将消息路由到所有目标终结点。 您必须确定哪些终结点具有兼容形状，并定义一个或多个服务终结点，这些服务终结点将用于接收要路由到目标终结点的消息。

> [!NOTE]
> 当使用指定了多种通信模式（例如，单向和双向混合运行模式）的协定时，一种解决方法是在路由服务中使用双工协定，例如，<xref:System.ServiceModel.Routing.IDuplexSessionRouter>。 这意味着绑定必须支持双工通信，而这并非在所有情况下都能够实现。 在不可能实现双工通信的情况下，可能需要在多个终结点中考虑通信方式，或者修改应用程序。

有关路由约定的详细信息，请参阅[路由约定](routing-contracts.md)。

定义服务终结点后，可以使用**RoutingBehavior**将特定**RoutingConfiguration**与终结点相关联。 使用配置文件配置路由服务时，将使用**RoutingBehavior**来指定筛选器表，该表包含用于处理此终结点上接收的消息的路由逻辑。 如果以编程方式配置路由服务，则可以使用**RoutingConfiguration**指定筛选器表。

下面的示例定义路由服务按编程方式或通过使用配置文件来使用的服务和客户端终结点。

```xml
<services>
  <!--ROUTING SERVICE -->
  <service behaviorConfiguration="routingData"
            name="System.ServiceModel.Routing.RoutingService">
    <host>
      <baseAddresses>
        <add baseAddress="http://localhost:8000/routingservice/router"/>
      </baseAddresses>
    </host>
    <!-- Define the service endpoints that are receive messages -->
    <endpoint address=""
              binding="wsHttpBinding"
              name="reqReplyEndpoint"
              contract="System.ServiceModel.Routing.IRequestReplyRouter" />
  </service>
</services>
<behaviors>
  <serviceBehaviors>
    <behavior name="routingData">
      <serviceMetadata httpGetEnabled="True"/>
      <!-- Add the RoutingBehavior and specify the Routing Table to use -->
      <routing filterTableName="routingTable1" />
    </behavior>
  </serviceBehaviors>
</behaviors>
<client>
<!-- Define the client endpoint(s) to route messages to -->
  <endpoint name="CalculatorService"
            address="http://localhost:8000/servicemodelsamples/service"
            binding="wsHttpBinding" contract="*" />
</client>
```

```csharp
//set up some communication defaults
string clientAddress = "http://localhost:8000/servicemodelsamples/service";
string routerAddress = "http://localhost:8000/routingservice/router";
Binding routerBinding = new WSHttpBinding();
Binding clientBinding = new WSHttpBinding();
//add the endpoint the router uses to receive messages
serviceHost.AddServiceEndpoint(
     typeof(IRequestReplyRouter),
     routerBinding,
     routerAddress);
//create the client endpoint the router routes messages to
ContractDescription contract = ContractDescription.GetContract(
     typeof(IRequestReplyRouter));
ServiceEndpoint client = new ServiceEndpoint(
     contract,
     clientBinding,
     new EndpointAddress(clientAddress));
//create a new routing configuration object
RoutingConfiguration rc = new RoutingConfiguration();
….
rc.FilterTable.Add(new MatchAllMessageFilter(), endpointList);
//attach the behavior to the service host
serviceHost.Description.Behaviors.Add(
     new RoutingBehavior(rc));
```

此示例将路由服务配置为公开一个终结点，该终结点的地址用于接收要路由的消息 `http://localhost:8000/routingservice/router`。 由于消息路由到请求-答复终结点，因此该服务终结点使用 <xref:System.ServiceModel.Routing.IRequestReplyRouter> 协定。 此配置还定义消息路由到 `http://localhost:8000/servicemodelsample/service` 单个客户端终结点。 名为 "routingTable1" 的筛选器表（未显示）包含用于路由消息的路由逻辑，并通过使用**RoutingBehavior** （适用于配置文件）或**RoutingConfiguration** （用于编程配置）与服务终结点相关联。

### <a name="routing-logic"></a>路由逻辑

若要定义用于路由消息的路由逻辑，必须确定传入消息包含的哪些数据可以采用唯一方式执行操作。 例如，如果要路由到的所有目标终结点共享相同的 SOAP 操作，则消息中包含的 Action 的值无法很好地指示应将消息路由到的特定终结点。 如果必须以唯一方式将消息路由至某一特定终结点，您应当根据唯一标识消息将路由至的目标终结点的数据进行筛选。

路由服务提供多个**MessageFilter**实现，这些实现检查消息中的特定值，如地址、操作、终结点名称甚至是 XPath 查询。 如果这些实现都不能满足你的需求，则可以创建自定义**MessageFilter**实现。 有关消息筛选器和路由服务使用的实现比较的详细信息，请参阅[消息筛选](message-filters.md)器和[选择筛选器](choosing-a-filter.md)。

多个消息筛选器组合到不同的筛选器表中，后者将每个**MessageFilter**与目标终结点相关联。 或者，也可以使用筛选器表指定一个备份终结点列表，当传输失败时，路由服务将尝试向这些终结点发送消息。

默认情况下，将同时计算筛选器表中的所有消息筛选器；但是，您可以指定 <xref:System.ServiceModel.Routing.Configuration.FilterTableEntryElement.Priority%2A>，以便按特定顺序计算消息筛选器。 将首先计算具有最高优先级的所有条目，如果在较高优先级级别中找到匹配项，则不会计算具有较低优先级的消息筛选器。 有关筛选器表的详细信息，请参阅[消息筛选器](message-filters.md)。

下面的示例使用 <xref:System.ServiceModel.Dispatcher.MatchAllMessageFilter>，它对所有消息的计算结果均为 `true`。 此**MessageFilter**将添加到 "routingTable1" 筛选器表，该表将**MessageFilter**与名为 "CalculatorService" 的客户端终结点相关联。 然后， **RoutingBehavior**指定应使用该表来路由由服务终结点处理的消息。

```xml
<behaviors>
  <serviceBehaviors>
    <behavior name="routingData">
      <serviceMetadata httpGetEnabled="True"/>
      <!-- Add the RoutingBehavior and specify the Routing Table to use -->
      <routing filterTableName="routingTable1" />
    </behavior>
  </serviceBehaviors>
</behaviors>
<!--ROUTING SECTION -->
<routing>
  <filters>
    <filter name="MatchAllFilter1" filterType="MatchAll" />
  </filters>
  <filterTables>
    <table name="routingTable1">
      <filters>
        <add filterName="MatchAllFilter1" endpointName="CalculatorService" />
      </filters>
    </table>
  </filterTables>
</routing>
```

```csharp
//create a new routing configuration object
RoutingConfiguration rc = new RoutingConfiguration();
//create the endpoint list that contains the endpoints to route to
//in this case we have only one
List<ServiceEndpoint> endpointList = new List<ServiceEndpoint>();
endpointList.Add(client);
//add a MatchAll filter to the Router's filter table
//map it to the endpoint list defined earlier
//when a message matches this filter, it is sent to the endpoint contained in the list
rc.FilterTable.Add(new MatchAllMessageFilter(), endpointList);
```

> [!NOTE]
> 默认情况下，路由服务仅计算消息标头。 若要使筛选器访问消息正文，必须将 <xref:System.ServiceModel.Routing.RoutingConfiguration.RouteOnHeadersOnly%2A> 设置为 `false`。

**路**

许多路由服务配置都采用将消息仅路由至一个特定终结点的专有筛选器逻辑，但是，您可能需要将给定消息路由至多个目标终结点。 若要将消息多播到多个目标，必须满足以下条件：

- 通道形状不能为请求-答复（但可以为单向或双工），因为客户端应用程序在响应请求时只能接收一个答复。

- 多个筛选器在计算消息时必须返回 `true`。

如果满足这些条件，则将消息路由至计算结果为 `true` 的所有筛选器的所有终结点。 下面的示例定义路由配置，如果消息中的终结点地址 `http://localhost:8000/routingservice/router/rounding`，则会导致消息路由到这两个终结点。

```xml
<!--ROUTING SECTION -->
<routing>
  <filters>
    <filter name="MatchAllFilter1" filterType="MatchAll" />
    <filter name="RoundingFilter1" filterType="EndpointAddress"
            filterData="http://localhost:8000/routingservice/router/rounding" />
  </filters>
  <filterTables>
    <table name="routingTable1">
      <filters>
        <add filterName="MatchAllFilter1" endpointName="CalculatorService" />
        <add filterName="RoundingFilter1" endpointName="RoundingCalcService" />
      </filters>
    </table>
  </filterTables>
</routing>
```

```csharp
rc.FilterTable.Add(new MatchAllMessageFilter(), calculatorEndpointList);
rc.FilterTable.Add(new EndpointAddressMessageFilter(new EndpointAddress(
    "http://localhost:8000/routingservice/router/rounding")),
    roundingCalcEndpointList);
```

### <a name="soap-processing"></a>SOAP 处理

为了支持在不同协议之间路由消息，默认情况下， **RoutingBehavior**会将 <xref:System.ServiceModel.Routing.SoapProcessingBehavior> 添加到消息要路由到的所有客户端终结点。 此行为在将消息路由到终结点之前，会自动创建新的**MessageVersion** ，并在将其返回给发出请求的客户端应用程序之前为任何响应文档创建兼容的**MessageVersion** 。

为出站消息创建新**MessageVersion**所采取的步骤如下所示：

**请求处理**

- 获取出站绑定/通道的**MessageVersion** 。

- 获取原始消息的正文读取器。

- 使用相同操作、正文读取器和新的**MessageVersion**创建一条新消息。

- 如果 <xref:System.ServiceModel.Channels.MessageVersion.Addressing%2A>！ =**寻址**，则将从、从、FaultTo 和 RelatesTo 标头复制到新消息。

- 将所有消息属性复制到新消息。

- 存储原始请求消息，以备在处理响应时使用。

- 返回新的请求消息。

**响应处理**

- 获取原始请求消息的**MessageVersion** 。

- 获取接收的响应消息的正文读取器。

- 使用相同操作、正文读取器和原始请求消息的**MessageVersion**创建新的响应消息。

- 如果 <xref:System.ServiceModel.Channels.MessageVersion.Addressing%2A>！ =**寻址**，则将从、从、FaultTo 和 RelatesTo 标头复制到新消息。

- 将所有消息属性复制到新消息。

- 返回新的响应消息。

默认情况下，当服务启动时，<xref:System.ServiceModel.Routing.RoutingBehavior> 会自动将**SoapProcessingBehavior**添加到客户端终结点;但是，可以使用 <xref:System.ServiceModel.Routing.RoutingConfiguration.SoapProcessingEnabled%2A> 属性控制是否将 SOAP 处理添加到所有客户端终结点。 此外，如果需要更为精细地控制 SOAP 处理，您还可以向特定终结点直接添加该行为，并在终结点级别上启用或禁用该行为。

> [!NOTE]
> 对于所需的 MessageVersion 不同于原始请求消息的 MessageVersion 的终结点，如果要禁用 SOAP 处理，则必须提供自定义机制，以便在将消息发送到目标终结点之前执行所有必要的 SOAP 修改。

在下面的示例中， **soapProcessingEnabled**属性用于阻止**SoapProcessingBehavior**自动添加到所有客户端终结点。

```xml
<behaviors>
  <!--default routing service behavior definition-->
  <serviceBehaviors>
    <behavior name="routingConfiguration">
      <routing filterTableName="filterTable1" soapProcessingEnabled="false"/>
    </behavior>
  </serviceBehaviors>
</behaviors>
```

```csharp
//create the default RoutingConfiguration
RoutingConfiguration rc = new RoutingConfiguration();
rc.SoapProcessingEnabled = false;
```

### <a name="dynamic-configuration"></a>动态配置

当添加其他客户端终结点或需要修改用于路由消息的筛选器时，您必须采用某种方法在运行时动态更新配置，以免中断当前正在通过路由服务接收消息的终结点的服务。 修改配置文件或宿主应用程序的代码不一定能够始终满足需求，这是因为两种方法都需要回收应用程序，这可能导致丢失当前正在传输的任何消息，并可能在等待服务重新启动时发生停机。

只能以编程方式修改**RoutingConfiguration** 。 虽然最初可以使用配置文件来配置服务，但只能在运行时修改配置，只需要构造一个新的**RoutingConfiguration**并将其作为参数传递到由 <xref:System.ServiceModel.Routing.RoutingExtension> 服务扩展公开的 <xref:System.ServiceModel.Routing.RoutingExtension.ApplyConfiguration%2A> 方法。 当前正在传输的任何消息都将继续使用以前的配置进行路由，而在对**ApplyConfiguration**的调用后收到的消息将使用新的配置。 下面的示例演示如何创建路由服务的实例并随后修改配置。

```csharp
RoutingConfiguration routingConfig = new RoutingConfiguration();
routingConfig.RouteOnHeadersOnly = true;
routingConfig.FilterTable.Add(new MatchAllMessageFilter(), endpointList);
RoutingBehavior routing = new RoutingBehavior(routingConfig);
routerHost.Description.Behaviors.Add(routing);
routerHost.Open();
// Construct a new RoutingConfiguration
RoutingConfiguration rc2 = new RoutingConfiguration();
ServiceEndpoint clientEndpoint = new ServiceEndpoint();
ServiceEndpoint clientEndpoint2 = new ServiceEndpoint();
// Add filters to the FilterTable in the new configuration
rc2.FilterTable.add(new MatchAllMessageFilter(),
       new List<ServiceEndpoint>() { clientEndpoint });
rc2.FilterTable.add(new MatchAllMessageFilter(),
       new List<ServiceEndpoint>() { clientEndpoint2 });
rc2.RouteOnHeadersOnly = false;
// Apply the new configuration to the Routing Service hosted in
routerHost.routerHost.Extensions.Find<RoutingExtension>().ApplyConfiguration(rc2);
```

> [!NOTE]
> 如果采用此种方式更新路由服务，则只能传递新配置。 无法仅修改当前配置的选定元素，也不能向现有配置追加新条目；必须创建并传递用于替换现有配置的新配置。

> [!NOTE]
> 使用以前的配置打开的所有会话都将继续使用以前的配置。 新配置仅用于新会话。

## <a name="error-handling"></a>错误处理

如果在尝试发送消息时遇到任何 <xref:System.ServiceModel.CommunicationException>，将会进行错误处理。 这些异常通常指示在尝试与定义的客户端终结点进行通信时遇到了问题，例如 <xref:System.ServiceModel.EndpointNotFoundException>、<xref:System.ServiceModel.ServerTooBusyException> 或 <xref:System.ServiceModel.CommunicationObjectFaultedException>。 错误处理-代码还将捕获并尝试在发生 <xref:System.TimeoutException> 时重试发送，这是不是派生自**CommunicationException**的另一个常见异常。

如果发生上述一种异常，路由服务会向备份终结点列表进行故障转移。 如果所有备份终结点均失败并出现通信故障，或者如果某个终结点返回指示目标服务内部故障的异常，路由服务则会向客户端应用程序返回错误。

> [!NOTE]
> 错误处理功能捕获并处理在尝试发送消息和尝试关闭通道时发生的异常。 错误处理代码不用于检测或处理由其通信的应用程序终结点所创建的异常;服务引发的 <xref:System.ServiceModel.FaultException> 在路由服务中显示为**FaultMessage** ，并流向客户端。
>
> 如果在路由服务尝试中继消息时出现错误，则您可能会在客户端收到 <xref:System.ServiceModel.FaultException>，而不是在缺少路由服务时通常收到的 <xref:System.ServiceModel.EndpointNotFoundException>。 路由服务因而可能屏蔽异常，不会提供完全透明度，除非您检查嵌套异常。

### <a name="tracing-exceptions"></a>跟踪异常

当向列表中的终结点发送消息失败时，路由服务将跟踪产生的异常数据，并将异常详细信息**作为名为**exception 的消息属性附加。 这可以保存异常数据，并允许用户通过消息检查器进行编程访问。  异常数据根据消息存储在字典中，该字段将终结点名称映射到在尝试向其发送消息时遇到的异常的详细信息。

### <a name="backup-endpoints"></a>备份终结点

筛选器表中的各个筛选器条目可以选择指定一个备份终结点列表，如果在向主终结点发送消息时出现传输故障，则会使用这些备份终结点。 如果出现此类故障，路由服务会尝试将消息传输到备份终结点列表中的第一个条目。 如果此发送尝试同样遇到传输故障，则会尝试备份列表中的下一个终结点。 路由服务将继续向列表中的每个终结点发送消息，直到成功接收此消息、所有终结点均返回传输故障或某个终结点返回非传输故障为止。

下面的示例将路由服务配置为使用备份列表。

```xml
<routing>
  <filters>
    <!-- Create a MatchAll filter that catches all messages -->
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
    <!-- Add an endpoint list that contains the backup destinations -->
    <backupList name="backupEndpointList">
      <add endpointName="realDestination" />
      <add endpointName="backupDestination" />
    </backupList>
  </backupLists>
</routing>
```

```csharp
//create the endpoint list that contains the service endpoints we want to route to
List<ServiceEndpoint> backupList = new List<ServiceEndpoint>();
//add the endpoints in the order that the Routing Service should contact them
//first add the endpoint that we know is down
//clearly, normally you wouldn't know that this endpoint was down by default
backupList.Add(fakeDestination);
//then add the real Destination endpoint
//the Routing Service attempts to send to this endpoint only if it
//encounters a TimeOutException or CommunicationException when sending
//to the previous endpoint in the list.
backupList.Add(realDestination);
//add the backupDestination endpoint
//the Routing Service attempts to send to this endpoint only if it
//encounters a TimeOutException or CommunicationsException when sending
//to the previous endpoints in the list
backupList.Add(backupDestination);
//create the default RoutingConfiguration option
RoutingConfiguration rc = new RoutingConfiguration();
//add a MatchAll filter to the Routing Configuration's filter table
//map it to the list of endpoints defined above
//when a message matches this filter, it is sent to the endpoints in the list in order
//if an endpoint is down or does not respond (which the first endpoint won't
//since the client does not exist), the Routing Service automatically moves the message
//to the next endpoint in the list and try again.
rc.FilterTable.Add(new MatchAllMessageFilter(), backupList);
```

### <a name="supported-error-patterns"></a>支持的错误模式

下表说明了与使用备份终结点列表相兼容的模式，并提供了特定模式的错误处理详细信息的说明。

|模式|会话|Transaction|接收上下文|是否支持备份列表|注意|
|-------------|-------------|-----------------|---------------------|---------------------------|-----------|
|单向||||是|尝试在备份终结点上重新发送消息。 如果正在多播此消息，则仅将故障通道上的消息移至其备份目标。|
|单向||✔️||是|引发异常，并回滚事务。|
|单向|||✔️|是|尝试在备份终结点上重新发送消息。 成功接收消息后，完成所有接收上下文。 如果任何终结点未成功接收消息，则不会完成接收上下文。<br /><br /> 如果正在多播此消息，仅当至少有一个终结点（主终结点或备份终结点）已成功接收消息时，才会完成接收上下文。 如果所有多播路径中没有任何终结点成功接收消息，则不会完成接收上下文。|
|单向||✔️|✔️|是|中止先前事务，创建新事务，并重新发送所有消息。 遇到错误的消息将传输到备份目标。<br /><br /> 在创建已成功完成其中的所有传输的事务之后，完成接收上下文并提交该事务。|
|单向|✔️|||是|尝试在备份终结点上重新发送消息。 在多播情况下，仅向备份目标重新发送已遇到错误或其会话关闭失败的消息。|
|单向|✔️|✔️||是|引发异常，并回滚事务。|
|单向|✔️||✔️|是|尝试在备份终结点上重新发送消息。 在完成所有消息发送操作且未发生错误之后，会话将指示没有任何其他消息，路由服务将成功关闭所有出站会话通道，将完成所有接收上下文，并关闭入站会话通道。|
|单向|✔️|✔️|✔️|是|中止当前事务，并创建新事务。 重新发送会话中以前的所有消息。 在创建已成功发送其中的所有消息的事务，并且会话指示没有任何其他消息之后，将关闭所有出站会话通道，使用事务完成所有接收上下文，关闭入站会话通道，并提交该事务。<br /><br /> 如果会话是多播会话，无任何错误的消息将重新发送到与以前相同的目标，而遇到错误的消息将发送到备份目标。|
|双向||||是|发送到备份目标。  在通道返回响应消息之后，系统会将响应返回到原始客户端。|
|双向|✔️|||是|将通道中的所有消息发送到备份目标。  在通道返回响应消息之后，系统会将响应返回到原始客户端。|
|双向||✔️||是|引发异常，并回滚事务。|
|双向|✔️|✔️||是|引发异常，并回滚事务。|
|双面打印||||是|当前不支持非会话双工通信。|
|双面打印|✔️|||是|发送到备份目标。|

## <a name="hosting"></a>托管

由于路由服务是作为 WCF 服务实现的，因此它必须自承载在应用程序中，或者由 IIS 或 WAS 承载。 建议在 IIS、WAS 或 Windows 服务应用程序中承载路由服务，以便利用这些宿主环境中提供的自动启动和生命周期管理功能。

下面的示例演示如何在应用程序中承载路由服务。

```csharp
using (ServiceHost serviceHost =
                new ServiceHost(typeof(RoutingService)))
```

若要在 IIS 或 WAS 中承载路由服务，您必须创建服务文件 (.svc) 或使用基于配置的服务激活功能。 当使用服务文件时，必须使用 Service 参数指定 <xref:System.ServiceModel.Routing.RoutingService>。 下面的示例包含一个示例服务文件，它可用于在 IIS 或 WAS 中承载路由服务。

```aspx-csharp
<%@ ServiceHost Language="C#" Debug="true" Service="System.ServiceModel.Routing.RoutingService,
     System.ServiceModel.Routing, version=4.0.0.0, Culture=neutral,
     PublicKeyToken=31bf3856ad364e35" %>
```

## <a name="routing-service-and-impersonation"></a>路由服务和模拟

WCF 路由服务可以与模拟结合使用来发送和接收消息。 模拟的所有常用 Windows 约束将适用。 如果您在编写自己的服务时需要设置服务或帐户权限才能使用模拟，则必须执行这些相同步骤，从而将模拟用于路由服务。 有关详细信息，请参阅[委派和模拟](delegation-and-impersonation-with-wcf.md)。

将模拟用于路由服务要求在 ASP.NET 兼容模式下使用 ASP.NET 模拟，或使用已配置为允许模拟的 Windows 凭据。 有关 ASP.NET 兼容模式的详细信息，请参阅[WCF 服务和 ASP.NET](wcf-services-and-aspnet.md)。

> [!WARNING]
> WCF 路由服务不支持将模拟用于基本身份验证。

要将 ASP 模拟用于路由服务，可对服务承载环境启用 ASP.NET 兼容模式。 路由服务已被标记为允许 ASP.NET 兼容模式，并且模拟会自动启用。 模拟是将 ASP.NET 与路由服务相集成的唯一支持的用法。

要将 Windows 凭据模拟用于路由服务，您需要同时配置凭据和服务。 客户端凭据对象（<xref:System.ServiceModel.Security.WindowsClientCredential>，可从 <xref:System.ServiceModel.ChannelFactory> 访问）定义了一个 <xref:System.ServiceModel.Security.WindowsClientCredential.AllowedImpersonationLevel%2A> 属性，此属性必须设置为允许模拟。 最后，您需要在服务上配置 <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior> 行为，以便将 `ImpersonateCallerForAllOperations` 设置为 `true`。 路由服务使用此标志来决定是否创建客户端，以便转发启用模拟的消息。

## <a name="see-also"></a>另请参阅

- [消息筛选器](message-filters.md)
- [路由协定](routing-contracts.md)
- [选择筛选器](choosing-a-filter.md)
