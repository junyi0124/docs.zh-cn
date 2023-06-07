---
title: 如何：使用筛选器
ms.date: 03/30/2017
ms.assetid: f2c7255f-c376-460e-aa20-14071f1666e5
ms.openlocfilehash: 149c0809820d6a4a9c8dabfb545258b9a3ffb40b
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96280840"
---
# <a name="how-to-use-filters"></a>如何：使用筛选器

本主题概述创建使用多个筛选器的路由配置所需执行的基本步骤。 在本示例中，消息将路由到两个计算器服务实现，即 regularCalc 和 roundingCalc。 这两个实现都支持相同的运算；但其中一个服务在返回计算结果前会将所有计算结果舍入到最接近的整数值。 客户端应用程序必须能够指示是否使用服务的舍入版本；如果未表示任何服务首选项，则消息将在这两个服务间执行负载平衡。 这两个服务公开的运算包括：  
  
- 添加  
  
- 减  
  
- 乘  
  
- 除  
  
 鉴于这两个服务实现相同的运算，您无法使用 Action 筛选器，这是因为在消息中指定的操作将并非唯一。 您必须执行其他工作来确保将消息路由到适当的终结点。  
  
### <a name="determine-unique-data"></a>确定唯一数据  
  
1. 由于两个服务实现处理相同的运算，并且除了返回的数据之外基本相同，因此从客户端应用程序发送的消息中包含的基本数据不具备足够的独特性，无法确定如何路由请求。 但是，如果客户端应用程序向消息添加了唯一的标头值，则您可以使用此值确定应如何路由消息。  
  
     在本示例中，如果客户端应用程序需要由舍入计算器处理消息，它将使用下面的代码添加自定义标头：  
  
    ```csharp  
    messageHeadersElement.Add(MessageHeader.CreateHeader("RoundingCalculator",
                                   "http://my.custom.namespace/", "rounding"));  
    ```  
  
     现在，您可以使用 XPath 筛选器检查消息中是否含有此标头，并将包含该标头的消息路由到 roundCalc 服务。  
  
2. 此外，路由服务还公开两个虚拟服务终结点，可将这两个终结点与 EndpointName、EndpointAddress 或 PrefixEndpointAddress 筛选器配合使用，以基于客户端应用程序提交请求的目标终结点将传入消息唯一路由到特定计算器实现。  
  
### <a name="define-endpoints"></a>定义终结点  
  
1. 定义路由服务使用的终结点时，首先应确定客户端和服务使用的通道的形状。 在此方案中，两个目标服务都使用请求-答复模式，因此使用 <xref:System.ServiceModel.Routing.IRequestReplyRouter>。 下面的示例定义路由服务公开的服务终结点。  
  
    ```xml  
    <services>  
          <service behaviorConfiguration="routingConfiguration"  
                   name="System.ServiceModel.Routing.RoutingService">  
            <host>  
              <baseAddresses>  
                <add baseAddress="http://localhost/routingservice/router" />  
              </baseAddresses>  
            </host>  
            <!--Set up the inbound endpoints for the Routing Service-->  
            <!--first create the general router endpoint-->  
            <endpoint address="general"  
                      binding="wsHttpBinding"  
                      name="routerEndpoint"  
                      contract="System.ServiceModel.Routing.IRequestReplyRouter" />  
            <!--create a virtual endpoint for the regular calculator service-->  
            <endpoint address="regular/calculator"  
                      binding="wsHttpBinding"  
                      name="calculatorEndpoint"  
                      contract="System.ServiceModel.Routing.IRequestReplyRouter" />  
            <!--now create a virtual endpoint for the rounding calculator-->  
            <endpoint address="rounding/calculator"  
                      binding="wsHttpBinding"  
                      name="roundingEndpoint"  
                      contract="System.ServiceModel.Routing.IRequestReplyRouter" />  
  
          </service>  
    </services>  
    ```  
  
     通过此配置，路由服务公开三个单独的终结点。 客户端应用程序根据运行时选项将消息发送到其中一个地址。 到达 "虚拟" 服务终结点之一的消息 ( "舍入/计算器" 或 "常规/计算器" ) 被转发到相应的计算器实现。 如果客户端应用程序未将请求发送到特定终结点，则将消息发送到常规终结点。 不论选择哪个终结点，客户端应用程序还可以选择包括自定义标头，以指示应将消息转发到舍入计算器实现。  
  
