---
title: 创建用户定义的绑定
ms.date: 03/30/2017
helpviewer_keywords:
- user-defined bindings [WCF]
ms.assetid: c4960675-d701-4bc9-b400-36a752fdd08b
ms.openlocfilehash: ba40bcfd8a9e9fea1a422ed124b9a966819bf184
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96257849"
---
# <a name="creating-user-defined-bindings"></a>创建用户定义的绑定

有多种方式可以创建系统未提供的绑定：  
  
- 基于 <xref:System.ServiceModel.Channels.CustomBinding> 类（可向其中填充绑定元素的容器）创建一个自定义绑定。 然后将自定义绑定添加到服务终结点。 可以通过编程方式或者在应用程序配置文件中创建自定义绑定。 若要从应用程序配置文件中使用绑定元素，该绑定元素必须扩展 <xref:System.ServiceModel.Configuration.BindingElementExtensionElement>。 有关自定义绑定的详细信息，请参阅 [自定义绑定](custom-bindings.md) 和 <xref:System.ServiceModel.Channels.CustomBinding> 。  
  
- 可以创建一个从标准绑定派生的类。 例如，可以从 <xref:System.ServiceModel.WSHttpBinding> 派生一个类并重写 <xref:System.ServiceModel.Channels.CustomBinding.CreateBindingElements%2A> 方法，以获取绑定元素并插入自定义绑定元素或建立一个特定的安全值。  
  
- 可以创建一个新的 <xref:System.ServiceModel.Channels.Binding> 类型来完全控制整个绑定实现。  
  
## <a name="the-order-of-binding-elements"></a>绑定元素的顺序  

 发送或接收消息时，每个绑定元素都表示一个处理步骤。 在运行时，绑定元素会创建必要的通道和侦听器，用以生成传出和传入通道堆栈。  
  
 有三种主要的绑定元素类型：协议绑定元素、编码绑定元素和传输绑定元素。  
  
 协议绑定元素 – 这些元素表示对消息执行的更高级别的处理步骤。 由这些绑定元素创建的通道和侦听器可以添加、移除或修改消息内容。 给定的绑定可以具有任意数量的协议绑定元素，每一个元素都从 <xref:System.ServiceModel.Channels.BindingElement> 继承。 Windows Communication Foundation (WCF) 包含若干协议绑定元素，其中包括 <xref:System.ServiceModel.Channels.ReliableSessionBindingElement> 和 <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> 。  
  
 编码绑定元素 – 这些元素表示消息与准备用于网络传输的编码之间的转换。 典型的 WCF 绑定只包含一个编码绑定元素。 编码绑定元素的示例包括 <xref:System.ServiceModel.Channels.MtomMessageEncodingBindingElement>、<xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement> 和 <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement>。 如果未对绑定指定编码绑定元素，则使用默认的编码。 当传输协议是 HTTP 时，默认编码为文本，对于其他传输协议，默认编码为二进制。  
  
 传输绑定元素 – 这些元素表示传输协议上编码消息的传输。 典型的 WCF 绑定只包含一个从继承的传输绑定元素 <xref:System.ServiceModel.Channels.TransportBindingElement> 。 传输绑定元素的示例包括 <xref:System.ServiceModel.Channels.TcpTransportBindingElement>、<xref:System.ServiceModel.Channels.HttpTransportBindingElement> 和 <xref:System.ServiceModel.Channels.NamedPipeTransportBindingElement>。  
  
 创建新的绑定时，添加绑定元素的顺序很重要。 应始终按照以下顺序添加绑定元素：  
  
|层|选项|必须|  
|-----------|-------------|--------------|  
|事务流|<xref:System.ServiceModel.Channels.TransactionFlowBindingElement?displayProperty=nameWithType>|否|  
|可靠性|<xref:System.ServiceModel.Channels.ReliableSessionBindingElement?displayProperty=nameWithType>|否|  
|安全性|<xref:System.ServiceModel.Channels.SecurityBindingElement?displayProperty=nameWithType>|否|  
|复合双工|<xref:System.ServiceModel.Channels.CompositeDuplexBindingElement?displayProperty=nameWithType>|否|  
|编码|文本、二进制、MTOM、自定义|是\*|  
|Transport|TCP、命名管道、HTTP、HTTPS、MSMQ、自定义|是|  
  
