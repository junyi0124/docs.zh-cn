---
title: 创建 BindingElement
ms.date: 03/30/2017
ms.assetid: 01a35307-a41f-4ef6-a3db-322af40afc99
ms.openlocfilehash: 285bed029cf8487b37757de6a56075abe448f3ce
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96257862"
---
# <a name="creating-a-bindingelement"></a>创建 BindingElement

绑定和绑定元素 (<xref:System.ServiceModel.Channels.Binding?displayProperty=nameWithType> 分别扩展和 <xref:System.ServiceModel.Channels.BindingElement?displayProperty=nameWithType> 的对象) 是 WINDOWS COMMUNICATION FOUNDATION (WCF) 应用程序模型与通道工厂和通道侦听器关联的位置。 如果没有绑定，则使用自定义通道需要按 [服务 Channel-Level 编程](service-channel-level-programming.md) 和 [客户端 Channel-Level 编程](client-channel-level-programming.md)中所述在通道级别进行编程。 本主题讨论在 WCF 中启用通道所需的最低要求，为 <xref:System.ServiceModel.Channels.BindingElement> 通道开发，并允许从应用程序使用，如 [开发通道](developing-channels.md)的步骤4中所述。  
  
## <a name="overview"></a>概述  

 <xref:System.ServiceModel.Channels.BindingElement>为通道创建，使开发人员能够在 WCF 应用程序中使用它。 <xref:System.ServiceModel.Channels.BindingElement> 可以从类中使用对象将 <xref:System.ServiceModel.ServiceHost?displayProperty=nameWithType> WCF 应用程序连接到通道，而不必使用通道的精确类型信息。  
  
 创建完毕后 <xref:System.ServiceModel.Channels.BindingElement> ，你可以根据你的需求启用更多功能，具体取决于 [开发渠道](developing-channels.md)中所述的其余通道开发步骤。  
  
## <a name="adding-a-binding-element"></a>添加绑定元素  

 若要实现自定义 <xref:System.ServiceModel.Channels.BindingElement>，请编写一个继承自 <xref:System.ServiceModel.Channels.BindingElement> 的类。 例如，如果您已开发了一个可以将大消息拆分为多个块并在另一端重新组合这些块的 `ChunkingChannel`，则可以在任意绑定中使用此通道，方法是实现一个 <xref:System.ServiceModel.Channels.BindingElement> 并将绑定配置为使用此元素。 本主题的其余部分以 `ChunkingChannel` 作为示例，演示实现绑定元素的需求。  
  
 `ChunkingBindingElement` 负责创建 `ChunkingChannelFactory` 和 `ChunkingChannelListener`。 它重写 <xref:System.ServiceModel.Channels.BindingElement.CanBuildChannelFactory%2A> 和 <xref:System.ServiceModel.Channels.BindingElement.CanBuildChannelListener%2A> 实现，并检查类型参数是否为 <xref:System.ServiceModel.Channels.IDuplexSessionChannel>（在我们的示例中这是 `ChunkingChannel` 支持的唯一通道形状）以及绑定中的其他绑定元素是否支持此通道形状。  
  
 <xref:System.ServiceModel.Channels.BindingElement.BuildChannelFactory%2A> 首先检查是否可以生成请求的通道形状，然后获取要分割的消息操作列表。 然后，它创建一个新的 `ChunkingChannelFactory`，并为其传递内部通道工厂。 （如果你要创建传输绑定元素，该元素将是绑定堆栈中的最后一个元素，因此必须创建一个通道侦听器或通道工厂。）  
  
 <xref:System.ServiceModel.Channels.BindingElement.BuildChannelListener%2A> 有一个类似的实现，用于创建 `ChunkingChannelListener` 并为其传递内部通道侦听器。  
  
 作为另一个使用传输通道的示例， [transport： UDP](../samples/transport-udp.md) 示例提供以下替代。  
  
 在下面的示例中，绑定元素为 `UdpTransportBindingElement`，它派生自 <xref:System.ServiceModel.Channels.TransportBindingElement>。 它重写以下方法以生成与通道关联的工厂。  
  
```csharp  
public IChannelFactory<TChannel> BuildChannelFactory<TChannel>(BindingContext context)  
{  
    return (IChannelFactory<TChannel>)(object)new UdpChannelFactory(this, context);  
}  
  
public IChannelListener<TChannel> BuildChannelListener<TChannel>(BindingContext context)  
{  
    return (IChannelListener<TChannel>)(object)new UdpChannelListener(this, context);  
}  
```  
  
 它还包含用于克隆 `BindingElement` 并返回我们自己的方案 (soap.udp) 的成员。  
  