2. 下面的示例定义路由服务将消息路由到的客户端（目标）终结点。  
  
    ```xml  
    <client>  
          <endpoint name="regularCalcEndpoint"  
                    address="net.tcp://localhost:9090/servicemodelsamples/service/"  
                    binding="netTcpBinding"  
                    contract="*" />  
  
          <endpoint name="roundingCalcEndpoint"  
                    address="net.tcp://localhost:8080/servicemodelsamples/service/"  
                    binding="netTcpBinding"  
                    contract="*" />  
    </client>  
    ```  
  
     这些终结点用于在筛选器表中指示当匹配特定筛选器时将消息发送到的目标终结点。  
  
### <a name="define-filters"></a>定义筛选器  
  
1. 若要根据客户端应用程序添加到消息的 "RoundingCalculator" 自定义标头路由消息，请定义一个筛选器，该筛选器使用 XPath 查询来检查是否存在此标头。 由于此标头是使用自定义命名空间定义的，因此还添加了一个命名空间项，用于定义在 XPath 查询中使用的自定义命名空间前缀 "custom"。 下面的示例定义所需的路由节、命名空间表和 XPath 筛选器。  
  
    ```xml  
    <routing>  
          <!-- use the namespace table element to define a prefix for our custom namespace-->  
          <namespaceTable>  
            <add prefix="custom" namespace="http://my.custom.namespace/"/>  
          </namespaceTable>  
          <filters>  
            <!--define the different message filters-->  
            <!--define an xpath message filter to look for the custom header coming from the client-->  
            <filter name="XPathFilter" filterType="XPath"
                    filterData="/s12:Envelope/s12:Header/custom:RoundingCalculator = 'rounding'"/>  
          </filters>  
    </routing>  
    ```  
  
     此 **MessageFilter** 在包含值 "舍入" 的消息中查找 RoundingCalculator 标头。 此标头由客户端设置，用于指示应将消息路由到 roundingCalc 服务。  
  
    > [!NOTE]
    > 默认情况下，在命名空间表中定义 s12 命名空间前缀，表示命名空间 `http://www.w3.org/2003/05/soap-envelope` 。
  
2. 您还必须定义用于查找在两个虚拟终结点上接收到的消息的筛选器。 第一个虚拟终结点是 "常规/计算器" 终结点。 客户端可以将请求发送到此终结点，以指示应将消息路由到 regularCalc 服务。 下面的配置定义一个筛选器，该筛选器使用 <xref:System.ServiceModel.Dispatcher.EndpointNameMessageFilter> 确定消息是否通过具有 filterData 中指定的名称的终结点到达。  
  
    ```xml  
    <!--define an endpoint name filter looking for messages that show up on the virtual regular calculator endpoint-->  
    <filter name="EndpointNameFilter" filterType="EndpointName" filterData="calculatorEndpoint"/>  
    ```  
  
     如果名为 "calculatorEndpoint" 的服务终结点收到消息，此筛选器的计算结果为 `true` 。  
  
