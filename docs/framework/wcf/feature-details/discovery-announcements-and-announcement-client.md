---
title: 发现公告和公告客户端
ms.date: 03/30/2017
ms.assetid: 426c6437-f8d2-4968-b23a-18afd671aa4b
ms.openlocfilehash: 4ad0b3ea5c257fa3117c426391bd59ad7b560d4f
ms.sourcegitcommit: 581ab03291e91983459e56e40ea8d97b5189227e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2019
ms.locfileid: "70040182"
---
# <a name="discovery-announcements-and-announcement-client"></a>发现公告和公告客户端

WCF 发现功能使组件能够公布其可用性。 如果对某个服务进行了此配置，则该服务将发送 Hello 和 Bye 公告。 客户端或其他组件可以侦听此类公告消息并据此采取操作。 这就向客户端提供了另一种感知服务的方法。 公告功能具有多种用途，例如，如果服务频繁进入和退出网络，则相比搜索服务，公告是较好的替代方法。 通过此方法可以减少网络流量，并且客户端在收到公告时可立即了解到服务是进入还是退出状态。

## <a name="discovery-announcements"></a>发现公告

如果配置了公告的服务加入网络并可以被检测到，该服务将发送 Hello 消息以向侦听客户端公告其可用性。 该消息包含与发现相关的服务信息，如服务的协定、终结点地址和相关范围。 可以使用 <xref:System.ServiceModel.Discovery.AnnouncementEndpoint> 类指定公告消息的发送位置。 如果公告终结点是 <xref:System.ServiceModel.Discovery.UdpAnnouncementEndpoint>，则相应地多播 Hello 和 Bye；而如果公告终结点是单播，则消息将直接发送到指定终结点。

> [!NOTE]
> 在服务主机打开和关闭时发送公告。 如果这些调用没有正确完成，则可能不发出公告消息。例如，如果服务出错，则不发送 Bye 公告消息。

> [!TIP]
> 您可以自定义公告功能，在选择的任何时间发送公告。

[!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)]将 <xref:System.ServiceModel.Discovery.AnnouncementEndpoint> 和 <xref:System.ServiceModel.Discovery.UdpAnnouncementEndpoint> 定义为标准终结点，以允许服务和客户端方便地发送 Hello 和 Bye 公告。

### <a name="announcements-on-the-service"></a>服务上的公告

若要将服务配置为发送公告，请添加 <xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior> 以及公告终结点。 下面的示例演示如何以编程方式将此行为添加到服务主机。 此示例将使用 `UdpAnnouncementEndpoint`，这表明公告将多播到由该标准终结点指定的位置。

```csharp
ServiceDiscoveryBehavior serviceDiscoveryBehavior = new ServiceDiscoveryBehavior();
serviceDiscoveryBehavior.AnnouncementEndpoints.Add(new UdpAnnouncementEndpoint());
serviceHost.Description.Behaviors.Add(serviceDiscoveryBehavior);
```

在配置文件中也可配置该行为，如下面的示例所示。

```xml
<services>
  <service behaviorConfiguration="CalculatorBehavior" name="Microsoft.Samples.Discovery.CalculatorService">
    <!--Add Discovery Endpoint-->
    <endpoint name="udpDiscoveryEpt" kind="udpDiscoveryEndpoint" />
  </service>
</services>
<behaviors>
  <serviceBehaviors>
    <behavior name="CalculatorBehavior">
      <!--Add Discovery behavior-->
      <serviceDiscovery>
        <announcementEndpoints>
          <endpoint kind="udpAnnouncementEndpoint" />
        </announcementEndpoints>
      </serviceDiscovery>
    </behavior>
  </serviceBehaviors>
</behaviors>
```

前面的示例将服务配置为自动发送公告消息。 您还可以使用 <xref:System.ServiceModel.Discovery.AnnouncementClient> 类显式发送公告消息。

### <a name="announcements-on-the-client"></a>客户端上的公告

客户端应用程序必须承载公告服务，才能响应 Hello 和 Bye 消息以及订阅 <xref:System.ServiceModel.Discovery.AnnouncementService.OnlineAnnouncementReceived> 和 <xref:System.ServiceModel.Discovery.AnnouncementService.OfflineAnnouncementReceived> 事件。 下面的示例演示如何执行此操作。

```csharp
// Create an AnnouncementService instance
AnnouncementService announcementService = new AnnouncementService();

// Subscribe the announcement events
announcementService.OnlineAnnouncementReceived += OnOnlineEvent;
announcementService.OfflineAnnouncementReceived += OnOfflineEvent;

// Create ServiceHost for the AnnouncementService
using (ServiceHost announcementServiceHost = new ServiceHost(announcementService))
{
    // Listen for the announcements sent over UDP multicast
    announcementServiceHost.AddServiceEndpoint(new UdpAnnouncementEndpoint());
    announcementServiceHost.Open();

    Console.WriteLine("Press <ENTER> to terminate.");
    Console.ReadLine();
}
```

在收到 Hello 或 Bye 消息时，可以通过 <xref:System.ServiceModel.Discovery.AnnouncementEventArgs> 访问终结点发现元数据，如下面的示例所示。

```csharp
static void OnOnlineEvent(object sender, AnnouncementEventArgs e)
{
    Console.WriteLine("Received an online announcement from {0}",
e.EndpointDiscoveryMetadata.Address);
}

static void OnOfflineEvent(object sender, AnnouncementEventArgs e)
{
    Console.WriteLine("Received an offline announcement from {0}",
e.EndpointDiscoveryMetadata.Address);
}
```
