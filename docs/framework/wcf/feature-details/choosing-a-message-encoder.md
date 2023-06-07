---
title: 选择消息编码器
ms.date: 03/30/2017
ms.assetid: 2204d82d-d962-4922-a79e-c9a231604f19
ms.openlocfilehash: 8b53c17cccc74153e652494ec9753302cda8679b
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96295153"
---
# <a name="choose-a-message-encoder"></a>选择消息编码器

本文讨论在 Windows Communication Foundation (WCF) ：二进制、文本和消息传输优化机制 (MTOM) 中所包含的消息编码器之间进行选择的条件。  
  
 在 WCF 中，可以指定如何通过 *绑定*（由一系列 *绑定元素* 组成）在终结点之间通过网络传输数据。 消息编码器由绑定堆栈中的消息编码绑定元素表示。 绑定包括多个可选协议绑定元素（如安全绑定元素或可靠消息绑定元素）、一个必需的消息编码绑定元素以及一个必需的传输绑定元素。  
  
 消息编码绑定元素位于可选协议绑定元素之下和必需的传输绑定元素之上。 在传出端，消息编码器序列化传出 <xref:System.ServiceModel.Channels.Message> 并将其传递到传输层。 在传入端，消息编码器从传输层接收已序列化的 <xref:System.ServiceModel.Channels.Message> 并将其传递到更高的协议层（如果存在），如果不存在此协议层，则传递到应用程序。  
  
 当连接到预先存在的客户端或服务器时，因为您需要将消息以另一端预期的方式来解码，所以您不能选择使用特定消息编码。 但是，如果您正在编写 WCF 服务，则可以通过多个终结点公开服务，每个终结点使用不同的消息编码。 这使客户端可以选择最佳的编码以通过最适合的终结点与您的服务通话，还使客户端可以灵活地选择最适合的编码。 使用多个终结点还使你可以将不同消息编码的优点与其他绑定元素结合起来。  
  
## <a name="system-provided-encoders"></a>系统提供的编码器  

 WCF 包含三种消息编码器，由以下三个类表示：  
  
- <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement>，文本消息编码器，同时支持纯 XML 编码和 SOAP 编码。 文本消息编码器的纯 XML 编码模式称为“纯旧式 XML”(POX)，以便与基于文本的 SOAP 编码进行区分。 若要启用 POX，请将 <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement.MessageVersion%2A> 属性设置为 <xref:System.ServiceModel.Channels.MessageVersion.None%2A>。 使用短信编码器与非 WCF 终结点互操作。  
  
- <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement>二进制消息编码器使用精简二进制格式，并针对 WCF 到 WCF 的通信进行了优化，因此无法互操作。 这也是 WCF 提供的所有编码器中性能最高的编码器。  
  
- <xref:System.ServiceModel.Channels.MtomMessageEncodingBindingElement>，绑定元素，指定使用 MTOM 编码的消息的字符编码和消息版本。 MTOM 是一种用于在 WCF 消息中传输二进制数据的有效技术。 MTOM 编码器会尝试在效率和互操作性之间建立平衡。 MTOM 编码以文本形式传输大多数 XML，但是会通过按原样（即不转换为文本）的方式传输来优化大型二进制数据块。 就效率而言，WCF 提供的编码器之间的 MTOM 介于文本 (最慢的) 和二进制 (最快的) 。  
  
## <a name="how-to-choose-a-message-encoder"></a>如何选择消息编码器  

 下表说明了用于选择消息编码器的常用因素。 确定因素对于您的应用程序重要性顺序，然后选择最适用于这些因素的消息编码器。 请确保考虑此表中未列出的其他因素和在您的应用程序中可能需要的任何自定义消息编码器。  
  
