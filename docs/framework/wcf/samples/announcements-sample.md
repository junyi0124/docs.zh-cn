---
title: 公告示例
ms.date: 03/30/2017
ms.assetid: 954a75e4-9a97-41d6-94fc-43765d4205a9
ms.openlocfilehash: c3824fb0dc7ab4169c309d1a5154127d6bc3b78f
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746997"
---
# <a name="announcements-sample"></a>公告示例

此示例显示如何使用发现功能的公告功能。 服务使用公告可以发出包含有关服务的元数据的公告消息。 默认情况下，服务启动时会发送 Hello 公告，服务关闭时会发送 Bye 公告。 这些公告可以进行多播，也可点到点发送。 此示例由两个项目组成，即服务项目和客户端项目。

## <a name="service"></a>服务

此项目包含一个自承载计算器服务。 在 `Main` 方法中，会创建一个服务主机，并向该主机添加服务终结点。 接下来创建 <xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior>。 若要启用公告，必须向 <xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior> 添加公告终结点。 在此例中，添加一个标准终结点（使用 UDP 多播）作为公告终结点。 这会向已知的 UDP 地址广播公告。

```csharp
Uri baseAddress = new Uri("http://localhost:8000/" + Guid.NewGuid().ToString());

// Create a ServiceHost for the CalculatorService type.
using (ServiceHost serviceHost = new ServiceHost(typeof(CalculatorService), baseAddress))
{
     serviceHost.AddServiceEndpoint(typeof(ICalculatorService), new WSHttpBinding(), String.Empty);

     ServiceDiscoveryBehavior serviceDiscoveryBehavior = new ServiceDiscoveryBehavior();

     // Announce the availability of the service over UDP multicast
    serviceDiscoveryBehavior.AnnouncementEndpoints.Add(new UdpAnnouncementEndpoint());

    // Make the service discoverable over UDP multicast.
    serviceHost.Description.Behaviors.Add(serviceDiscoveryBehavior);
    serviceHost.AddServiceEndpoint(new UdpDiscoveryEndpoint());
    serviceHost.Open();
    // ...
}
```

## <a name="client"></a>客户端

在此项目中请注意，客户端承载 <xref:System.ServiceModel.Discovery.AnnouncementService>。 此外，会向事件注册两个委托。 这些事件指示客户端在接收到联机和脱机公告时执行的操作。

```csharp
// Create an AnnouncementService instance
AnnouncementService announcementService = new AnnouncementService();

// Subscribe the announcement events
announcementService.OnlineAnnouncementReceived += OnOnlineEvent;
announcementService.OfflineAnnouncementReceived += OnOfflineEvent;
```

`OnOnlineEvent` 和 `OnOfflineEvent` 方法分别处理 Hello 和 Bye 公告消息。

```csharp
static void OnOnlineEvent(object sender, AnnouncementEventArgs e)
{
    Console.WriteLine();
    Console.WriteLine("Received an online announcement from {0}:", e.AnnouncementMessage.EndpointDiscoveryMetadata.Address);
PrintEndpointDiscoveryMetadata(e.AnnouncementMessage.EndpointDiscoveryMetadata);
}

static void OnOfflineEvent(object sender, AnnouncementEventArgs e)
{
    Console.WriteLine();
    Console.WriteLine("Received an offline announcement from {0}:", e.AnnouncementMessage.EndpointDiscoveryMetadata.Address);
            PrintEndpointDiscoveryMetadata(e.AnnouncementMessage.EndpointDiscoveryMetadata);
}
```

#### <a name="to-use-this-sample"></a>使用此示例

1. 此示例使用 HTTP 终结点，若要运行此示例，必须添加正确的 URL ACL。 有关详细信息，请参阅[配置 HTTP 和 HTTPS](../feature-details/configuring-http-and-https.md)。 使用提升的特权执行下面的命令应添加相应的 ACL。 如果该命令无效，则可能需要使用你的域和用户名替换以下自变量。 `netsh http add urlacl url=http://+:8000/ user=%DOMAIN%\%UserName%`

2. 生成解决方案。

3. 运行 client.exe 应用程序。

4. 运行 service.exe 应用程序。 请注意，客户端会接收联机公告。

5. 关闭 service.exe 应用程序。 请注意，客户端会接收脱机公告。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Discovery\Announcements`