\*由于每个绑定都需要一个编码，因此，如果未指定编码，WCF 将为你添加默认编码。 对于 HTTP 和 HTTPS 传输，默认编码为 Text/XML，对于其他传输，默认编码为二进制。  
  
## <a name="creating-a-new-binding-element"></a>创建新的绑定元素  

 除了 <xref:System.ServiceModel.Channels.BindingElement> 由 WCF 提供的派生自的类型之外，您还可以创建自己的绑定元素。 这样，您就可以通过创建自己的、可与堆栈中其他的系统提供的类型组合的 <xref:System.ServiceModel.Channels.BindingElement>，自定义创建绑定堆栈的方式和进入其中的组件。  
  
 例如，如果实现一个 `LoggingBindingElement` 以提供将消息记录到数据库中的能力，则必须将其放置在通道堆栈中传输通道的上方。 在此情况下，应用程序创建一个将 `LoggingBindingElement` 与 `TcpTransportBindingElement` 组合在一起的自定义绑定，如以下示例中所示。  
  
```csharp  
Binding customBinding = new CustomBinding(  
  new LoggingBindingElement(),
  new TcpTransportBindingElement()  
);  
```  
  
 写入新的绑定元素的方式取决于元素的确切功能。 下面是一个示例，即 [传输： UDP](../samples/transport-udp.md)，提供了如何实现一种绑定元素的详细说明。  
  
## <a name="creating-a-new-binding"></a>创建新的绑定  

 可以通过两种方式使用用户创建的绑定元素。 上一节演示了第一种方式：通过自定义绑定。 自定义绑定允许用户基于任意一组绑定元素（包括用户创建的绑定元素）创建自己的绑定。  
  
 如果在多个应用程序中使用绑定，则可创建自己的绑定并扩展 <xref:System.ServiceModel.Channels.Binding>。 这样就避免了在每次使用时都需要手动创建自定义绑定。 用户定义的绑定允许你定义绑定的行为并包括用户定义的绑定元素。 并且它是 *预打包* 的：您不必在每次使用时都重新生成绑定。  
  
 用户定义的绑定至少必须实现 <xref:System.ServiceModel.Channels.Binding.CreateBindingElements%2A> 方法和 <xref:System.ServiceModel.Channels.Binding.Scheme%2A> 属性。  
  
 <xref:System.ServiceModel.Channels.Binding.CreateBindingElements%2A> 方法返回一个新的 <xref:System.ServiceModel.Channels.BindingElementCollection>，其中包含绑定的绑定元素。 此集合已经过排序，应首先包含协议绑定元素，接下来是编码绑定元素，再接下来是传输绑定元素。 使用 WCF 系统提供的绑定元素时，必须遵循 [自定义绑定](custom-bindings.md)中指定的绑定元素排序规则。 此集合不得引用在用户定义的绑定类中引用的对象；因此，绑定作者必须在每次调用 `Clone()` 时返回 <xref:System.ServiceModel.Channels.BindingElementCollection> 的 <xref:System.ServiceModel.Channels.Binding.CreateBindingElements%2A>。  
  
 <xref:System.ServiceModel.Channels.Binding.Scheme%2A> 属性表示在绑定上使用的传输协议的 URI 方案。 例如， *WSHttpBinding* 和 *NetTcpBinding* 从其各自的属性返回 "http" 和 "net.tcp" <xref:System.ServiceModel.Channels.Binding.Scheme%2A> 。  
  
 有关用户定义的绑定的可选方法和属性的完整列表，请参见 <xref:System.ServiceModel.Channels.Binding>。  
  
### <a name="example"></a>示例  

 本示例在派生自 `SampleProfileUdpBinding` 的 <xref:System.ServiceModel.Channels.Binding> 中实现配置文件绑定。 `SampleProfileUdpBinding`最多包含四个绑定元素：一个用户创建 `UdpTransportBindingElement` ; 三个系统提供的： `TextMessageEncodingBindingElement` 、 `CompositeDuplexBindingElement` 和 `ReliableSessionBindingElement` 。  
  
