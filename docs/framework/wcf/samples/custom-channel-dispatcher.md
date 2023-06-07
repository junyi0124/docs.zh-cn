---
title: 自定义通道调度程序
ms.date: 03/30/2017
ms.assetid: 813acf03-9661-4d57-a3c7-eeab497321c6
ms.openlocfilehash: 38d39cbba15c95a43444108d634d3c30524c70f4
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96241767"
---
# <a name="custom-channel-dispatcher"></a>自定义通道调度程序

此示例演示如何通过直接实现 <xref:System.ServiceModel.ServiceHostBase> 以自定义方式生成通道堆栈，以及如何在 Web 宿主环境中创建自定义通道调度程序。 通道调度程序与 <xref:System.ServiceModel.Channels.IChannelListener> 交互以接受通道并从通道堆栈中检索消息。 此示例还提供一个基本示例，用于演示如何使用 <xref:System.ServiceModel.Activation.VirtualPathExtension> 在 Web 宿主环境中生成通道堆栈。  
  
## <a name="custom-servicehostbase"></a>自定义 ServiceHostBase  

 此示例实现基类型， <xref:System.ServiceModel.ServiceHostBase> 而不是 <xref:System.ServiceModel.ServiceHost> 以演示如何将 WINDOWS COMMUNICATION FOUNDATION (WCF) stack 实现替换为在通道堆栈顶部使用自定义消息处理层。 重写虚拟方法 <xref:System.ServiceModel.ServiceHostBase.InitializeRuntime%2A> 可生成通道侦听器和通道调度程序。  
  
 若要实现 Web 承载的服务，请从 <xref:System.ServiceModel.Activation.VirtualPathExtension> 集合获取服务扩展 <xref:System.ServiceModel.ServiceHostBase.Extensions%2A> 并将其添加到 <xref:System.ServiceModel.Channels.BindingParameterCollection>，以便传输层知道如何基于宿主环境设置（即，Internet 信息服务 (IIS)/Windows 进程激活服务 (WAS) 设置）来配置通道侦听器。  
  
## <a name="custom-channel-dispatcher"></a>自定义通道调度程序  

 自定义通道调度程序可扩展 <xref:System.ServiceModel.Dispatcher.ChannelDispatcherBase> 类型。 此类型实现通道层编程逻辑。 在此示例中，请求-答复消息交换模式仅支持 <xref:System.ServiceModel.Channels.IReplyChannel>，但自定义通道调度程序可轻松扩展为其他通道类型。  
  
 调度程序首先打开通道侦听器，然后接受单一答复通道。 通过该通道，调度程序开始在一个无限循环中发送消息（请求）。 对于每个请求，它都会创建一条答复消息，并将其发送回客户端。  
  
## <a name="creating-a-response-message"></a>创建响应消息  

 消息处理是在 `MyServiceManager` 类型中实现的。 在 `HandleRequest` 方法中，将首先检查消息的 `Action` 标头以查看是否支持该请求。 定义了预定义的 SOAP 操作 " http://tempuri.org/HelloWorld/Hello " 以提供消息筛选。 这类似于的 WCF 实现中的服务协定概念 <xref:System.ServiceModel.ServiceHost> 。  
  
 对于正确的 SOAP 操作情况，此示例将检索请求的消息数据，并生成一个与该请求对应的响应，类似于在 <xref:System.ServiceModel.ServiceHost> 示例中所看到的内容。  
  
 在此示例中，通过返回自定义 HTML 消息对 HTTP-GET 谓词进行了特殊处理，以便您可以通过浏览器来浏览服务，查看是否已正确编译该服务。 如果 SOAP 操作不匹配，则发送回一条错误消息，指示不支持该请求。  
  
 此示例的客户端是一个普通的 WCF 客户端，它不会假设来自服务的任何内容。 因此，该服务经过专门设计，可匹配从普通 WCF 实现获得的内容 <xref:System.ServiceModel.ServiceHost> 。 因此，在客户端上仅需要一个服务协定。  
  
## <a name="using-the-sample"></a>使用示例  

 运行客户端应用程序将直接生成以下输出。  
  
```output  
Client is talking to a request/reply WCF service.
Type what you want to say to the server: Howdy  
Server replied: You said: Howdy. Message id: 1  
Server replied: You said: Howdy. Message id: 2  
Server replied: You said: Howdy. Message id: 3  
Server replied: You said: Howdy. Message id: 4  
Server replied: You said: Howdy. Message id: 5  
```  
  
 还可以通过浏览器来浏览该服务，以便在服务器上处理 HTTP-GET 消息。 这会让您获得一个格式合理的 HTML 文本。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Channels\CustomChannelDispatcher`