3. 接着，定义一个筛选器，该筛选器查找发送到 roundingEndpoint 地址的消息。 客户端可以将请求发送到此终结点，以指示应将消息路由到 roundingCalc 服务。 下面的配置定义一个筛选器，该筛选器使用 <xref:System.ServiceModel.Dispatcher.PrefixEndpointAddressMessageFilter> 来确定消息是否到达 "舍入/计算器" 终结点。  
  
    ```xml  
    <!--define a filter looking for messages that show up with the address prefix.  The corresponds to the rounding calc virtual endpoint-->  
    <filter name="PrefixAddressFilter" filterType="PrefixEndpointAddress"  
            filterData="http://localhost/routingservice/router/rounding/"/>  
    ```  
  
     如果在以开头的地址收到消息， `http://localhost/routingservice/router/rounding/` 则此筛选器的计算结果为 **true**。 由于此配置使用的基址为，为 `http://localhost/routingservice/router` roundingEndpoint 指定的地址为 "舍入/计算器"，用于与此终结点通信的完整地址为 `http://localhost/routingservice/router/rounding/calculator` ，与此筛选器匹配。  
  
    > [!NOTE]
    > PrefixEndpointAddress 筛选器在执行匹配时不会计算主机名，因为可以使用多种主机名形式引用单个主机，而所有这些主机名可能都是从客户端应用程序引用主机的有效方式。 例如，下面的所有主机名可能引用同一主机：  
    >
    > - localhost  
    > - 127.0.0.1  
    > - `www.contoso.com`  
    > - ContosoWeb01  
  
4. 最后一个筛选器必须支持路由到达常规终结点但没有自定义标头的消息。 对于此种情况，消息应在 regularCalc 和 roundingCalc 服务之间交替。 若要支持这些消息的 "轮循机制" 路由，请使用自定义筛选器，以允许一个筛选器实例与每个已处理消息匹配。  下面定义了 RoundRobinMessageFilter 的两个实例，这两个实例组合在一起以指示它们应彼此交替。  
  
    ```xml  
    <!-- Set up the custom message filters.  In this example,   
         we'll use the example round robin message filter,   
         which alternates between the references-->  
    <filter name="RoundRobinFilter1" filterType="Custom"  
                    customType="CustomFilterAssembly.RoundRobinMessageFilter, CustomFilterAssembly"  
                    filterData="group1"/>  
    <filter name="RoundRobinFilter2" filterType="Custom"  
                    customType="CustomFilterAssembly.RoundRobinMessageFilter, CustomFilterAssembly"  
                    filterData="group1"/>  
    ```  
  
     在运行期间，此筛选器类型在定义的此类型的所有筛选器实例（这些实例在一个集合中配置为同一组）之间交替。 这会导致此自定义筛选器处理的消息在 `true` 和的返回之间交替 `RoundRobinFilter1` `RoundRobinFilter2` 。  
  
### <a name="define-filter-tables"></a>定义筛选器表  
  
1. 若要将筛选器与特定客户端终结点关联，必须将筛选器放置到筛选器表中。 此示例方案还采用筛选器优先级设置，该可选设置用于指示筛选器的处理顺序。 如果未指定筛选器优先级，则将同时计算所有筛选器。  
  
    > [!NOTE]
    > 虽然指定筛选器优先级可以控制筛选器的处理顺序，但可能会对路由服务的性能产生负面影响。 如有可能，请构造筛选器逻辑，以免使用筛选器优先级。  
  
     下面定义了筛选器表，并将之前定义的 "XPathFilter" 添加到优先级为2的表中。 此项还指定，如果与 `XPathFilter` 消息匹配，消息将路由到 `roundingCalcEndpoint` 。  
  
    ```xml  
    <routing>  
    ...      <filters>  
    ...      </filters>  
          <filterTables>  
            <table name="filterTable1">  
              <entries>  
                <!--add the filters to the message filter table-->  
                <!--first look for the custom header, and if we find it,  
                    send the message to the rounding calc endpoint-->  
                <add filterName="XPathFilter" endpointName="roundingCalcEndpoint" priority="2"/>  
              </entries>  
            </table>  
          </filterTables>  
    </routing>  
    ```  
  
     如果指定了筛选器优先级，将首先计算优先级最高的筛选器。 如果处于特定优先级级别的一个或多个筛选器匹配，将不会计算较低优先级级别的筛选器。 对于此方案，指定的最高优先级是 2，这是此级别的唯一筛选器条目。  
  
