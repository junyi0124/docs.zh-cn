---
title: 传输：WSE 3.0 TCP 互操作性
ms.date: 03/30/2017
ms.assetid: 5f7c3708-acad-4eb3-acb9-d232c77d1486
ms.openlocfilehash: c268043a9d5c3f6a48b7b66dc807e4e30f029f61
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96292501"
---
# <a name="transport-wse-30-tcp-interoperability"></a>传输：WSE 3.0 TCP 互操作性

WSE 3.0 TCP 互操作性传输示例演示如何将 TCP 双工会话作为自定义 Windows Communication Foundation (WCF) 传输实现。 还演示如何通过网络，使用通道层的扩展性与已经过部署的现有系统进行交互。 以下步骤演示如何生成此自定义 WCF 传输：  
  
1. 从 TCP 套接字开始，创建 <xref:System.ServiceModel.Channels.IDuplexSessionChannel> 的客户端和服务器实现以使用 DIME 组帧来描述消息边界。  
  
2. 创建一个连接到 WSE TCP 服务并通过客户端 <xref:System.ServiceModel.Channels.IDuplexSessionChannel> 发送组帧消息的通道工厂。  
  
3. 创建一个通道侦听器以接受传入的 TCP 连接并生成相应的通道。  
  
4. 请确保将特定于网络的任何异常正常化为 <xref:System.ServiceModel.CommunicationException> 的相应派生类。  
  
5. 添加一个用来向通道堆栈中添加自定义传输的绑定元素。 有关详细信息，请参阅 [添加绑定元素]。  
  
## <a name="creating-iduplexsessionchannel"></a>创建 IDuplexSessionChannel  

 编写 WSE 3.0 TCP 互操作性传输的第一步是在 <xref:System.ServiceModel.Channels.IDuplexSessionChannel> 的顶部创建 <xref:System.Net.Sockets.Socket> 的实现。 `WseTcpDuplexSessionChannel` 派生自 <xref:System.ServiceModel.Channels.ChannelBase>。 消息的发送逻辑主要由以下两个部分组成：(1) 将消息编码为字节；(2) 对这些字节进行组帧并通过网络发送它们。  
  
 `ArraySegment<byte> encodedBytes = EncodeMessage(message);`  
  
 `WriteData(encodedBytes);`  
  
 另外，设置了一个锁，以保证在调用 Send() 时可以按顺序保留 IDuplexSessionChannel，以便对基础套接字的调用能够正确地同步。  
  
 `WseTcpDuplexSessionChannel` 使用 <xref:System.ServiceModel.Channels.MessageEncoder> 在 <xref:System.ServiceModel.Channels.Message> 和 byte[] 之间相互转换。 由于 `WseTcpDuplexSessionChannel` 为传输，因此它还负责应用通道所配置的远程地址。 `EncodeMessage` 封装此转换的逻辑。  
  
 `this.RemoteAddress.ApplyTo(message);`  
  
 `return encoder.WriteMessage(message, maxBufferSize, bufferManager);`  
  
 一旦将 <xref:System.ServiceModel.Channels.Message> 编码为字节，就必须通过线路传输它。 这要求系统定义消息边界。 WSE 3.0 使用某个版本的 [DIME](/archive/msdn-magazine/2002/december/sending-files-attachments-and-soap-messages-via-dime) 作为其帧协议。 `WriteData` 封装框架逻辑以便将 byte[] 包装到一组 DIME 记录中。  
  
 用于接收消息的逻辑类似。 主要的复杂性是处理套接字读取返回的字节数可能小于请求的字节数这一事实。 若要接收消息，`WseTcpDuplexSessionChannel` 读取网络中的字节，对 DIME 组帧进行解码，然后使用 <xref:System.ServiceModel.Channels.MessageEncoder> 将 byte[] 转换为 <xref:System.ServiceModel.Channels.Message>。  
  
 基 `WseTcpDuplexSessionChannel` 假设它接收连接的套接字。 这个基类处理套接字的关闭。 可通过三个位置与套接字关闭进行交互：  
  
