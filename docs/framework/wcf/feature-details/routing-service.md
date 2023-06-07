---
title: 路由服务
ms.date: 03/30/2017
ms.assetid: ca7c216a-5141-4132-8193-102c181d2eba
ms.openlocfilehash: 833c824e17d70a982a2f7bb13fe388b9b2b0dec1
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84590444"
---
# <a name="routing-service"></a>路由服务

路由服务是充当消息路由器的泛型 SOAP 中介。 路由服务的核心功能是基于消息内容来路由消息，通过该功能，可基于消息本身（标头或消息正文）中的值将消息转发到客户端终结点。

<xref:System.ServiceModel.Routing.RoutingService>作为命名空间中的 Windows Communication Foundation （WCF）服务实现 <xref:System.ServiceModel.Routing> 。 路由服务公开一个或多个用于接收消息的服务终结点，然后基于消息内容将每个消息路由到一个或多个客户端终结点。 该服务提供了以下功能：

- 基于内容的路由

  - 服务聚合

  - 服务版本控制

  - 优先级路由

  - 动态配置

- 协议桥接

- SOAP 处理

- 高级错误处理

- 备份终结点

尽管可以创建可实现上述一个或多个目标的中介服务，但是此实现通常与特定方案或解决方案相关，并且不能轻松应用于新应用程序。

路由服务提供一个可动态配置的泛型可插入 SOAP 中介，与 WCF 服务和通道模型兼容，并允许您对基于 SOAP 的消息执行基于内容的路由。