#### <a name="protocol-binding-elements"></a>协议绑定元素  

 新绑定元素可以替换或扩充所包括的任何绑定元素，从而添加新的传输、编码或高级协议。 若要创建新的协议绑定元素，请首先扩展 <xref:System.ServiceModel.Channels.BindingElement> 类。 然后，你必须至少实现 <xref:System.ServiceModel.Channels.BindingElement.Clone%2A?displayProperty=nameWithType> 并使用来实现 `ChannelProtectionRequirements` <xref:System.ServiceModel.Channels.IChannel.GetProperty%2A?displayProperty=nameWithType> 。 这会返回此绑定元素的 <xref:System.ServiceModel.Security.ChannelProtectionRequirements>。  有关详细信息，请参阅 <xref:System.ServiceModel.Security.ChannelProtectionRequirements>。  
  
 <xref:System.ServiceModel.Channels.BindingElement.Clone%2A> 应返回此绑定元素的新副本。 作为一种最佳做法，我们建议绑定元素作者实现 <xref:System.ServiceModel.Channels.BindingElement.Clone%2A>，方法是使用复制构造函数首先调用基复制构造函数，然后克隆此类中的所有附加字段。  
  
#### <a name="transport-binding-elements"></a>传输绑定元素  

 若要创建新的传输绑定元素，请扩展 <xref:System.ServiceModel.Channels.TransportBindingElement> 接口。 之后，至少必须实现 <xref:System.ServiceModel.Channels.BindingElement.Clone%2A> 方法和 <xref:System.ServiceModel.Channels.TransportBindingElement.Scheme%2A?displayProperty=nameWithType> 属性。  
  
 <xref:System.ServiceModel.Channels.BindingElement.Clone%2A> – 应返回此绑定元素的新副本。  作为一种最佳做法，我们建议绑定元素作者实现 Clone，方法是使用可调用基复制构造函数的复制构造函数，然后克隆此类中的任何其他字段。  
  
 <xref:System.ServiceModel.Channels.TransportBindingElement.Scheme%2A> – <xref:System.ServiceModel.Channels.TransportBindingElement.Scheme%2A> 获取属性返回由该绑定元素表示的传输协议的 URI 方案。 例如， <xref:System.ServiceModel.Channels.HttpTransportBindingElement?displayProperty=nameWithType> 和 <xref:System.ServiceModel.Channels.TcpTransportBindingElement?displayProperty=nameWithType> 返回各自的属性中的 "http" 和 "net.tcp" <xref:System.ServiceModel.Channels.TransportBindingElement.Scheme%2A> 。  
  
#### <a name="encoding-binding-elements"></a>编码绑定元素  

 若要创建新的编码绑定元素，请首先扩展 <xref:System.ServiceModel.Channels.BindingElement> 类并实现 <xref:System.ServiceModel.Channels.MessageEncodingBindingElement?displayProperty=nameWithType> 类。 之后，至少必须实现 <xref:System.ServiceModel.Channels.BindingElement.Clone%2A>、<xref:System.ServiceModel.Channels.MessageEncodingBindingElement.CreateMessageEncoderFactory%2A?displayProperty=nameWithType> 方法和 <xref:System.ServiceModel.Channels.MessageEncodingBindingElement.MessageVersion%2A?displayProperty=nameWithType> 属性。  
  
- <xref:System.ServiceModel.Channels.BindingElement.Clone%2A>. 返回此绑定元素的新副本。 作为一种最佳做法，我们建议绑定元素作者实现 <xref:System.ServiceModel.Channels.BindingElement.Clone%2A>，方法是使用复制构造函数首先调用基复制构造函数，然后克隆此类中的所有附加字段。  
  
- <xref:System.ServiceModel.Channels.MessageEncodingBindingElement.CreateMessageEncoderFactory%2A>. 返回一个 <xref:System.ServiceModel.Channels.MessageEncoderFactory>，它为实现新编码器的实际类提供句柄并扩展 <xref:System.ServiceModel.Channels.MessageEncoder>。 有关详细信息，请参阅 <xref:System.ServiceModel.Channels.MessageEncoderFactory> 和 <xref:System.ServiceModel.Channels.MessageEncoder>。  
  
- <xref:System.ServiceModel.Channels.MessageEncodingBindingElement.MessageVersion%2A>. 返回此编码中使用的 <xref:System.ServiceModel.Channels.MessageVersion>，表示正在使用的 SOAP 和 WS-Addressing 的版本。  
  
 有关用户定义的编码绑定元素的可选方法和属性的完整列表，请参见 <xref:System.ServiceModel.Channels.MessageEncodingBindingElement>。  
  
 有关创建新的绑定元素的详细信息，请参阅 [创建 User-Defined 绑定](creating-user-defined-bindings.md)。  
  
 为通道创建绑定元素后，返回到 " [开发通道](developing-channels.md) " 主题，以查看是否要将配置文件支持添加到绑定元素、是否以及如何添加元数据发布支持，以及是否以及如何构造使用绑定元素的用户定义绑定。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Channels.BindingElement>
- [开发通道](developing-channels.md)
- [传输：UDP](../samples/transport-udp.md)
