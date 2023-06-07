---
title: WCF Discovery 对象模型
ms.date: 03/30/2017
ms.assetid: 8365a152-eacd-4779-9130-bbc48fa5c5d9
ms.openlocfilehash: 06984766fa8199a18197255c57492863a888e3aa
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96281932"
---
# <a name="wcf-discovery-object-model"></a>WCF Discovery 对象模型

WCF Discovery 由一组类型组成，通过这些类型提供的统一编程模型，您可以编写在运行时可发现的服务以及查找并使用这些服务的客户端。  
  
## <a name="making-a-service-discoverable-and-finding-services"></a>使服务成为可发现的服务和查找服务  

 若要使 WCF 服务成为可发现的服务，请将 <xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior> 添加到服务主机的 <xref:System.ServiceModel.Description.ServiceDescription>，并添加一个发现终结点。 如果服务已配置为发送公告消息（通过添加 <xref:System.ServiceModel.Discovery.AnnouncementEndpoint>），则在打开和关闭服务主机时，将发送公告。  
  
 需要侦听服务公告消息的客户端承载公告服务并添加一个或多个公告终结点。 公告服务接收公告消息并引发公告事件。  
  
 客户端使用 <xref:System.ServiceModel.Discovery.DiscoveryClient> 类来搜索可用服务。 客户端应用程序实例化 <xref:System.ServiceModel.Discovery.DiscoveryClient> 类，并传入一个指定将发现消息发送到的位置的发现终结点。 客户端调用 <xref:System.ServiceModel.Discovery.DiscoveryClient.Find%2A> 方法，该方法发送 `Probe` 请求。 侦听发现消息的服务接收此 `Probe` 请求。 如果服务与 `Probe` 中指定的条件匹配，则会将 `ProbeMatch` 消息发送回客户端。  
  
## <a name="object-model"></a>对象模型  

 WCF Discovery API 定义了以下类：  
  
- <xref:System.ServiceModel.Discovery.AnnouncementClient>  
  
- <xref:System.ServiceModel.Discovery.AnnouncementEndpoint>  
  
- <xref:System.ServiceModel.Discovery.AnnouncementService>  
  
- <xref:System.ServiceModel.Discovery.DiscoveryClient>  
  
- <xref:System.ServiceModel.Discovery.DiscoveryEndpoint>  
  
- <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement>  
  
- <xref:System.ServiceModel.Discovery.DiscoveryMessageSequenceGenerator>  
  
- <xref:System.ServiceModel.Discovery.ServiceDiscoveryMode>  
  
- <xref:System.ServiceModel.Discovery.DiscoveryProxy>  
  
- <xref:System.ServiceModel.Discovery.DiscoveryService>  
  
- <xref:System.ServiceModel.Discovery.DiscoveryVersion>  
  
- <xref:System.ServiceModel.Discovery.DynamicEndpoint>  
  
- <xref:System.ServiceModel.Discovery.EndpointDiscoveryBehavior>  
  
- <xref:System.ServiceModel.Discovery.EndpointDiscoveryMetadata>  
  
- <xref:System.ServiceModel.Discovery.FindCriteria>  
  
- <xref:System.ServiceModel.Discovery.FindRequestContext>  
  
- <xref:System.ServiceModel.Discovery.FindResponse>  
  
- <xref:System.ServiceModel.Discovery.ResolveCriteria>  
  
- <xref:System.ServiceModel.Discovery.ResolveResponse>  
  
- <xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior>  

- <xref:System.ServiceModel.Discovery.UdpAnnouncementEndpoint>  
  
- <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint>  
  
## <a name="announcementclient"></a>AnnouncementClient  

 <xref:System.ServiceModel.Discovery.AnnouncementClient> 类提供用于发送公告消息的同步和异步方法。 公告消息有两种类型：Hello 和 Bye。 发送 Hello 消息可指示服务变为可用，发送 Bye 消息可指示现有服务变为不可用。 开发人员创建一个 <xref:System.ServiceModel.Discovery.AnnouncementClient> 实例，并将 <xref:System.ServiceModel.Discovery.AnnouncementEndpoint> 的实例作为构造函数参数进行传递。  
  
## <a name="announcementendpoint"></a>AnnouncementEndpoint  

 <xref:System.ServiceModel.Discovery.AnnouncementEndpoint> 表示具有固定公告协定的标准终结点。 该类由服务或客户端用来发送和接收公告消息。 默认情况下，<xref:System.ServiceModel.Discovery.AnnouncementEndpoint> 类设置为使用 WS_Discovery 11 协议版本。  
  
