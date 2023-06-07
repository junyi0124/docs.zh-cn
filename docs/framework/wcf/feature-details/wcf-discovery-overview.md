---
title: WCF Discovery 概述
ms.date: 03/30/2017
ms.assetid: 84fad0e4-23b1-45b5-a2d4-c9cdf90bbb22
ms.openlocfilehash: 5876605f5096bfb75c18680faaef4ba0cd15c082
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96281412"
---
# <a name="wcf-discovery-overview"></a>WCF Discovery 概述

Discovery API 提供了统一的编程模型来使用 WS-Discovery 协议动态发布和发现 Web 服务。 通过这些 API，服务可以发布自身，客户端可以查找已发布的服务。 服务一旦可供检测，即可发送公告消息，并侦听和响应发现请求。 可检测到的服务可以发送 Hello 消息和 Bye 消息，前者用于公告服务将到达网络，后者用于公告服务将离开网络。 若要查找服务，客户端将在网络上发送包含特定条件（如服务协定类型、关键字和范围）的 `Probe` 请求。 服务接收到此 `Probe` 请求，并确定它们是否匹配该条件。 如果某一服务匹配该条件，该服务会做出响应，向客户端回发一条 `ProbeMatch` 消息，该消息包含与该服务联系所需的信息。 客户端还可以发送 `Resolve` 请求，以便查找可能已更改终结点地址的服务。 匹配的服务会向客户端回发一条 `Resolve` 消息，以此来响应 `ResolveMatch` 请求。  
  
## <a name="ad-hoc-and-managed-modes"></a>临时模式和托管模式  

 Discovery API 支持两种不同模式：托管模式和临时模式。 托管模式中存在一台称为发现代理的中央服务器，用于维护有关可用服务的信息。 可以采用多种方式使用服务相关信息填充发现代理。 例如，服务可以在启动时向发现代理发送公告消息，或者代理也可以从数据库或配置文件中读取数据以确定可用服务。 发现代理的填充方式完全由开发人员决定。 客户端使用发现代理检索有关可用服务的信息。 当客户端搜索服务时，它会向发现代理发送一条 `Probe` 消息，然后由代理确定已知的任何服务是否与客户端搜索的服务匹配。 如果存在匹配服务，发现代理会向客户端回发 `ProbeMatch` 响应。 然后，客户端可以使用代理返回的服务信息，直接与该服务联系。 托管模式所依据的关键原理是：以单播方式向一个机构（即发现代理）发送发现请求。 通过 .NET Framework 包含的关键组件，您可以生成自己的代理。 客户端和服务可以采用多种方法来定位代理：  
  
- 代理可以响应临时消息。  
  
- 代理可以在启动时发送公告消息。  
  
- 可以编写客户端和服务来查找特定的已知终结点。  
  
 临时模式中没有任何中央服务器。 服务公告和客户端请求等所有发现消息都以多播方式发送。 默认情况下，.NET Framework 包含对基于 UDP 协议的临时发现的支持。 例如，如果将服务配置为在启动时发出 Hello 公告，则该服务采用 UDP 协议通过已知多播地址来发出此公告。 客户端必须主动侦听这些公告，并对这些公告进行相应处理。 当客户端针对某一服务发送 `Probe` 消息时，会通过采用多播协议的网络进行发送。 接收到请求的各服务确定自身是否与 `Probe` 消息中的条件匹配，如果服务与 `ProbeMatch` 消息中指定的条件匹配，服务将直接使用 `Probe` 消息响应客户端。  
  
## <a name="benefits-of-using-wcf-discovery"></a>使用 WCF Discovery 的好处  

 由于 WCF Discovery 是采用 WS-Discovery 协议实现的，因此它可以与其他客户端、服务以及实现 WS-Discovery 的代理进行互操作。 WCF Discovery 建立在现有 WCF API 的基础上，从而可以向现有服务和客户端方便地添加 Discovery 功能。 通过应用程序配置设置，可以轻松地添加服务发现功能。 此外，WCF Discovery 还支持在其他传输（如对等网络、命名覆盖和 HTTP）中使用发现协议。 WCF Discovery 支持采用发现代理的托管运行模式。 由于消息直接发送到发现代理，而不是将多播消息发送到整个网络，因此这样可以减少网络流量。 使用 Web 服务时，WCF Discovery 还可以提高灵活性。 例如，您可以更改服务地址，而不必重新配置客户端或服务。 当客户端必须访问该服务时，它通过 `Probe` 请求发出 `Find` 消息，并希望该服务使用其当前地址进行响应。 WCF Discovery 允许客户端基于不同条件搜索服务，这些条件包括协定类型、绑定元素、命名空间、范围以及关键字或版本号。 WCF Discovery 支持运行时和设计时发现功能。 可以将发现功能添加到应用程序中，这一特点可用于启用其他方案，如容错和自动配置。  
  
