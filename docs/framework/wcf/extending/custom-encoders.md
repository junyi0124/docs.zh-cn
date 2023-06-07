---
title: 自定义编码器
ms.date: 03/30/2017
ms.assetid: fa0e1d7f-af36-4bf4-aac9-cd4eab95bc4f
ms.openlocfilehash: c2ad0c947afd293d0923faa3e9d914b6911ce941
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96254774"
---
# <a name="custom-encoders"></a>自定义编码器

本主题讨论如何创建自定义编码器。  
  
 在 Windows Communication Foundation (WCF) ，使用 *绑定* 指定如何在终结点之间跨网络传输数据。 绑定由一系列 *绑定元素* 组成。 绑定包括可选的协议绑定元素，如安全性、必需的 *消息编码器* 绑定元素以及必需的传输绑定元素。 消息编码器由消息编码绑定元素表示。 WCF 中包括三个消息编码器：二进制、消息传输优化机制 (MTOM) 和文本。  
  
 消息编码绑定元素将序列化传出 <xref:System.ServiceModel.Channels.Message> 并将其传递到传输层，或从传输层接收已序列化的消息并将其传递到协议层（如果存在），如果不存在协议层，则传递到应用程序。  
  
 消息编码器可将 <xref:System.ServiceModel.Channels.Message> 实例与网络表示形式互相转换。 尽管编码器被描述为位于通道堆栈的传输层之上，但它们实际上驻留在传输层中。 传输（如 HTTP）根据传输标准的需求格式化消息。 编码器（如文本 Xml）仅对消息进行编码。  
  
 当连接至预先存在的客户端或服务器时，您不能选择使用特定消息编码。 但是，可以通过多个终结点访问 WCF 服务，每个终结点具有不同的消息编码器。 当一个编码器不涵盖服务的全部用户时，请考虑在多个终结点上公开您的服务。 然后，客户端应用程序即可选择最适用的终结点。 使用多个终结点使你可以将不同消息编码器的优点与其他绑定元素结合起来。  
  
## <a name="system-provided-encoders"></a>系统提供的编码器  

 WCF 提供了多个系统提供的绑定，这些绑定旨在涵盖最常见的应用程序方案。 这些绑定中的每个绑定均由传输选项、消息编码器选项和其他选项（如安全性）组成。 本主题介绍如何扩展 WCF 中 `Text` 包含的、 `Binary` 和 `MTOM` 消息编码器，或创建你自己的自定义编码器。 文本消息编码器同时支持纯 XML 编码和 SOAP 编码。 文本消息编码器的纯 XML 编码模式称为 POX（“Plain Old XML”）编码器，以便与基于文本的 SOAP 编码进行区分。  
  
 有关由系统提供的绑定提供的绑定元素组合的详细信息，请参阅 [选择传输](../feature-details/choosing-a-transport.md)中的相应部分。  
  
## <a name="how-to-work-with-system-provided-encoders"></a>如何使用系统提供的编码器  

 使用派生自 <xref:System.ServiceModel.Channels.MessageEncodingBindingElement> 的类将编码添加到绑定中。  
  
 WCF 提供了以下类型的绑定元素，这些元素派生自 <xref:System.ServiceModel.Channels.MessageEncodingBindingElement> 可提供给文本、二进制和消息传输优化机制 (MTOM) 编码的类：  
  
- <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement>：互操作性最强，但效率最低的 XML 消息编码器。 Web 服务或 Web 服务客户端通常都能理解文本 XML。 但是，将大型二进制数据块作为文本传输不是有效的传输方式。  
  
- <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement>：表示指定基于二进制的 XML 消息所使用的字符编码和消息版本管理的绑定元素。 这是对编码选项最有效的，但最不具互操作性，因为它仅由 WCF 终结点支持。  
  