> [!NOTE]
> 路由服务当前不支持路由 WCF REST 服务。  若要路由 REST 调用，请考虑使用 <xref:System.Web.Routing> 或[应用程序请求路由](https://go.microsoft.com/fwlink/?LinkId=164589)。

## <a name="content-based-routing"></a>基于内容的路由

基于内容的路由是指根据消息中包含的一个或多个值路由消息的能力。 路由服务检查每条消息，并根据消息内容和您创建的路由逻辑将其路由至目标终结点。 基于内容的路由为服务聚合、服务版本控制和优先级路由提供了基础。

路由服务依赖于 <xref:System.ServiceModel.Dispatcher.MessageFilter> 实现来完成基于内容的路由，此类实现用于匹配要路由的消息中的特定值。 如果**MessageFilter**与消息相匹配，则会将消息路由到与**MessageFilter**关联的目标终结点。  将若干个消息筛选器一起组合成筛选器表 (<xref:System.ServiceModel.Routing.Configuration.FilterTableCollection>) 可以构造复杂的路由逻辑。 例如，一个筛选器表可能包含五个互相排斥的消息筛选器，这会导致只将消息路由到五个目标终结点中的一个。

路由服务允许您配置执行基于内容的路由所采用的逻辑，并在运行时动态更新路由逻辑。

通过将消息筛选器组合到筛选器表，您可以构造路由逻辑来处理如下所示的多个路由方案：

- 服务聚合

- 服务版本控制

- 优先级路由

- 动态配置

有关消息筛选器和筛选器表的详细信息，请参阅[路由介绍](routing-introduction.md)和[消息筛选](message-filters.md)器。

### <a name="service-aggregation"></a>服务聚合

使用基于内容的路由，您可以公开一个用于接收外部客户端应用程序发送的消息的终结点，然后基于消息中的值将每条消息路由到相应的内部终结点。 这有助于为各种后端应用程序提供一个特定终结点，同时也有助于在将应用程序分解为各种服务时向客户提供一个应用程序终结点。

### <a name="service-versioning"></a>服务版本控制

当迁移到新版解决方案时，您可能需要并行维护旧版本，以便向现有客户提供服务。 通常，这要求连接到较新版本的客户端在与解决方案通信时必须使用不同的地址。 路由服务允许您根据消息中包含的版本特定信息将消息路由至相应解决方案，从而公开一个可为解决方案的两个版本提供服务的服务终结点。 有关此类实现的示例，请参阅[如何：服务版本控制](how-to-service-versioning.md)。

### <a name="priority-routing"></a>优先级路由

向多个客户端提供服务时，您可以与某些合作伙伴达成服务级别协议 (SLA)，该协议要求对来自这些合作伙伴的所有数据与其他客户端数据单独处理。 使用查找消息中包含的客户特定信息的筛选器，您可以轻松地将来自特定合作伙伴的消息路由至为满足其 SLA 需求而创建的终结点。

## <a name="dynamic-configuration"></a>动态配置

若要支持任务关键型系统（在其中处理消息时不得出现任何服务中断），能够在运行时修改系统中组件的配置非常重要。 为了支持此需求，路由服务提供了一种 <xref:System.ServiceModel.IExtension%601> 实现（即 <xref:System.ServiceModel.Routing.RoutingExtension>），该实现允许在运行时动态更新路由服务的配置。

有关路由服务的动态配置的详细信息，请参阅[路由简介](routing-introduction.md)。

## <a name="protocol-bridging"></a>协议桥接

中介方案面临的挑战之一是：内部终结点与接收消息的终结点可能具有不同的传输或 SOAP 版本要求。 若要支持此方案，路由服务可以桥接协议，包括根据目标终结点需要的 <xref:System.ServiceModel.Channels.MessageVersion> 处理 SOAP 消息。 这样，一个协议可用于内部通信，而其他协议可用于外部通信。

为了支持在具有不同传输方式的终结点之间路由消息，路由服务使用系统提供的绑定，这些绑定使服务可以起到不同协议之间的桥梁的作用。 如果路由服务公开的服务终结点与消息路由到的客户端终结点使用的协议不同，这种情况将自动发生。

## <a name="soap-processing"></a>SOAP 处理

常见路由要求是在具有不同 SOAP 要求的终结点之间路由消息。 为了支持此要求，路由服务提供了一个 <xref:System.ServiceModel.Routing.SoapProcessingBehavior> ，它会在消息路由到目标终结点之前自动创建满足目标终结点的要求的新**MessageVersion** 。 此行为还会在将响应消息返回给发出请求的客户端应用程序之前为任何响应消息创建新的**MessageVersion** ，以确保响应的 MessageVersion 与原始请求的**MessageVersion**相匹配。

有关 SOAP 处理的详细信息，请参阅[路由简介](routing-introduction.md)。

## <a name="error-handling"></a>错误处理

在由依赖于网络通信的分布式服务组成的系统中，确保系统中的通信不受暂时网络故障的影响是非常重要的。  路由服务会实现错误处理，这样您便可以处理多种通信故障情况。如果不处理这些故障，可能会导致服务中断。

如果路由服务在尝试发送消息时遇到 <xref:System.ServiceModel.CommunicationException>，将会进行错误处理。  这些异常通常指示在尝试与定义的客户端终结点进行通信时遇到了问题，例如 <xref:System.ServiceModel.EndpointNotFoundException>、<xref:System.ServiceModel.ServerTooBusyException> 或 <xref:System.ServiceModel.CommunicationObjectFaultedException>。  发生**TimeoutException**时，错误处理代码还将捕获并尝试重试发送，这是不是派生自**CommunicationException**的另一个常见异常。

有关错误处理的详细信息，请参阅[路由简介](routing-introduction.md)。

## <a name="backup-endpoints"></a>备份终结点

除了与筛选器表中的每个筛选器定义关联的目标客户端终结点，您还可以创建在传输失败时将消息路由到的备份终结点的列表。 如果为筛选器条目定义了备份列表，则一旦出现错误，路由服务会尝试将消息发送到列表中定义的第一个终结点。 如果此传输尝试失败，路由服务将尝试发送到下一个终结点，并一直继续此过程，直到传输尝试成功、返回与传输无关的错误或者备份列表中的所有终结点都返回传输错误。

有关备份终结点的详细信息，请参阅[路由简介](routing-introduction.md)和[消息筛选器](message-filters.md)。

## <a name="streaming"></a>流式处理

如果你设置绑定以支持流式处理，则路由服务可以成功对消息进行流式处理。  但是，在某些情况下可能需要对消息进行缓冲：

- 多播（缓冲以创建更多消息副本）

- 故障转移（缓冲以免消息需要发送到备份）

- System.ServiceModel.Routing.RoutingConfiguration.RouteOnHeadersOnly 为 false（缓冲以提供带有 MessageBuffer 的 MessageFilterTable，以便筛选器可以检查正文）

- 动态配置

## <a name="see-also"></a>另请参阅

- [路由简介](routing-introduction.md)
- [路由协定](routing-contracts.md)
- [消息筛选器](message-filters.md)