## <a name="announcementservice"></a>AnnouncementService  

 <xref:System.ServiceModel.Discovery.AnnouncementService> 是系统提供的公告服务实现，用于接收和处理公告消息。 接收到 Hello 或 Bye 消息时，<xref:System.ServiceModel.Discovery.AnnouncementService> 实例将调用相应的虚拟方法 <xref:System.ServiceModel.Discovery.AnnouncementService.OnBeginOnlineAnnouncement%2A> 或 <xref:System.ServiceModel.Discovery.AnnouncementService.OnBeginOfflineAnnouncement%2A>，从而引发公告事件。  
  
## <a name="discoveryclient"></a>DiscoveryClient  

 <xref:System.ServiceModel.Discovery.DiscoveryClient> 类由客户端应用程序用来查找和解析可用服务。 它根据指定的 <xref:System.ServiceModel.Discovery.FindCriteria> 和 <xref:System.ServiceModel.Discovery.ResolveCriteria>，分别提供用于查找和解析服务的同步和异步方法。 开发人员创建一个 <xref:System.ServiceModel.Discovery.DiscoveryClient> 实例，并作为构造函数参数提供 <xref:System.ServiceModel.Discovery.DiscoveryEndpoint> 的实例。  
  
 为了查找服务，开发人员需调用同步或异步 `Find` 方法，该方法提供了一个包含要使用的搜索条件的 <xref:System.ServiceModel.Discovery.FindCriteria> 实例。 <xref:System.ServiceModel.Discovery.DiscoveryClient> 创建一个具有相应标头的 `Probe` 消息并发送查找请求。 由于任何时候都可能存在多个未处理的 `Find` 请求，因此客户端会将接收到的响应关联并验证这些响应。 然后，客户端使用 `Find` 将结果传递给 <xref:System.ServiceModel.Discovery.FindResponse> 操作的调用方。  
  
 为了解析已知服务，开发人员将调用同步或异步 `Resolve` 方法，该方法提供包含已知服务的 <xref:System.ServiceModel.Discovery.ResolveCriteria> 的 <xref:System.ServiceModel.EndpointAddress> 实例。 <xref:System.ServiceModel.Discovery.DiscoveryClient> 创建具有相应标头的 `Resolve` 消息并发送解析请求。 使用 `Resolve` 将接收到的响应与未处理的解析请求关联，并将结果传递给 <xref:System.ServiceModel.Discovery.ResolveResponse> 操作的调用方。  
  
 如果网络中存在发现代理，并且 <xref:System.ServiceModel.Discovery.DiscoveryClient> 以多播方式发送发现请求，则发现代理可以使用多播禁止 Hello 消息进行响应。 <xref:System.ServiceModel.Discovery.DiscoveryClient> 在接收到 Hello 消息时将引发 `ProxyAvailable` 事件，以便响应未处理的 `Find` 或 `Resolve` 请求。 `ProxyAvailable` 事件包含有关发现代理的 <xref:System.ServiceModel.Discovery.EndpointDiscoveryMetadata>。 开发人员可以决定是否使用此信息从临时模式切换到托管模式。  
  
## <a name="discoveryendpoint"></a>DiscoveryEndpoint  

 <xref:System.ServiceModel.Discovery.DiscoveryEndpoint> 表示具有固定发现协定的标准终结点。 该类由服务或客户端用来发送或接收发现消息。 默认情况下，<xref:System.ServiceModel.Discovery.DiscoveryEndpoint> 设置为使用 <xref:System.ServiceModel.Discovery.ServiceDiscoveryMode.Managed?displayProperty=nameWithType> 方法和 WSDiscovery11 WS-Discovery 版本。  
  
## <a name="discoverymessagesequencegenerator"></a>DiscoveryMessageSequenceGenerator  

 <xref:System.ServiceModel.Discovery.DiscoveryMessageSequenceGenerator> 用于在服务发出发现消息或公告消息时生成 <xref:System.ServiceModel.Discovery.DiscoveryMessageSequence>。  
  