- <xref:System.ServiceModel.Channels.MtomMessageEncodingBindingElement>：表示指定使用消息传输优化机制 (MTOM) 编码的消息所使用的字符编码和消息版本管理的绑定元素。 MTOM 是一种用于在 WCF 消息中传输二进制数据的有效技术。 MTOM 编码器力图在效率和互操作性之间取得平衡。 MTOM 编码以文本形式传输大多数 XML，但是会通过按原样（即不转换为文本）的方式传输来优化大型二进制数据块。  
  
 绑定元素创建二进制、MTOM 或文本 <xref:System.ServiceModel.Channels.MessageEncoderFactory>。 工厂创建二进制、MTOM 或文本 <xref:System.ServiceModel.Channels.MessageEncoderFactory> 实例。 通常，只有一个实例。 但是如果使用会话，将为每个会话提供一个不同的编码器。 二进制编码器用此来调整动态字典（请参见“XML 基础结构”）。  
  
 <xref:System.ServiceModel.Channels.MessageEncoder.ReadMessage%2A> 和 <xref:System.ServiceModel.Channels.MessageEncoder.WriteMessage%2A> 方法是编码器的核心。 这些方法的作用是从流或 <xref:System.Byte> 数组中读取消息。 当在缓冲模式下进行传输时，将使用字节数组。 消息总是写入流中。 如果传输必须缓冲消息，它将提供一个执行缓冲的流。  
  
 其他成员则使用支持内容、媒体类型和 <xref:System.ServiceModel.Channels.MessageEncoder.MessageVersion%2A>。 传输调用这些编码器方法来测试传入的消息是否可以使用此方法解码，或者确定传出消息是否对此编码器有效。  
  
 这第三个编码器实现时每个都会添加与特定编码相关的属性，而且这些属性全都可以进行配置。 这些编码器还会公开具有安全默认值的读取器配额。 有关配额的讨论，请参见“XML 基础结构”。  
  
## <a name="features-of-system-provided-encoders"></a>系统提供的编码器的功能  

 系统提供的编码器具有多种功能。  
  
### <a name="pooling"></a>Pooling  

 每个编码器实现都会尝试生成尽可能多的池。 减少分配是提高托管代码性能的主要方式。 为了完成此生成池操作，实现将使用 `SynchronizedPool` 类。 C# 文件包含此类使用的其他优化的说明。  
  
 <xref:System.Xml.XmlDictionaryReader> 和 <xref:System.Xml.XmlDictionaryWriter> 实例均存入池中，并重新初始化以防止对每条消息添加新的分配。 对于读取器，`OnClose` 回调将在调用 `Close()` 时回收读取器。 编码器也会在构造消息时回收某些已使用的消息状态对象。 这些池的大小可以通过派生自 `MaxReadPoolSize` 的这三个类之一的 `MaxWritePoolSize` 和 <xref:System.ServiceModel.Channels.MessageEncodingBindingElement> 属性配置。  
  
### <a name="binary-encoding"></a>二进制编码  

 当二进制编码使用会话时，动态字典字符串必须与消息的接收方通信。 这可以通过在消息之前添加动态字典字符串实现。 接收方将剥离这些字符串，然后将它们添加到会话中，再处理消息。 要正确传递字典字符串就需要对传输进行缓冲。  
  
 这些字符串通过内部 `AddSessionInformationToMessage` 方法附加到消息中。 此方法将字符串作为 UTF-8 添加到以它们的长度为前缀的消息前面。 随后，整个字典标头都将以数据长度作为前缀。 反向操作可通过内部 `ExtractSessionInformationFromMessage` 方法执行。  
  
 除了处理动态字典关键字之外，缓冲的会话消息将采用独有的方式接收。 二进制编码器不是对文档创建一个编码器并处理该文档，而是使用内部 `MessagePatterns` 类解构二进制流。 其思路是，大多数消息都具有一组特定的标头，这些标头在由 WCF 生成时按特定顺序显示。 模式系统将基于它所期待的方式拆分消息。 如果成功了，它将初始化 <xref:System.ServiceModel.Channels.MessageHeaders> 对象而无需分析 XML。 如果不成功，将转而使用标准方法。  
  