```csharp
public override BindingElementCollection CreateBindingElements()  
{
    BindingElementCollection bindingElements = new BindingElementCollection();  
    if (ReliableSessionEnabled)  
    {  
        bindingElements.Add(session);  
        bindingElements.Add(compositeDuplex);  
    }  
    bindingElements.Add(encoding);  
    bindingElements.Add(transport);  
    return bindingElements.Clone();  
}  
```  
  
## <a name="security-restrictions-with-duplex-contracts"></a>双工协定的安全限制  

 不是所有绑定元素都彼此兼容。 具体而言，安全绑定元素在用于双工协定时存在一些限制。  
  
### <a name="one-shot-security"></a>单步安全  

 通过将配置元素的属性设置为，可以实现 "一次" 安全，其中所有必需的安全凭据都通过将 `negotiateServiceCredential` 配置元素的属性设置 \<message> 为来在单个消息中发送 `false` 。  
  
 单步身份验证无法与双工协定一起工作。  
  
 对于请求-答复协定，只有在安全绑定元素下的绑定堆栈支持创建 <xref:System.ServiceModel.Channels.IRequestChannel> 或 <xref:System.ServiceModel.Channels.IRequestSessionChannel> 实例的情况下，单步身份验证才可以工作。  
  
 对于单向协定，如果安全绑定元素下的绑定堆栈支持创建 <xref:System.ServiceModel.Channels.IRequestChannel>、<xref:System.ServiceModel.Channels.IRequestSessionChannel>、<xref:System.ServiceModel.Channels.IOutputChannel> 或 <xref:System.ServiceModel.Channels.IOutputSessionChannel> 实例，则单步身份验证可以工作。  
  
### <a name="cookie-mode-security-context-tokens"></a>Cookie 模式安全上下文标记  

 Cookie 模式安全上下文标记不能与双工协定一起使用。  
  
 对于请求-答复协定，只有在安全绑定元素下的绑定堆栈支持创建 <xref:System.ServiceModel.Channels.IRequestChannel> 或 <xref:System.ServiceModel.Channels.IRequestSessionChannel> 实例的情况下，Cookie 模式安全上下文标记才可以工作。  
  
 对于单向协定，如果安全绑定元素下的绑定堆栈支持创建 <xref:System.ServiceModel.Channels.IRequestChannel> 或 <xref:System.ServiceModel.Channels.IRequestSessionChannel> 实例，则 Cookie 模式安全上下文标记可以工作。  
  
### <a name="session-mode-security-context-tokens"></a>会话模式安全上下文标记  

 如果安全绑定元素下的绑定堆栈支持创建 <xref:System.ServiceModel.Channels.IDuplexChannel> 或 <xref:System.ServiceModel.Channels.IDuplexSessionChannel> 实例，则会话模式 SCT 适用于双工协定。  
  
 如果安全绑定元素下的绑定堆栈支持创建 <xref:System.ServiceModel.Channels.IDuplexChannel>、<xref:System.ServiceModel.Channels.IDuplexSessionChannel>、<xref:System.ServiceModel.Channels.IRequestChannel> 或 <xref:System.ServiceModel.Channels.IRequestSessionChannel> 实例，则会话模式 SCT 适用于请求-答复协定。  
  
 如果安全绑定元素下的绑定堆栈支持创建 <xref:System.ServiceModel.Channels.IDuplexChannel>、<xref:System.ServiceModel.Channels.IDuplexSessionChannel>、<xref:System.ServiceModel.Channels.IRequestChannel> 或 <xref:System.ServiceModel.Channels.IRequestSessionChannel> 实例，则会话模式 SCT 适用于单向协定。  
  
## <a name="deriving-from-a-standard-binding"></a>派生自标准绑定  

 也许可以扩展一个现有的系统提供的绑定，而不用创建一个全新的绑定类。 与上一个示例非常类似，必须重写 <xref:System.ServiceModel.Channels.Binding.CreateBindingElements%2A> 方法和 <xref:System.ServiceModel.Channels.Binding.Scheme%2A> 属性。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Channels.Binding>
- [自定义绑定](custom-bindings.md)
