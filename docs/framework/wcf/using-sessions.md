---
title: 使用会话
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- sessions [WCF]
ms.assetid: 864ba12f-3331-4359-a359-6d6d387f1035
ms.openlocfilehash: 42159b3871d974e53751468b422cbe9f72b6f908
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96273680"
---
# <a name="using-sessions"></a>使用会话

在 Windows Communication Foundation (WCF) 应用程序， *会话* 将一组消息关联到会话中。 WCF 会话不同于 ASP.NET 应用程序中提供的会话对象，支持不同的行为，并且以不同的方式进行控制。 本主题介绍了在 WCF 应用程序中启用会话的功能以及如何使用这些功能。  
  
## <a name="sessions-in-windows-communication-foundation-applications"></a>Windows Communication Foundation 应用程序中的会话  

 当某个服务协定指定它需要会话时，该协定会指定所有调用（即，支持调用的基础消息交换）必须是同一对话的一部分。 如果某个协定指定它允许使用会话但不要求使用会话，则客户端可以进行连接，并选择建立会话或不建立会话。 如果会话结束，然后在同一个通道上发送消息，将会引发异常。  
  
 WCF 会话具有以下主要概念功能：  
  
- 它们由调用应用程序（WCF 客户端）显式启动和终止。  
  
- 会话期间传递的消息按照接收消息的顺序进行处理。  
  
- 会话将一组消息相互关联，从而形成对话。 可以有不同类型的关联。 例如，一个基于会话的通道可能会根据共享网络连接来关联消息，而另一个基于会话的通道可能会根据消息正文中的共享标记来关联消息。 可以从会话派生的功能取决于关联的性质。  
  
- 没有与 WCF 会话相关联的常规数据存储区。  
  
 如果你熟悉 <xref:System.Web.SessionState.HttpSessionState?displayProperty=nameWithType> ASP.NET 应用程序中的类以及它提供的功能，你可能会注意到这种类型的会话和 WCF 会话之间存在以下差异：  
  
- ASP.NET 会话始终由服务器启动。  
  
- ASP.NET 会话是隐式无序的。  
  
- ASP.NET 会话提供跨请求的常规数据存储机制。  
  
 本主题介绍：  
  
- 在服务模型层中使用基于会话的绑定时的默认执行行为。  
  
- 基于 WCF 会话的、系统提供的绑定所提供的功能的类型。  
  
- 如何创建声明会话要求的协定。  
  
- 如何了解和控制会话的创建和终止以及会话与服务实例的关系。  
  
## <a name="default-execution-behavior-using-sessions"></a>使用会话的默认执行行为  

 尝试启动会话的绑定称为基于会话的  绑定。 通过将服务协定接口（或类）上的 <xref:System.ServiceModel.ServiceContractAttribute.SessionMode%2A?displayProperty=nameWithType> 属性设置为 <xref:System.ServiceModel.SessionMode?displayProperty=nameWithType> 枚举值之一，服务协定指定它们要求、允许或拒绝基于会话的绑定。 默认情况下，此属性的值为 <xref:System.ServiceModel.SessionMode.Allowed> ，这意味着，如果客户端使用基于会话的绑定和 WCF 服务实现，则服务将建立并使用提供的会话。  
  
 当 WCF 服务接受客户端会话时，默认情况下会启用以下功能：  
  
1. WCF 客户端对象之间的所有调用都由同一个服务实例处理。  
  
2. 不同的基于会话的绑定还会提供其他功能。  
  
## <a name="system-provided-session-types"></a>系统提供的会话类型  

 基于会话的绑定支持服务实例与特定会话的默认关联。 但是，除了启用前面介绍的基于会话的实例化控制之外，不同的基于会话的绑定还支持不同的功能。  
  
 WCF 提供了以下类型的基于会话的应用程序行为：  
  
- <xref:System.ServiceModel.Channels.SecurityBindingElement?displayProperty=nameWithType> 支持基于安全的会话，其中，两个通信端采用统一的安全对话。 有关详细信息，请参阅 [保护服务](securing-services.md)。 例如， <xref:System.ServiceModel.WSHttpBinding?displayProperty=nameWithType> 绑定（包含对安全会话和可靠会话的支持）默认情况下只使用对消息进行加密和数字签名的安全会话。  
  
- <xref:System.ServiceModel.NetTcpBinding?displayProperty=nameWithType> 绑定支持基于 TCP/IP 的会话，以确保所有消息都由套接字级别的连接进行关联。  
  
- <xref:System.ServiceModel.Channels.ReliableSessionBindingElement?displayProperty=nameWithType> 元素实现 WS-ReliableMessaging 规范，并提供对可靠会话的支持。在可靠会话中，可以配置消息以按顺序传递并且只传递一次，从而使消息在对话期间即使经过多个节点也可以确保收到。 有关详细信息，请参阅 [可靠会话](./feature-details/reliable-sessions.md)。  
  
