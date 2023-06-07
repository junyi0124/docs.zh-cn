---
title: 客户端体系结构
ms.date: 03/30/2017
ms.assetid: 02624403-0d77-41cb-9a86-ab55e98c7966
ms.openlocfilehash: d2a7c25b73562155a7120ecee4998b0de364ca45
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96295101"
---
# <a name="client-architecture"></a>客户端体系结构

应用程序使用 Windows Communication Foundation (WCF) 客户端对象来调用服务操作。 本主题讨论 WCF 客户端对象、WCF 客户端信道及其与基础通道体系结构的关系。 有关 WCF 客户端对象的基本概述，请参阅 [Wcf 客户端概述](../wcf-client-overview.md)。 有关通道层的详细信息，请参阅 [扩展通道层](../extending/extending-the-channel-layer.md)。  
  
## <a name="overview"></a>概述  

 服务模型运行时创建 WCF 客户端，这些客户端由以下内容组成：  
  
- 自动生成的服务协定客户端实现，该实现可以将应用程序代码中的调用转换为传出消息，将响应消息转换为输出参数并返回应用程序可以检索的值。  
  
- 控制接口 (<xref:System.ServiceModel.IClientChannel?displayProperty=nameWithType>) 的实现，它可将各个接口组合在一起，并提供对控制功能（特别是用于关闭客户端会话和释放通道的能力）的访问。  
  
