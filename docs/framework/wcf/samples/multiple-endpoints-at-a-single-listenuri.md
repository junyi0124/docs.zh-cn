---
title: 在单个 ListenUri 中承载多个终结点
ms.date: 03/30/2017
ms.assetid: 911ffad4-4d47-4430-b7c2-79192ce6bcbd
ms.openlocfilehash: 09696ec8170915f29dae7510f8953565bcc67436
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96260183"
---
# <a name="multiple-endpoints-at-a-single-listenuri"></a>在单个 ListenUri 中承载多个终结点

此示例演示了一个在单个 `ListenUri` 中承载多个终结点的服务。 此示例基于实现计算器服务的 [入门](getting-started-sample.md) 。  
  
> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。  
  
 如 [多终结点](multiple-endpoints.md) 示例中所示，服务可以托管多个终结点，每个终结点具有不同的地址，并且可能也有不同的绑定。 此示例演示了可以在同一个地址承载多个终结点， 还演示了服务终结点所具有的两种地址（`EndpointAddress` 和 `ListenUri`）之间的区别。  
  
 `EndpointAddress` 是服务的逻辑地址， 它是 SOAP 消息所发送到的地址。 `ListenUri` 是服务的物理地址， 它具有服务终结点在当前计算机上实际侦听消息时使用的端口和地址信息。 在多数情况下，无需对这些地址进行区分；如果没有明确指定 `ListenUri`，则地址默认为终结点 `EndpointAddress` 的 URI。 在少数情况下（例如，在配置路由器时），有必要区分这些地址，因为路由器可能会接受发送到大量服务的消息。  
  
## <a name="service"></a>服务  

 此示例中的服务有两个协定：`ICalculator` 和 `IEcho`。 除了通常的 `IMetadataExchange` 终结点，还有三个应用程序终结点，如下面的代码中所示。  
  
```xml  
<endpoint address="urn:Stuff"  
        binding="wsHttpBinding"  
        contract="Microsoft.ServiceModel.Samples.ICalculator"
        listenUri="http://localhost/servicemodelsamples/service.svc" />  
<endpoint address="urn:Stuff"  
        binding="wsHttpBinding"  
        contract="Microsoft.ServiceModel.Samples.IEcho"
        listenUri="http://localhost/servicemodelsamples/service.svc" />  
<endpoint address="urn:OtherEcho"  
        binding="wsHttpBinding"  
        contract="Microsoft.ServiceModel.Samples.IEcho"
        listenUri="http://localhost/servicemodelsamples/service.svc" />  
```  
  
 所有这三个终结点都在同一个 `ListenUri` 中承载，而且使用相同的 `binding`（即，位于同一个 `ListenUri` 的终结点必须具有相同的绑定），这是由于它们共享一个通道堆栈，而且该通道堆栈在计算机中的该物理地址上侦听消息。 每个终结点的 `address` 都是一个 URN；尽管地址通常表示物理位置，但实际上地址可以是任何种类的 URI，因为地址可用于匹配和筛选目的，如该示例中所示。  
  
 由于所有三个终结点共享相同的 `ListenUri` 消息，因此当消息到达该处时，Windows Communication Foundation (WCF) 必须确定消息的目标终结点。 每个终结点都有一个消息筛选器，该消息筛选器由地址筛选器和协定筛选器两部分组成。 地址筛选器将 SOAP 消息的 `To` 与服务终结点的地址相匹配。 例如，只有发送到 `To "Urn:OtherEcho"` 的消息才是该服务的第三个候选终结点。 协定筛选器与那些与特定协定的操作相关联的 Action 相匹配。 例如，具有 `IEcho` 操作的消息。 `Echo` 与该服务的第二个终结点和第三个终结点的协定筛选器均匹配，因为这两个终结点均承载 `IEcho` 协定。  
  
 因此，有了地址筛选器和协定筛选器的这一组合，可以将到达该服务 `ListenUri` 的每条消息路由到正确的终结点。 第三个终结点与其他两个终结点不同，因为它接受从其他终结点发送到另一个地址的消息。 前两个终结点的区别在于它们的协定（传入消息的 Action）不同。  
  
## <a name="client"></a>客户端  

 正如服务器上的终结点有两个不同的地址一样，客户端终结点也有两个地址。 在服务器和客户端上，逻辑地址均称为 `EndpointAddress`。 但是，物理地址在服务器上称为 `ListenUri`，在客户端上称为 `Via`。  
  
 而在服务器上，这两个地址在默认情况下是相同的。 若要在客户端上指定一个不同于终结点地址的 `Via`，可以使用 `ClientViaBehavior`：  
  
```csharp  
Uri via = new Uri("http://localhost/ServiceModelSamples/service.svc");  
CalculatorClient calcClient = new CalculatorClient();  
calcClient.ChannelFactory.Endpoint.Behaviors.Add(  
        new ClientViaBehavior(via));  
```  
  
 和平常一样，地址来自由 Svcutil.exe 生成的客户端配置文件。 `Via`（与服务的 `ListenUri` 相对应）不出现在服务的元数据中，因此该信息必须在带外传递到客户端（就像服务的元数据地址一样）。  
  
 此示例中的客户端将消息发送到每台服务器的三个应用程序终结点，以演示它可以与所有这三个终结点通信并能够对这些终结点进行区分，即使这些终结点具有相同的 `Via` 也是如此。  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。  
  
3. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。  
  
    > [!NOTE]
    > 若要跨计算机运行，则必须将 Client.cs 文件中的 localhost 替换为服务计算机的名称。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\MultipleEndpointsSingleUri`  
