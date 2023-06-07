---
title: 消息筛选器
ms.date: 03/30/2017
helpviewer_keywords:
- routing [WCF], message filters
ms.assetid: cb33ba49-8b1f-4099-8acb-240404a46d9a
ms.openlocfilehash: a0cc4663b9a3044d0ab80f03479a024acba50a3f
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96279774"
---
# <a name="message-filters"></a>消息筛选器

为了实现基于内容的路由，路由服务使用 <xref:System.ServiceModel.Dispatcher.MessageFilter> 实现，这些实现检查消息的特定部分，例如地址、终结点名称或特定 XPath 语句。 如果随 [!INCLUDE[netfx_current_short](../../../../includes/netfx-current-short-md.md)] 一起提供的消息筛选器均无法满足您的需求，则您可以通过创建 <xref:System.ServiceModel.Dispatcher.MessageFilter> 基类的新实现来创建自定义筛选器。  
  
 配置路由服务时，您必须 (对象定义筛选器元素， <xref:System.ServiceModel.Routing.Configuration.FilterElement>) 描述 **MessageFilter** 的类型以及创建筛选器所需的任何支持数据（例如，要在消息中搜索的特定字符串值）。 请注意，创建筛选器元素仅定义了单独的消息筛选器；若要使用筛选器计算和路由消息，还必须定义筛选器表 (<xref:System.ServiceModel.Routing.Configuration.FilterTableEntryCollection>)。  
  
 筛选器表中的每个条目都引用一个筛选器元素，并指定当消息与筛选器匹配时消息将路由到的客户端终结点。 通过筛选器表条目，您还可以指定一个备份终结点集合 (<xref:System.ServiceModel.Routing.Configuration.BackupEndpointCollection>)，该集合定义在向主终结点发送消息但出现传输故障时，该消息将传输到的目标终结点的列表。 将按指定顺序尝试这些终结点，直至某个终结点成功为止。  
  
## <a name="message-filters"></a>消息筛选器  

 路由服务使用的消息筛选器提供常用消息选择功能，例如，计算消息已发送到的终结点的名称、SOAP 操作或消息已发送到的地址或地址前缀。 也可以使用 `AND` 条件连接筛选器，这样，仅当消息同时与两个筛选器匹配时，才会将消息路由至某个终结点。 此外，还可以通过创建您自己的 <xref:System.ServiceModel.Dispatcher.MessageFilter> 实现来创建自定义筛选器。  
  
 下表列出了路由服务使用的 <xref:System.ServiceModel.Routing.Configuration.FilterType>、实现特定消息筛选器的类，以及所需的 <xref:System.ServiceModel.Routing.Configuration.FilterElement.FilterData%2A> 参数。  
  
