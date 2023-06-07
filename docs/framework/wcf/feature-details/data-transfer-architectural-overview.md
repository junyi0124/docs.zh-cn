---
title: 数据传输体系结构概述
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data transfer [WCF], architectural overview
ms.assetid: 343c2ca2-af53-4936-a28c-c186b3524ee9
ms.openlocfilehash: efb6933ba90975d5ba35deb2bf22af12fc7c2cdc
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96291682"
---
# <a name="data-transfer-architectural-overview"></a>数据传输体系结构概述

Windows Communication Foundation (WCF) 可视为消息传递基础结构。 它可以接收消息，处理消息，根据用户代码调度消息以便进一步操作，或者从用户代码给定的数据构造消息并将消息发送到目标。 本主题旨在向高级开发人员说明用于处理消息和所包含数据的体系结构。 有关如何发送和接收数据的面向任务的更简单介绍，请参阅 [Specifying Data Transfer in Service Contracts](specifying-data-transfer-in-service-contracts.md)。  
  
> [!NOTE]
> 本主题讨论 wcf 实现的详细信息，这些详细信息在检查 WCF 对象模型时不可见。 对于有记载的实现详细信息，请记住两点。 首先，说明非常简单；由于优化或其他原因，实际实现可能更复杂。 其次，切勿依赖于特定的实现详细信息（即使是已记载的详细信息），因为这些信息在版本之间，甚至在服务发行过程中有可能在不另行通知的情况下发生更改。  
  