## <a name="service-publication"></a>服务发布  

 若要使服务可供检测，必须向服务主机添加 <xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior>，并且必须添加发现终结点，以便指定侦听发现消息的位置。 下面的代码示例演示如何修改自承载服务，以使该服务可供检测。  
  
```csharp  
Uri baseAddress = new Uri($"http://{System.Net.Dns.GetHostName()}:8000/discovery/scenarios/calculatorservice/{Guid.NewGuid().ToString()}/");

// Create a ServiceHost for the CalculatorService type.
using (ServiceHost serviceHost = new ServiceHost(typeof(CalculatorService), baseAddress))
{
    // Add calculator endpoint
    serviceHost.AddServiceEndpoint(typeof(ICalculator), new WSHttpBinding(), string.Empty);

    // ** DISCOVERY ** //
    // Make the service discoverable by adding the discovery behavior
    serviceHost.Description.Behaviors.Add(new ServiceDiscoveryBehavior());

    // ** DISCOVERY ** //
    // Add the discovery endpoint that specifies where to publish the services
    serviceHost.AddServiceEndpoint(new UdpDiscoveryEndpoint());

    // Open the ServiceHost to create listeners and start listening for messages.
    serviceHost.Open();

    // The service can now be accessed.
    Console.WriteLine("Press <ENTER> to terminate service.");
    Console.ReadLine();
}
```  
  
 必须在服务说明中添加 <xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior> 实例，以使服务可供检测。 必须向服务主机添加 <xref:System.ServiceModel.Discovery.DiscoveryEndpoint> 实例，以将发现请求的侦听位置告知服务。 在本示例中，添加了 <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint>（派生自 <xref:System.ServiceModel.Discovery.DiscoveryEndpoint>），用于指定服务应通过 UDP 多播传输侦听发现请求。 由于所有消息均以多播方式发送，因此，<xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> 用于临时发现。  
  
## <a name="announcement"></a>公告  

 默认情况下，发布服务时不会发出公告消息。 必须对服务进行配置才能发出公告消息。 这就为服务编写器提供了额外的灵活性，因为它们可以分别通告服务和侦听发现消息。 服务公告还可用作向发现代理或其他服务注册表注册服务的机制。 下面的代码演示如何将服务配置为通过 UDP 绑定发送公告消息。  
  
```csharp  
Uri baseAddress = new Uri($"http://{System.Net.Dns.GetHostName()}:8000/discovery/scenarios/calculatorservice/{Guid.NewGuid().ToString()}/");

// Create a ServiceHost for the CalculatorService type.
using (ServiceHost serviceHost = new ServiceHost(typeof(CalculatorService), baseAddress))
{
    // Add calculator endpoint
    serviceHost.AddServiceEndpoint(typeof(ICalculator), new WSHttpBinding(), string.Empty);

    // ** DISCOVERY ** //
    // Make the service discoverable by adding the discovery behavior
    ServiceDiscoveryBehavior discoveryBehavior = new ServiceDiscoveryBehavior();
    serviceHost.Description.Behaviors.Add(discoveryBehavior);

    // Send announcements on UDP multicast transport
    discoveryBehavior.AnnouncementEndpoints.Add(
      new UdpAnnouncementEndpoint());

    // ** DISCOVERY ** //
    // Add the discovery endpoint that specifies where to publish the services
    serviceHost.Description.Endpoints.Add(new UdpDiscoveryEndpoint());

    // Open the ServiceHost to create listeners and start listening for messages.
    serviceHost.Open();

    // The service can now be accessed.
    Console.WriteLine("Press <ENTER> to terminate service.");
    Console.ReadLine();
}
```  
  