2. 定义了筛选器条目，以通过检查终结点名称或地址前缀来检查是否在特定终结点上收到了消息。 下面的条目将这两个筛选器条目添加到筛选器表，并将这两个条目与消息将路由到的目标终结点相关联。 这些筛选器的优先级设置为 1，指示近当前面的 XPath 筛选器与消息不匹配时才应运行这些筛选器。  
  
    ```xml  
    <!--if the header wasn't there, send the message based on which virtual endpoint it arrived at-->  
    <!--we determine this through the endpoint name, or through the address prefix-->  
    <add filterName="EndpointNameFilter" endpointName="regularCalcEndpoint" priority="1"/>  
    <add filterName="PrefixAddressFilter" endpointName="roundingCalcEndpoint" priority="1"/>  
    ```  
  
     由于这些筛选器的筛选器优先级为 1，因此，仅当优先级级别为 2 的筛选器与消息不匹配时，才会计算这些筛选器。 此外，由于这两个筛选器具有相同的优先级级别，因此将同时计算它们。 由于两个筛选器是互斥的，因此其中只能有一个筛选器与消息相匹配。  
  
3. 如果消息与前面的所有筛选器均不匹配，则将通过泛型服务终结点接收该消息，并且该消息未包含指示其路由到的目标位置的标头信息。 这些消息将由自定义筛选器处理，该筛选器在两个计算器服务之间对这些消息实现负载平衡。 下面的示例演示如何将筛选器条目添加到筛选器表；每个筛选器均与两个目标终结点之一相关联。  
  
    ```xml  
    <!--if none of the other filters have matched,   
        this message showed up on the default router endpoint,   
        with no custom header-->  
    <!--round robin these requests between the two services-->  
    <add filterName="RoundRobinFilter1" endpointName="regularCalcEndpoint" priority="0"/>  
    <add filterName="RoundRobinFilter2" endpointName="roundingCalcEndpoint" priority="0"/>  
    ```  
  
     由于这些条目指定了优先级 0，因此，仅当较高优先级的筛选器与消息均不匹配时，才会计算这些筛选器。 此外，由于这两个筛选器具有相同的优先级级别，因此将同时计算它们。  
  
     如前所述，对于接收到的每条消息，在这些筛选器定义使用的自定义筛选器中，只有其中一个筛选器的计算结果为 `true`。 由于仅使用此筛选器定义了两个筛选器，并且两个筛选器具有指定的相同组设置，因此其结果是：路由服务在将消息发送到 regularCalcEndpoint 和 RoundingCalcEndpoint 之间交替。  
  
4. 若要根据筛选器计算消息，筛选器表必须首先与将用来接收消息的服务终结点相关联。  下面的示例演示如何使用路由行为将路由表与服务终结点相关联：  
  
    ```xml  
    <behaviors>  
      <!--default routing service behavior definition-->  
      <serviceBehaviors>  
        <behavior name="routingConfiguration">  
          <routing filterTableName="filterTable1" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
    ```  
  
## <a name="example"></a>示例  

 下面是配置文件的完整代码清单。  
  