|因子|说明|支持此因素的编码器|  
|------------|-----------------|---------------------------------------|  
|支持的字符集|<xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement>和 <xref:System.ServiceModel.Channels.MtomMessageEncodingBindingElement> 仅支持 UTF8 和 UTF16 Unicode (*大字节序*) 编码 *little-endian* 。 如果需要其他编码（如 UTF7 或 ASCII），则必须使用自定义编码器。 有关示例自定义编码器，请参阅 [自定义消息编码器](../samples/custom-message-encoder-custom-text-encoder.md)。|文本|  
|检查|检查是在传送期间检查消息的功能。 使用或不使用 SOAP 的文本编码使很多程序不用专用工具就可以检查和分析消息。 使用传输安全时，在消息或传输级别，会影响检查消息的能力。 保密性会保护消息免于检查，完整性会保护消息免于修改。|文本|  
|可靠性|可靠性是编码器传输错误的复原能力。 也可以在消息层、传输层和应用程序层提供可靠性。 所有标准 WCF 编码器都假定另一层提供可靠性。 编码器几乎没有从传输错误恢复的能力。|无|  
|简单|简单性表示为编码规范创建编码器和解码器的简便性。 文本编码在简单性方面具有明显的优势，POX 文本编码的另一个优点在于不需要支持处理 SOAP。|文本 (POX)|  
|大小|编码确定在内容上施加的开销数量。 编码消息的大小与服务操作的最大吞吐量直接相关。 二进制编码通常比文本编码更精简。 当消息大小超出限制时，还可以考虑在编码过程中压缩消息内容。 但是，压缩增加了消息发送方和接收方的处理开销。|二进制|  
|流式处理|流处理使应用程序可以在整个消息到达之前开始处理此消息。 有效地使用流处理要求消息的重要数据位于消息的开始处，以便接收应用程序无需等待这些数据到达。 而且，使用流传输的应用程序必须在消息中以增量方式组织数据，以使内容没有前向相关性。 在很多情况下，您必须在对内容进行流处理和使内容具有尽可能小的传输大小之间进行折衷选择。|无|  
|第三方工具支持|编码的支持范围包括开发和诊断。 对于用于处理以 POX 格式编码的消息的库和工具包，第三方开发人员已经进行了很大的投入。|文本 (POX)|  
|互操作性|此因素是指 WCF 编码器与非 WCF 服务进行互操作的能力。|文本<br /><br /> MTOM（部分）|  
  
注意：使用二进制编码器时，在创建 XMLReader 时使用 IgnoreWhitespace 设置不会有任何影响。  例如，如果您在服务操作内执行以下操作：  

```csharp
public void OperationContract(XElement input)
{
    Console.WriteLine("{0}", input.Value);
    int counter = 0;
    var xreader = input.CreateReader();
    var reader = XmlReader.Create(xreader, new XmlReaderSettings() { IgnoreWhitespace = true });
    while (reader.Read())
    {
        counter++;
    }

    Console.WriteLine("Read {0} lines with reader", counter);
}
```  
  
IgnoreWhitespace 设置被忽略。  
  
## <a name="compression-and-the-binary-encoder"></a>压缩和二进制编码器

从 WCF 4.5 开始，WCF 二进制编码器添加了对压缩的支持。 这使您能够使用 gzip/deflate 算法从 WCF 客户端发送压缩的消息，此外还以来自自承载 WCF 服务的压缩消息进行响应。 此功能同时对 HTTP 和 TCP 传输启用压缩。 可以始终启用 IIS 承载的 WCF 服务，以通过配置 IIS 主机服务器发送压缩的响应。 可使用 <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement.CompressionFormat%2A?displayProperty=nameWithType> 属性来配置压缩类型。 此属性设置为 <xref:System.ServiceModel.Channels.CompressionFormat?displayProperty=nameWithType> 枚举值之一：

- <xref:System.ServiceModel.Channels.CompressionFormat.Deflate>
- <xref:System.ServiceModel.Channels.CompressionFormat.GZip>
- <xref:System.ServiceModel.Channels.CompressionFormat.None>
  
由于此属性仅在 binaryMessageEncodingBindingElement 上公开，因此你将需要创建如下所示的自定义绑定来使用此功能：

 ```xml
 <customBinding>
   <binding name="BinaryCompressionBinding">
     <binaryMessageEncoding compressionFormat ="GZip" />
     <httpTransport />
  </binding>
</customBinding>
 ```

客户端和服务都需要同意发送和接收压缩消息，因此必须在客户端和服务上的 binaryMessageEncoding 元素上配置 compressionFormat 属性。 如果未将服务或客户端配置为压缩，而另一端为，则会引发 ProtocolException。 应该仔细考虑如何启用压缩。 如果网络带宽是一个瓶颈，则压缩非常有用。 如果 CPU 是瓶颈，则压缩会减小吞吐量。 必须在模拟环境中进行相应的测试，以确定这对于应用程序是否有好处  
  
## <a name="see-also"></a>另请参阅

- [绑定](bindings.md)