- OnAbort - 以非正常方式关闭套接字（硬关闭）。  
  
- On[Begin]Close - 正常关闭套接字（软关闭）。  
  
- 会议.CloseOutputSession--关闭出站数据流 (半近接近) 。  
  
## <a name="channel-factory"></a>通道工厂  

 编写 TCP 传输的下一步是为客户端通道创建 <xref:System.ServiceModel.Channels.IChannelFactory> 的实现。  
  
- `WseTcpChannelFactory`派生自 <xref:System.ServiceModel.Channels.ChannelFactoryBase> \<IDuplexSessionChannel> 。 它是一个用来重写 `OnCreateChannel` 以生成客户端通道的工厂。  
  
 `protected override IDuplexSessionChannel OnCreateChannel(EndpointAddress remoteAddress, Uri via)`  
  
 `{`  
  
 `return new ClientWseTcpDuplexSessionChannel(encoderFactory, bufferManager, remoteAddress, via, this);`  
  
 `}`  
  
- `ClientWseTcpDuplexSessionChannel` 向基添加逻辑 `WseTcpDuplexSessionChannel` 以在时间连接到 TCP 服务器 `channel.Open` 。 首先，主机名解析为 IP 地址，如下面的代码所示。  
  
 `hostEntry = Dns.GetHostEntry(Via.Host);`  
  
- 然后，主机名连接到循环中的第一个可用 IP 地址，如下面的代码所示。  
  
 `IPAddress address = hostEntry.AddressList[i];`  
  
 `socket = new Socket(address.AddressFamily, SocketType.Stream, ProtocolType.Tcp);`  
  
 `socket.Connect(new IPEndPoint(address, port));`  
  
- 包装了所有特定于域的异常（如 `SocketException` 中的 <xref:System.ServiceModel.CommunicationException>），作为通道协定的一部分。  
  
## <a name="channel-listener"></a>通道侦听器  

 编写 TCP 传输的下一步是创建用来接受服务器通道的 <xref:System.ServiceModel.Channels.IChannelListener> 实现。  
  
- `WseTcpChannelListener`派生自 <xref:System.ServiceModel.Channels.ChannelListenerBase> \<IDuplexSessionChannel> 并在 [Begin] 打开和在 [Begin] Close 上用于控制其侦听套接字的生存期。 在 OnOpen 中，需要创建一个用来侦听 IP_ANY 的套接字。 在更高级的实现中，可以再创建一个同时侦听 IPv6 的套接字。 这些高级实现还允许在主机名中指定 IP 地址。  
  
 `IPEndPoint localEndpoint = new IPEndPoint(IPAddress.Any, uri.Port);`  
  
 `this.listenSocket = new Socket(localEndpoint.AddressFamily, SocketType.Stream, ProtocolType.Tcp);`  
  
 `this.listenSocket.Bind(localEndpoint);`  
  
 `this.listenSocket.Listen(10);`  
  
 在接受了新套接字之后，将用这个套接字来初始化服务器通道。 所有的输入和输出都已经在该基类中实现，因此该通道负责初始化此套接字。  
  
## <a name="adding-a-binding-element"></a>添加绑定元素  

 现在已经生成了工厂和通道，必须通过绑定将它们向 ServiceModel 运行库公开。 绑定是指绑定元素的集合，该集合表示与服务地址相关联的通信堆栈。 该堆栈中的每个元素都由一个绑定元素来表示。  
  
 在下面的示例中，绑定元素为 `WseTcpTransportBindingElement`，它派生自 <xref:System.ServiceModel.Channels.TransportBindingElement>。 它支持 <xref:System.ServiceModel.Channels.IDuplexSessionChannel> 并重写下列方法以生成与绑定相关联的工厂。  
  
 `public IChannelFactory<TChannel> BuildChannelFactory<TChannel>(BindingContext context)`  
  
 `{`  
  
 `return (IChannelFactory<TChannel>)(object)new WseTcpChannelFactory(this, context);`  
  
 `}`  
  
 `public IChannelListener<TChannel> BuildChannelListener<TChannel>(BindingContext context)`  
  
 `{`  
  
 `return (IChannelListener<TChannel>)(object)new WseTcpChannelListener(this, context);`  
  
 `}`  
  
 它还包含用于克隆 `BindingElement` 并返回我们自己的方案 (wse.tcp) 的成员。  
  
