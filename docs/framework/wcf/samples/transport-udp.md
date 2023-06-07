---
title: 传输：UDP
ms.date: 03/30/2017
ms.assetid: 738705de-ad3e-40e0-b363-90305bddb140
ms.openlocfilehash: 815a0a50e42e68040673e34e5ebccad17dfeecda
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96258746"
---
# <a name="transport-udp"></a>传输：UDP

UDP 传输示例演示如何将 UDP 单播和多播作为自定义 Windows Communication Foundation (WCF) 传输实现。 该示例介绍了使用通道框架和以下 WCF 最佳做法在 WCF 中创建自定义传输的建议过程。 创建自定义传输的步骤如下：  
  
1. 确定你的 ChannelFactory 和 IReplyChannel) 哪些通道 [消息交换模式](#MessageExchangePatterns) (IOutputChannel、IInputChannel、IDuplexChannel、IRequestChannel 或 ChannelListener。 然后确定是否要支持这些接口的会话变体。  
  
2. 创建支持您的消息交换模式的通道工厂和侦听器。  
  
3. 请确保将特定于网络的任何异常正常化为 <xref:System.ServiceModel.CommunicationException> 的相应派生类。  
  
4. 添加一个 [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) 元素，该元素将自定义传输添加到通道堆栈。 有关详细信息，请参阅 [添加绑定元素](#AddingABindingElement)。  
  
5. 添加一个绑定元素扩展部分，以便将新的绑定元素公开到配置系统。  
  
6. 添加元数据扩展以将各种功能传递给其他终结点。  
  
7. 添加一个绑定，该绑定根据定义完善的配置文件来预配置绑定元素堆栈。 有关详细信息，请参阅 [添加标准绑定](#AddingAStandardBinding)。  
  
8. 添加一个绑定部分和绑定配置元素，以便将该绑定公开到配置系统。 有关详细信息，请参阅 [添加配置支持](#AddingConfigurationSupport)。  
  
<a name="MessageExchangePatterns"></a>

## <a name="message-exchange-patterns"></a>消息交换模式  

 编写自定义传输的第一步是决定该传输需要哪些消息交换模式 (MEP)。 有三种 MEP 可供选择：  
  
- 数据报 (IInputChannel/IOutputChannel)  
  
     当使用数据报 MEP 时，客户端使用“启动后不管”交换形式发送消息。 “发后不理”交换形式是一种要求带外确认成功传递的交换形式。 消息在传输过程中可能会丢失，而永远不能到达服务。 如果在客户端成功完成发送操作，这并不保证远程终结点已经收到消息。 数据报是消息传递的基本构造块，因为您可以在它上面构建自己的协议，包括可靠的协议和安全的协议。 客户端数据报通道实现 <xref:System.ServiceModel.Channels.IOutputChannel> 接口，而服务数据报通道实现 <xref:System.ServiceModel.Channels.IInputChannel> 接口。  
  
- 请求/响应 (IRequestChannel/IReplyChannel)  
  
     在此 MEP 中，将发送一个消息并接收一个答复。 此模式由请求-响应对组成。 请求-响应调用的示例有远程过程调用 (RPC) 和浏览器 GET。 此模式也称为半双工。 在此 MEP 中，客户端通道实现 <xref:System.ServiceModel.Channels.IRequestChannel>，而服务通道实现 <xref:System.ServiceModel.Channels.IReplyChannel>。  
  
- 双工 (IDuplexChannel)  
  
     通过双工 MEP，客户端可以发送任意数目的消息，并以任意顺序接收消息。 双工 MEP 就像电话通话，所说的每一个字都是一条消息。 由于在这种 MEP 中两端都可发送和接收，因此，由客户端和服务通道实现的接口为 <xref:System.ServiceModel.Channels.IDuplexChannel>。  
  
 每个 MEP 还可以支持会话。 具有会话功能的通道所提供的额外功能是它能够关联在一个通道上发送和接收的所有消息。 请求-响应模式是一种由两个消息组成的独立会话，因为请求和响应是相关的。 与此形成对照的是，支持会话的请求-响应模式意味着该通道上的所有请求-响应对都是相关的。 这样总共提供了六个 MEP 供您选择：数据报、请求-响应、双工、具有会话的数据报、具有会话的请求-响应以及具有会话的双工。  
  
> [!NOTE]
> 对于 UDP 传输，所支持的唯一 MEP 是数据报，因为 UDP 的性质是一个“启动后不管”协议。  
  
### <a name="the-icommunicationobject-and-the-wcf-object-lifecycle"></a>ICommunicationObject 和 WCF 对象生存期  

 WCF 具有一个通用状态计算机，用于管理用于通信的对象（如、和）的生命周期 <xref:System.ServiceModel.Channels.IChannel> <xref:System.ServiceModel.Channels.IChannelFactory> <xref:System.ServiceModel.Channels.IChannelListener> 。 这些通信对象可以处于五种状态。 这些状态通过 <xref:System.ServiceModel.CommunicationState> 枚举来表示，如下所示：  
  
- 已创建：这是 <xref:System.ServiceModel.ICommunicationObject> 在首次实例化后的状态。 在此状态中不会发生输入/输出 (I/O)。  
  
- 正在打开：当调用 <xref:System.ServiceModel.ICommunicationObject.Open%2A> 时，对象即转换到此状态。 此时，属性被设置为不可变属性，并且可以开始输入/输出。 此转换只有从“已创建”状态转换才有效。  
  
- 已打开：当打开进程完成后，对象即转换到此状态。 此转换只有从“正在打开”状态转换才有效。 此时，对象完全可用于传送。  
  
- 正在关闭：当调用 <xref:System.ServiceModel.ICommunicationObject.Close%2A> 以完成正常关闭时，对象即转换到此状态。 此转换只有从“已打开”状态转换才有效。  
  
- 已关闭：在“已关闭”状态下，对象无法再使用。 一般情况下，仍然可以访问大多数配置以进行检查，但不能进行通信。 此状态相当于被释放。  
  
- 出错：在“出错”状态下，可以访问对象以进行检查，但对象无法再使用。 当发生不可恢复的错误时，对象即转换到此状态。 此状态的唯一有效转换为 `Closed` 状态。  
  
 每次状态转换都会触发事件。 可以随时调用 <xref:System.ServiceModel.ICommunicationObject.Abort%2A> 方法，它将导致对象立即从当前状态转换到“已关闭”状态。 调用 <xref:System.ServiceModel.ICommunicationObject.Abort%2A> 将终止任何未完成的工作。  
  
<a name="ChannelAndChannelListener"></a>

## <a name="channel-factory-and-channel-listener"></a>通道工厂和通道侦听器  

 编写自定义传输的下一步是为客户端通道创建 <xref:System.ServiceModel.Channels.IChannelFactory> 的实现，并为服务通道创建 <xref:System.ServiceModel.Channels.IChannelListener> 的实现。 通道层使用工厂模式构建通道。 WCF 为此过程提供基类帮助程序。  
  
- <xref:System.ServiceModel.Channels.CommunicationObject> 类实现 <xref:System.ServiceModel.ICommunicationObject> 并强制执行前面步骤 2 中所述的状态机。

- <xref:System.ServiceModel.Channels.ChannelManagerBase> 类实现 <xref:System.ServiceModel.Channels.CommunicationObject> 并为 <xref:System.ServiceModel.Channels.ChannelFactoryBase> 和 <xref:System.ServiceModel.Channels.ChannelListenerBase> 提供统一的基类。 <xref:System.ServiceModel.Channels.ChannelManagerBase> 类与 <xref:System.ServiceModel.Channels.ChannelBase>（用来实现 <xref:System.ServiceModel.Channels.IChannel> 的基类）结合使用。  
  
- <xref:System.ServiceModel.Channels.ChannelFactoryBase>类实现 <xref:System.ServiceModel.Channels.ChannelManagerBase> 和 <xref:System.ServiceModel.Channels.IChannelFactory> ，并将重载合并 `CreateChannel` 到一个 `OnCreateChannel` 抽象方法中。  
  
- <xref:System.ServiceModel.Channels.ChannelListenerBase> 类实现 <xref:System.ServiceModel.Channels.IChannelListener>。 它负责执行基本状态管理。  
  
 在此示例中，工厂实现包含在 UdpChannelFactory.cs 中，侦听器实现包含在 UdpChannelListener.cs 中。 <xref:System.ServiceModel.Channels.IChannel> 实现位于 UdpOutputChannel.cs 和 UdpInputChannel.cs 中。  
  
### <a name="the-udp-channel-factory"></a>UDP 通道工厂  

 `UdpChannelFactory` 派生自 <xref:System.ServiceModel.Channels.ChannelFactoryBase>。 该示例重写 <xref:System.ServiceModel.Channels.ChannelFactoryBase.GetProperty%2A> 以提供对消息编码器的消息版本的访问。 此示例还重写了 <xref:System.ServiceModel.Channels.ChannelFactoryBase.OnClose%2A>，因此我们可以在状态机转换时拆开 <xref:System.ServiceModel.Channels.BufferManager> 的实例。  
  
#### <a name="the-udp-output-channel"></a>UDP 输出通道  

 `UdpOutputChannel` 实现 <xref:System.ServiceModel.Channels.IOutputChannel>。 构造函数对自变量进行验证，并基于传入的 <xref:System.Net.EndPoint> 来构造目标 <xref:System.ServiceModel.EndpointAddress> 对象。  
  
```csharp
this.socket = new Socket(this.remoteEndPoint.AddressFamily, SocketType.Dgram, ProtocolType.Udp);  
```  
  
 通道可以正常关闭或非正常关闭。 如果通道正常关闭，则套接字也将关闭，并调用基类 `OnClose` 方法。 如果引发异常，则基础结构将调用 `Abort` 以确保清理该通道。  
  
```csharp
this.socket.Close(0);  
```  
  
 然后，实现 `Send()` 和 `BeginSend()` / `EndSend()` 。 这将分解为两个主要部分。 首先，将消息序列化为字节数组。  
  
```csharp
ArraySegment<byte> messageBuffer = EncodeMessage(message);  
```  
  
 然后，在网络上发送生成的数据。  
  
```csharp
this.socket.SendTo(messageBuffer.Array, messageBuffer.Offset, messageBuffer.Count, SocketFlags.None, this.remoteEndPoint);  
```  
  
### <a name="the-udpchannellistener"></a>UdpChannelListener  

 `UdpChannelListener`示例实现的派生自 <xref:System.ServiceModel.Channels.ChannelListenerBase> 类。 它使用单个 UDP 套接字来接收数据报。 `OnOpen` 方法使用该 UDP 套接字以异步循环形式接收数据。 收到的数据随后将借助于消息编码系统转换为消息。  
  
```csharp
message = MessageEncoderFactory.Encoder.ReadMessage(new ArraySegment<byte>(buffer, 0, count), bufferManager);  
```  
  
 由于可以用同一个数据报通道来表示来自多个源的消息，因此 `UdpChannelListener` 是一个单一实例侦听器。 一次最多有一个 <xref:System.ServiceModel.Channels.IChannel> 与此侦听器关联的活动。 只有当随后释放了由 `AcceptChannel` 方法返回的通道时，该示例才生成另一个通道。 收到的消息将排入这个单一实例通道的队列中。  
  
#### <a name="udpinputchannel"></a>UdpInputChannel  

 `UdpInputChannel` 类实现 `IInputChannel`。 该类包括一个传入消息队列，该队列由 `UdpChannelListener` 的套接字来填充。 这些消息可以由 `IInputChannel.Receive` 方法取消排队。  
  
<a name="AddingABindingElement"></a>

## <a name="adding-a-binding-element"></a>添加绑定元素  

 现在已经生成了工厂和通道，必须通过绑定将它们公开给 ServiceModel 运行库。 绑定是指绑定元素的集合，该集合表示与服务地址相关联的通信堆栈。 堆栈中的每个元素都由 [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) 元素表示。  
  
 在下面的示例中，绑定元素为 `UdpTransportBindingElement`，它派生自 <xref:System.ServiceModel.Channels.TransportBindingElement>。 它重写以下方法以生成与我们的绑定相关联的工厂。  
  
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
  
## <a name="adding-metadata-support-for-a-transport-binding-element"></a>为传输绑定元素添加元数据支持  

 若要将我们的传输集成到元数据系统中，我们必须支持策略的导入和导出。 这样，我们便可以通过 " [ ( # A0) ](../servicemodel-metadata-utility-tool-svcutil-exe.md)使用  
  
### <a name="adding-wsdl-support"></a>添加 WSDL 支持  

 绑定中的传输绑定元素负责导出和导入元数据中的寻址信息。 当使用 SOAP 绑定时，传输绑定元素还应在元数据中导出正确的传输 URI。  
  
#### <a name="wsdl-export"></a>WSDL 导出  

 若要导出寻址信息，`UdpTransportBindingElement` 需要实现 `IWsdlExportExtension` 接口。 `ExportEndpoint` 方法将正确的寻址信息添加到 WSDL 端口中。  
  
```csharp
if (context.WsdlPort != null)  
{  
    AddAddressToWsdlPort(context.WsdlPort, context.Endpoint.Address, encodingBindingElement.MessageVersion.Addressing);  
}  
```  
  
 当终结点使用 SOAP 绑定时，`UdpTransportBindingElement` 方法的 `ExportEndpoint` 实现也会导出传输 URI。  
  
```csharp
WsdlNS.SoapBinding soapBinding = GetSoapBinding(context, exporter);  
if (soapBinding != null)  
{  
    soapBinding.Transport = UdpPolicyStrings.UdpNamespace;  
}  
```  
  
#### <a name="wsdl-import"></a>WSDL 导入  

 若要扩展 WSDL 导入系统以处理导入地址，必须将以下配置添加到 Svcutil.exe 的配置文件中，如 Svcutil.exe.config 文件所示。  
  
```xml
<configuration>  
  <system.serviceModel>  
    <client>  
      <metadata>  
        <policyImporters>  
          <extension type=" Microsoft.ServiceModel.Samples.UdpBindingElementImporter, UdpTransport" />  
        </policyImporters>  
      </metadata>  
    </client>  
  </system.serviceModel>  
</configuration>  
```  
  
 当运行 Svcutil.exe 时，有两个选项可用来获取 Svcutil.exe 以加载 WSDL 导入扩展：  
  
1. 使用/SvcutilConfig 将 Svcutil.exe 点指向配置文件： \<file> 。  
  
2. 将配置节添加到与 Svcutil.exe 处于同一目录的 Svcutil.exe.config 中。  
  
 `UdpBindingElementImporter` 类型实现 `IWsdlImportExtension` 接口。 `ImportEndpoint` 方法从 WSDL 端口中导入地址。  
  
```csharp
BindingElementCollection bindingElements = context.Endpoint.Binding.CreateBindingElements();  
TransportBindingElement transportBindingElement = bindingElements.Find<TransportBindingElement>();  
if (transportBindingElement is UdpTransportBindingElement)  
{  
    ImportAddress(context);  
}  
```  
  
### <a name="adding-policy-support"></a>添加策略支持  

 自定义绑定元素可以在 WSDL 绑定中为服务终结点导出策略断言以表示该绑定元素的功能。  
  
#### <a name="policy-export"></a>策略导出  

 `UdpTransportBindingElement`实现 `IPolicyExportExtension` 以添加对导出策略的支持的类型。 因此，`System.ServiceModel.MetadataExporter` 在为任何包含它的绑定而生成策略时都包含 `UdpTransportBindingElement`。  
  
 在 `IPolicyExportExtension.ExportPolicy` 中，如果我们在多播模式下，则添加 UDP 的断言和另一个断言。 这是因为，多路广播模式影响通信堆栈的构造方式，因此必须在两端之间进行协调。  
  
```csharp
ICollection<XmlElement> bindingAssertions = context.GetBindingAssertions();  
XmlDocument xmlDocument = new XmlDocument();  
bindingAssertions.Add(xmlDocument.CreateElement(  
UdpPolicyStrings.Prefix, UdpPolicyStrings.TransportAssertion, UdpPolicyStrings.UdpNamespace));  
if (Multicast)  
{  
    bindingAssertions.Add(xmlDocument.CreateElement(
        UdpPolicyStrings.Prefix,
        UdpPolicyStrings.MulticastAssertion,
        UdpPolicyStrings.UdpNamespace));  
}  
```  
  
 因为自定义传输绑定元素负责处理寻址，所以 `IPolicyExportExtension` 上的 `UdpTransportBindingElement` 实现还必须处理导出相应的 WS-Addressing 策略断言以指示正在使用的 WS-Addressing 的版本。  
  
```csharp
AddWSAddressingAssertion(context, encodingBindingElement.MessageVersion.Addressing);  
```  
  
#### <a name="policy-import"></a>策略导入  

 若要扩展策略导入系统，必须将以下配置添加到 Svcutil.exe 的配置文件中，如 Svcutil.exe.config 文件所示。  
  
```xml
<configuration>  
  <system.serviceModel>  
    <client>  
      <metadata>  
        <policyImporters>  
          <extension type=" Microsoft.ServiceModel.Samples.UdpBindingElementImporter, UdpTransport" />  
        </policyImporters>  
      </metadata>  
    </client>  
  </system.serviceModel>  
</configuration>  
```  
  
 然后从已注册的类 (`IPolicyImporterExtension`) 中实现 `UdpBindingElementImporter`。 在 `ImportPolicy()` 中，浏览命名空间中的断言，处理用于生成传输的断言，并检查它是否为多播。 还必须从绑定断言列表中移除已经处理的断言。 同样，当运行 Svcutil.exe 时，有两个用于集成的选项：  
  
1. 使用/SvcutilConfig 将 Svcutil.exe 点指向配置文件： \<file> 。  
  
2. 将配置节添加到与 Svcutil.exe 处于同一目录的 Svcutil.exe.config 中。  
  
<a name="AddingAStandardBinding"></a>

## <a name="adding-a-standard-binding"></a>添加标准绑定  

 绑定元素可以按照以下两种方式使用：  
  
- 通过自定义绑定：自定义绑定允许用户根据任意一组绑定元素创建自己的绑定。  
  
- 通过使用系统提供的、包含我们的绑定元素的绑定。 WCF 提供了许多这些系统定义的绑定，如 `BasicHttpBinding` 、 `NetTcpBinding` 和 `WsHttpBinding` 。 这些绑定中的每个绑定与一个准确定义的配置文件相关联。  
  
 此示例在从 `SampleProfileUdpBinding` 派生的 <xref:System.ServiceModel.Channels.Binding> 中实现配置文件绑定。 `SampleProfileUdpBinding` 中最多包含四个绑定元素：`UdpTransportBindingElement`、`TextMessageEncodingBindingElement CompositeDuplexBindingElement` 和 `ReliableSessionBindingElement`。  
  
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
  
### <a name="adding-a-custom-standard-binding-importer"></a>添加自定义标准绑定导入程序  

 Svcutil.exe 和 `WsdlImporter` 类型默认情况下识别并导入系统定义的绑定。 否则，绑定将作为 `CustomBinding` 实例而导入。 若要启用 Svcutil.exe 和 `WsdlImporter` 以导入 `SampleProfileUdpBinding`，`UdpBindingElementImporter` 还需充当自定义标准绑定导入程序。  
  
 自定义标准绑定导入程序在 `ImportEndpoint` 接口上实现 `IWsdlImportExtension` 方法，以检查从元数据中导入的 `CustomBinding` 实例，了解它是否由特定的标准绑定生成。  
  
```csharp
if (context.Endpoint.Binding is CustomBinding)  
{  
    Binding binding;  
    if (transportBindingElement is UdpTransportBindingElement)  
    {  
        //if TryCreate is true, the CustomBinding will be replace by a SampleProfileUdpBinding in the  
        //generated config file for better typed generation.  
        if (SampleProfileUdpBinding.TryCreate(bindingElements, out binding))  
        {  
            binding.Name = context.Endpoint.Binding.Name;  
            binding.Namespace = context.Endpoint.Binding.Namespace;  
            context.Endpoint.Binding = binding;  
        }  
    }  
}  
```  
  
 通常，实现自定义标准绑定导入程序包括检查导入的绑定元素的属性以验证是否仅更改了标准绑定可以设置的属性，而所有其他属性都是各自的默认值。 用于实现标准绑定导入程序的基本策略是创建标准绑定的实例，将绑定元素中的属性传播到标准绑定支持的标准绑定实例中，然后将标准绑定中的绑定元素与导入的绑定元素进行比较。  
  
<a name="AddingConfigurationSupport"></a>

## <a name="adding-configuration-support"></a>添加配置支持  

 若要通过配置公开传输，必须实现两个配置节。 第一个是 `BindingElementExtensionElement` 的 `UdpTransportBindingElement`。 这样使 `CustomBinding` 实现可以引用绑定元素。 第二个是 `Configuration` 的 `SampleProfileUdpBinding`。  
  
### <a name="binding-element-extension-element"></a>绑定元素扩展元素  

 `UdpTransportElement` 节是一个 `BindingElementExtensionElement`，它向配置系统公开 `UdpTransportBindingElement`。 通过一些基本重写，我们定义了配置节名称、绑定元素的类型以及如何创建绑定元素。 然后，我们可以注册配置文件中的扩展节，如下面的示例代码所示。  
  
```xml
<configuration>  
  <system.serviceModel>  
    <extensions>  
      <bindingElementExtensions>  
        <add name="udpTransport" type="Microsoft.ServiceModel.Samples.UdpTransportElement, UdpTransport" />  
      </bindingElementExtensions>  
    </extensions>  
  </system.serviceModel>  
</configuration>  
```  
  
 可以从自定义绑定来引用该扩展以将 UDP 用作传输协议。  
  
```xml
<configuration>  
  <system.serviceModel>  
    <bindings>  
      <customBinding>  
       <binding configurationName="UdpCustomBinding">  
         <udpTransport/>  
       </binding>  
      </customBinding>  
    </bindings>  
  </system.serviceModel>  
</configuration>  
```  
  
### <a name="binding-section"></a>绑定节  

 `SampleProfileUdpBindingCollectionElement` 节是一个 `StandardBindingCollectionElement`，它向配置系统公开 `SampleProfileUdpBinding`。 批量实现委派给从 `SampleProfileUdpBindingConfigurationElement` 派生的 `StandardBindingElement`。 `SampleProfileUdpBindingConfigurationElement`具有与的属性相对应的属性 `SampleProfileUdpBinding` ，以及要从绑定中映射的函数 `ConfigurationElement` 。 最后，在 `OnApplyConfiguration` 中重写 `SampleProfileUdpBinding` 方法，如下面的示例代码所示。  
  
```csharp
protected override void OnApplyConfiguration(string configurationName)  
{  
    if (binding == null)
        throw new ArgumentNullException("binding");

    if (binding.GetType() != typeof(SampleProfileUdpBinding))
    {
        throw new ArgumentException(string.Format(CultureInfo.CurrentCulture,
            "Invalid type for binding. Expected type: {0}. Type passed in: {1}.",
            typeof(SampleProfileUdpBinding).AssemblyQualifiedName,
            binding.GetType().AssemblyQualifiedName));
    }
    SampleProfileUdpBinding udpBinding = (SampleProfileUdpBinding)binding;

    udpBinding.OrderedSession = this.OrderedSession;
    udpBinding.ReliableSessionEnabled = this.ReliableSessionEnabled;
    udpBinding.SessionInactivityTimeout = this.SessionInactivityTimeout;
    if (this.ClientBaseAddress != null)
        udpBinding.ClientBaseAddress = ClientBaseAddress;
}
```  
  
 为了向配置系统注册此处理程序，我们将下面一节添加到相关的配置文件中。  
  
```xml
<configuration>  
  <configSections>  
     <sectionGroup name="system.serviceModel">  
        <sectionGroup name="bindings">  
          <section name="sampleProfileUdpBinding" type="Microsoft.ServiceModel.Samples.SampleProfileUdpBindingCollectionElement, UdpTransport" />  
        </sectionGroup>  
     </sectionGroup>  
  </configSections>  
</configuration>  
```  
  
 然后即可从 serviceModel 配置节引用它。  
  
```xml
<configuration>  
  <system.serviceModel>  
    <client>  
      <endpoint configurationName="calculator"  
                address="soap.udp://localhost:8001/"
                bindingConfiguration="CalculatorServer"  
                binding="sampleProfileUdpBinding"
                contract= "Microsoft.ServiceModel.Samples.ICalculatorContract">  
      </endpoint>  
    </client>  
  </system.serviceModel>  
</configuration>  
```  
  
## <a name="the-udp-test-service-and-client"></a>UDP 测试服务和客户端  

 用于使用此示例传输的测试代码位于 UdpTestService 和 UdpTestClient 目录中。 服务代码包含两个测试 - 一个测试从代码中设置绑定和终结点，另一个测试通过配置完成这些操作。 这两个测试都使用两个终结点。 一个终结点使用 `SampleUdpProfileBinding` [\<reliableSession>](/previous-versions/ms731375(v=vs.90)) 设置为的 `true` 。 另一个终结点使用具有 `UdpTransportBindingElement` 的自定义绑定。 这等效于在 `SampleUdpProfileBinding` 设置为的情况下使用 [\<reliableSession>](/previous-versions/ms731375(v=vs.90)) `false` 。 这两个测试都创建一个服务，为每个绑定添加一个终结点，打开该服务，然后等待用户按 Enter 后再关闭该服务。  
  
 当您启动服务测试应用程序时，应看到如下输出。  
  
```console
Testing Udp From Code.  
Service is started from code...  
Press <ENTER> to terminate the service and start service from config...  
```  
  
 然后你可以根据发布的终结点运行测试客户端应用程序。 客户端测试应用程序为每个终结点创建一个客户端，并向每个终结点发送五条消息。 下面是客户端输出。  
  
```console
Testing Udp From Imported Files Generated By SvcUtil.  
0  
3  
6  
9  
12  
Press <ENTER> to complete test.  
```  
  
 下面是服务的完整输出。  
  
```console
Service is started from code...  
Press <ENTER> to terminate the service and start service from config...  
Hello, world!  
Hello, world!  
Hello, world!  
Hello, world!  
Hello, world!  
   adding 0 + 0  
   adding 1 + 2  
   adding 2 + 4  
   adding 3 + 6  
   adding 4 + 8  
```  
  
 若要根据使用配置发布的终结点运行客户端应用程序，请在服务中按 Enter，然后再次运行测试客户端。 您将在服务上看到如下输出。  
  
```console
Testing Udp From Config.  
Service is started from config...  
Press <ENTER> to terminate the service and exit...  
```  
  
 重新运行客户端产生的结果与前面的结果相同。  
  
 若要使用 Svcutil.exe 重新生成客户端代码和配置，请启动服务应用程序，然后从示例的根目录中运行以下 Svcutil.exe。  
  
```console
svcutil http://localhost:8000/udpsample/ /reference:UdpTransport\bin\UdpTransport.dll /svcutilConfig:svcutil.exe.config  
```  
  
 请注意，Svcutil.exe 不会为 `SampleProfileUdpBinding` 生成绑定扩展配置；所以您必须手动添加它。  
  
```xml
<configuration>  
  <system.serviceModel>
    <extensions>  
      <!-- This was added manually because svcutil.exe does not add this extension to the file -->  
      <bindingExtensions>  
        <add name="sampleProfileUdpBinding" type="Microsoft.ServiceModel.Samples.SampleProfileUdpBindingCollectionElement, UdpTransport" />  
      </bindingExtensions>  
    </extensions>  
  </system.serviceModel>  
</configuration>  
```  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 若要生成解决方案，请按照 [生成 Windows Communication Foundation 示例](building-the-samples.md)中的说明进行操作。  
  
2. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。  
  
3. 请参阅前面的“UDP 测试服务和客户端”一节。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Transport\Udp`
