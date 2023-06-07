---
title: 会话、实例化和并发
description: 了解会话、实例化和并发性，以及如何使用它们以及 WFC 中它们之间的交互。
ms.date: 03/30/2017
ms.assetid: 50797a3b-7678-44ed-8138-49ac1602f35b
ms.openlocfilehash: 7335d1c4d4ddf5247fc42c70cdc5e8ae33be7292
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96254202"
---
# <a name="sessions-instancing-and-concurrency"></a>会话、实例化和并发

 “会话”是在两个终结点之间发送的所有消息的一种相互关系。  “实例化”是指对用户定义的服务对象以及与其相关的 <xref:System.ServiceModel.InstanceContext> 对象的生存期的控制。  “并发”一词是指对 <xref:System.ServiceModel.InstanceContext> 中同时执行的线程数量的控制。  
  
 本主题描述这些设置、它们的使用方法以及它们之间的各种交互作用。  
  
## <a name="sessions"></a>会话  

 当服务协定将 <xref:System.ServiceModel.ServiceContractAttribute.SessionMode%2A?displayProperty=nameWithType> 属性设置为 <xref:System.ServiceModel.SessionMode.Required?displayProperty=nameWithType>时，该协定表示所有调用（即，支持调用的基础消息交换）都必须是同一个对话的一部分。 如果某个协定指定它允许使用会话但不要求使用会话，则客户端可以进行连接，并选择建立会话或不建立会话。 如果会话结束，然后在同一个基于会话的通道上发送消息，将会引发异常。  
  
 WCF 会话具有以下主要概念功能：  
  
- 它们由调用应用程序显式启动和终止。  
  
- 会话期间传递的消息按照接收消息的顺序进行处理。  
  
- 会话将一组消息相互关联，从而形成对话。 该关联的含义是抽象的。 例如，一个基于会话的通道可能会根据共享网络连接来关联消息，而另一个基于会话的通道可能会根据消息正文中的共享标记来关联消息。 可以从会话派生的功能取决于关联的性质。  
  
- 没有与 WCF 会话相关联的常规数据存储区。  
  
 如果你熟悉 <xref:System.Web.SessionState.HttpSessionState?displayProperty=nameWithType> ASP.NET 应用程序中的类以及它提供的功能，你可能会注意到这种类型的会话和 WCF 会话之间存在以下差异：  
  
- ASP.NET 会话始终由服务器启动。  
  
- ASP.NET 会话是隐式无序的。  
  
- ASP.NET 会话提供跨请求的常规数据存储机制。  
  
 客户端应用程序和服务应用程序以不同方式与会话交互。 客户端应用程序启动会话，然后接收并处理在该会话内发送的消息。 服务应用程序可以将会话用作扩展点，以添加其他行为。 通过直接使用 <xref:System.ServiceModel.InstanceContext> 或实现一个自定义实例上下文提供程序，可以做到这一点。  
  
## <a name="instancing"></a>实例化  

 实例化行为（使用 <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A?displayProperty=nameWithType> 属性进行设置）控制如何创建 <xref:System.ServiceModel.InstanceContext> 以响应传入的消息。 默认情况下，每个 <xref:System.ServiceModel.InstanceContext> 都与一个用户定义服务对象相关联，因此（在默认情况下）设置 <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A> 属性也可以控制用户定义服务对象的实例化。 <xref:System.ServiceModel.InstanceContextMode> 枚举定义了实例化模式。  
  
 可以使用下列实例化模式：  
  
- <xref:System.ServiceModel.InstanceContextMode.PerCall>：为每个客户端请求创建一个新的 <xref:System.ServiceModel.InstanceContext> （以及相应的服务对象）。  
  
- <xref:System.ServiceModel.InstanceContextMode.PerSession>：为每个新的客户端会话创建一个新的 <xref:System.ServiceModel.InstanceContext> （以及相应的服务对象），并在该会话的生存期内对其进行维护（这需要使用支持会话的绑定）。  
  
