---
title: 通道分块
ms.date: 03/30/2017
ms.assetid: e4d53379-b37c-4b19-8726-9cc914d5d39f
ms.openlocfilehash: 7a5e5292bcb37e83de21458716e34887a0557d91
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84585540"
---
# <a name="chunking-channel"></a>通道分块

使用 Windows Communication Foundation （WCF）发送大消息时，通常需要限制用于缓冲这些消息的内存量。 一种可能的解决方案是流处理消息正文（假定数据主要集中在正文中）。 不过，有些协议要求对整个消息进行缓冲。 可靠消息和消息安全就是两个这样的示例。 另一个可能的解决方案是将大消息分割成称为消息块的小消息，一次发送一个消息块，并在接收端重建大消息。 应用程序本身就能实现这种分块和取消分块，或者使用自定义通道来实现。 块区通道示例演示如何使用自定义协议或分层通道为任意大的消息进行分块和取消分块。

应始终在构造了要发送的整个消息后才使用块区。 块区通道应始终在安全通道和可靠会话通道之下分层。

> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Channels\ChunkingChannel`

## <a name="chunking-channel-assumptions-and-limitations"></a>块区通道的假设和限制

### <a name="message-structure"></a>消息结构

块区通道假定要分块的消息具有下面的消息结构：

```xml
<soap:Envelope>
  <!-- headers -->
  <soap:Body>
    <operationElement>
      <paramElement>data to be chunked</paramElement>
    </operationElement>
  </soap:Body>
</soap:Envelope>
```

在使用 ServiceModel 时，具有一个输入参数的协定操作会使其输入消息符合此消息形状。 同样，具有一个输出参数或一个返回值的协定操作会使其输出消息符合此消息形状。 下面是这种操作的示例：

```csharp
[ServiceContract]
interface ITestService
{
    [OperationContract]
    Stream EchoStream(Stream stream);

    [OperationContract]
    Stream DownloadStream();

    [OperationContract(IsOneWay = true)]
    void UploadStream(Stream stream);
}
```

### <a name="sessions"></a>会话

块区通道要求消息以消息（消息块）的有序传递方式只传送一次。 这意味着基础通道堆栈必须是会话式的。 会话可以由传输提供（例如 TCP 传输），也可以由会话协议通道提供（例如 ReliableSession 通道）。

### <a name="asynchronous-send-and-receive"></a>异步发送和接收

此版本的块区通道示例中未实现异步发送和接收方法。

## <a name="chunking-protocol"></a>块区协议

块区通道定义一个协议，该协议指示一系列消息块的开始点和结束点以及每个消息块的序列号。 下面三个示例消息演示开始消息、消息块消息和结束消息，并有说明每个消息主要特征的注释。

### <a name="start-message"></a>开始消息

```xml
<s:Envelope xmlns:a="http://www.w3.org/2005/08/addressing"
            xmlns:s="http://www.w3.org/2003/05/soap-envelope">
  <s:Header>
<!--Original message action is replaced with a chunking-specific action. -->
    <a:Action s:mustUnderstand="1">http://samples.microsoft.com/chunkingAction</a:Action>
<!--
Original message is assigned a unique id that is transmitted
in a MessageId header. Note that this is different from the WS-Addressing MessageId header.
-->
    <MessageId s:mustUnderstand="1" xmlns="http://samples.microsoft.com/chunking">
53f183ee-04aa-44a0-b8d3-e45224563109
</MessageId>
<!--
ChunkingStart header signals the start of a chunked message.
-->
    <ChunkingStart s:mustUnderstand="1" i:nil="true" xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://samples.microsoft.com/chunking" />
<!--
Original message action is transmitted in OriginalAction.
This is required to re-create the original message on the other side.
-->
    <OriginalAction xmlns="http://samples.microsoft.com/chunking">
http://tempuri.org/ITestService/EchoStream
    </OriginalAction>
   <!--
    All original message headers are included here.
   -->
  </s:Header>
  <s:Body>
<!--
Chunking assumes this structure of Body content:
<element>
  <childelement>large data to be chunked<childelement>
</element>
The start message contains just <element> and <childelement> without
the data to be chunked.
-->
    <EchoStream xmlns="http://tempuri.org/">
      <stream />
    </EchoStream>
  </s:Body>
