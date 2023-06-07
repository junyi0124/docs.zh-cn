---
title: 自定义消息筛选器
ms.date: 03/30/2017
ms.assetid: 98dd0af8-fce6-4255-ac32-42eb547eea67
ms.openlocfilehash: 24dab723a06c128337c1d956a98a1aa85a258e33
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96240922"
---
# <a name="custom-message-filter"></a>自定义消息筛选器

此示例演示如何替换 Windows Communication Foundation (WCF) 用于将消息调度到终结点的消息筛选器。  
  
> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。  
  
 当通道上的第一个消息到达服务器时，服务器必须确定哪个（如果有）与该 URI 关联的终结点应该接收消息。 此过程受附加到 <xref:System.ServiceModel.Dispatcher.MessageFilter> 的 <xref:System.ServiceModel.Dispatcher.EndpointDispatcher> 对象控制。  
  
 服务的每个终结点都有一个 <xref:System.ServiceModel.Dispatcher.EndpointDispatcher>。 <xref:System.ServiceModel.Dispatcher.EndpointDispatcher> 同时具有 <xref:System.ServiceModel.Dispatcher.EndpointDispatcher.AddressFilter%2A> 和 <xref:System.ServiceModel.Dispatcher.EndpointDispatcher.ContractFilter%2A>。 这两个筛选器的联合是用于该终结点的消息筛选器。  
  
 默认情况下，终结点的 <xref:System.ServiceModel.Dispatcher.EndpointDispatcher.AddressFilter%2A> 与发送到某个地址的任何消息匹配，该地址与服务终结点的 <xref:System.ServiceModel.EndpointAddress> 的匹配。 默认情况下， <xref:System.ServiceModel.Dispatcher.EndpointDispatcher.ContractFilter%2A> 终结点的会检查传入消息的操作，并将与服务终结点协定操作的操作之一对应的任何消息匹配 (仅 `IsInitiating` = `true` 将操作视为) 。 因此，默认情况下，仅当消息的 To 标头为终结点的 <xref:System.ServiceModel.EndpointAddress> 并且消息的动作与终结点操作的动作之一匹配时，终结点的筛选器才与此消息匹配。  
  
 使用行为可以更改这些筛选器。 在本示例中，服务创建一个 <xref:System.ServiceModel.Description.IEndpointBehavior>，该行为替换 <xref:System.ServiceModel.Dispatcher.EndpointDispatcher.AddressFilter%2A> 上的 <xref:System.ServiceModel.Dispatcher.EndpointDispatcher.ContractFilter%2A> 和 <xref:System.ServiceModel.Dispatcher.EndpointDispatcher>：  
  
```csharp
class FilteringEndpointBehavior : IEndpointBehavior
{
    //...
}
```  
  
 定义两个地址筛选器：  
  
```csharp
// Matches any message whose To address contains the letter 'e'  
class MatchEAddressFilter : MessageFilter { }
// Matches any message whose To address does not contain the letter 'e'  
class MatchNoEAddressFilter : MessageFilter { }  
```  
  
 使 `FilteringEndpointBehavior` 变得可配置并允许两个不同变体。  
  
```csharp
public class FilteringEndpointBehaviorExtension : BehaviorExtensionElement { }
```  
  
 变体 1 仅匹配包含“e”的地址（但具有任何操作），而变体 2 仅匹配不含“e”的地址：  
  
```csharp
if (Variation == 1)  
    return new FilteringEndpointBehavior(  
        new MatchEAddressFilter(), new MatchAllMessageFilter());  
else  
    return new FilteringEndpointBehavior(  
        new MatchNoEAddressFilter(), new MatchAllMessageFilter());  
```  
  
 在配置文件中，服务注册新行为：  
  
```xml  
<extensions>  
    <behaviorExtensions>  
        <add name="filteringEndpointBehavior" type="Microsoft.ServiceModel.Samples.FilteringEndpointBehaviorExtension, service" />  
    </behaviorExtensions>  
</extensions>
```  
  
 然后，服务为每个变体创建 `endpointBehavior` 配置：  
  
```xml  
<endpointBehaviors>  
    <behavior name="endpoint1">  
        <filteringEndpointBehavior variation="1" />  
    </behavior>  
    <behavior name="endpoint2">  
        <filteringEndpointBehavior variation="2" />  
    </behavior>  
</endpointBehaviors>  
```  
  
 最后，服务的终结点引用 `behaviorConfigurations` 之一：  
  
```xml  
<endpoint address=""  
        bindingConfiguration="ws"  
        listenUri=""
        binding="wsHttpBinding"  
        contract="Microsoft.ServiceModel.Samples.IHello"
        behaviorConfiguration="endpoint2" />  
```  
  
 客户端应用程序的实现非常简单；它通过将该值作为到 `via` 的第二个 (<xref:System.ServiceModel.Channels.IChannelFactory%601.CreateChannel%28System.ServiceModel.EndpointAddress%29>) 参数传入并在每个通道上发送单个消息，但分别使用不同的终结点地址，创建两个到达服务的 URI 的通道。 因此，来自客户端的出站消息具有不同的 To 标记，服务器做出相应响应，如客户端的输出所示：  
  
```console  
Sending message to urn:e...  
Exception: The message with To 'urn:e' cannot be processed at the receiver, due to an AddressFilter mismatch at the EndpointDispatcher.  Check that the sender and receiver's EndpointAddresses agree.  
  
Sending message to urn:a...  
Hello  
```  
  
 切换服务器配置文件中的变体导致交换筛选器，客户端看到相反的行为（到 `urn:e` 的消息成功，而到 `urn:a` 的消息失败）。  
  
```xml  
<endpoint address=""  
          bindingConfiguration="ws"  
          listenUri=""
          binding="wsHttpBinding"  
          contract="Microsoft.ServiceModel.Samples.IHello"
          behaviorConfiguration="endpoint1" />  
```  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\MessageFilter`  
  
### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 若要生成解决方案，请按照 [生成 Windows Communication Foundation 示例](building-the-samples.md)中的说明进行操作。  
  
2. 若要在单计算机配置中运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。  
  
3. 若要在跨计算机配置中运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md) 中的说明进行操作，并在 Client.cs 中更改以下行。  
  
    ```csharp
    Uri serviceVia = new Uri("http://localhost/ServiceModelSamples/service.svc");  
    ```  
  
     用服务器的名称替换 localhost。  
  
    ```csharp
    Uri serviceVia = new Uri("http://servermachinename/ServiceModelSamples/service.svc");  
    ```  