- 基于由使用的绑定指定的配置设置而生成的客户端通道。  
  
 应用程序可以按需创建这类客户端， <xref:System.ServiceModel.ChannelFactory?displayProperty=nameWithType> 或者通过创建派生类的实例， <xref:System.ServiceModel.ClientBase%601> 因为它是由 "工作的 [元数据实用工具" 工具生成的， ( # A0) ](../servicemodel-metadata-utility-tool-svcutil-exe.md)。 这些预先生成的客户端类可以封装并委托给由 <xref:System.ServiceModel.ChannelFactory> 动态构造的客户端通道实现。 因此，客户端通道和生成这些客户端通道的客户端工厂是此处讨论的焦点。  
  
## <a name="client-objects-and-client-channels"></a>客户端对象和客户端通道  

 WCF 客户端的基接口是 <xref:System.ServiceModel.IClientChannel?displayProperty=nameWithType> 接口，该接口公开核心客户端功能以及的基本通信对象功能、的 <xref:System.ServiceModel.ICommunicationObject?displayProperty=nameWithType> 上下文功能以及的 <xref:System.ServiceModel.IContextChannel?displayProperty=nameWithType> 可扩展行为 <xref:System.ServiceModel.IExtensibleObject%601?displayProperty=nameWithType> 。  
  
 但 <xref:System.ServiceModel.IClientChannel> 接口并不定义服务协定本身。 这些是由服务协定接口声明的 (通常使用 [ ( # A0) ](../servicemodel-metadata-utility-tool-svcutil-exe.md)) 等工具从服务元数据生成的。 WCF 客户端类型同时扩展 <xref:System.ServiceModel.IClientChannel> 和目标服务协定接口，以使应用程序能够直接调用操作，同时还可以访问客户端运行时功能。 创建 WCF 客户端将 <xref:System.ServiceModel.ChannelFactory?displayProperty=nameWithType> 为 wcf 对象提供必要的信息，以创建可连接并与配置的服务终结点交互的运行时间。  
  
 如前文所述，必须先配置这两个 WCF 客户端类型，然后才能使用它们。 最简单的 WCF 客户端类型是派生自 <xref:System.ServiceModel.ClientBase%601> (的对象; <xref:System.ServiceModel.DuplexClientBase%601> 如果服务协定是双工协定) ，则为。 通过使用以编程方式进行配置的构造函数，或者通过使用配置文件，然后直接调用该配置文件以调用服务操作，可以创建这些类型。 有关对象的基本概述 <xref:System.ServiceModel.ClientBase%601> ，请参阅 [WCF 客户端概述](../wcf-client-overview.md)。  
  
 第二个类型是在运行时从对 <xref:System.ServiceModel.ChannelFactory%601.CreateChannel%2A> 方法的调用中生成的。 与严格控制通信细节相关的应用程序通常使用这种客户端类型，称为 *客户端信道对象*，因为它可以实现比底层客户端运行时和通道系统更直接的交互。  
  
## <a name="channel-factories"></a>通道工厂  

 负责创建可支持客户端调用的基础运行时的类为 <xref:System.ServiceModel.ChannelFactory%601?displayProperty=nameWithType> 类。 WCF 客户端对象和 WCF 客户端通道对象均使用 <xref:System.ServiceModel.ChannelFactory%601> 对象来创建实例; <xref:System.ServiceModel.ClientBase%601> 派生的客户端对象封装通道工厂的处理，但对于许多方案，直接使用通道工厂是完全合理的。 对此，常见的情况是从现有工厂重复创建新的客户端通道。 如果你使用的是客户端对象，则可以通过调用属性来从 WCF 客户端对象获取基础通道工厂 <xref:System.ServiceModel.ClientBase%601.ChannelFactory%2A?displayProperty=nameWithType> 。  
  
 应该记住的是，在调用 <xref:System.ServiceModel.ChannelFactory%601.CreateChannel%2A?displayProperty=nameWithType> 之前，通道工厂会为所提供的配置创建客户端通道的新实例。 在对 <xref:System.ServiceModel.ChannelFactory%601.CreateChannel%2A> <xref:System.ServiceModel.ClientBase%601.Open%2A?displayProperty=nameWithType> <xref:System.ServiceModel.ClientBase%601.CreateChannel%2A?displayProperty=nameWithType> WCF 客户端对象) 调用 (或、或任何操作后，就不能修改通道工厂，并且应将通道获取到不同的服务实例，即使只是更改目标终结点地址也是如此。 如果您想用不同配置创建客户端对象或客户端通道，则必须首先创建新的通道工厂。  
  
 有关使用 WCF 客户端对象和 WCF 客户端通道的各种问题的详细信息，请参阅 [使用 Wcf 客户端访问服务](accessing-services-using-a-client.md)。  
  
 以下两节介绍了如何创建和使用 WCF 客户端通道对象。  
  
#### <a name="creating-a-new-wcf-client-channel-object"></a>创建新的 WCF 客户端通道对象  

 为了说明客户端通道的用法，假设已生成以下服务协定。  
  
 [!code-csharp[C_GeneratedCodeFiles#12](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_generatedcodefiles/cs/proxycode.cs#12)]  
  
 若要连接到 `ISampleService` 服务，请直接与通道工厂 (<xref:System.ServiceModel.ChannelFactory%601>) 一起使用生成的协定接口。 一旦为特定协定创建并配置通道工厂后，即可以调用 <xref:System.ServiceModel.ChannelFactory%601.CreateChannel%2A> 方法以返回可用于与 `ISampleService` 服务通信的客户端通道对象。  
  
 当 <xref:System.ServiceModel.ChannelFactory%601> 类与服务协定接口一起使用时，您必须强制转换到 <xref:System.ServiceModel.IClientChannel> 接口以显式打开、关闭或中止通道。 为便于操作，Svcutil.exe 工具还生成一个帮助器接口，该接口可同时实现服务协定接口和 <xref:System.ServiceModel.IClientChannel> ，以使您无需进行强制转换就能够与客户端通道基础结构进行交互。 下面的代码演示实现上述服务协定的帮助器客户端通道的定义。  
  
 [!code-csharp[C_GeneratedCodeFiles#13](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_generatedcodefiles/cs/proxycode.cs#13)]  
  
#### <a name="creating-a-new-wcf-client-channel-object"></a>创建新的 WCF 客户端通道对象  

 若要使用客户端通道连接到 `ISampleService` 服务，请直接与通道工厂一起使用生成的协定接口（或帮助器版本），将协定接口的类型作为类型参数进行传递。 一旦为特定协定创建并配置通道工厂后，即可以调用 <xref:System.ServiceModel.ChannelFactory%601.CreateChannel%2A?displayProperty=nameWithType> 方法以返回可用于与 `ISampleService` 服务通信的客户端通道对象。  
  
 客户端通道对象在创建后可实现 <xref:System.ServiceModel.IClientChannel> 和协定接口。 因此，可以直接使用这些对象来调用操作，与支持该协定的服务进行交互。  
  
 使用客户端对象和客户端通道对象之间的区别仅仅在于其易于使用性和开发人员的喜好。 很多熟悉使用类和对象的开发人员将更喜欢使用 WCF 客户端对象，而不是 WCF 客户端信道。  
  
 有关示例，请参阅 [如何：使用 ChannelFactory](how-to-use-the-channelfactory.md)。