</s:Envelope>
```

### <a name="chunk-message"></a>消息块消息

```xml
<s:Envelope
  xmlns:a="http://www.w3.org/2005/08/addressing"
  xmlns:s="http://www.w3.org/2003/05/soap-envelope">
  <s:Header>
   <!--
    All chunking protocol messages have this action.
   -->
    <a:Action s:mustUnderstand="1">
      http://samples.microsoft.com/chunkingAction
    </a:Action>
<!--
Same as MessageId in the start message. The GUID indicates which original message this chunk belongs to.
-->
    <MessageId s:mustUnderstand="1"
               xmlns="http://samples.microsoft.com/chunking">
      53f183ee-04aa-44a0-b8d3-e45224563109
    </MessageId>
<!--
The sequence number of the chunk.
This number restarts at 1 with each new sequence of chunks.
-->
    <ChunkNumber s:mustUnderstand="1"
                 xmlns="http://samples.microsoft.com/chunking">
      1096
    </ChunkNumber>
  </s:Header>
  <s:Body>
<!--
The chunked data is wrapped in a chunk element.
The encoding of this data (and the entire message)
depends on the encoder used. The chunking channel does not mandate an encoding.
-->
    <chunk xmlns="http://samples.microsoft.com/chunking">
kfSr2QcBlkHTvQ==
    </chunk>
  </s:Body>
</s:Envelope>
```

### <a name="end-message"></a>结束消息

```xml
<s:Envelope xmlns:a="http://www.w3.org/2005/08/addressing"
            xmlns:s="http://www.w3.org/2003/05/soap-envelope">
  <s:Header>
    <a:Action s:mustUnderstand="1">
      http://samples.microsoft.com/chunkingAction
    </a:Action>
<!--
Same as MessageId in the start message. The GUID indicates which original message this chunk belongs to.
-->
    <MessageId s:mustUnderstand="1"
               xmlns="http://samples.microsoft.com/chunking">
      53f183ee-04aa-44a0-b8d3-e45224563109
    </MessageId>
<!--
ChunkingEnd header signals the end of a chunk sequence.
-->
    <ChunkingEnd s:mustUnderstand="1" i:nil="true"
                 xmlns:i="http://www.w3.org/2001/XMLSchema-instance"
                 xmlns="http://samples.microsoft.com/chunking" />
<!--
ChunkingEnd messages have a sequence number.
-->
    <ChunkNumber s:mustUnderstand="1"
                 xmlns="http://samples.microsoft.com/chunking">
      79
    </ChunkNumber>
  </s:Header>
  <s:Body>
<!--
The ChunkingEnd message has the same <element><childelement> structure
as the ChunkingStart message.
-->
    <EchoStream xmlns="http://tempuri.org/">
      <stream />
    </EchoStream>
  </s:Body>
</s:Envelope>
```

## <a name="chunking-channel-architecture"></a>块区通道体系结构

块区通道是一个在高级别遵循典型通道体系结构的 `IDuplexSessionChannel`。 有一个 `ChunkingBindingElement`，可用于生成 `ChunkingChannelFactory` 和 `ChunkingChannelListener`。 在请求时，`ChunkingChannelFactory` 可创建 `ChunkingChannel` 的实例。 当接受新的内部通道时，`ChunkingChannelListener` 可创建 `ChunkingChannel` 的实例。 `ChunkingChannel` 本身负责发送和接收消息。

在下一个较低级别，`ChunkingChannel` 依赖于若干组件来实现块区协议。 在发送端，通道使用一个名为 <xref:System.Xml.XmlDictionaryWriter> 的自定义 `ChunkingWriter`，它完成实际的分块。 `ChunkingWriter` 直接使用内部通道发送消息块。 使用自定义 `XmlDictionaryWriter` 可以在编写原始消息的大型正文的同时发送消息块。 这意味着不对整个原始消息进行缓冲。

![显示分块通道发送体系结构的关系图。](./media/chunking-channel/chunking-channel-send.gif)

在接收端，`ChunkingChannel` 从内部通道提取消息并将其传递到名为 <xref:System.Xml.XmlDictionaryReader> 的自定义 `ChunkingReader`，后者将从传入的消息块重组原始消息。 `ChunkingChannel` 将此 `ChunkingReader` 包装到一个名为 `Message` 的自定义 `ChunkingMessage` 实现中并将此消息返回到上一层。 通过 `ChunkingReader` 和 `ChunkingMessage` 的这一组合，可以在上一层读取原始消息正文时取消消息的分块，而不必缓冲整个原始消息正文。 `ChunkingReader` 有一个队列可用来将传入的消息块缓冲为缓冲的消息块，缓冲的消息块最多可以达到可配置的最大缓冲消息块数量。 当达到此最大限度时，读取器将等待上一层将消息从队列中排出（即仅从原始消息正文中读取）或等待直到达到最大接收超时值。

![显示分块通道接收体系结构的关系图。](./media/chunking-channel/chunking-channel-receive.gif)

## <a name="chunking-programming-model"></a>块区编程模型

服务开发人员可以通过在协定中对操作应用 `ChunkingBehavior` 属性来指定哪些消息需要分块。 该属性公开一个 `AppliesTo` 属性，允许开发人员指定是否对输入消息和/或输出消息应用分块。 下面的示例演示 `ChunkingBehavior` 属性的用法：

```csharp
[ServiceContract]
interface ITestService
{
    [OperationContract]
    [ChunkingBehavior(ChunkingAppliesTo.Both)]
    Stream EchoStream(Stream stream);