```xml  
<?xml version="1.0" encoding="utf-8" ?>  
<!-- Copyright (c) Microsoft Corporation. All rights reserved -->  
<configuration>  
  <system.serviceModel>  
    <services>  
      <service behaviorConfiguration="routingConfiguration"  
               name="System.ServiceModel.Routing.RoutingService">  
        <host>  
          <baseAddresses>  
            <add baseAddress="http://localhost/routingservice/router" />  
          </baseAddresses>  
        </host>  
        <!--Set up the inbound endpoints for the Routing Service-->  
        <!--first create the general router endpoint-->  
        <endpoint address="general"  
                  binding="wsHttpBinding"  
                  name="routerEndpoint"  
                  contract="System.ServiceModel.Routing.IRequestReplyRouter" />  
        <!--create a virtual endpoint for the regular calculator service-->  
        <endpoint address="regular/calculator"  
                  binding="wsHttpBinding"  
                  name="calculatorEndpoint"  
                  contract="System.ServiceModel.Routing.IRequestReplyRouter" />  
        <!--now create a virtual endpoint for the rounding calculator-->  
        <endpoint address="rounding/calculator"  
                  binding="wsHttpBinding"  
                  name="roundingEndpoint"  
                  contract="System.ServiceModel.Routing.IRequestReplyRouter" />  
  
      </service>  
    </services>  
    <behaviors>  
      <!--default routing service behavior definition-->  
      <serviceBehaviors>  
        <behavior name="routingConfiguration">  
          <routing filterTableName="filterTable1" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  
    <client>  
<!--set up the destination endpoints-->  
      <endpoint name="regularCalcEndpoint"  
                address="net.tcp://localhost:9090/servicemodelsamples/service/"  
                binding="netTcpBinding"  
                contract="*" />  
  
      <endpoint name="roundingCalcEndpoint"  
                address="net.tcp://localhost:8080/servicemodelsamples/service/"  
                binding="netTcpBinding"  
                contract="*" />  
    </client>  
    <routing>  
      <!-- use the namespace table element to define a prefix for our custom namespace-->  
      <namespaceTable>  
        <add prefix="custom" namespace="http://my.custom.namespace/"/>  
      </namespaceTable>  
      <filters>  
        <!--define the different message filters-->  
        <!--define an xpath message filter to look for the custom header coming from the client-->  
        <filter name="XPathFilter" filterType="XPath" filterData="/s12:Envelope/s12:Header/custom:RoundingCalculator = 'rounding'"/>  
  
        <!--define an endpoint name filter looking for messages that show up on the virtual regular calculator endpoint-->  
        <filter name="EndpointNameFilter" filterType="EndpointName" filterData="calculatorEndpoint"/>  
  
        <!--define a filter looking for messages that show up with the address prefix.  The corresponds to the rounding calc virtual endpoint-->  
        <filter name="PrefixAddressFilter" filterType="PrefixEndpointAddress" filterData="http://localhost/routingservice/router/rounding/"/>  
  
        <!--Set up the custom message filters.  In this example, we'll use the example round robin message filter, which alternates between the references-->  
        <filter name="RoundRobinFilter1" filterType="Custom" customType="CustomFilterAssembly.RoundRobinMessageFilter, CustomFilterAssembly" filterData="group1"/>  
        <filter name="RoundRobinFilter2" filterType="Custom" customType="CustomFilterAssembly.RoundRobinMessageFilter, CustomFilterAssembly" filterData="group1"/>  
      </filters>  
      <filterTables>  
        <table name="filterTable1">  
          <entries>  
            <!--add the filters to the message filter table-->  
            <!--first look for the custom header, and if we find it, send the message to the rounding calc endpoint-->  
            <add filterName="XPathFilter" endpointName="roundingCalcEndpoint" priority="2"/>  
  
            <!--if the header wasn't there, send the message based on which virtual endpoint it arrived at-->  
            <!--we determine this through the endpoint name, or through the address prefix-->  
            <add filterName="EndpointNameFilter" endpointName="regularCalcEndpoint" priority="1"/>  
            <add filterName="PrefixAddressFilter" endpointName="roundingCalcEndpoint" priority="1"/>  
  
            <!--if none of the other filters have matched, this message showed up on the default router endpoint, with no custom header-->  
            <!--round robin these requests between the two services-->  
            <add filterName="RoundRobinFilter1" endpointName="regularCalcEndpoint" priority="0"/>  
            <add filterName="RoundRobinFilter2" endpointName="roundingCalcEndpoint" priority="0"/>  
          </entries>  
        </table>  
      </filterTables>  
    </routing>  
  </system.serviceModel>  
</configuration>  
```  
  
## <a name="see-also"></a>另请参阅

- [路由服务](../samples/routing-services.md)