- <xref:System.ServiceModel.InstanceContextMode.Single>：单个 <xref:System.ServiceModel.InstanceContext> （以及相应的服务对象）处理应用程序生存期内的所有客户端请求。  
  
 下面的代码示例演示 <xref:System.ServiceModel.InstanceContextMode> 的默认值（在服务类上显式设置了 <xref:System.ServiceModel.InstanceContextMode.PerSession> ）。  
  
```csharp  
[ServiceBehavior(InstanceContextMode=InstanceContextMode.PerSession)]
public class CalculatorService : ICalculatorInstance
{
    ...  
}  
```  
  
 <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A?displayProperty=nameWithType> 属性控制释放 <xref:System.ServiceModel.InstanceContext> 的频率，而 <xref:System.ServiceModel.OperationBehaviorAttribute.ReleaseInstanceMode%2A?displayProperty=nameWithType> 和 <xref:System.ServiceModel.ServiceBehaviorAttribute.ReleaseServiceInstanceOnTransactionComplete%2A?displayProperty=nameWithType> 属性则控制释放服务对象的时间。  
  
### <a name="well-known-singleton-services"></a>已知的单一实例服务  

 有时，单个实例服务对象的变体是有用的：您可以自己创建一个服务对象，然后创建使用该对象的服务主机。 为此，您还必须将 <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A?displayProperty=nameWithType> 属性设置为 <xref:System.ServiceModel.InstanceContextMode.Single> ，否则在打开该服务主机时将引发异常。  
  
 可使用 <xref:System.ServiceModel.ServiceHost.%23ctor%28System.Object%2CSystem.Uri%5B%5D%29> 构造函数创建此类服务。 当您希望提供一个特定的对象实例供单一实例服务使用时，可以使用它作为实现自定义 <xref:System.ServiceModel.Dispatcher.IInstanceContextInitializer?displayProperty=nameWithType> 的替代方法。 如果服务实现类型难以构造，则可以使用此重载 (例如，如果它不实现无参数的公共构造函数) 。  
  
 请注意，向此构造函数提供对象时，与 Windows Communication Foundation (WCF) 实例化行为的某些功能的工作方式有所不同。 例如，在提供单一实例对象实例时，调用 <xref:System.ServiceModel.InstanceContext.ReleaseServiceInstance%2A?displayProperty=nameWithType> 没有任何效果。 同样，其他任何实例释放机制也都会被忽略。 <xref:System.ServiceModel.ServiceHost> 的行为总是像对于所有操作都将 <xref:System.ServiceModel.OperationBehaviorAttribute.ReleaseInstanceMode%2A?displayProperty=nameWithType> 属性设置为 <xref:System.ServiceModel.ReleaseInstanceMode.None?displayProperty=nameWithType> 一样。  
  
### <a name="sharing-instancecontext-objects"></a>共享 InstanceContext 对象  

 通过自己执行关联，您还可以控制将哪个有会话通道或调用与哪个 <xref:System.ServiceModel.InstanceContext> 对象相关联。  
  
## <a name="concurrency"></a>并发  

 并发是对 <xref:System.ServiceModel.InstanceContext> 中在任一时刻处于活动状态的线程数量的控制。 此控制是通过将 <xref:System.ServiceModel.ServiceBehaviorAttribute.ConcurrencyMode%2A?displayProperty=nameWithType> 与 <xref:System.ServiceModel.ConcurrencyMode> 枚举结合使用来实现的。  
  
 有以下三种可用的并发模式：  
  
- <xref:System.ServiceModel.ConcurrencyMode.Single>：最多允许每个实例上下文同时拥有一个对该实例上下文中的消息进行处理的线程。 其他希望使用同一个实例上下文的线程必须一直阻塞，直到原始线程退出该实例上下文为止。  
  
- <xref:System.ServiceModel.ConcurrencyMode.Multiple>：每个服务实例都可以拥有多个同时处理消息的线程。 若要使用此并发模式，服务实现必须是线程安全的。  
  
- <xref:System.ServiceModel.ConcurrencyMode.Reentrant>：每个服务实例一次只能处理一个消息，但可以接受可重入的操作调用。 服务仅在通过 WCF 客户端对象调用时接受这些调用。  
  