## <a name="discoveryservice"></a>DiscoveryService  

 <xref:System.ServiceModel.Discovery.DiscoveryService> 抽象类提供用于接收并处理 `Probe` 和 `Resolve` 消息的框架。 接收到 `Probe` 消息时，<xref:System.ServiceModel.Discovery.DiscoveryService> 将根据传入消息创建一个 <xref:System.ServiceModel.Discovery.FindRequestContext> 实例并调用 <xref:System.ServiceModel.Discovery.DiscoveryService.OnBeginFind%2A> 虚拟方法。 接收到 `Resolve` 消息时，<xref:System.ServiceModel.Discovery.DiscoveryService> 将调用 <xref:System.ServiceModel.Discovery.DiscoveryService.OnBeginResolve%2A> 虚拟方法。 可以从此类继承以提供自定义发现服务实现。  
  
## <a name="discoveryproxy"></a>DiscoveryProxy  

 <xref:System.ServiceModel.Discovery.DiscoveryProxy> 抽象类提供用于接收并处理发现消息和公告消息的框架。 实现自定义发现代理时可以从此类继承。 通过多播接收到 `Probe` 消息时，<xref:System.ServiceModel.Discovery.DiscoveryProxy> 类将调用 `BeginShouldRedirectFind` 虚拟方法，确定是否应发送多播禁止消息。 如果开发人员决定不发送多播禁止消息或者 `Probe` 消息是通过单播接收的，该类将根据传入消息创建 <xref:System.ServiceModel.Discovery.FindRequestContext> 类的实例并调用 `OnBeginFind` 虚拟方法。 通过多播接收到 `Resolve` 消息时，<xref:System.ServiceModel.Discovery.DiscoveryProxy> 类将调用 `ShouldRedirectResolve` 虚拟方法，确定是否应发送多播禁止消息。 如果开发人员决定不发送多播禁止消息或者 `Resolve` 消息是通过单播接收的，该类将根据传入消息创建 <xref:System.ServiceModel.Discovery.ResolveCriteria> 类的实例并调用 `OnBeginResolve` 虚拟方法。 接收到 Hello 或 Bye 消息时，<xref:System.ServiceModel.Discovery.DiscoveryProxy> 将调用相应的虚拟方法（`OnBeginOnlineAnnouncement` 或 `OnBeingOfflineAnnouncement`），从而引发公告事件。  
  
## <a name="discoveryversion"></a>DiscoveryVersion  

 <xref:System.ServiceModel.Discovery.DiscoveryVersion> 类表示要使用的发现协议版本。  
  
## <a name="endpointdiscoverybehavior"></a>EndpointDiscoveryBehavior  

 <xref:System.ServiceModel.Discovery.EndpointDiscoveryBehavior> 类用于控制终结点的可发现性，指定扩展、附加协定类型名称， 以及与该终结点关联的作用域。 将此行为添加到应用程序终结点可以配置其 <xref:System.ServiceModel.Discovery.EndpointDiscoveryMetadata>。 将 <xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior> 添加到服务主机后，默认情况下，该服务主机承载的所有应用程序终结点都变为可发现的。 开发人员通过将 <xref:System.ServiceModel.Discovery.EndpointDiscoveryBehavior.Enabled%2A> 属性设置为 `false`，可以关闭发现特定终结点的能力。  
  
## <a name="endpointdiscoverymetadata"></a>EndpointDiscoveryMetadata  

 <xref:System.ServiceModel.Discovery.EndpointDiscoveryMetadata> 类提供服务发布的终结点的版本无关的表示形式。 该类包含服务开发人员指定的终结点地址、侦听 URI、协定类型名称、作用域、元数据版本和扩展。 客户端在 <xref:System.ServiceModel.Discovery.FindCriteria> 操作期间发送的 `Probe` 将与 <xref:System.ServiceModel.Discovery.EndpointDiscoveryMetadata> 进行匹配。 如果条件匹配，则会将 <xref:System.ServiceModel.Discovery.EndpointDiscoveryMetadata> 返回到客户端。 <xref:System.ServiceModel.Discovery.ResolveCriteria> 中的终结点地址将与 <xref:System.ServiceModel.Discovery.EndpointDiscoveryMetadata> 的终结点地址进行匹配。 如果条件匹配，则会将 <xref:System.ServiceModel.Discovery.EndpointDiscoveryMetadata> 返回到客户端。  
  