    [OperationContract]
    [ChunkingBehavior(ChunkingAppliesTo.OutMessage)]
    Stream DownloadStream();

    [OperationContract(IsOneWay=true)]
    [ChunkingBehavior(ChunkingAppliesTo.InMessage)]
    void UploadStream(Stream stream);

}
```

`ChunkingBindingElement` 将在此编程模型中编译一个操作 URI 的列表，用于标识要分块的消息。 每个传出消息的操作都将与此列表进行对比以确定该消息是应该进行分块还是直接发送。

## <a name="implementing-the-send-operation"></a>实现 Send 操作

在高级别上，Send 操作首先检查传出消息是否必须分块，如果不是，则使用内部通道直接发送消息。

如果消息必须分块，则 Send 操作将创建一个新的 `ChunkingWriter` 并对传出消息调用 `WriteBodyContents`，同时向该消息传递此 `ChunkingWriter`。 `ChunkingWriter` 然后进行消息分块（包括将原始消息头复制到开始消息块消息中）并使用内部通道发送消息块。

值得注意的一些详细信息：

- Send 首先调用 `ThrowIfDisposedOrNotOpened` 以确保 `CommunicationState` 处于已打开状态。

- 发送是同步的，以便每个会话一次只发送一个消息。 有一个名为 `ManualResetEvent` 的 `sendingDone`，在发送分块消息时将会重置。 发送结束消息块消息后，将设置此事件。 Send 方法在尝试发送传出消息之前将等待设置此事件。

- Send 将锁定 `CommunicationObject.ThisLock` 以防止在发送时更改同步状态。 有关 <xref:System.ServiceModel.Channels.CommunicationObject> 状态和状态机的更多信息，请参见 <xref:System.ServiceModel.Channels.CommunicationObject> 文档。

- 传递给 Send 的超时值用作整个发送操作（包括发送所有消息块）的超时值。

- 为避免对整个原始消息正文进行缓存，选择了自定义 <xref:System.Xml.XmlDictionaryWriter> 设计。 如果要使用 <xref:System.Xml.XmlDictionaryReader> 对正文获取 `message.GetReaderAtBodyContents`，则将缓冲整个正文。 相反，我们有一个 <xref:System.Xml.XmlDictionaryWriter> 传递给的自定义 `message.WriteBodyContents` 。 由于消息会在该编写器上调用 WriteBase64，因此编写器会将消息块包装成消息并使用内部通道发送消息。 在发送信息块之前，WriteBase64 处于阻止状态。

## <a name="implementing-the-receive-operation"></a>实现 Receive 操作

在高级别上，Receive 操作首先检查传入消息是否不为 `null` 以及其操作是否为 `ChunkingAction`。 如果传入消息不符合这两个条件，则会从 Receive 按原样返回该消息。 否则，Receive 将创建一个新的 `ChunkingReader` 和一个包装在其周围的新的 `ChunkingMessage`（通过调用 `GetNewChunkingMessage`）。 在返回该新 `ChunkingMessage` 之前，Receive 将使用线程池的线程来执行 `ReceiveChunkLoop`，它会循环调用 `innerChannel.Receive` 并将消息块发送到 `ChunkingReader`，直到收到结束消息块消息或达到接收超时值。

值得注意的一些详细信息：

- 和 Send 一样，Receive 首先调用 `ThrowIfDisposedOrNotOepned` 以确保 `CommunicationState` 处于已打开状态。

- 接收也是同步的，以便一次只能从会话接收一个消息。 这一点特别重要，因为一旦接收了一个开始消息块消息，所有随后接收的消息都将是此新消息块序列中的消息块，直到接收到结束消息块消息为止。 在接收属于当前正在取消分块的消息的所有消息块之前，Receive 无法从内部通道提取消息。 为了完成此操作，Receive 使用一个名为 `ManualResetEvent` 的 `currentMessageCompleted`，它是在接收到结束消息块消息时设置的，并在接收到新的开始消息块消息时重置。

- 与 Send 不同，Receive 在接收时不阻止同步状态的转换。 例如，可以在正在接收时调用 Close，并等待挂起的原始消息接收完成或达到指定的超时值。

- 传递给 Receive 的超时值用作整个接收操作（包括接收所有消息块）的超时值。

- 如果使用消息的层在使用消息正文时的速率低于传入消息块消息的速率，则 `ChunkingReader` 会缓冲这些传入的消息块，直至达到由 `ChunkingBindingElement.MaxBufferedChunks` 指定的限制。 一旦达到该制，将不再从下一层拉取消息块，直到用完缓冲消息块或达到接收超时值。

## <a name="communicationobject-overrides"></a>CommunicationObject 重写

### <a name="onopen"></a>OnOpen

`OnOpen` 调用 `innerChannel.Open` 以打开内部通道。

### <a name="onclose"></a>OnClose

`OnClose` 首先将 `stopReceive` 设置为 `true` 以通知挂起的 `ReceiveChunkLoop` 停止。 然后，它会等待 `receiveStopped` <xref:System.Threading.ManualResetEvent> ，后者在停止时设置 `ReceiveChunkLoop` 。 如果 `ReceiveChunkLoop` 在指定的超时之内停止，则 `OnClose` 将使用剩余超时调用 `innerChannel.Close`。

### <a name="onabort"></a>OnAbort

`OnAbort` 调用 `innerChannel.Abort` 以中止内部通道。 如果有挂起的 `ReceiveChunkLoop`，则它会从挂起的 `innerChannel.Receive` 调用获取一个异常。

### <a name="onfaulted"></a>OnFaulted

当通道出错时，`ChunkingChannel` 不需要特殊行为，因此不重写 `OnFaulted`。

## <a name="implementing-channel-factory"></a>实现通道工厂

`ChunkingChannelFactory` 负责创建 `ChunkingDuplexSessionChannel` 的实例和负责级联状态向内部通道工厂的转换。

`OnCreateChannel` 使用内部通道工厂来创建 `IDuplexSessionChannel` 内部通道。 然后创建新的 `ChunkingDuplexSessionChannel`，同时为其传递此内部通道以及要分块的消息操作的列表和在接收时要缓冲的消息块的最大数量。 要分块的消息操作的列表和在接收时要缓冲的消息块的最大数量是在构造函数中传递给 `ChunkingChannelFactory` 的两个参数。 有关 `ChunkingBindingElement` 的一节说明了这些值的来源。

`OnOpen`、`OnClose`、`OnAbort` 及其异步等效方法调用内部通道工厂上的相应状态转换方法。

## <a name="implementing-channel-listener"></a>实现通道侦听器

`ChunkingChannelListener` 是围绕内部通道侦听器的包装程序。 除了将调用委托给该内部通道侦听器以外，其主要功能是在从内部通道侦听器接收的通道周围包装新的 `ChunkingDuplexSessionChannels`。 此操作在 `OnAcceptChannel` 和 `OnEndAcceptChannel` 中完成。 将会向新创建的 `ChunkingDuplexSessionChannel` 传递该内部通道以及前述的其他参数。

## <a name="implementing-binding-element-and-binding"></a>实现绑定元素和绑定

`ChunkingBindingElement` 负责创建 `ChunkingChannelFactory` 和 `ChunkingChannelListener`。 `ChunkingBindingElement`检查和中的 T 是否 `CanBuildChannelFactory` \<T> `CanBuildChannelListener` \<T> 属于类型 `IDuplexSessionChannel` （块区通道支持的唯一通道）以及绑定中的其他绑定元素是否支持此通道类型。

`BuildChannelFactory`\<T>首先检查是否可以生成请求的通道类型，然后获取要分块的消息操作的列表。 有关详细信息，请参阅以下部分。 然后它创建一个新的 `ChunkingChannelFactory`，同时为其传递内部通道工厂（从 `context.BuildInnerChannelFactory<IDuplexSessionChannel>` 返回）、消息操作列表和要缓冲的消息块的最大数量。 消息块的最大数量来自一个名为 `MaxBufferedChunks` 的属性，此属性由 `ChunkingBindingElement` 公开。

`BuildChannelListener<T>` 有一个类似的实现，用于创建 `ChunkingChannelListener` 并为其传递内部通道侦听器。

此示例中包括一个名为 `TcpChunkingBinding` 的示例绑定。 此绑定由两个绑定元素组成：`TcpTransportBindingElement` 和 `ChunkingBindingElement`。 除了公开 `MaxBufferedChunks` 属性以外，该绑定还设置几个 `TcpTransportBindingElement` 属性，如 `MaxReceivedMessageSize`（对于标头，将它设置为 `ChunkingUtils.ChunkSize` + 100KB 字节）。

`TcpChunkingBinding` 也实现 `IBindingRuntimePreferences` 并从 `ReceiveSynchronously` 方法返回 true，指示只实现同步的 Receive 调用。

### <a name="determining-which-messages-to-chunk"></a>确定对哪些消息分块

块区通道只对通过 `ChunkingBehavior` 属性标识的消息进行分块。 `ChunkingBehavior` 类实现 `IOperationBehavior` 并通过调用 `AddBindingParameter` 方法来实现。 在此方法中，`ChunkingBehavior` 将检查其 `AppliesTo` 属性（`InMessage` 和/或 `OutMessage`）的值以确定应该对哪些消息进行分块。 然后获取这些消息中每个消息的操作（从 `OperationDescription` 上的消息集合中获取），并将其添加到包含在 `ChunkingBindingParameter` 的实例内的字符串集合中。 然后将此 `ChunkingBindingParameter` 添加到所提供的 `BindingParameterCollection` 中。

当绑定元素生成通道工厂或通道侦听器时，会在 `BindingParameterCollection` 内将此 `BindingContext` 传递给绑定中的每个绑定元素。 的 `ChunkingBindingElement` 实现 `BuildChannelFactory<T>` ，并 `BuildChannelListener<T>` `ChunkingBindingParameter` 将其从中提取出来 `BindingContext’` `BindingParameterCollection` 。 然后将包含在 `ChunkingBindingParameter` 内的操作集合传递给 `ChunkingChannelFactory` 或 `ChunkingChannelListener`，后者又将它传递给 `ChunkingDuplexSessionChannel`。

## <a name="running-the-sample"></a>运行示例

#### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 使用以下命令安装 ASP.NET 4.0。

    ```console
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable
    ```

2. 确保已对[Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。

3. 若要生成解决方案，请按照[生成 Windows Communication Foundation 示例](building-the-samples.md)中的说明进行操作。

4. 若要以单机配置或跨计算机配置来运行示例，请按照[运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。

5. 先运行 Service.exe，然后运行 Client.exe 并观察两个控制台窗口的输出。

运行此示例时，应生成下面的输出。

Client：

```console
Press enter when service is available

 > Sent chunk 1 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 > Sent chunk 2 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 > Sent chunk 3 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 > Sent chunk 4 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 > Sent chunk 5 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 > Sent chunk 6 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 > Sent chunk 7 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 > Sent chunk 8 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 > Sent chunk 9 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 > Sent chunk 10 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 < Received chunk 1 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 < Received chunk 2 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 < Received chunk 3 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 < Received chunk 4 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 < Received chunk 5 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 < Received chunk 6 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 < Received chunk 7 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 < Received chunk 8 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 < Received chunk 9 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 < Received chunk 10 of message 5b226ad5-c088-4988-b737-6a565e0563dd
```

服务器:

```console
Service started, press enter to exit
 < Received chunk 1 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 < Received chunk 2 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 < Received chunk 3 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 < Received chunk 4 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 < Received chunk 5 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 < Received chunk 6 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 < Received chunk 7 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 < Received chunk 8 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 < Received chunk 9 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 < Received chunk 10 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 > Sent chunk 1 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 > Sent chunk 2 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 > Sent chunk 3 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 > Sent chunk 4 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 > Sent chunk 5 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 > Sent chunk 6 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 > Sent chunk 7 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 > Sent chunk 8 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 > Sent chunk 9 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 > Sent chunk 10 of message 5b226ad5-c088-4988-b737-6a565e0563dd
```