### <a name="mtom-encoding"></a>MTOM 编码  

 <xref:System.ServiceModel.Channels.MtomMessageEncodingBindingElement> 类具有一个称为 <xref:System.ServiceModel.Channels.MtomMessageEncodingBindingElement.MaxBufferSize%2A> 的附加配置属性。 此属性用于设置在读取消息的过程中允许缓冲的数据量上限。 XML 信息集 (Infoset)，或其他 MIME 部分，可能需要进行缓冲以便将所有 MIME 部分集合到一条消息中。  
  
 为了可以正确使用 HTTP，内部 MTOM 消息编码器类为 `GetContentType`（内部）和 `WriteMessage` 提供了一些内部 API，这些 API 是公用的，可以重写。 必须进行更多的通信以确保 HTTP 标头中的值与 MIME 标头中的值一致。  
  
 在内部，MTOM 消息编码器使用 WCF 的文本读取器，类似于文本编码器。 主要区别在于它可优化大型二进制块，或“二进制大型对象”(BLOB)，方法是将它们嵌入消息字节中之前不将其转换为 Base-64 编码。 而是将这些 BLOB 保持提取状态，并以 MIME 附件的形式进行引用。  
  
## <a name="writing-your-own-encoder"></a>编写自己的编码器  

 若要实现自己的自定义消息编码器，您必须提供下列抽象基类的自定义实现：  
  
- <xref:System.ServiceModel.Channels.MessageEncoder>  
  
- <xref:System.ServiceModel.Channels.MessageEncoderFactory>  
  
- <xref:System.ServiceModel.Channels.MessageEncodingBindingElement>  
  
 将消息从内存中的表示形式转换为可以写入流中的表示形式封装在 <xref:System.ServiceModel.Channels.MessageEncoder> 类中，它可作为支持特定类型的 XML 编码的 XML 读取器和 XML 编写器的工厂。  
  
- 您必须重写的此类的主要方法包括：  
  
- <xref:System.ServiceModel.Channels.MessageEncoder.WriteMessage%2A>，此方法采用 <xref:System.ServiceModel.Channels.MessageEncodingBindingElement> 对象并将其写入 <xref:System.IO.Stream> 对象。  
  
- <xref:System.ServiceModel.Channels.MessageEncoder.ReadMessage%2A>，此方法采用 <xref:System.IO.Stream> 对象以及最大标头大小，并返回一个 <xref:System.ServiceModel.Channels.Message> 对象。  
  
 你写入这些方法中的代码处理标准传输协议和你自定义的编码之间的转换。  
  
 接下来，你需要编写一条创建自定义编码器的工厂类代码。 重写 <xref:System.ServiceModel.Channels.MessageEncoderFactory.Encoder%2A> 以返回自定义 <xref:System.ServiceModel.Channels.MessageEncoder> 的实例。  
  
 然后通过重写 <xref:System.ServiceModel.Channels.MessageEncoderFactory> 方法将自定义的 <xref:System.ServiceModel.Channels.MessageEncodingBindingElement.CreateMessageEncoderFactory%2A> 连接至用于配置服务或客户端的绑定元素堆栈，以便返回此工厂的实例。  
  
 WCF 提供了两个示例，其中的示例代码演示了此过程： [自定义消息编码器：自定义文本编码器](../samples/custom-message-encoder-custom-text-encoder.md) 和 [自定义消息编码器：压缩编码器](../samples/custom-message-encoder-compression-encoder.md)。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Channels.MessageEncodingBindingElement>
- <xref:System.ServiceModel.Channels.MessageEncoderFactory>
- <xref:System.ServiceModel.Channels.MessageEncoder>
- [数据传输体系结构概述](../feature-details/data-transfer-architectural-overview.md)
- [选择消息编码器](../feature-details/choosing-a-message-encoder.md)
- [选择传输方式](../feature-details/choosing-a-transport.md)
