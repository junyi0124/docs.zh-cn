---
title: 流消息传输
ms.date: 03/30/2017
ms.assetid: 72a47a51-e5e7-4b76-b24a-299d51e0ae5a
ms.openlocfilehash: 4b90e5bf15f5cb1c4684a3ba224371f6cc232034
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96246324"
---
# <a name="streaming-message-transfer"></a>流消息传输

Windows Communication Foundation (WCF) 传输支持两种模式传输消息：  
  
- 缓冲传输将整个消息保留在内存缓冲区中，直到传输完成。 必须完整传递缓冲的消息，接收方才能读取该消息。  
  
- 流传输以流的形式公开消息。 接收方在消息完整传递之前即可开始处理消息。  
  
- 流传输消除了对大型内存缓冲区的要求，从而提高了服务的可伸缩性。 更改传输模式是否能够提高可伸缩性取决于所传输的消息大小。 消息大小越大，使用流传输越有利。  
  
 默认情况下，HTTP、TCP/IP 和命名管道传输协议使用缓冲传输。 本文档介绍如何将这些传输协议从缓冲传输模式切换到流传输模式以及这样做的结果。  
  
## <a name="enabling-streamed-transfers"></a>启用流传输  

 在缓冲和流传输模式之间进行选择是在传输协议的绑定元素上完成的。 该绑定元素具有一个 <xref:System.ServiceModel.TransferMode> 属性，该属性可以设置为 `Buffered`、`Streamed`、`StreamedRequest` 或 `StreamedResponse`。 将传输模式设置为 `Streamed` 将在两个方向上启用流通信。 将传输模式设置为 `StreamedRequest` 或 `StreamedResponse` 将仅在指示的方向上启用流通信。  
  
 <xref:System.ServiceModel.BasicHttpBinding>、<xref:System.ServiceModel.NetTcpBinding> 和 <xref:System.ServiceModel.NetNamedPipeBinding> 绑定公开了 <xref:System.ServiceModel.TransferMode> 属性。 对于其他传输，必须创建一个自定义绑定以设置传输模式。  
  
 使用缓冲传输还是流传输是在终结点本地决定的。 对于 HTTP 传输协议，传输模式不会跨连接传播，也不会传播到服务器和其他中介。 设置传输模式不会反映在服务接口的说明中。 在为服务生成一个客户端类后，必须为准备用于流传输的服务编辑配置文件，以设置该传输模式。 对于 TCP 和命名管道传输协议，该传输模式将作为策略断言传播。  
  
 有关代码示例，请参阅 [如何：启用流式处理](how-to-enable-streaming.md)。  
  
## <a name="enabling-asynchronous-streaming"></a>启用异步流处理  

 若要启用异步流，请将 <xref:System.ServiceModel.Description.DispatcherSynchronizationBehavior> 终结点行为添加到服务主机，并将其 <xref:System.ServiceModel.Description.DispatcherSynchronizationBehavior.AsynchronousSendEnabled%2A> 属性设置为 `true`。  
  
 此版本的 WCF 还在发送端添加真正异步流处理的能力。 在将消息流式处理到多个客户端的方案中，这可提高服务的可扩展性；其中某些客户端的读取速度较慢，这可能由于网络拥塞或根本无法阅读造成的。 在这些方案中，WCF 不再因每个客户端的服务阻止各个线程。 这确保服务能够处理多得多的客户端，进而提高服务的可扩展性。  
  
## <a name="restrictions-on-streamed-transfers"></a>流传输的限制  

 使用流传输模式会导致运行库实施附加限制。  
  
 在整个流传输中发生的操作最多只能与一个输入或输出参数之间具有协定。 该参数对应于整个消息正文，并且必须为 <xref:System.ServiceModel.Channels.Message>、<xref:System.IO.Stream> 的派生类型或 <xref:System.Xml.Serialization.IXmlSerializable> 实现。 具有某个操作的返回值等效于具有一个输出参数。  
  
 某些 WCF 功能（如可靠消息传递、事务和 SOAP 消息级安全）依赖于缓冲消息以便进行传输。 使用这些功能可能减小或消除通过使用流获得的性能好处。 若要保证流传输的安全，请只使用传输级安全，或者使用传输级安全外加仅进行身份验证的消息安全。  
  
 即使传输模式设置为流模式，SOAP 标头也总是被缓冲。 消息的标头不得超出 `MaxBufferSize` 传输配额的大小。 有关此设置的详细信息，请参阅 [传输配额](transport-quotas.md)。  
  
## <a name="differences-between-buffered-and-streamed-transfers"></a>缓冲传输和流传输之间的差异  

 将传输模式从缓冲模式更改为流模式还会更改 TCP 和命名管道传输协议的本机通道形状。 对于缓冲传输模式，本机通道形状为 <xref:System.ServiceModel.Channels.IDuplexSessionChannel>。 对于流传输模式，本机通道为 <xref:System.ServiceModel.Channels.IRequestChannel> 和 <xref:System.ServiceModel.Channels.IReplyChannel>。 在直接（即，不是通过服务协定）使用这些传输协议的现有应用程序中更改传输模式需要更改通道工厂和侦听器的预期通道形状。  
  
## <a name="see-also"></a>另请参阅

- [如何：启用流处理](how-to-enable-streaming.md)