- <xref:System.ServiceModel.NetMsmqBinding?displayProperty=nameWithType> 绑定提供 MSMQ 数据报会话。 有关详细信息，请参阅 [WCF 中的队列](./feature-details/queues-in-wcf.md)。  
  
 设置 <xref:System.ServiceModel.ServiceContractAttribute.SessionMode%2A> 属性并不指定协定需要的会话类型，而只是指定协定需要一个会话。  
  
## <a name="creating-a-contract-that-requires-a-session"></a>创建一个需要会话的协定  

 创建需要会话的协定的原则是，必须在同一会话内执行服务协定声明的所有操作组，并且必须按顺序传递消息。 若要断言服务协定需要的会话支持级别，请将服务协定接口或类上的 <xref:System.ServiceModel.ServiceContractAttribute.SessionMode%2A?displayProperty=nameWithType> 属性设置为 <xref:System.ServiceModel.SessionMode?displayProperty=nameWithType> 枚举的值以指定协定是否：  
  
- 需要会话。  
  
- 允许客户端建立会话。  
  
- 禁止会话。  
  
 但是，设置 <xref:System.ServiceModel.ServiceContractAttribute.SessionMode%2A> 属性并不指定协定需要的基于会话的行为的类型。 它指示 WCF 在运行时确认配置的绑定 (这将为服务创建通信通道) ，不会，也不会在实现服务时建立会话。 同样，绑定可以使用它选择的任何类型的基于会话的行为（安全、传输、可靠或某种组合）来满足该要求。 具体的行为取决于选择的 <xref:System.ServiceModel.SessionMode?displayProperty=nameWithType> 值。 如果为服务配置的绑定不符合 <xref:System.ServiceModel.ServiceContractAttribute.SessionMode%2A>的值，则会引发异常。 绑定及其创建的支持会话的通道可认为是基于会话的。  
  
 下面的服务协定指定 `ICalculatorSession` 中的所有操作必须在会话中进行交换。 除了 `Equals` 方法之外，任何操作都不会向调用方返回值。 但是， `Equals` 方法不使用参数，因此，在已将其中的数据传递到其他操作的会话内部只可以返回非零值。 此协定需要会话正常工作。 如果没有与特定客户端相关联的会话，则服务实例无法知道此客户端前面已经发送了哪些数据。  
  
 [!code-csharp[S_Service_Session#1](../../../samples/snippets/csharp/VS_Snippets_CFX/s_service_session/cs/service.cs#1)]
 [!code-vb[S_Service_Session#1](../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_service_session/vb/service.vb#1)]  
  
 如果服务允许会话，则当客户端启动一个会话时将建立并使用该会话；否则，不建立会话。  
  
## <a name="sessions-and-service-instances"></a>会话和服务实例  

 如果使用 WCF 中的默认实例化行为，则 WCF 客户端对象之间的所有调用都由同一服务实例处理。 因此，在应用程序级别上，可以将会话视为启用与本地调用行为相似的应用程序行为。 例如，在创建本地对象时：  
  
- 调用构造函数。  
  
- 对 WCF 客户端对象引用所做的所有后续调用都由同一个对象实例处理。  
  
- 在销毁对象引用时调用析构函数。  
  
 只要使用默认的服务实例行为，会话就会在客户端和服务之间启用一个相似的行为。 如果服务协定需要或支持会话，则通过设置 <xref:System.ServiceModel.OperationContractAttribute.IsInitiating%2A> 和 <xref:System.ServiceModel.OperationContractAttribute.IsTerminating%2A> 属性，可以将一个或多个协定操作标记为启动或终止会话。  
  
 启动操作 是必须作为新会话的第一个操作而调用的操作。 只有在已调用至少一个启动操作之后才可以调用非启动操作。 因此，可以通过声明一个启动操作来为服务创建某种会话构造函数，启动操作应设计为从与服务实例的开始相对应的客户端接收输入。 （状态与会话相关联，但是，不与服务对象相关联。）  
  
 相反，终止操作是必须作为现有会话中的最后消息而调用的操作。 默认情况下，在关闭与服务相关联的会话之后，WCF 会回收服务对象及其上下文。 因此，可以通过声明终止操作来创建某种析构函数，终止操作应设计为执行与服务实例的结束相对应的函数。  
  
> [!NOTE]
> 尽管默认的行为与本地构造函数和析构函数有相似之处，但仅仅是相似。 任何 WCF 服务操作都可以是启动或终止操作，也可以同时为两者。 另外，在默认情况下，可以按任意顺序调用启动操作任意次数；一旦建立会话并与实例相关联后，便不会再创建其他会话，除非显式控制服务实例的生存期（通过操作 <xref:System.ServiceModel.InstanceContext?displayProperty=nameWithType> 对象）。 最后，状态与会话相关联，而不是与服务对象相关联。  
  
 例如， `ICalculatorSession` 在前面的示例中使用的约定要求 WCF 客户端对象首先在执行 `Clear` 任何其他操作之前调用操作，并且具有此 WCF 客户端对象的会话应在调用操作时终止 `Equals` 。 下面的代码示例演示强制执行这些要求的协定。 必须首先调用`Clear` 来启动会话，并且会话在调用 `Equals` 时结束。  
  
 [!code-csharp[SCA.IsInitiatingIsTerminating#1](../../../samples/snippets/csharp/VS_Snippets_CFX/sca.isinitiatingisterminating/cs/service.cs#1)]
 [!code-vb[SCA.IsInitiatingIsTerminating#1](../../../samples/snippets/visualbasic/VS_Snippets_CFX/sca.isinitiatingisterminating/vb/service.vb#1)]  
  
 服务不会启动与客户端的会话。 在 WCF 客户端应用程序中，基于会话的通道的生存期和会话本身的生存期之间存在直接关系。 因此，客户端可通过创建新的基于会话的通道来创建新会话，并通过正常关闭基于会话的通道来关闭现有的会话。 客户端通过调用以下项目之一来启动与服务终结点的会话：  
  
- 通过调用<xref:System.ServiceModel.ICommunicationObject.Open%2A?displayProperty=nameWithType> 返回的通道上的 <xref:System.ServiceModel.ChannelFactory%601.CreateChannel%2A?displayProperty=nameWithType>。  
  
- <xref:System.ServiceModel.ClientBase%601.Open%2A?displayProperty=nameWithType>[ ( # A0) ](servicemodel-metadata-utility-tool-svcutil-exe.md)上的 WCF 客户端对象。  
  
- 默认情况下，对任一类型的 WCF 客户端对象 (启动操作都将) 启动所有操作。 调用第一个操作时，WCF 客户端对象自动打开通道并启动会话。  
  
 通常，客户端通过调用以下项目之一来结束与服务终结点的会话：  
  
- 通过调用<xref:System.ServiceModel.ICommunicationObject.Close%2A?displayProperty=nameWithType> 返回的通道上的 <xref:System.ServiceModel.ChannelFactory%601.CreateChannel%2A?displayProperty=nameWithType>。  
  
- <xref:System.ServiceModel.ClientBase%601.Close%2A?displayProperty=nameWithType> Svcutil.exe 生成的 WCF 客户端对象。  
  
- 默认情况下，对任一类型的 WCF 客户端对象 (终止操作，都不会终止任何操作;协定必须显式指定终止操作) 。 调用第一个操作时，WCF 客户端对象自动打开通道并启动会话。  
  
 有关示例，请参阅 [How to: Create a Service That Requires Sessions](./feature-details/how-to-create-a-service-that-requires-sessions.md) 以及 [Default Service Behavior](./samples/default-service-behavior.md) 和 [Instancing](./samples/instancing.md) 示例。  
  
 有关客户端和会话的详细信息，请参阅 [使用 WCF 客户端访问服务](./feature-details/accessing-services-using-a-client.md)。  
  
## <a name="sessions-interact-with-instancecontext-settings"></a>会话与 InstanceContext 设置进行交互  

 协定中的 <xref:System.ServiceModel.SessionMode> 枚举与 <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A?displayProperty=nameWithType> 属性之间存在交互，该属性可控制通道和特定服务对象之间的关联。 有关详细信息，请参阅 [会话、实例化和并发](./feature-details/sessions-instancing-and-concurrency.md)。  
  
### <a name="sharing-instancecontext-objects"></a>共享 InstanceContext 对象  

 通过自己执行关联，您还可以控制将哪个基于会话的通道或调用与哪个 <xref:System.ServiceModel.InstanceContext> 对象相关联。
  
## <a name="sessions-and-streaming"></a>会话和流  

 当有大量数据要传输时，WCF 中的流传输模式是一种可用于在内存中缓冲和处理消息的默认行为的可行替代方法。 在流与基于会话的绑定一起调用时可能会产生意外行为。 可通过单一通道（数据报通道）执行所有流调用，该通道不支持会话，即使将正在使用的绑定配置为使用会话也是如此。 如果多个客户端通过基于会话的绑定对相同的服务对象进行流调用，并且将服务对象的并发模式设置为 single，同时将其实例上下文模式设置为 `PerSession`，则所用的调用必须经过数据报通道，因此一次只处理一个调用。 一个或多个客户端可能会超时。可以通过将服务对象的设置为或将并发设置为 `InstanceContextMode` 多个，来解决此问题 `PerCall` 。  
  
> [!NOTE]
> MaxConcurrentSessions 在此情况下不会产生任何影响，因为只有一个“会话”可用。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.OperationContractAttribute.IsInitiating%2A>
- <xref:System.ServiceModel.OperationContractAttribute.IsTerminating%2A>