> [!NOTE]
> 理解和开发能够安全地使用多个线程的代码可能比较困难。 在使用 <xref:System.ServiceModel.ConcurrencyMode.Multiple> 或 <xref:System.ServiceModel.ConcurrencyMode.Reentrant> 值之前，应确保已针对这些模式对服务进行了适当设计。 有关详细信息，请参阅 <xref:System.ServiceModel.ServiceBehaviorAttribute.ConcurrencyMode%2A>。  
  
 并发的使用与实例化模式有关。 在 <xref:System.ServiceModel.InstanceContextMode.PerCall> 实例化中，并发性不相关，因为每条消息都由一个新的处理， <xref:System.ServiceModel.InstanceContext> 因此，中的每个线程都不会处于活动状态 <xref:System.ServiceModel.InstanceContext> 。  
  
 下面的代码示例演示如何将 <xref:System.ServiceModel.ServiceBehaviorAttribute.ConcurrencyMode%2A> 属性设置为 <xref:System.ServiceModel.ConcurrencyMode.Multiple>。  
  
```csharp
[ServiceBehavior(ConcurrencyMode=ConcurrencyMode.Multiple, InstanceContextMode = InstanceContextMode.Single)]
public class CalculatorService : ICalculatorConcurrency
{
    ...  
}  
```  
  
## <a name="sessions-interact-with-instancecontext-settings"></a>会话与 InstanceContext 设置进行交互  

 会话和 <xref:System.ServiceModel.InstanceContext> 根据协定中 <xref:System.ServiceModel.SessionMode> 枚举的值和服务实现上的 <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A?displayProperty=nameWithType> 属性的组合进行交互，该组合控制着通道和特定服务对象之间的联系。  
  
 下表显示了在给定服务的 <xref:System.ServiceModel.ServiceContractAttribute.SessionMode%2A?displayProperty=nameWithType> 属性值和 <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A?displayProperty=nameWithType> 属性值组合的情况下，支持会话或不支持会话的传入通道的结果。  
  
|InstanceContextMode 值|<xref:System.ServiceModel.SessionMode.Required>|<xref:System.ServiceModel.SessionMode.Allowed>|<xref:System.ServiceModel.SessionMode.NotAllowed>|  
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|  
|PerCall|-会话通道的行为：会话和 <xref:System.ServiceModel.InstanceContext> 每个调用。<br />-无会话通道的行为：将引发异常。|-会话通道的行为：会话和 <xref:System.ServiceModel.InstanceContext> 每个调用。<br />-无会话通道的行为： <xref:System.ServiceModel.InstanceContext> 每个调用的。|-会话通道的行为：将引发异常。<br />-无会话通道的行为： <xref:System.ServiceModel.InstanceContext> 每个调用的。|  
|PerSession|-会话通道的行为：会话和 <xref:System.ServiceModel.InstanceContext> 每个通道。<br />-无会话通道的行为：将引发异常。|-会话通道的行为：会话和 <xref:System.ServiceModel.InstanceContext> 每个通道。<br />-无会话通道的行为： <xref:System.ServiceModel.InstanceContext> 每个调用的。|-会话通道的行为：将引发异常。<br />-无会话通道的行为： <xref:System.ServiceModel.InstanceContext> 每个调用的。|  
|Single|-会话通道的行为：一个会话，一个 <xref:System.ServiceModel.InstanceContext> 用于所有调用。<br />-无会话通道的行为：将引发异常。|-会话通道的行为：会话以及 <xref:System.ServiceModel.InstanceContext> 创建的或用户指定的单一实例。<br />-无会话通道的行为： <xref:System.ServiceModel.InstanceContext> 用于创建的或用户指定的单一实例的。|-会话通道的行为：将引发异常。<br />-无会话通道的行为： <xref:System.ServiceModel.InstanceContext> 用于每个已创建的单一实例或用户指定的单一实例的。|  
  
## <a name="see-also"></a>另请参阅

- [使用会话](../using-sessions.md)
- [如何：创建要求会话的服务](how-to-create-a-service-that-requires-sessions.md)
- [如何：控制服务实例化](how-to-control-service-instancing.md)
- [并发](../samples/concurrency.md)
- [实例化](../samples/instancing.md)
- [会话](../samples/session.md)
