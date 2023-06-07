---
title: 大型数据和流
description: 了解基于 WCF XML 的通信、编码器和流式处理数据（包括二进制数据传输）的注意事项。
ms.date: 03/30/2017
ms.assetid: ab2851f5-966b-4549-80ab-c94c5c0502d2
ms.openlocfilehash: 09e020801486c09c027883fca3d67a6c2e2fe8d7
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96234695"
---
# <a name="large-data-and-streaming"></a>大型数据和流

Windows Communication Foundation (WCF) 是一种基于 XML 的通信基础结构。 由于 XML 数据通常采用 [xml 1.0 规范](https://www.w3.org/TR/REC-xml/)中定义的标准文本格式进行编码，因此连接的系统开发人员和架构师通常会担心线路需求量 (或大小) 网络上发送的消息，而基于文本的 XML 编码对于有效传输二进制数据会带来特殊的挑战。  
  
## <a name="basic-considerations"></a>基本考虑事项  

 为了提供有关 WCF 的以下信息的背景信息，本部分重点介绍了通常适用于连接的系统基础结构的编码、二进制数据和流式处理的一些一般注意事项和注意事项。  
  
### <a name="encoding-data-text-vs-binary"></a>编码数据：文本和二进制  

 开发人员最常有的顾虑包括：认为与二进制格式相比 XML 的开销非常可观（因为其开始标记和结束标记的重复性），数值的编码可能要大得多（因为它们是以文本值来表示的），并且无法有效地表示二进制数据（因为它们必须进行特殊的编码才能嵌入到文本格式中）。  
  
 尽管上述以及其他许多类似的问题是存在的，但是 XML Web services 环境中的 XML 文本编码消息与旧式的远程过程调用 (RPC) 环境中的二进制编码消息之间的实际差异，通常远远小于最初的考虑中所预想的程度。  
  
 XML 文本编码消息是透明的并且是“易读的”，而比较而言，二进制消息通常相当晦涩，在无工具的情况下很难解码。 这种易读性方面的差异会使人忽视这样一点：二进制消息的负载中通常还携带内联元数据，这样会和 XML 文本消息一样增加开销。 对于旨在提供松散耦合和动态调用功能的二进制格式，这种情况尤为突出。  
  
 不过，二进制格式通常在“头”中携带此类描述性元数据信息，而且头中还声明了后续数据记录的数据布局。 之后负载将遵循这一公共元数据块声明，这样就将进一步的开销控制在最小的程度。 相反，XML 将每个数据项都包括在一个元素或属性中，这样会为每个序列化的负载对象重复包括包含负载对象用的元数据。 因此，将文本表示法与二进制表示法进行比较，会发现单个序列化的负载对象的大小相差不大，因为二者都必须有一些描述性元数据。但由于二进制格式的每个额外传输的负载对象都共享同一元数据描述，因此总体开销较低，也就是说二进制格式更有优势。  
  
 但是，对于某些数据类型（如数字），使用固定大小的二进制数字表示法（例如，使用 128 位的十进制类型，而不是纯文本）可能是不利的，因为纯文本表示法可能会小若干字节。 由于文本数据可以选择通常更为灵活的 XML 文本编码选项，因此在大小上也可能具有优势，而一些二进制格式可能默认为 16 位甚至 32 位 Unicode，这不适用于 .NET 二进制 XML 格式。  
  
 因此，要在文本与二进制之间进行选择，不能简单地认为二进制消息总是小于 XML 文本消息。  
  
 XML 文本消息的一个明确优点为：它们是基于标准的消息，并且提供最为广泛的互操作性选项和平台支持。 有关详细信息，请参阅本主题后面的 "编码" 一节。  
  
### <a name="binary-content"></a>二进制内容  

 从最终的消息大小这一角度而言，二进制编码优于基于文本编码的一个方面就在于大型二进制数据项，例如，图片、视频、音效剪辑或者必须在服务与其使用者之间交换的任何其他形式的非透明二进制数据。 为了使这些类型的数据也适合 XML 文本，常用的方法就是使用 Base64 编码对其进行编码。  
  
 在 Base64 编码字符串中，每个字符都表示原始 8 位数据的 6 位，这导致 Base64 的编码开销比率是 4:3，且未计算额外的格式字符（回车符/换行符），而按惯例这些字符通常是会添加的。 虽然 XML 编码与二进制编码之间的差异显著与否通常视具体情况而定，但当传送 500 MB 负载时大小增加超过 33% 通常是不可接受的。  
  
 为避免这种编码开销，消息传输优化机制 (MTOM) 标准允许将消息中包含的大型数据元素外部化，并将其作为无任何特殊编码的二进制数据随消息一起传送。 使用 MTOM，消息以类似方式与简单邮件传输协议进行交换 (SMTP) 电子邮件，其中包含附件或嵌入内容 (图片和其他嵌入内容) ;MTOM 消息会打包为多部分/相关 MIME 序列，根部分作为实际的 SOAP 消息。  
  
 MTOM SOAP 消息是在其未编码版本的基础上进行修改的，这样引用各自 MIME 部分的特殊元素标记会替代消息中包含二进制数据的原始元素。 因此，SOAP 消息通过指向随其发送的 MIME 部分来引用二进制内容，但除此之外则仅仅携带 XML 文本数据。 因为此模型与完善的 SMTP 模型基本一致，所以在多种平台上编码和解码 MTOM 消息有着广泛的工具支持，这令其成为一个极其具有可互操作性的选择。  
  
 但是，与 Base64 一样，对于 MIME 格式，MTOM 也有一些必要的开销，这样仅在二进制数据元素的大小超过大约 1 KB 时，才能体现出使用 MTOM 的好处。 由于这一开销，如果二进制负载保持在该阈值之下，则 MTOM 编码的消息可能会大于对二进制数据使用 Base64 编码的消息。 有关详细信息，请参阅本主题后面的 "编码" 一节。  
  
### <a name="large-data-content"></a>大型数据内容  

 即使不考虑线路需求量，前面提及的 500 MB 负载也对服务和客户端本身提出了很大的考验。 默认情况下，WCF 处理 *缓冲模式下* 的消息。 这意味着消息的整个内容在发送前或接收后都存在于内存中。 尽管对于大多数情形来说这是个很好的策略，并且是消息传递功能（如数字签名）和可靠传递所必需的，但是大型消息可能会耗尽系统资源。  
  
 处理大型负载的策略是流。 尽管消息（尤其是以 XML 表示的消息）通常会被认为是相对紧凑的数据包，但消息大小也可能达到 GB 数量级，这样的大小与连续的数据流而不是数据包相仿。 当以流模式而不是缓冲模式传输数据时，发送方会以流的形式将消息正文的内容提供给接收方，并且消息基础结构会不断地将就绪的数据从发送方转发给接收方。  
  
 传输此类大型数据内容的最常见情形是传输具有以下特点的二进制数据对象：  
  
- 无法方便地分成消息序列。  
  
- 必须以及时方式传递。  
  
- 当开始传输时，还不是已全部就绪。  
  
 对于不具有上述限制条件的数据，通常最好在一个会话的范围内发送消息序列，而不是一次性地发送一个大消息。 有关详细信息，请参阅本主题后面的 "流式处理数据" 一节。  
  
 发送大量数据时，需要设置 `maxAllowedContentLength` iis 设置 (有关详细信息，请参阅 [配置 Iis 请求限制](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/)) 和 `maxReceivedMessageSize` 绑定设置 (例如 [BasicHttpBinding. MaxReceivedMessageSize](xref:System.ServiceModel.HttpBindingBase.MaxReceivedMessageSize%2A) 或 <xref:System.ServiceModel.NetTcpBinding.MaxReceivedMessageSize%2A>) 。 `maxAllowedContentLength`属性默认为 28.6 MB， `maxReceivedMessageSize` 属性默认为64kb。  
  
## <a name="encodings"></a>编码  

 *编码* 定义有关如何在网络上显示消息的一组规则。 *编码器* 实现此类编码，并负责在发送方，将内存中的内存 <xref:System.ServiceModel.Channels.Message> 转换为可通过网络发送的字节流或字节缓冲区。 在接收方，编码器会将一系列字节转变为内存中的消息。  
  
 WCF 包括三个编码器，并允许你编写并插入你自己的编码器（如有必要）。  
  
 每个标准绑定都包括一个预配置编码器，因此默认情况下带 Net* 前缀的绑定使用二进制编码器（通过包括 <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement> 类），而 <xref:System.ServiceModel.BasicHttpBinding> 和 <xref:System.ServiceModel.WSHttpBinding> 类则使用文本消息编码器（通过 <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement> 类）。  
  
|编码器绑定元素|描述|  
|-----------------------------|-----------------|  
|<xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement>|文本消息编码器是所有基于 HTTP 的绑定的默认编码器，并且是最关注互操作性的所有自定义绑定的正确选择。 此编码器读取和编写标准 SOAP 1.1/SOAP 1.2 文本消息，而不会对二进制数据进行任何特殊处理。 如果 <xref:System.ServiceModel.Channels.MessageVersion?displayProperty=nameWithType> 消息的属性设置为 <xref:System.ServiceModel.Channels.MessageVersion.None?displayProperty=nameWithType> ，则会从输出中省略 SOAP 信封包装，并且仅序列化消息正文内容。|  
|<xref:System.ServiceModel.Channels.MtomMessageEncodingBindingElement>|MTOM 消息编码器是一个文本编码器，实现对二进制数据的特殊处理，默认情况下在任何标准绑定中都不会使用，因为它是一个严格按具体情况进行优化的实用工具。 只有当二进制数据的量不超过某个阈值时，MTOM 编码才具有优势，如果消息包含的二进制数据超过了这个阈值，则这些数据会外部化到消息信封之后的 MIME 部分。 请参见本节后面部分中的“启用 MTOM”。|  
|<xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement>|二进制消息编码器是 Net * 绑定的默认编码器，只要两个通信方都基于 WCF，就可以选择相应的编码器。 二进制消息编码器使用 .NET 二进制 XML 格式，该格式是 XML 信息集 (Information Sets, Infosets) 的 Microsoft 特定二进制表示法，与等效的 XML 1.0 表示法相比产生的需求量通常较小，并将二进制数据编码为字节流。|  
  
 通常，文本消息编码是要求互操作性的任意通信途径的最佳选择，而二进制消息编码则是其他任意通信途径的最佳选择。 通常，对于单个消息而言，二进制消息编码生成的消息大小要小于文本编码，并且在通信会话期间消息大小会逐渐变得更小。 与文本编码不同的是，二进制编码不需要对二进制数据使用特殊处理（例如，使用 Base64），但会将字节表示为字节。  
  
 如果您的解决方案不要求互操作性，但您仍希望使用 HTTP 传输，则可以将 <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement> 编写为一个使用 <xref:System.ServiceModel.Channels.HttpTransportBindingElement> 类进行传输的自定义绑定。 如果您的服务上有许多客户端要求互操作性，则建议您向各个启用的客户端公开其中每一个都具有适当的传输和编码选择的并行终结点。  
  
### <a name="enabling-mtom"></a>启用 MTOM  

 当要求互操作性，并且必须发送大型二进制数据时，MTOM 消息编码是一个备选的编码策略，您可以在标准 <xref:System.ServiceModel.BasicHttpBinding> 或 <xref:System.ServiceModel.WSHttpBinding> 绑定上启用它，方法是：将该绑定的 `MessageEncoding` 属性设置为 <xref:System.ServiceModel.WSMessageEncoding.Mtom>，或者将 <xref:System.ServiceModel.Channels.MtomMessageEncodingBindingElement> 编写为 <xref:System.ServiceModel.Channels.CustomBinding>。 下面的示例代码（从 [MTOM 编码](../samples/mtom-encoding.md) 示例中提取）演示了如何在配置中启用 MTOM。  
  
```xml  
<system.serviceModel>  
     …  
    <bindings>  
      <wsHttpBinding>  
        <binding name="ExampleBinding" messageEncoding="Mtom"/>  
      </wsHttpBinding>  
    </bindings>  
     …  
</system.serviceModel>  
```  
  
 如上所述，是否使用 MTOM 编码取决于您要发送的数据量。 另外，因为 MTOM 是在绑定级别启用的，所以启用 MTOM 会影响给定终结点上的所有操作。  
  
 因为 MTOM 编码器总是会发出 MTOM 编码的 MIME/多部分消息，而与二进制数据是否最终进行外部化无关，所以通常情况下，只有在终结点交换超过 1 KB 二进制数据的消息时，才应对终结点启用 MTOM。 另外，在可能的情况下，应限定设计为与启用 MTOM 的终结点一起使用的服务协定仅指定此类数据传输操作。 相关的控制功能应当位于一个单独的协定中。 这一“仅 MTOM”规则仅适用于通过已启用 MTOM 的终结点发送的消息；MTOM 编码器也可以对传入的非 MTOM 消息进行解码和分析。  
  
 使用 MTOM 编码器符合所有其他 WCF 功能。 请注意，可能不是在所有情况下都能遵守此规则，例如，当需要会话支持时。  
  
### <a name="programming-model"></a>编程模型  

 不论在应用程序中使用三个内置编码器中的哪一个，您在传输二进制数据方面的编程体验都是相同的。 不同之处在于 WCF 如何根据数据类型处理数据。  
  
```csharp
[DataContract]  
class MyData  
{  
    [DataMember]  
    byte[] binaryBuffer;  
    [DataMember]  
    string someStringData;  
}
```  
  
 当使用 MTOM 时，将根据以下规则序列化上面的数据协定：  
  
- 如果 `binaryBuffer` 不是 `null`，并且有个别包含足够的数据，使得需要 Base64 编码所没有的 MTOM 外部化开销（MIME 头等等），则这些数据将外部化并作为二进制 MIME 部分随消息一起传送。 如果未超过阈值，则数据会编码为 Base64。  
  
- 字符串（和其他所有非二进制的类型）无论多大，始终表示为消息正文内的字符串。  
  
 无论您是否使用显式数据协定（如上面的示例中所示）、操作中是否使用参数列表、是否有嵌套的数据协定或者是否传输集合内的数据协定对象，MTOM 编码的效果都是一样的。 字节数组始终是优化的候选项，如果达到优化阈值，将会对字节数组进行优化。  
  
> [!NOTE]
> 您不应在数据协定内使用 <xref:System.IO.Stream?displayProperty=nameWithType> 派生类型。 应使用流模型传输流数据，如下面的“数据的流模式”一节所述。  
  
## <a name="streaming-data"></a>数据的流模式  

 当有大量数据要传输时，WCF 中的流传输模式是一种可用于在内存中缓冲和处理消息的默认行为的可行替代方法。  
  
 如上所述，只应对数据无法分段、消息必须以及时的方式传递或者当传输启动时数据尚未完全就绪的大型消息（带文本或二进制内容）启用流模式。  
  
### <a name="restrictions"></a>限制  

 启用流式处理时，不能使用大量的 WCF 功能：  
  
- 无法执行消息正文的数字签名，因为它们需要对整个消息内容进行哈希运算。 采用流模式的情况下，当构造和发送消息头时，内容尚未完全就绪，因此无法计算数字签名。  
  
- 加密依赖于数字签名来验证是否已正确地重新构造数据。  
  
- 如果消息在传输过程中丢失，可靠的会话必须在客户端上缓冲已发送的消息以便可以重新传递，并且在将消息传递给服务实现之前必须在服务上保留消息以保存消息顺序，以备在未按顺序接收消息时可以按照正确的顺序重新排列消息。  
  
 由于上述功能约束，您只能对流模式使用传输级安全选项，并且无法打开可靠会话。 流处理仅在下列系统定义的绑定中可用：  
  
- <xref:System.ServiceModel.BasicHttpBinding>  
  
- <xref:System.ServiceModel.NetTcpBinding>  
  
- <xref:System.ServiceModel.NetNamedPipeBinding>  
  
- <xref:System.ServiceModel.WebHttpBinding>  
  
 由于 <xref:System.ServiceModel.NetTcpBinding> 和 <xref:System.ServiceModel.NetNamedPipeBinding> 的基础传输具有内在的可靠传递和基于连接的会话支持，因此与 HTTP 不同，这两个绑定在实践中受上述约束的影响非常小。  
  
 流模式在消息队列 (MSMQ) 传输中不可用，因此不能与 <xref:System.ServiceModel.NetMsmqBinding> 或 <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding> 类一起使用。 消息队列传输仅支持限定消息大小的缓冲数据传输，而大多数情形下其他所有传输都不具有任何实际的消息大小限制。  
  
 当使用对等通道传输时，流模式也不可用，因此流模式在 <xref:System.ServiceModel.NetPeerTcpBinding> 中不可用。  
  
#### <a name="streaming-and-sessions"></a>流和会话  

 在流与基于会话的绑定一起调用时可能会产生意外行为。 可通过单一通道（数据报通道）执行所有流调用，该通道不支持会话，即使将正在使用的绑定配置为使用会话也是如此。 如果多个客户端通过基于会话的绑定对同一服务对象进行流调用，并且该服务对象的并发模式设置为“单个”，同时其实例上下文模式设置为 PerSession，则所用调用都必须经过数据报通道，因此一次只处理一个调用。 一个或多个客户端可能会超时。可以通过将服务对象的实例上下文模式设置为 PerCall 或并发性到多个，来解决此问题。  
  
> [!NOTE]
> MaxConcurrentSessions 在此情况下不会产生任何影响，因为只有一个“会话”可用。  
  
### <a name="enabling-streaming"></a>启用流模式  

 您可以通过以下方式启用流模式：  
  
- 以流模式发送和接受请求，以缓冲模式接受和返回响应 (<xref:System.ServiceModel.TransferMode.StreamedRequest>)。  
  
- 以缓冲模式发送和接受请求，以流模式接受和返回响应 (<xref:System.ServiceModel.TransferMode.StreamedResponse>)。  
  
- 在两个方向均以流模式发送/接收请求/响应 (<xref:System.ServiceModel.TransferMode.Streamed>).  
  
 您可以通过将传输模式设置为 <xref:System.ServiceModel.TransferMode.Buffered> 来禁用流模式，该设置是所有绑定的默认设置。 下面的代码演示如何在配置中设置传输模式。  
  
```xml  
<system.serviceModel>  
     …  
    <bindings>  
      <basicHttpBinding>  
        <binding name="ExampleBinding" transferMode="Streamed"/>  
      </basicHttpBinding>  
    </bindings>  
     …  
</system.serviceModel>  
```  
  
 当通过代码实例化绑定时，必须将该绑定（如果您创建自定义绑定，则为传输绑定元素）的 `TransferMode` 属性设置为上面提到的某个值。  
  
 您可以在不影响功能的情况下在通信双方的任何一方独立地对请求和答复或者同时对两个方向启用流模式。 不过，您应始终认为已传输数据的大小非常大，完全需要在通信链路的两个终结点上均启用流模式。 对于其中一个终结点未使用 WCF 实现的跨平台通信，使用流式处理的能力取决于平台的流式处理功能。 另一个极少见的例外可能是一种内存消耗驱动情形，在这种情形下，客户端或服务必须尽量减小其工作集，并且只能提供较小的缓冲区大小。  
  
### <a name="enabling-asynchronous-streaming"></a>启用异步流处理  

 若要启用异步流，请将 <xref:System.ServiceModel.Description.DispatcherSynchronizationBehavior> 终结点行为添加到服务主机，并将其 <xref:System.ServiceModel.Description.DispatcherSynchronizationBehavior.AsynchronousSendEnabled%2A> 属性设置为 `true`。 我们还在发送端添加真正异步流处理的能力。 在将消息流式处理到多个客户端的方案中，这可提高服务的可扩展性；其中某些客户端的读取速度较慢，这可能由于网络拥塞或根本无法阅读造成的。 在这些方案中，我们现在不针对每个客户端对服务阻止各个线程。 这确保服务能够处理多得多的客户端，进而提高服务的可扩展性。  
  
### <a name="programming-model-for-streamed-transfers"></a>流式传输的编程模型  

 流式传输的编程模型非常简单。 要接收流数据，请指定具有单个 <xref:System.IO.Stream> 类型化输入参数的操作协定。 为了返回流数据，应返回一个 <xref:System.IO.Stream> 引用。  
  
```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
public interface IStreamedService  
{  
    [OperationContract]  
    Stream Echo(Stream data);  
    [OperationContract]  
    Stream RequestInfo(string query);  
    [OperationContract(OneWay=true)]  
    void ProvideInfo(Stream data);  
}  
```  
  
 上面示例中的操作 `Echo` 接收并返回一个流，因此应当用在具有 <xref:System.ServiceModel.TransferMode.Streamed> 的绑定上。 对于操作 `RequestInfo`，<xref:System.ServiceModel.TransferMode.StreamedResponse> 最适合，因为它仅仅返回 <xref:System.IO.Stream>。 单向操作最适合于 <xref:System.ServiceModel.TransferMode.StreamedRequest>。  
  
 请注意，向后面的 `Echo` 或 `ProvideInfo` 操作添加第二个参数会导致服务模型回复为缓冲策略，并使用流的运行时序列化表示。 只有具有单个输入流参数的操作才与端对端请求流兼容。  
  
 类似地，此规则也适用于消息协定。 如下面的消息协定所示，在流模式的消息协定中，只能有一个正文成员。 如果希望使用流传送更多信息，必须在消息头中携带这一信息。 消息正文是专为流内容保留的。  
  
```csharp
[MessageContract]  
public class UploadStreamMessage  
{  
   [MessageHeader]  
   public string appRef;  
   [MessageBodyMember]  
   public Stream data;  
}
```  
  
 当流到达文件尾 (EOF) 时，流传输结束，消息关闭。 发送消息 (返回值或) 调用操作时，可以传递 <xref:System.IO.FileStream> ，然后 WCF 基础结构将从该流中提取所有数据，直到流已完全读取并到达 EOF。 要为不存在此类预置 <xref:System.IO.Stream> 派生类的源传输流数据，请构造这样一个类，用该类覆盖流源，并将其用作自变量或返回值。  
  
 当接收消息时，WCF 会在 Base64 编码的消息正文内容 (或各自的 MIME 部分中构造流，前提是使用 MTOM) 并且当内容已读取时，流到达 EOF。  
  
 传输级流还可以与其他任何消息协定类型（参数列表、数据协定参数和显式消息协定）一起工作，但是由于此类型化消息的序列化和反序列化要求由序列化程序进行缓冲，因此不建议使用此类协定变体。  
  
### <a name="special-security-considerations-for-large-data"></a>关于大型数据的特殊安全考虑事项  

 所有绑定都允许您限制传入消息的大小，以阻止拒绝服务攻击。 <xref:System.ServiceModel.BasicHttpBinding>例如，公开[BasicHttpBinding MaxReceivedMessageSize](xref:System.ServiceModel.HttpBindingBase.MaxReceivedMessageSize%2A)属性，该属性限制传入消息的大小，因此也会限制处理消息时所访问的最大内存量。 此单元是以字节为单位设置的，默认值为 65,536 个字节。  
  
 大型数据流情形所特有的安全威胁会在接收方希望数据以流模式发送时导致数据缓冲，从而促使拒绝服务。 例如，WCF 始终会缓冲消息的 SOAP 标头，因此攻击者可能会构建一个完全由标头组成的大型恶意消息来强制缓冲数据。 当启用流模式时，`MaxReceivedMessageSize` 可能设置为一个极其大的值，因为接收方绝预料不到会一次性地在内存中缓冲整个消息。 如果强制 WCF 对消息进行缓冲，则会发生内存溢出。  
  
 因此，这种情况下仅限制最大传入消息大小是不够的。 `MaxBufferSize`需要属性来约束 WCF 缓冲区的内存。 进行流处理时，将此属性设置为一个安全值（或保留为默认值）很重要。 例如，假设您的服务必须接收大至 4 GB 的文件，并将其存储在本地磁盘上。 另外，还假设您的内存存在一些约束，一次只能缓冲 64 KB 的数据。 这样，您应该将 `MaxReceivedMessageSize` 设置为 4 GB，将 `MaxBufferSize` 设置为 64 KB。 另外，在您的服务实现中，必须确保仅按 64 KB 大小的块从传入流中读取数据，并且在上一块写入到磁盘并从内存中丢弃之前，不读取下一块。  
  
 还必须了解，此配额仅限制由 WCF 完成的缓冲，而不能防止您在自己的服务或客户端实现中执行任何缓冲。 有关其他安全注意事项的详细信息，请参阅 [数据的安全注意事项](security-considerations-for-data.md)。  
  
> [!NOTE]
> 使用缓冲传输还是流传输是在终结点本地决定的。 对于 HTTP 传输，传输模式不会通过连接传播，也不会传播到代理服务器和其他中间方。 设置传输模式不会反映在服务接口的说明中。 将 WCF 客户端生成到服务后，必须编辑要与流式传输一起使用的服务的配置文件，以设置模式。 对于 TCP 和命名管道传输协议，该传输模式将作为策略断言传播。  
  
## <a name="see-also"></a>另请参阅

- [如何：启用流处理](how-to-enable-streaming.md)