## <a name="basic-architecture"></a>基本体系结构  

 WCF 消息处理功能的核心是 <xref:System.ServiceModel.Channels.Message> 类，该类在 [使用 message 类](using-the-message-class.md)中进行了详细说明。 WCF 的运行时组件可分为两个主要部分：通道堆栈和服务框架， <xref:System.ServiceModel.Channels.Message> 该类是连接点。  
  
 通道堆栈负责在有效的 <xref:System.ServiceModel.Channels.Message> 实例和对应于发送或接收消息数据的某种操作之间进行转换。 在发送端，通道堆栈采用有效的 <xref:System.ServiceModel.Channels.Message> 实例，经过一些处理后，执行逻辑上对应于发送消息的某种操作。 该操作可能是发送 TCP 或 HTTP 数据包、让消息在消息队列中排队、将消息写入数据库、将消息保存到文件共享或其他任何操作，具体取决于实现。 最常见的操作是通过网络协议发送消息。 在接收端发生相反的情况：检测操作（可能是 TCP 或 HTTP 数据包接收操作或任何其他操作），处理后，通道堆栈将此操作转换为有效的 <xref:System.ServiceModel.Channels.Message> 实例。  
  
 您可以 <xref:System.ServiceModel.Channels.Message> 直接使用类和通道堆栈来使用 WCF。 但是，这样做很麻烦且很费时。 此外， <xref:System.ServiceModel.Channels.Message> 对象不提供元数据支持，因此如果您以这种方式使用 wcf，则不能生成强类型的 wcf 客户端。  
  
 因此，WCF 包含提供易于使用的编程模型的服务框架，您可以使用该模型来构造和接收 `Message` 对象。 服务框架通过服务协定概念将服务映射到 .NET Framework 类型，并将消息调度给用户操作，这些操作仅 .NET Framework 使用特性标记的方法 <xref:System.ServiceModel.OperationContractAttribute> (有关更多详细信息，请参阅 [设计服务协定](../designing-service-contracts.md)) 。 这些方法可能具有参数和返回值。 在服务端，服务框架将传入的 <xref:System.ServiceModel.Channels.Message> 实例转换为参数，并将返回值转换为传出的 <xref:System.ServiceModel.Channels.Message> 实例。 在客户端，执行相反的操作。 例如，考虑下面的 `FindAirfare` 操作。  
  
 [!code-csharp[c_DataArchitecture#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_dataarchitecture/cs/source.cs#1)]
 [!code-vb[c_DataArchitecture#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_dataarchitecture/vb/source.vb#1)]  
  
 假设在客户端上调用了 `FindAirfare` 。 客户端上的服务框架会将 `FromCity` 和 `ToCity` 参数转换为传出的 <xref:System.ServiceModel.Channels.Message> 实例并将该实例传递到通道堆栈以便发送。  
  
 在服务端，来自通道堆栈的 <xref:System.ServiceModel.Channels.Message> 实例到达时，服务框架会从消息中提取相关数据来填充 `FromCity` 和 `ToCity` 参数，然后调用服务端 `FindAirfare` 方法。 该方法返回时，服务框架会采用返回的整数值和 `IsDirectFlight` 输出参数，并创建一个包含此信息的 <xref:System.ServiceModel.Channels.Message> 对象实例。 然后，服务框架会将 `Message` 实例传递到通道堆栈以便发回到客户端。  
  
 在客户端，包含响应消息的 <xref:System.ServiceModel.Channels.Message> 实例会从通道堆栈中显现出来。 服务框架会提取返回值和 `IsDirectFlight` 值并将这些值返回到客户端的调用方。  
  
## <a name="message-class"></a>Message 类  

 <xref:System.ServiceModel.Channels.Message> 类用作消息的抽象表示形式，但其设计在很大程度上依赖于 SOAP 消息。 <xref:System.ServiceModel.Channels.Message> 包含三个主要信息部分：消息正文、消息头和消息属性。  
  
## <a name="message-body"></a>消息正文  

 消息正文用于表示消息的实际数据负载。 消息正文始终表示为 XML Infoset。 这并不意味着 WCF 中创建或接收的所有消息都必须为 XML 格式。 这要由通道堆栈来确定如何解释消息正文。 通道堆栈可能会将消息正文作为 XML 发出、将转换为某种其他格式，甚至可能会完全忽略该消息正文。 当然，大多数绑定 WCF 提供时，消息正文在 SOAP 信封的正文部分中都表示为 XML 内容。  
  
 必须认识到的是， `Message` 类并不一定对表示正文的 XML 数据包含缓冲区。 从逻辑上说， `Message` 包含 XML Infoset，但可以动态构造此 Infoset，因此它可能永远也不会实际存在于内存中。  
  
### <a name="putting-data-into-the-message-body"></a>将数据放入消息正文  

 将数据放入消息正文并没有统一的机制。 <xref:System.ServiceModel.Channels.Message> 类有一个抽象方法 <xref:System.ServiceModel.Channels.Message.OnWriteBodyContents%28System.Xml.XmlDictionaryWriter%29>，该方法采用 <xref:System.Xml.XmlDictionaryWriter>。 <xref:System.ServiceModel.Channels.Message> 类的每个子类负责重写此方法并写出其自己的内容。 消息正文逻辑上包含 `OnWriteBodyContent` 生成的 XML Infoset。 例如，考虑下面的 `Message` 子类。  
  
 [!code-csharp[c_DataArchitecture#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_dataarchitecture/cs/source.cs#2)]
 [!code-vb[c_DataArchitecture#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_dataarchitecture/vb/source.vb#2)]  
  
 实际上，一个 `AirfareRequestMessage` 实例仅包含两个字符串（“fromCity”和“toCity”）。 但在逻辑上，消息中包含的是下面的 XML Infoset：  
  
```xml  
<airfareRequest>  
    <from>Tokyo</from>  
    <to>London</to>  
</airfareRequest>  
```  
  
 当然，您通常不会用这种方式创建消息，因为您可以使用服务框架从操作协定参数创建诸如前面的消息。 另外， <xref:System.ServiceModel.Channels.Message> 类具有静态 `CreateMessage` 方法，您可以使用此方法创建具有常见内容类型的消息：空消息、包含用 <xref:System.Runtime.Serialization.DataContractSerializer>序列化为 XML 的对象的消息、包含 SOAP 错误的消息、包含由 <xref:System.Xml.XmlReader>表示的 XML 的消息等。  
  
### <a name="getting-data-from-a-message-body"></a>从消息正文获取数据  

 可以采用两种主要方式提取消息正文中存储的数据：  
  
- 通过调用 <xref:System.ServiceModel.Channels.Message.WriteBodyContents%28System.Xml.XmlDictionaryWriter%29> 方法并传入一个 XML 编写器，可以一次获取整个消息正文。 完整的消息正文会写出到此编写器中。 一次获取整个消息正文也称为“写入消息” 。 编写主要由通道堆栈在发送消息时完成：通道堆栈的某些部分通常会访问整个消息正文、进行编码，然后发送整个消息正文。  
  
- 从消息正文获取信息的另一种方式是调用 <xref:System.ServiceModel.Channels.Message.GetReaderAtBodyContents> 并获取一个 XML 读取器。 之后可以通过在读取器上调用方法，根据需要按顺序访问消息正文。 逐段获取消息正文也称为“读取消息” 。 读取消息主要由服务框架在接收消息时使用。 例如，使用 <xref:System.Runtime.Serialization.DataContractSerializer> 时，服务框架将使用 XML 读取器获取消息正文并将其传递给反序列化引擎，之后，反序列化引擎将开始逐元素地读取消息并构造相应对象图。  
  
 消息正文只能检索一次。 这为使用只进流提供了可能。 例如，您可以写入一个从 <xref:System.ServiceModel.Channels.Message.OnWriteBodyContents%28System.Xml.XmlDictionaryWriter%29> 读取数据的 <xref:System.IO.FileStream> 重写，并以 XML Infoset 形式返回结果。 永远不需要 "后退" 到文件的开头。  
  
 `WriteBodyContents` 和 `GetReaderAtBodyContents` 方法只检查以前是否从未检索过该消息正文，然后分别调用 `OnWriteBodyContents` 或 `OnGetReaderAtBodyContents`。  
  
## <a name="message-usage-in-wcf"></a>WCF 中的消息用法  

 大多数消息都可以分类为传出消息  （由服务框架创建并由通道堆栈发送的消息）或传入消息  （来自通道堆栈并由服务框架解释的消息）。 而且，通道堆栈可以以缓冲模式或流模式操作。 服务框架还可以公开流处理或非流处理的编程模型。 下表列出了可能出现的各种情况，并提供了有关其实现的简单说明。  
  
|消息类型|消息中的正文数据|写入 (OnWriteBodyContents) 实现|读取 (OnGetReaderAtBodyContents) 实现|  
|------------------|--------------------------|--------------------------------------------------|-------------------------------------------------------|  
|传出，从非流处理编程模型创建|写入消息所需的数据（例如，序列化消息所需的对象和 <xref:System.Runtime.Serialization.DataContractSerializer> 实例）*|用于基于存储的数据写出消息的自定义逻辑（例如，在使用的序列化程序 `WriteObject` 上调用 `DataContractSerializer` ）*|调用 `OnWriteBodyContents`，缓冲结果，通过缓冲区返回 XML 读取器|  
|传出，从流处理编程模型创建|具有要写入数据的 `Stream` *|使用 <xref:System.Xml.IStreamProvider> 机制从存储的流中写出数据*|调用 `OnWriteBodyContents`，缓冲结果，通过缓冲区返回 XML 读取器|  
|从流通道堆栈传入|一个 `Stream` 对象，表示通过网络传入的、具有 <xref:System.Xml.XmlReader> 的数据|使用 `XmlReader` 从存储的 `WriteNode`中写出内容|返回存储的 `XmlReader`|  
|从非流处理通道堆栈传入|一个缓冲区，其中包含具有 `XmlReader` 的正文数据|使用 `XmlReader` 从存储的 `WriteNode`中写出内容|返回存储的 lang|  
  
 \* 这些项不是直接在子类中实现的 `Message` ，而是在类的子类中实现的 <xref:System.ServiceModel.Channels.BodyWriter> 。 有关 <xref:System.ServiceModel.Channels.BodyWriter>的详细信息，请参阅 [Using the Message Class](using-the-message-class.md)。  
  
## <a name="message-headers"></a>消息头  

 消息可以包含标头。 标头在逻辑上由与名称、命名空间和几个其他属性相关联的 XML Infoset 组成。 在 `Headers` 上使用 <xref:System.ServiceModel.Channels.Message>属性可以访问消息头。 每个标头由一个 <xref:System.ServiceModel.Channels.MessageHeader> 类表示。 消息头通常在使用配置的通道堆栈处理 SOAP 消息时映射到 SOAP 消息头。  
  
 将信息放入消息头和从消息头中提取信息类似于使用消息正文。 由于不支持流处理，因此过程有些简化。 可以多次访问同一个标头的内容，并且可以按任意顺序访问标头，强制标头始终进行缓冲。 没有可用于通过标头获取 XML 读取器的通用机制，但是 WCF 内部有一个子类， `MessageHeader` 它表示具有此类功能的可读标头。 这种类型的 `MessageHeader` 是在传入具有自定义应用程序标头的消息时由通道堆栈创建的。 它使服务框架能够使用反序列化引擎（如 <xref:System.Runtime.Serialization.DataContractSerializer>）来解释这些标头。  
  
 有关详细信息，请参阅 [使用 Message 类](using-the-message-class.md)。  
  
## <a name="message-properties"></a>消息属性  

 消息可以包含属性。 *属性* 是任何与字符串名称关联的 .NET Framework 对象。 通过 `Properties` 上的 `Message`属性可以访问这些属性。  
  
 与消息正文和消息头不同（通常分别映射到 SOAP 正文和 SOAP 标头），消息属性通常不与消息一起发送或接收。 消息属性主要作为一种通信机制，用于在通道堆栈中的各个通道之间以及通道堆栈和服务模块之间传递有关消息的数据。  
  
 例如，WCF 中包含的 HTTP 传输通道可以生成各种 HTTP 状态代码，如 "404 (找不到) " 和 "500 (内部服务器错误) "，然后将回复发送到客户端。 在发送回复消息之前，它会检查以确定是否 `Properties` `Message` 包含一个名为 "httpresponse.cache" 的属性，该属性包含类型的对象 <xref:System.ServiceModel.Channels.HttpResponseMessageProperty> 。 如果找到此属性，HTTP 传输通道将查看 <xref:System.ServiceModel.Channels.HttpResponseMessageProperty.StatusCode%2A> 属性并使用该状态代码。 如果未找到此属性，则使用默认的“200（确定）”代码。  
  
 有关详细信息，请参阅 [使用 Message 类](using-the-message-class.md)。  
  
### <a name="the-message-as-a-whole"></a>整体消息  

 到目前为止，我们已经讨论了用于单独访问消息各个部分的方法。 不过， <xref:System.ServiceModel.Channels.Message> 类还提供了以整体形式处理整个消息的方法。 例如， `WriteMessage` 方法可将整个消息写出到 XML 编写器。  
  
 为此，必须在整个 `Message` 实例和 XML Infoset 之间定义一个映射。 事实上，这种映射是存在的： WCF 使用 SOAP 标准定义此映射。 在将 `Message` 实例作为 XML Infoset 写出时，生成的 Infoset 是包含该消息的有效 SOAP 信封。 因此， `WriteMessage` 通常会执行以下步骤：  
  
1. 写入 SOAP 信封元素开始标记。  
  
2. 写入 SOAP 标头元素开始标记，写出所有标头并关闭标头元素。  
  
3. 写入 SOAP 正文元素开始标记。  
  
4. 调用 `WriteBodyContents` 或等效方法写出正文。  
  
5. 关闭正文和信封元素。  
  
 前面的步骤与 SOAP 标准密切相关。 实际上，由于存在多个版本的 SOAP，因此情况会更复杂，例如，不知道使用的 SOAP 版本将不可能正确写出 SOAP 信封元素。 而且，有时可能需要完全关闭这种特定于 SOAP 的复杂映射。  
  
 为此， `Version` 上提供了 `Message`属性。 该属性可以设置为写出消息时要使用的 SOAP 版本，也可以设置为 `None` 以阻止任何特定于 SOAP 的映射。 如果将 `Version` 属性设置为 `None`，则处理整个消息的方法会将消息视为仅由正文构成，例如， `WriteMessage` 将只调用 `WriteBodyContents` 而不执行上面列出的多个步骤。 对于传入消息，需要自动检测并正确设置 `Version` 。  
  
## <a name="the-channel-stack"></a>通道堆栈  
  
### <a name="channels"></a>信道  

 如前所述，通道堆栈负责将传出的 <xref:System.ServiceModel.Channels.Message> 实例转换为某种操作（如通过网络发送数据包），或者将某种操作（如接收网络数据包）转换为传入的 `Message` 实例。  
  
 通道堆栈由一个或多个按顺序排序的通道组成。 传出的 `Message` 实例传递到堆栈中的第一个通道（也称为“最顶端通道” ），该通道将该实例向下传递到堆栈中的下一个通道，依此类推。 消息在最后一个通道中终止，此通道称为“传输通道” 。 传入消息源自传输通道，在堆栈中从一个通道向上传递到另一个通道。 消息通常从最顶端通道传入服务框架。 这是应用程序消息的通常模式，但某些通道的工作方式可能略有不同，例如这些通道可能会发送自己的基础结构消息，而不从其上面的通道传入消息。  
  
 消息在堆栈中传递时，通道可能会对消息执行各种操作。 最常见的操作是给传出消息添加标头和读取传入消息的标头。 例如，通道可能会计算消息的数字签名并将此签名添加为标头。 通道也可能检查传入消息的数字签名标头，并阻止没有有效签名的消息在通道堆栈中向上传递。 通道通常还设置或检查消息属性。 通常不会修改消息正文，但这是允许的，例如，WCF 安全通道可以加密消息正文。  
  
### <a name="transport-channels-and-message-encoders"></a>传输通道和消息编码器  

 堆栈中的最底端通道负责实际将传出的 <xref:System.ServiceModel.Channels.Message>（由其他通道修改后）转换为某种操作。 在接收端，这是将某种操作转换为由其他通道进行处理的 `Message` 的通道。  
  
 如前所述，这些操作可能有所不同，例如：通过各种协议发送或接收网络数据包、在数据库中读取或写入消息、在“消息队列”队列中对消息进行排队或取消排队，等等。 所有这些操作都有一个共同点：它们要求在 WCF `Message` 实例与可以发送、接收、读取、写入、排队或取消排队的实际字节组之间进行转换。 将 `Message` 转换为字节组的过程称为“编码” ，从字节组创建 `Message` 的逆过程称为“解码” 。  
  
 大多数传输通道都使用称为“消息编码器”  的组件来完成编码和解码工作。 消息编码器是 <xref:System.ServiceModel.Channels.MessageEncoder> 类的子类。 `MessageEncoder` 包括各种 `ReadMessage` 和 `WriteMessage` 方法重载，可在 `Message` 和字节组之间转换。  
  
 在发送端，缓冲传输通道将从上面通道中接收的 `Message` 对象传递到 `WriteMessage`。 该通道返回字节数组，然后使用此数组执行其操作（如将这些字节包装为有效的 TCP 数据包并将这些数据包发送到正确目标）。 流传输通道首先创建一个 `Stream` （例如通过传出 TCP 连接），然后传递需要的 `Stream` 和 `Message` 以便发送相应的 `WriteMessage` 重载，从而写出消息。  
  
 在接收端，缓冲传输通道将传入字节（例如传入的 TCP 数据包中的字节）提取到数组中并调用 `ReadMessage` 以获取可在通道堆栈中继续向上传递的 `Message` 对象。 流传输通道创建 `Stream` 对象（例如通过传入 TCP 连接传输的网络流）并将该对象传递到 `ReadMessage` 以返回 `Message` 对象。  
  
 传输通道和消息编码器之间并不强制分离；可以写入不使用消息编码器的传输通道。 但这种分离具有易于撰写的优势。 只要传输通道仅使用基本 <xref:System.ServiceModel.Channels.MessageEncoder> ，它就能与任何 WCF 或第三方消息编码器一起使用。 同样，这个编码器通常可用于任何传输通道。  
  
### <a name="message-encoder-operation"></a>消息编码器操作  

 为了说明编码器的典型操作，有必要考虑以下四种情况。  
  
|操作|注释|  
|---------------|-------------|  
|编码，缓冲|在缓冲模式中，编码器通常创建可变大小的缓冲区，然后在其上创建 XML 编写器。 然后，编码器在编码的消息上调用 <xref:System.ServiceModel.Channels.Message.WriteMessage%28System.Xml.XmlWriter%29> ，使用 <xref:System.ServiceModel.Channels.Message.WriteBodyContents%28System.Xml.XmlDictionaryWriter%29>依次写出标头和正文，如本主题前面有关 `Message` 的一节所述。 然后为要使用的传输通道返回缓冲区的内容（表示为字节数组）。|  
|编码，流处理|在流处理模式中，操作与上面类似，但更简单。 不需要使用缓冲区。 通常在流上创建 XML 编写器并在 <xref:System.ServiceModel.Channels.Message.WriteMessage%28System.Xml.XmlWriter%29> 上调用 `Message` 以将消息写出到此编写器。|  
|解码，缓冲|在缓冲模式中解码时，通常创建包含缓冲数据的特殊的 `Message` 子类。 读取消息头，在消息正文上创建 XML 读取器。 这将是用 <xref:System.ServiceModel.Channels.Message.GetReaderAtBodyContents>返回的读取器。|  
|解码，流处理|在流处理模式中解码时，通常创建特殊的 Message 子类。 流的前进速度刚好足以读取所有标头并将标头定位在消息正文上。 然后在流上创建 XML 读取器。 这将是用 <xref:System.ServiceModel.Channels.Message.GetReaderAtBodyContents>返回的读取器。|  
  
 编码器也可以执行其他功能。 例如，编码器可以共享 XML 读取器和编写器。 每次需要新 XML 读取器或编写器时都进行创建很浪费资源。 因此，编码器通常保持一个可配置大小的读取器池和编写器池。 在前面所述的编码器操作说明中，每当使用短语 "创建 XML 读取器/编写器" 时，它通常表示 "从池中获取一个，如果没有，则创建一个"。 编码器（及其在解码时创建的 `Message` 子类）中包含用于在不需要读取器和编写器时（例如关闭 `Message` 时）将其返回到池的逻辑。  
  
 WCF 提供了三种消息编码器，但也可以创建其他自定义类型。 提供的类型为文本、二进制和消息传输优化机制 (MTOM)。 这些编码器将在 [Choosing a Message Encoder](choosing-a-message-encoder.md)中详细说明。  
  
### <a name="the-istreamprovider-interface"></a>IStreamProvider 接口  

 在将包含经过流处理的正文的传出消息写入 XML 编写器时， <xref:System.ServiceModel.Channels.Message> 会在其 <xref:System.ServiceModel.Channels.Message.OnWriteBodyContents%28System.Xml.XmlDictionaryWriter%29> 实现中使用类似于下面的一系列调用：  
  
- 写入流之前的所有必要信息（例如 XML 开始标记）。  
  
- 写入流。  
  
- 写入流之后的任何信息（例如 XML 结束标记）。  
  
 这对于类似于文本 XML 编码的编码可以正常工作。 但是，有些编码不将 XML Infoset 信息（例如，开始和结束 XML 元素的标记）和元素中包含的数据放在一起。 例如，在 MTOM 编码中，消息拆分为多个部分。 一部分包含 XML Infoset，其中可能包含对实际元素内容的其他部分的引用。 由于 XML Infoset 通常比流处理的内容小，因此有必要缓冲 Infoset，将其写出，然后以流处理方式写入内容。 这意味着在写入结束元素标记时，应当尚未写出流。  
  
 为此，应使用 <xref:System.Xml.IStreamProvider> 接口。 该接口具有一个可返回要写入的流的 <xref:System.Xml.IStreamProvider.GetStream> 方法。 写出 <xref:System.ServiceModel.Channels.Message.OnWriteBodyContents%28System.Xml.XmlDictionaryWriter%29> 中经过流处理的消息正文的正确方式如下：  
  
1. 写入流之前的所有必要信息（例如 XML 开始标记）。  
  
2. 对采用 `WriteValue` 、具有 <xref:System.Xml.XmlDictionaryWriter> 实现（该实现可返回要写入的流）的 <xref:System.Xml.IStreamProvider>调用 `IStreamProvider` 重载。  
  
3. 写入流之后的任何信息（例如 XML 结束标记）。  
  
 使用此方法时，XML 编写器可以选择何时调用 <xref:System.Xml.IStreamProvider.GetStream> 和写出经过流处理的数据。 例如，文本和二进制 XML 编写器将立即调用此方法并写出开始标记和结束标记之间的经过流处理的内容。 MTOM 编写器准备写入消息的相应部分时，它可以决定以后调用 <xref:System.Xml.IStreamProvider.GetStream> 。  
  
## <a name="representing-data-in-the-service-framework"></a>在服务框架中表示数据  

 如本主题的 "基本体系结构" 一节中所述，服务框架是 WCF 的一部分，它还负责在用户友好的消息数据编程模型和实际实例之间进行转换 `Message` 。 通常，消息交换在服务框架中表示为用特性标记的 .NET Framework 方法 <xref:System.ServiceModel.OperationContractAttribute> 。 此方法可以接受参数并可以返回一个返回值和/或 out 参数。 在服务端，输入参数表示传入消息，返回值和 out 参数表示传出消息。 在客户端，情况正好相反。 使用参数和返回值来说明消息的编程模型将在 [Specifying Data Transfer in Service Contracts](specifying-data-transfer-in-service-contracts.md)中详细说明。 不过，本节将提供简要概述。  
  
## <a name="programming-models"></a>编程模型  

 WCF 服务框架支持五种不同的编程模型，用于描述消息：  
  
### <a name="1-the-empty-message"></a>1.空消息  

 这是最简单的情况。 若要说明空的传入消息，请不要使用任何输入参数。  
  
 [!code-csharp[C_DataArchitecture#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_dataarchitecture/cs/source.cs#3)]
 [!code-vb[C_DataArchitecture#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_dataarchitecture/vb/source.vb#3)]  
  
 若要说明空的传出消息，请使用一个空的返回值且不使用任何 out 参数：  
  
 [!code-csharp[C_DataArchitecture#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_dataarchitecture/cs/source.cs#4)]
 [!code-vb[C_DataArchitecture#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_dataarchitecture/vb/source.vb#4)]  
  
 请注意，这不同于单向操作协定：  
  
 [!code-csharp[C_DataArchitecture#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_dataarchitecture/cs/source.cs#5)]
 [!code-vb[C_DataArchitecture#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_dataarchitecture/vb/source.vb#5)]  
  
 在 `SetDesiredTemperature` 示例中，说明了双向消息交换模式。 从操作中返回了消息，但该消息为空。 可以从操作中返回错误。 在“Set Lightbulb”示例中，消息交换模式为单向，因此没有要说明的传出消息。 在此例中，服务无法将任何状态传回客户端。  
  
### <a name="2-using-the-message-class-directly"></a>2.直接使用 Message 类  

 在操作协定中可以直接使用 <xref:System.ServiceModel.Channels.Message> 类（或它的一个子类）。 在这种情况下，服务框架只将 `Message` 从操作传递到通道堆栈（或从通道堆栈传递到操作），不进行其他处理。  
  
 直接使用 `Message` 有两种主要使用场合。 如果任何其他编程模型都不能灵活地说明消息，则可以对高级方案使用此模型。 例如，您可能想使用磁盘上的文件来说明消息，用文件的属性作为消息头，用文件的内容作为消息正文。 您可以创建类似于下面的内容。  
  
 [!code-csharp[C_DataArchitecture#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_dataarchitecture/cs/source.cs#6)]
 [!code-vb[C_DataArchitecture#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_dataarchitecture/vb/source.vb#6)]  
  
 操作协定中 `Message` 的第二种常见用途是，服务并不关心特定的消息内容，而是像对待黑盒子一样对待消息。 例如，您可能具有将消息转发给多个其他收件人的服务。 可以按以下方式编写协定。  
  
 [!code-csharp[C_DataArchitecture#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_dataarchitecture/cs/source.cs#7)]
 [!code-vb[C_DataArchitecture#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_dataarchitecture/vb/source.vb#7)]  
  
 Action = "*" 行有效地关闭消息调度并确保发送到协定的所有消息都 `IForwardingService` 可以进行 `ForwardMessage` 操作。  (通常情况下，调度程序会检查消息的 "Action" 标头以确定其用途。 Action = " \* " 表示 "操作标头的所有可能值"。 ) 操作 = "" 的组合 \* ，并使用 Message 作为参数，因为它能够接收所有可能的消息。 若要能够发送所有可能的消息，请使用 Message 作为返回值，并将设置 `ReplyAction` 为 " \* "。 这将阻止服务框架添加其自己的 Action 标头，使您能够使用返回的 `Message` 对象控制此标头。  
  
### <a name="3-message-contracts"></a>3.消息协定  

 WCF 提供了一种声明性编程模型，用于描述消息（称为 *消息协定*）。 此模型将在 [Using Message Contracts](using-message-contracts.md)中详细说明。 实质上，整个消息由单个 .NET Framework 类型表示，该类型使用和等特性 <xref:System.ServiceModel.MessageBodyMemberAttribute> <xref:System.ServiceModel.MessageHeaderAttribute> 来说明消息协定类的哪些部分应映射到消息的哪个部分。  
  
 消息协定对生成的 `Message` 实例提供广泛的控制（虽然明显没有直接使用 `Message` 类所提供的控制那样广泛）。 例如，消息正文通常由多段信息组成，每段消息由其各自的 XML 元素表示。 这些元素可以直接出现在正文中（空 模式），也可以包装  在包含 XML 元素中。 使用消息协定编程模型时可以进行空与包装决策并控制包装名称和命名空间的名称。  
  
 下面的消息协定代码示例演示了这些功能。  
  
 [!code-csharp[C_DataArchitecture#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_dataarchitecture/cs/source.cs#9)]
 [!code-vb[C_DataArchitecture#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_dataarchitecture/vb/source.vb#9)]  
  
 标记为要进行序列化（使用 <xref:System.ServiceModel.MessageBodyMemberAttribute>、 <xref:System.ServiceModel.MessageHeaderAttribute>或其他相关特性）的项必须为可序列化的，以参与消息协定。 有关详细信息，请参阅本主题后面的 "序列化" 一节。  
  
### <a name="4-parameters"></a>4.参数  

 通常，想要说明对多段数据执行的操作的开发人员并不需要消息协定所提供的那样高的控制度。 例如，在创建新服务时，开发人员通常不需要进行空与包装决策和确定包装元素名称。 进行这些决策通常需要了解有关 Web 服务和 SOAP 的深入知识。  
  
 WCF 服务框架可以自动挑选最佳和最具互操作性的 SOAP 表示形式来发送或接收多个相关信息段，而不会在用户上强制执行这些选择。 只要将这些信息段作为参数或操作协定的返回值进行说明就能实现此目的。 例如，请考虑下面的操作协定。  
  
 [!code-csharp[C_DataArchitecture#11](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_dataarchitecture/cs/source.cs#11)]
 [!code-vb[C_DataArchitecture#11](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_dataarchitecture/vb/source.vb#11)]  
  
 服务框架将自动决定将所有三段信息（`customerID`、 `item`和 `quantity`）放入消息正文并将它们包装在名为 `SubmitOrderRequest`的包装元素中。  
  
 推荐的方法是将要发送或接收的信息作为操作协定参数的简单列表进行说明，除非存在特殊原因需要移动到更复杂的消息协定或基于 `Message`的编程模型。  
  
### <a name="5-stream"></a>5.流  

 在操作协定中使用 `Stream` 或它的一个子类或者将其作为消息协定中唯一的消息正文部分，这种方式可视为与上述模型不同的一种单独的编程模型。 以这种方式使用 `Stream` 是除了编写您自己的流兼容 `Message` 子类以外，保证协定能够以流处理方式使用的唯一途径。 有关详细信息，请参阅 [大数据和流式处理](large-data-and-streaming.md)。  
  
 以这种方式使用 `Stream` 或它的一个子类时，不会调用序列化程序。 对于传出消息，会创建一个特殊流 `Message` 子类并写出流，如关于 <xref:System.Xml.IStreamProvider> 接口的一节中所述。 对于传入消息，服务框架会在传入消息上创建一个 `Stream` 子类并将该子类提供给操作。  
  
## <a name="programming-model-restrictions"></a>编程模型限制  

 上述编程模型不能任意组合。 例如，如果某个操作接受某种消息协定类型，则消息协定必须是其唯一的输入参数。 而且，操作必须返回空消息（void 返回类型）或另一个消息协定。 这些编程模型限制将在每个特定编程模型的主题： [Using Message Contracts](using-message-contracts.md)、 [Using the Message Class](using-the-message-class.md)和 [Large Data and Streaming](large-data-and-streaming.md)中进行说明。  
  
## <a name="message-formatters"></a>消息格式化程序  

 通过将称为“消息格式化程序”  的组件插入到服务框架中可以为上述编程模型提供支持。 消息格式化程序是实现 <xref:System.ServiceModel.Dispatcher.IClientMessageFormatter> 和 <xref:System.ServiceModel.Dispatcher.IDispatchMessageFormatter> /或接口的类型，分别用于客户端和服务 WCF 客户端。  
  
 消息格式化程序通常由行为插入。 例如， <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior> 可插入数据协定消息格式化程序。 通过在服务端在 <xref:System.ServiceModel.Dispatcher.DispatchOperation.Formatter%2A> 方法中将 <xref:System.ServiceModel.Description.IOperationBehavior.ApplyDispatchBehavior%28System.ServiceModel.Description.OperationDescription%2CSystem.ServiceModel.Dispatcher.DispatchOperation%29> 设置为正确的格式化程序，或者在客户端在 <xref:System.ServiceModel.Dispatcher.ClientOperation.Formatter%2A> 方法中将 <xref:System.ServiceModel.Description.IOperationBehavior.ApplyClientBehavior%28System.ServiceModel.Description.OperationDescription%2CSystem.ServiceModel.Dispatcher.ClientOperation%29> 设置为正确的格式化程序，可以完成此操作。  
  
 下表列出了消息格式化程序可能实现的方法。  
  
|接口|方法|操作|  
|---------------|------------|------------|  
|<xref:System.ServiceModel.Dispatcher.IDispatchMessageFormatter>|<xref:System.ServiceModel.Dispatcher.IDispatchMessageFormatter.DeserializeRequest%28System.ServiceModel.Channels.Message%2CSystem.Object%5B%5D%29>|将传入的 `Message` 转换为操作参数|  
|<xref:System.ServiceModel.Dispatcher.IDispatchMessageFormatter>|<xref:System.ServiceModel.Dispatcher.IDispatchMessageFormatter.SerializeReply%28System.ServiceModel.Channels.MessageVersion%2CSystem.Object%5B%5D%2CSystem.Object%29>|从操作返回值/out 参数创建传出的 `Message`|  
|<xref:System.ServiceModel.Dispatcher.IClientMessageFormatter>|<xref:System.ServiceModel.Dispatcher.IClientMessageFormatter.SerializeRequest%28System.ServiceModel.Channels.MessageVersion%2CSystem.Object%5B%5D%29>|从操作参数创建传出的 `Message`|  
|<xref:System.ServiceModel.Dispatcher.IClientMessageFormatter>|<xref:System.ServiceModel.Dispatcher.IClientMessageFormatter.DeserializeReply%28System.ServiceModel.Channels.Message%2CSystem.Object%5B%5D%29>|将传入的 `Message` 转换为返回值/out 参数|  
  
## <a name="serialization"></a>序列化  

 每当使用消息协定或参数描述消息内容时，必须使用序列化在 .NET Framework 类型和 XML 信息集表示形式之间进行转换。 例如，WCF 中的其他位置使用序列化， <xref:System.ServiceModel.Channels.Message> 它具有一个泛型 <xref:System.ServiceModel.Channels.Message.GetBody%2A> 方法，可用于读取反序列化为对象的整个消息正文。  
  
 WCF 支持 "现成" 的两种序列化技术，用于序列化和反序列化参数和消息部分： <xref:System.Runtime.Serialization.DataContractSerializer> 和 `XmlSerializer` 。 另外，您也可以编写自定义序列化程序。 不过，WCF (的其他部分（如泛型 `GetBody` 方法或 SOAP 错误序列化) ）可能会被限制为仅使用 <xref:System.Runtime.Serialization.XmlObjectSerializer> 子类 (<xref:System.Runtime.Serialization.DataContractSerializer> 和 <xref:System.Runtime.Serialization.NetDataContractSerializer> ，但) 不能使用， <xref:System.Xml.Serialization.XmlSerializer> 甚至硬编码为仅使用 <xref:System.Runtime.Serialization.DataContractSerializer> 。  
  
 `XmlSerializer`是在 ASP.NET Web 服务中使用的序列化引擎。 `DataContractSerializer` 是理解新数据协定编程模型的新序列化引擎。 `DataContractSerializer` 默认选择，可以通过使用 `XmlSerializer` 特性对每个操作选择使用 <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior.DataContractFormatAttribute%2A> 。  
  
 <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior> 和 <xref:System.ServiceModel.Description.XmlSerializerOperationBehavior> 是分别负责为 `DataContractSerializer` 和 `XmlSerializer`插入消息格式化程序的操作行为。 <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior> 行为实际上可操作从 <xref:System.Runtime.Serialization.XmlObjectSerializer>派生的任何序列化程序，包括 <xref:System.Runtime.Serialization.NetDataContractSerializer> （在“使用独立序列化”中进行详细说明）。 此行为调用 `CreateSerializer` 虚拟方法重载之一以获取序列化程序。 若要插入其他序列化程序，请创建一个新的 <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior> 子类并重写两个 `CreateSerializer` 重载。  
  
## <a name="see-also"></a>另请参阅

- [在服务协定中指定数据传输](specifying-data-transfer-in-service-contracts.md)