## <a name="findcriteria"></a>FindCriteria  

 <xref:System.ServiceModel.Discovery.FindCriteria> 类是一个版本无关的类，用于指定在查找服务时所使用的条件。 该类完全支持 WS-Discovery 定义的用于匹配服务的条件。 它还提供一些扩展，开发人员可以使用这些扩展指定可在匹配过程中使用的自定义值。 开发人员可以提供 `Find` 操作的终止条件，方法是指定 <xref:System.ServiceModel.Discovery.FindCriteria.MaxResults%2A>，它指定开发人员查找的服务总数，或者指定 <xref:System.ServiceModel.Discovery.FindCriteria.Duration%2A>（该值指定客户端等待响应的时长）。  
  
## <a name="findrequestcontext"></a>FindRequestContext  

 <xref:System.ServiceModel.Discovery.FindRequestContext> 类由发现服务根据其在客户端初始化 `Probe` 操作时接收的 `Find` 消息进行实例化。 该类包含客户端所指定的 <xref:System.ServiceModel.Discovery.FindCriteria> 的一个实例。  
  
## <a name="findresponse"></a>FindResponse  

 <xref:System.ServiceModel.Discovery.FindResponse> 类与 <xref:System.ServiceModel.Discovery.DiscoveryClient.Find%2A> 操作的响应一起返回到 `Find` 的调用方。 该类还存在于 <xref:System.ServiceModel.Discovery.FindCompletedEventArgs> 中。 该类包含 <xref:System.ServiceModel.Discovery.EndpointDiscoveryMetadata> 的集合（发现的终结点的集合）以及 <xref:System.ServiceModel.Discovery.EndpointDiscoveryMetadata> 和 <xref:System.ServiceModel.Discovery.DiscoveryMessageSequence> 的字典。  
  
## <a name="resolvecriteria"></a>ResolveCriteria  

 <xref:System.ServiceModel.Discovery.ResolveCriteria> 类是一个版本无关的类，用于指定解析已知服务时所使用的条件。 该类包含已知服务的终结点地址。 开发人员可以提供解析操作的终止条件，方法是指定 <xref:System.ServiceModel.Discovery.ResolveCriteria.Duration%2A>，它指定客户端等待响应的时长。  
  
## <a name="resolveresponse"></a>ResolveResponse  

 <xref:System.ServiceModel.Discovery.ResolveResponse> 与 <xref:System.ServiceModel.Discovery.DiscoveryClient.Resolve%2A> 操作的响应一起返回到 `Resolve` 方法的调用方。 该类还存在于 <xref:System.ServiceModel.Discovery.ResolveCompletedEventArgs> 中。 该类包含 <xref:System.ServiceModel.Discovery.EndpointDiscoveryMetadata>（已发现的终结点）的一个实例以及 <xref:System.ServiceModel.Discovery.DiscoveryMessageSequence> 的一个实例。  
  
## <a name="servicediscoverybehavior"></a>ServiceDiscoveryBehavior  

 通过 <xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior> 类，开发人员可以将发现功能添加到服务。 您可以将此行为添加到 <xref:System.ServiceModel.ServiceHost>。 <xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior> 类循环访问添加到服务主机的应用程序终结点，并根据可发现终结点创建 <xref:System.ServiceModel.Discovery.EndpointDiscoveryMetadata> 的集合。 默认情况下，所有终结点都是可发现的。 可以通过将 <xref:System.ServiceModel.Discovery.EndpointDiscoveryBehavior> 添加到特定终结点来控制该特定终结点的可发现性。 如果已将公告终结点添加到 <xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior>，则在打开或关闭服务主机时，将通过每个公告终结点发送所有可发现终结点的公告。  
  
## <a name="udpannouncementendpoint"></a>UdpAnnouncementEndpoint  

 <xref:System.ServiceModel.Discovery.UdpAnnouncementEndpoint> 类是一个标准公告终结点，它预先配置为可通过 UDP 多播绑定进行公告。 默认情况下，<xref:System.ServiceModel.Discovery.UdpAnnouncementEndpoint> 设置为使用 WSApril2005 WS_Discovery 版本。  
  
## <a name="udpdiscoveryendpoint"></a>UdpDiscoveryEndpoint  

 <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> 类一个标准发现终结点，它预先配置为可通过 UDP 多播绑定来发现。 默认情况下，<xref:System.ServiceModel.Discovery.DiscoveryEndpoint> 设置为使用 WSDiscovery11 WS-Discovery 版本和 <xref:System.ServiceModel.Discovery.ServiceDiscoveryMode.Adhoc?displayProperty=nameWithType> 模式。