## <a name="service-discovery"></a>服务发现  

 客户端应用程序可以使用 <xref:System.ServiceModel.Discovery.DiscoveryClient> 类查找服务。 开发人员可创建 <xref:System.ServiceModel.Discovery.DiscoveryClient> 类的实例，用于传入指定 `Probe` 或 `Resolve` 消息的发送位置的发现终结点。 然后，客户端调用 <xref:System.ServiceModel.Discovery.DiscoveryClient.Find%2A>，用于指定 <xref:System.ServiceModel.Discovery.FindCriteria> 实例中的搜索条件。 如果找到匹配的服务，则 <xref:System.ServiceModel.Discovery.DiscoveryClient.Find%2A> 返回 <xref:System.ServiceModel.Discovery.EndpointDiscoveryMetadata> 的集合。 下面的代码演示如何调用 `Find` 方法并连接到已发现的服务。  
  
```csharp  
class Client
{
    static EndpointAddress serviceAddress;
  
    static void Main()
    {  
        if (FindService())
        {
            InvokeService();
        }
    }  
  
    // ** DISCOVERY ** //  
    static bool FindService()  
    {  
        Console.WriteLine("\nFinding Calculator Service ..");  
        DiscoveryClient discoveryClient =
            new DiscoveryClient(new UdpDiscoveryEndpoint());  
  
        Collection<EndpointDiscoveryMetadata> calculatorServices =
            (Collection<EndpointDiscoveryMetadata>)discoveryClient.Find(new FindCriteria(typeof(ICalculator))).Endpoints;  
  
        discoveryClient.Close();  
  
        if (calculatorServices.Count == 0)  
        {  
            Console.WriteLine("\nNo services are found.");  
            return false;  
        }  
        else  
        {  
            serviceAddress = calculatorServices[0].Address;  
            return true;  
        }  
    }  
  
    static void InvokeService()  
    {  
        Console.WriteLine("\nInvoking Calculator Service at {0}\n", serviceAddress);  
  
        // Create a client  
        CalculatorClient client = new CalculatorClient();  
        client.Endpoint.Address = serviceAddress;  
        client.Add(10,3);  
    }
}  
```  
  
## <a name="discovery-and-message-level-security"></a>发现和消息级别安全  

 使用消息级别安全时，需要在服务发现终结点上指定 <xref:System.ServiceModel.EndpointIdentity>，并在客户端发现终结点上指定匹配的 <xref:System.ServiceModel.EndpointIdentity>。 有关消息级别安全性的详细信息，请参阅 [消息安全](message-security-in-wcf.md)。  
  
## <a name="discovery-and-web-hosted-services"></a>发现和 Web 承载的服务  

 若要使 WCF 服务可发现，这些服务必须正在运行。 在 IIS/WAS 接收到为服务绑定的消息之前，IIS 或 WAS 下承载的 WCF 服务不会运行，因此这些服务在默认情况下不可发现。  使 Web 承载的服务可发现的两种方法：  
  
1. 使用 Windows Server AppFabric 自动启动功能  
  
2. 使用发现代理代表服务进行通信  
  
 Windows Server AppFabric 具有自动启动功能，该功能允许服务在接收到任何消息之前启动。 设置了此自动启动功能时，IIS/WAS 承载的服务可配置为可发现。 有关自动启动功能的详细信息，请参阅 [Windows Server AppFabric 自动启动功能](/previous-versions/appfabric/ee677260(v=azure.10))。 必须随打开自动启动功能一起，针对发现配置服务。 有关详细信息，请参阅 [如何：以编程方式向 WCF 服务添加可发现性和](how-to-programmatically-add-discoverability-to-a-wcf-service-and-client.md)[在配置文件中配置发现](configuring-discovery-in-a-configuration-file.md)的客户端。  
  
 发现代理可以用于在服务未运行时，代表 WCF 服务进行通信。 代理可以为进行探测而侦听，或解析消息及对客户端的响应。 客户端随后可以直接向服务发送消息。 当客户端向服务发送消息时，它将实例化以响应消息。 有关实现发现代理的详细信息，请参阅 [实现发现代理](implementing-a-discovery-proxy.md)。  
  
> [!NOTE]
> 为了使 WCF 发现正常工作，所有 Nic (网络接口控制器) 应只有1个 IP 地址。