## <a name="the-wse-tcp-test-console"></a>WSE TCP 测试控制台  

 TestCode.cs 中提供了用于使用此示例传输的测试代码。 下面的说明演示如何设置 WSE `TcpSyncStockService` 示例。  
  
 该测试代码创建了一个自定义绑定，该绑定将 MTOM 用作编码并将 `WseTcpTransport` 用作传输。 该测试代码还设置了与 WSE 3.0 一致的 AddressingVersion，如下面的代码所示。  
  
 `CustomBinding binding = new CustomBinding();`  
  
 `MtomMessageEncodingBindingElement mtomBindingElement = new MtomMessageEncodingBindingElement();`  
  
 `mtomBindingElement.MessageVersion = MessageVersion.Soap11WSAddressingAugust2004;`  
  
 `binding.Elements.Add(mtomBindingElement);`  
  
 `binding.Elements.Add(new WseTcpTransportBindingElement());`  
  
 它由两个测试组成，第一个测试使用从 WSE 3.0 WSDL 生成的代码来设置类型化客户端， 第二个测试通过直接在通道 Api 之上发送消息，将 WCF 作为客户端和服务器使用。  
  
 运行此示例时，应生成下面的输出。  
  
 Client：  
  
```console  
Calling soap://stockservice.contoso.com/wse/samples/2003/06/TcpSyncStockService  
  
Symbol: FABRIKAM  
        Name: Fabrikam, Inc.  
        Last Price: 120  
  
Symbol: CONTOSO  
        Name: Contoso Corp.  
        Last Price: 50.07  
Press enter.  
  
Received Action: http://SayHello  
Received Body: to you.  
Hello to you.  
Press enter.  
  
Received Action: http://NotHello  
Received Body: to me.  
Press enter.  
```  
  
 服务器:  
  
```console  
Listening for messages at soap://stockservice.contoso.com/wse/samples/2003/06/TcpSyncStockService  
  
Press any key to exit when done...  
  
Request received.  
Symbols:  
        FABRIKAM  
        CONTOSO  
```  
  
## <a name="set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 若要运行此示例，您必须具有 [Web 服务增强功能 (wse) 3.0 （适用于 Microsoft .NET](https://www.microsoft.com/download/details.aspx?id=14089) ）和 wse `TcpSyncStockService` 示例。
  
> [!NOTE]
> 由于 Windows Server 2008 不支持 WSE 3.0，因此无法 `TcpSyncStockService` 在该操作系统上安装或运行该示例。  
  
1. 安装了 `TcpSyncStockService` 示例后，请执行下列操作：  
  
    1. `TcpSyncStockService`在 Visual Studio 中打开。  (TcpSyncStockService 示例随 WSE 3.0 一起安装。 它不属于此示例的代码。 )   
  
    2. 将 StockService 项目设置为启动项目。  
  
    3. 在 StockService 项目中打开 StockService.cs，并注释掉 `StockService` 类的 [Policy] 属性。 这会禁用该示例中的安全性。 尽管 WCF 可以与 WSE 3.0 安全终结点进行互操作，但禁用了安全性来使此示例专注于自定义 TCP 传输。  
  
    4. 按 F5 启动 `TcpSyncStockService`。 该服务将在一个新的控制台窗口中启动。  
  
    5. 在 Visual Studio 中打开这个 TCP 传输示例。  
  
    6. 更新 TestCode.cs 中的“hostname”变量，使其与运行 `TcpSyncStockService` 的计算机的名称相匹配。  
  
    7. 按 F5 启动 TCP 传输示例。  
  
    8. TCP 传输测试客户端将在一个新控制台中启动。 客户端从服务请求股票报价，然后将结果显示在其控制台窗口中。