|筛选器类型|说明|筛选器数据含义|示例筛选器|  
|------------------|-----------------|-------------------------|--------------------|  
|操作|使用 <xref:System.ServiceModel.Dispatcher.ActionMessageFilter> 类匹配包含特定操作的消息。|筛选器基于的操作。|\<filter name="action1" filterType="Action" filterData="http://namespace/contract/operation" />|  
|EndpointAddress|使用 <xref:System.ServiceModel.Dispatcher.EndpointAddressMessageFilter> 类 <xref:System.ServiceModel.Dispatcher.EndpointAddressMessageFilter.IncludeHostNameInComparison%2A>  ==  `true` 来匹配包含特定地址的消息。|筛选器基于的地址（在 To 标头中）。|\<filter name="address1" filterType="EndpointAddress" filterData="http://host/vdir/s.svc/b"  />|  
|EndpointAddressPrefix|使用 <xref:System.ServiceModel.Dispatcher.PrefixEndpointAddressMessageFilter> 类 <xref:System.ServiceModel.Dispatcher.PrefixEndpointAddressMessageFilter.IncludeHostNameInComparison%2A>  ==  `true` 来匹配包含特定地址前缀的消息。|筛选器基于的地址（使用最长的前缀匹配项）。|\<filter name="prefix1" filterType="EndpointAddressPrefix" filterData="http://host/" />|  
|且|使用始终在返回前计算两个条件的 <xref:System.ServiceModel.Dispatcher.StrictAndMessageFilter> 类。|不使用 filterData;相反，filter1 和 filter2 具有相应的消息筛选器的名称，在表) 中 (也应同时 **存在。**|\<filter name="and1" filterType="And" filter1="address1" filter2="action1" />|  
|自定义|一个用户定义的类型，此类型扩展 <xref:System.ServiceModel.Dispatcher.MessageFilter> 类并具有采用字符串的构造函数。|customType 特性是要创建的类的完全限定类型名称；filterData 是在创建筛选器时要传递给构造函数的字符串。|\<filter name="custom1" filterType="Custom" customType="CustomAssembly.CustomMsgFilter, CustomAssembly" filterData="Custom Data" />|  
|EndpointName|使用 <xref:System.ServiceModel.Dispatcher.EndpointNameMessageFilter> 类根据消息已到达的服务终结点的名称来匹配消息。|服务终结点的名称，例如： "serviceEndpoint1"。  该终结点应为在路由服务上公开的终结点之一。|\<filter name="stock1" filterType="Endpoint" filterData="SvcEndpoint" />|  
|MatchAll|使用 <xref:System.ServiceModel.Dispatcher.MatchAllMessageFilter> 类。 该筛选器匹配所有到达的消息。|不使用 filterData。 该筛选器将始终匹配所有消息。|\<filter name="matchAll1" filterType="MatchAll" />|  
|XPath|使用 <xref:System.ServiceModel.Dispatcher.XPathMessageFilter> 类匹配消息中的特定 XPath 查询。|在匹配消息时要使用的 XPath 查询。|\<filter name="XPath1" filterType="XPath" filterData="//ns:element" />|  
  
 下面的示例定义使用 XPath、EndpointName 和 PrefixEndpointAddress 消息筛选器的筛选器条目。 该示例还演示如何对 RoundRobinFilter1 和 RoundRobinFilter2 条目使用自定义筛选器。  
  
```xml  
<filters>  
     <filter name="XPathFilter" filterType="XPath"
             filterData="/s12:Envelope/s12:Header/custom:RoundingCalculator = 1"/>  
     <filter name="EndpointNameFilter" filterType="EndpointName"
             filterData="calculatorEndpoint"/>  
     <filter name="PrefixAddressFilter" filterType="PrefixEndpointAddress"
             filterData="http://localhost/routingservice/router/rounding/"/>  
     <filter name="RoundRobinFilter1" filterType="Custom"
             customType="RoutingServiceFilters.RoundRobinMessageFilter,
             RoutingService" filterData="group1"/>  
     <filter name="RoundRobinFilter2" filterType="Custom"
             customType="RoutingServiceFilters.RoundRobinMessageFilter,
             RoutingService" filterData="group1"/>  
</filters>  
```  
  
> [!NOTE]
> 仅定义筛选器不会导致根据筛选器计算消息。 必须将筛选器添加到筛选器表中，然后会将该筛选器与路由服务公开的服务终结点相关联。  
  
### <a name="namespace-table"></a>命名空间表  

 当使用 XPath 筛选器时，包含 XPath 查询的筛选器数据可能会因使用了命名空间而变得非常大。 为了解决此问题，路由服务提供使用命名空间表定义您自己的命名空间前缀的功能。  
  
 命名空间表是 <xref:System.ServiceModel.Routing.Configuration.NamespaceElement> 对象的集合，用于定义可在 XPath 中使用的常用命名空间的命名空间前缀。 下面列出了命名空间表中包含的默认命名空间和命名空间前缀。  
  
|前缀|命名空间|  
|------------|---------------|  
|s11|`http://schemas.xmlsoap.org/soap/envelope`|  
|s12|`http://www.w3.org/2003/05/soap-envelope`|  
|wsaAugust2004|`http://schemas.xmlsoap.org/ws/2004/08/addressing`|  
|wsa10|`http://www.w3.org/2005/08/addressing`|  
|sm|`http://schemas.microsoft.com/serviceModel/2004/05/xpathfunctions`|  
|tempuri|`http://tempuri.org`|  
|ser|`http://schemas.microsoft.com/2003/10/Serialization`|  
  
 如果您确定您将在 XPath 查询中使用特定命名空间，则可以将此命名空间和唯一的命名空间前缀添加到命名空间表中，并在所有 XPath 查询中使用该前缀而不是使用完整命名空间。 下面的示例为命名空间定义了一个 "custom" 前缀 `"http://my.custom.namespace"` ，然后在 filterData 中包含的 XPath 查询中使用该前缀。  
  
```xml  
<namespaceTable>  
     <add prefix="custom" namespace="http://my.custom.namespace/"/>  
</namespaceTable>  
<filters>  
     <filter name="XPathFilter" filterType="XPath" filterData="/s12:Envelope/s12:Header/custom:RoundingCalculator = 1"/>  
</filters>  
```  
  
## <a name="filter-tables"></a>筛选器表  

 各筛选器元素定义可应用于消息的逻辑比较，而筛选器表提供筛选器元素和目标客户端终结点之间的关联。 筛选器表是 <xref:System.ServiceModel.Routing.Configuration.FilterTableEntryElement> 对象的命名集合，用于定义筛选器、主目标终结点和替代备份终结点列表之间的关联。 筛选器表条目还允许您为每个筛选条件指定可选优先级。 下面的示例定义两个筛选器，然后定义一个用于将每个筛选器与目标终结点相关联的筛选器表。  
  
```xml  
<routing>  
     <filters>  
       <filter name="AddAction" filterType="Action" filterData="Add" />  
       <filter name="SubtractAction" filterType="Action" filterData="Subtract" />  
     </filters>  
     <filterTables>  
       <table name="routingTable1">  
         <filters>  
           <add filterName="AddAction" endpointName="Addition" />  
           <add filterName="SubtractAction" endpointName="Subtraction" />  
         </filters>  
       </table>  
     </filterTables>
</routing>  
```  
  
### <a name="filter-evaluation-priority"></a>筛选器计算优先级  

 默认情况下，将同时计算筛选器表中的所有条目，计算的消息将路由至与每个匹配的筛选器条目相关联的终结点。 如果多个筛选器的计算结果均为 `true`，并且消息是单向或双工消息，则会将此消息多播到所有匹配筛选器所对应的终结点。 由于只能向客户端返回一个答复，因此无法多播请求-答复消息。  
  
 通过为各筛选器指定优先级别，可以实现更复杂的路由逻辑；路由服务将首先计算处于最高优先级别的所有筛选器。 如果某一消息与此级别的筛选器相匹配，则不会处理较低优先级的筛选器。 例如，将首先根据优先级为 2 的所有筛选器计算传入的单向消息。 该消息不与此优先级别的任何筛选器匹配，因此，接着将根据优先级为 1 的筛选器比较此消息。 有两个优先级为 1 的筛选器与此消息匹配，并且由于该消息是单向消息，因此将被路由至两个目标终结点。  由于已在优先级为 1 的筛选器中找到了匹配项，因此不会计算优先级为 0 的筛选器。  
  
> [!NOTE]
> 如果未指定优先级，则使用默认优先级 0。  
  
 下面的示例定义一个筛选器表，该筛选器表为其引用的筛选器指定优先级 2、1 和 0。  
  
```xml  
<filterTables>  
     <filterTable name="filterTable1">  
          <add filterName="XPathFilter" endpointName="roundingCalcEndpoint"
               priority="2"/>  
          <add filterName="EndpointNameFilter" endpointName="regularCalcEndpoint"
               priority="1"/>  
          <add filterName="PrefixAddressFilter" endpointName="roundingCalcEndpoint"
               priority="1"/>  
          <add filterName="MatchAllMessageFilter" endpointName="defaultCalcEndpoint"
               priority="0"/>  
     </filterTable>  
</filterTables>  
```  
  
 在上面的示例中，如果消息与 XPathFilter 匹配，则将该消息路由至 roundingCalcEndpoint，并且不会计算表中的任何其他筛选器，这是因为所有其他筛选器都具有较低的优先级。 但是，如果此消息与 XPathFilter 不匹配，则会根据下一较低优先级的所有筛选器（即 EndpointNameFilter 和 PrefixAddressFilter）计算此消息。  
  
> [!NOTE]
> 如有可能，请使用独占筛选器，而不是指定优先级，因为优先级计算可能会导致性能降级。  
  
### <a name="backup-lists"></a>备份列表  

 筛选器表中的各个筛选器可以选择指定一个备份列表，即终结点的命名集合 (<xref:System.ServiceModel.Routing.Configuration.BackupEndpointCollection>)。 该集合包含在向主终结点（在 <xref:System.ServiceModel.CommunicationException> 中指定）发送消息期间发生 <xref:System.ServiceModel.Routing.Configuration.FilterTableEntryElement.EndpointName%2A> 时，消息将传输到的终结点的有序列表。 下面的示例定义了一个名为 "backupServiceEndpoints" 的备份列表，其中包含两个终结点。  
  
```xml  
<filterTables>  
     <filterTable name="filterTable1">  
          <add filterName="MatchAllFilter1" endpointName="Destination" backupList="backupEndpointList"/>  
     </filterTable>  
</filterTables>  
<backupLists>  
     <backupList name="backupEndpointList">  
          <add endpointName="backupServiceQueue" />  
          <add endpointName="alternateServiceQueue" />  
     </backupList>  
</backupLists>  
```  
  
 在前面的示例中，如果发送到主终结点 "Destination" 失败，则路由服务将尝试按列出的顺序发送到每个终结点，首先发送到 backupServiceQueue，然后在发送到 backupServiceQueue 失败时发送到 alternateServiceQueue。 如果所有备份终结点均失败，则会返回错误。
