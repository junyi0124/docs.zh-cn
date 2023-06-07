---
title: 数据的安全考虑事项
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: a7eb98da-4a93-4692-8b59-9d670c79ffb2
ms.openlocfilehash: b9b033f779b083be8bcec195caf8e55607f14d31
ms.sourcegitcommit: 7588b1f16b7608bc6833c05f91ae670c22ef56f8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2020
ms.locfileid: "93188310"
---
# <a name="security-considerations-for-data"></a>数据的安全考虑事项

在处理 Windows Communication Foundation (WCF) 中的数据时，必须考虑许多威胁类别。 以下列表显示了与数据处理相关的最重要的威胁类。 WCF 提供了缓解这些威胁的工具。

* 拒绝服务

  当接收不受信任的数据时，这些数据可能导致非常耗时的计算，致使接收方访问数量过于巨大的各种资源，例如，内存、线程、可用连接或处理器周期。 针对服务器的拒绝服务攻击可能导致它崩溃，从而无法处理来自其他合法客户端的消息。

* 恶意代码执行

  传入的不受信任的数据会导致接收方运行它本不想运行的代码。

* 信息泄露

  远程攻击方强制接收方以一种泄露它想要的更多信息的方式来响应它的请求。

## <a name="user-provided-code-and-code-access-security"></a>用户提供的代码和代码访问安全性

Windows Communication Foundation 中的多个位置 (WCF) 基础结构运行用户提供的代码。 例如， <xref:System.Runtime.Serialization.DataContractSerializer> 序列化引擎可能调用用户提供的属性 `set` 访问器和 `get` 访问器。 WCF 信道基础结构还可以调入到用户提供的类的派生类 <xref:System.ServiceModel.Channels.Message> 。

代码创作者应负责确保不存在任何安全漏洞。 例如，如果您创建一个具有整数类型的数据成员属性的数据协定类型，并在 `set` 访问器实现中基于该属性值分配一个数组，则当恶意消息中包含该数据成员的一个极其庞大的值时，您可能会遭到拒绝服务攻击。 通常，应当在用户提供的代码中避免任何基于传入数据或耗时处理的分配（尤其当耗时处理可以由少量的传入数据导致时）。 当对用户提供的代码执行安全分析时，应确保同时考虑所有失败情况（即，引发异常的所有代码分支）。

用户提供的代码的最终示例是每个操作的服务实现内部的代码。 确保服务实现的安全是您的责任。 很容易意外创建可能导致拒绝服务漏洞的非安全操作实现。 例如这样一个操作：它接受一个字符串，并从数据库中返回名称以该字符串开头的客户列表。 如果您处理的是一个大型数据库，而所传递的字符串仅仅是一个字母，则您的代码可能会尝试创建一个大于所有可用内存的消息，从而导致整个服务失败  (<xref:System.OutOfMemoryException> 在 .NET Framework 中是不可恢复的，并且总是会导致应用程序终止。 ) 

您应当确保没有任何恶意代码插入各个扩展点。 在部分信任情况下运行时、处理部分受信任的程序集中的类型时或创建可由部分受信任的代码使用的组件时，这一点尤为重要。 有关更多信息，请参见后面一节中的“部分信任威胁”。

请注意，在部分信任情况下运行时，数据协定序列化基础结构仅支持数据协定编程模型的有限子集，例如，不支持使用 <xref:System.SerializableAttribute> 属性的私有数据成员或类型。 有关详细信息，请参阅 [部分信任](partial-trust.md)。

## <a name="avoiding-unintentional-information-disclosure"></a>避免无意中的信息泄露

当在考虑到安全性的情况下设计可序列化类型时，信息泄露是一个可能需要考虑的问题。

请考虑以下要点：

- <xref:System.Runtime.Serialization.DataContractSerializer> 编程模型允许在序列化期间，在类型或程序集的外部公开私有数据和内部数据。 此外，在导出架构的过程中也可以公开类型的形状。 请务必了解类型的序列化工程。 如果您不希望公开任何内容，应禁止对它进行序列化（例如，如果是数据协定，则不应用 <xref:System.Runtime.Serialization.DataMemberAttribute> 属性即可实现此禁止）。

- 请注意，同一类型可能有多个序列化工程，具体取决于使用的序列化程序。 同一类型可能存在这样的情况：与 <xref:System.Runtime.Serialization.DataContractSerializer> 一起使用时公开一组数据，而与 <xref:System.Xml.Serialization.XmlSerializer>一起使用时则公开另一组数据。 意外地使用错误的序列化程序可能导致信息泄露。

- 当在旧式的远程过程调用 (RPC)/编码模式下使用 <xref:System.Xml.Serialization.XmlSerializer> 时，可能意外地将发送方的对象图的形状公开给接收方。

## <a name="preventing-denial-of-service-attacks"></a>防止拒绝服务攻击

### <a name="quotas"></a>配额

潜在的拒绝服务攻击导致接收方分配大量的内存。 虽然本节专门介绍因过大的消息而导致的内存消耗问题，但要提醒读者注意的是，还可能出现其他的攻击。 例如，消息可能使用过长的处理时间。

通常使用配额来缓解拒绝服务攻击。 当超过配额时，通常会引发 <xref:System.ServiceModel.QuotaExceededException> 异常。 如果没有配额，则恶意消息可能导致所有可用的内存都受到访问，从而产生 <xref:System.OutOfMemoryException> 异常，或者导致所有可用的堆栈都受到访问，从而产生 <xref:System.StackOverflowException>。

超过配额的情况是可恢复的；如果是在正在运行的服务中遇到此问题，则会丢弃当前正在处理的消息，服务继续运行并处理更多的消息。 但是，内存不足和堆栈溢出方案在 .NET Framework 中的任何位置都是不可恢复的;如果此服务遇到此类异常，则该服务将终止。

WCF 中的配额不涉及任何预分配。 例如，如果 <xref:System.ServiceModel.Channels.TransportBindingElement.MaxReceivedMessageSize%2A> 配额（在各个类中都可找到）设置为 128 KB，并不意味着会自动为每个消息分配 128 KB。 实际分配的量取决于实际的传入消息大小。

传输层有许多配额。 这些配额是由正在使用的特定传输通道（HTTP、TCP 等等）强制施加的。 虽然本主题讨论其中的一些配额，但这些配额的详细介绍是在 [Transport Quotas](transport-quotas.md)中。

### <a name="hashtable-vulnerability"></a>哈希表漏洞

当数据协定包含哈希表或集合时存在漏洞。 如果大量的值插入到哈希表，其中大量的这些值生成相同的哈希值，则会出现此问题。 这可用作 DOS 攻击。  可以通过设置 MaxReceivedMessageSize 绑定配额来缓解此漏洞。 当设置此配额以避免此类攻击时，请务必小心。 此配额对 WCF 消息的大小设置一个上限。 此外，避免在数据协定中使用哈希表或集合。

## <a name="limiting-memory-consumption-without-streaming"></a>在不使用流模式的情况下限制内存消耗

防止消息过大的安全模型取决于是否使用流模式。 在基本的非流模式的情况下，消息会缓冲到内存中。 在这种情况下，使用 <xref:System.ServiceModel.Channels.TransportBindingElement.MaxReceivedMessageSize%2A> 或系统提供的绑定的 <xref:System.ServiceModel.Channels.TransportBindingElement> 配额，可通过限制可访问的最大消息大小来防止消息过大。 请注意，服务可能同时处理多个消息，在这种情况下，所有这些消息都在内存中。 使用遏制功能可缓解这种威胁。

另外，请注意， `MaxReceivedMessageSize` 不对每个消息的内存消耗设置上限，但限制它不得超过一个常量系数。 例如，如果 `MaxReceivedMessageSize` 是 1 MB，那么当接收到一个 1 MB 的消息，之后又对它进行反序列化时，则需要额外的内存来存放反序列化的对象图，从而导致总内存消耗大大超过 1 MB。 为此，应避免创建无需过多传入数据即会导致消耗大量内存的可序列化类型。 例如，具有50可选数据成员字段和额外100私有字段的数据协定 "MyContract" 可使用 XML 构造 "" 实例化 \<MyContract/> 。 此 XML 将导致为 150 个字段访问内存。 请注意数据成员默认情况下是可选的。 当这样的类型是数组的一部分时，问题会变得更复杂。

仅仅`MaxReceivedMessageSize` 还不足以防止所有的拒绝服务攻击。 例如，传入消息可能强制反序列化程序反序列化深度嵌套的对象图（一个对象包含另一个对象，而后者又包含另一个对象，依此类推）。 <xref:System.Runtime.Serialization.DataContractSerializer> 和 <xref:System.Xml.Serialization.XmlSerializer> 均以嵌套方式调用方法来反序列化此类对象图。 方法调用的深度嵌套可能导致不可恢复的 <xref:System.StackOverflowException>。 通过设置 <xref:System.ServiceModel.Configuration.XmlDictionaryReaderQuotasElement.MaxDepth%2A> 配额来限制 XML 嵌套的级别（在本主题后面的“安全地使用 XML”一节中介绍），可以缓解这种威胁。

当使用二进制 XML 编码时，将其他配额设置为 `MaxReceivedMessageSize` 尤为重要。 使用二进制编码有一点像压缩：传入消息中的一小组字节可表示大量的数据。 这样，即使消息不超过 `MaxReceivedMessageSize` 限制，其完全展开形式也可能占用多得多的内存。 要缓解 XML 特有的此类威胁，必须正确设置所有 XML 读取器配额，具体如本主题后面的“安全地使用 XML”一节所述。

## <a name="limiting-memory-consumption-with-streaming"></a>在使用流模式的情况下限制内存消耗

当使用流模式时，您可能使用一个小的 `MaxReceivedMessageSize` 设置来防止拒绝服务攻击。 但是，在流模式下可能出现更复杂的情况。 例如，文件上载服务接受大于所有可用内存的文件。 在这种情况下，应当将 `MaxReceivedMessageSize` 设置为一个非常大的值，此时预计的情况是：几乎不会有任何数据缓冲在内存中，并且消息将以流的形式直接传送到磁盘中。 如果恶意消息可以通过某种方式强制 WCF 缓冲数据，而不是在这种情况下对其进行流式处理，则 `MaxReceivedMessageSize` 不再能阻止消息访问所有可用内存。

为了缓解这种威胁，限制缓冲的各种 WCF 数据处理组件上存在特定的配额设置。 其中最重要的一个设置是各个传输绑定元素和标准绑定的 `MaxBufferSize` 属性。 当采用流模式时，应当在考虑将为每个消息分配的最大内存量的情况下设置此配额。 与 `MaxReceivedMessageSize`一样，此设置也不对内存消耗施加一个绝对上限值，而只限定它不得超过某个常量系数。 此外，与 `MaxReceivedMessageSize`一样，也要注意同时处理多个消息的可能性。

### <a name="maxbuffersize-details"></a>MaxBufferSize 详细信息

`MaxBufferSize`属性限制任何大容量缓冲 WCF 执行的工作。 例如，WCF 始终会缓冲 SOAP 标头和 SOAP 错误，并且在消息传输优化机制中找到的任何 MIME 部分均不处于自然读取顺序 (MTOM) 消息。 此设置可限制所有这些情况下的缓冲量。

WCF 通过将 `MaxBufferSize` 值传递给可能会缓冲的各种组件来完成此工作。 例如， <xref:System.ServiceModel.Channels.Message.CreateMessage%2A> 类的一些 <xref:System.ServiceModel.Channels.Message> 重载采用 `maxSizeOfHeaders` 参数。 WCF 将 `MaxBufferSize` 值传递给此参数以限制 SOAP 标头缓冲的量。 当直接使用 <xref:System.ServiceModel.Channels.Message> 类时，设置此参数非常重要。 通常，在使用采用配额参数的 WCF 中的组件时，必须了解这些参数的安全含义并正确地设置它们，这一点很重要。

MTOM 消息编码器也有一个 `MaxBufferSize` 设置。 当使用标准绑定时，它自动设置为传输层的 `MaxBufferSize` 值。 但是，当使用 MTOM 消息编码器绑定元素来构造一个自定义绑定时，在使用流模式的情况下应当将 `MaxBufferSize` 属性设置为一个安全值，这一点非常重要。

## <a name="xml-based-streaming-attacks"></a>基于 XML 的流式攻击

`MaxBufferSize` 只需确保流式处理时，WCF 才能强制进入缓冲。 例如，WCF XML 读取器在开始读取新元素时，始终会缓冲整个 XML 元素开始标记。 这样做是为了正确处理命名空间和属性。 如果 `MaxReceivedMessageSize` 配置为一个很大的值（例如，为了启用直接到磁盘的大型文件流式传送方案），那么，当整个消息正文是一个大型的 XML 元素开始标记时，可能会构造恶意消息。 尝试读取它会导致 <xref:System.OutOfMemoryException>。 这是许多可能的基于 XML 的拒绝服务攻击之一，可以使用 XML 读取器配额来缓解这些攻击，本主题后面的 "安全地使用 XML" 一节对此进行了讨论。 当使用流模式时，设置所有这些配额尤为重要。

### <a name="mixing-streaming-and-buffering-programming-models"></a>混合流式和缓冲编程模型

许多可能的攻击都是因为在同一个服务中混合流式和非流式编程模型而引起的。 假定存在一个具有两个操作的服务协定：一个操作采用一个 <xref:System.IO.Stream> ，另一个操作采用某个自定义类型的数组。 另外还假定 `MaxReceivedMessageSize` 设置为一个较大的值，以使第一个操作可以处理大型流。 遗憾的是，这意味着大型消息现在也可以发送给第二个操作，并且在调用该操作之前，反序列化程序将数据作为一个数组缓冲在内存中。 这是潜在的拒绝服务攻击： `MaxBufferSize` 配额不限制反序列化程序所处理的消息正文的大小。

为此，应当避免在同一个协定中混合基于流的操作和非流式操作。 如果您确实需要混合两种编程模型，可以考虑以下预防措施：

- 将 <xref:System.Runtime.Serialization.IExtensibleDataObject> 的 <xref:System.ServiceModel.ServiceBehaviorAttribute.IgnoreExtensionDataObject%2A> 属性设置为 <xref:System.ServiceModel.ServiceBehaviorAttribute> 以禁用 `true`功能。 这确保了只有作为协定一部分的成员才会反序列化。

- 将 <xref:System.Runtime.Serialization.DataContractSerializer.MaxItemsInObjectGraph%2A> 的 <xref:System.Runtime.Serialization.DataContractSerializer> 属性设置为一个安全值。 此配额也可通过 <xref:System.ServiceModel.ServiceBehaviorAttribute> 属性或通过配置来进行设置。 此配额限制在一个反序列化段内可反序列化的对象数目。 通常，消息协定中的每个操作参数或消息正文部分都在一个段内反序列化。 当反序列化数组时，每个数组项都计为一个单独的对象。

- 将所有的 XML 读取器配额设置为安全值。 注意 <xref:System.Xml.XmlDictionaryReaderQuotas.MaxDepth%2A>、 <xref:System.Xml.XmlDictionaryReaderQuotas.MaxStringContentLength%2A>和 <xref:System.Xml.XmlDictionaryReaderQuotas.MaxArrayLength%2A> ，并避免在非流式操作中使用字符串。

- 检查已知类型列表，记住它们中的任何一个都可以随时实例化（请参见本主题后面的“防止加载意外类型”一节）。

- 不要使用用于实现缓冲大量数据的 <xref:System.Xml.Serialization.IXmlSerializable> 接口的任何类型。 不要将这些类型添加到已知类型列表中。

- 不要在协定中使用 <xref:System.Xml.XmlElement>、 <xref:System.Xml.XmlNode> 数组、 <xref:System.Byte> 数组或实现 <xref:System.Runtime.Serialization.ISerializable> 的类型。

- 不要在已知类型列表中使用 <xref:System.Xml.XmlElement>、 <xref:System.Xml.XmlNode> 数组、 <xref:System.Byte> 数组或实现 <xref:System.Runtime.Serialization.ISerializable> 的类型。

当非流式操作使用 <xref:System.Runtime.Serialization.DataContractSerializer>时，也可以采用前面的预防措施。 如果您使用的是 <xref:System.Xml.Serialization.XmlSerializer>，则切勿在同一服务中混合流式和非流式编程模型，因为它没有 <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior.MaxItemsInObjectGraph%2A> 配额的保护。

### <a name="slow-stream-attacks"></a>慢速流式攻击

流式拒绝服务攻击类不涉及内存消耗。 此类攻击涉及数据的慢速发送方或接收方。 当等待发送或接收数据时，诸如线程和可用连接之类的资源将消耗殆尽。 恶意攻击或者慢速网络连接上的合法发送方/接收方均会导致这种情形的发生。

若要缓解这样的攻击，应正确设置传输超时时间。 有关详细信息，请参阅 [传输配额](transport-quotas.md)。 其次，在 `Read` `Write` WCF 中处理流时从不使用同步或操作。

## <a name="using-xml-safely"></a>安全地使用 XML

> [!NOTE]
> 虽然本节是有关 XML 的内容，但是这些信息同样适用于 JavaScript 对象表示法 (JSON) 文档。 使用 [Mapping Between JSON and XML](mapping-between-json-and-xml.md)时，配额同样发挥作用。

### <a name="secure-xml-readers"></a>安全的 XML 读取器

XML 信息集构成 WCF 中所有消息处理的基础。 当接受来自不受信任源的 XML 数据时，可能存在许多必须缓解的拒绝服务攻击。 WCF 提供了特殊的安全 XML 读取器。 在 WCF 中使用标准编码之一时，会自动创建这些读取器 (文本、二进制或 MTOM) 。

这些读取器上的一些安全功能始终处于活动状态。 例如，这些读取器从不处理文档类型定义 (DTD)，这些定义是拒绝服务攻击的潜在来源，绝不应出现在合法的 SOAP 消息中。 其他安全功能包括必须配置的读取器配额，这些在下一节介绍。

当直接处理 XML 读取器时 (例如编写自己的自定义编码器时，或直接使用 <xref:System.ServiceModel.Channels.Message> 类) 时，在有可能使用不受信任的数据时，请始终使用 WCF 安全读取器。 通过在 <xref:System.Xml.XmlDictionaryReader.CreateTextReader%2A>类上调用 <xref:System.Xml.XmlDictionaryReader.CreateBinaryReader%2A>、 <xref:System.Xml.XmlDictionaryReader.CreateMtomReader%2A> 或 <xref:System.Xml.XmlDictionaryReader> 的静态工厂方法重载之一来创建安全读取器。 创建读取器时，应传入安全的配额值。 不要调用 `Create` 方法重载。 它们不会创建 WCF 读取器。 而是创建不受本节所介绍的安全功能保护的读取器。

### <a name="reader-quotas"></a>读取器配额

安全的 XML 读取器具有五个可配置的配额。 这些配额通常是使用编码绑定元素或标准绑定的 `ReaderQuotas` 属性配置的，或者是使用在创建读取器时传入的 <xref:System.Xml.XmlDictionaryReaderQuotas> 对象创建的。

#### <a name="maxbytesperread"></a>MaxBytesPerRead

此配额限制当读取元素开始标记及其属性时在一次 `Read` 操作中读取的字节数 （在非流模式情况下，不会根据配额对元素名称本身进行计数。） <xref:System.Xml.XmlDictionaryReaderQuotas.MaxBytesPerRead%2A> 非常重要：

- 当读取元素名称及其属性时，总是将它们缓冲在内存中。 因此，在流模式下必须正确地设置此配额以防止在预计使用流模式时进行过度缓冲，这一点非常重要。 有关发生的实际缓冲量的信息，请参见下面的 `MaxDepth` 配额部分。

- 具有太多的 XML 特性可能会耗用过长的处理时间，因为必须检查特性名称的唯一性。 `MaxBytesPerRead` 可缓解这一威胁。

#### <a name="maxdepth"></a>MaxDepth

此配额限制 XML 元素的最大嵌套深度。 例如，文档 "" 的 \<A> \<B> \<C/> \</B> \</A> 嵌套深度为3。 由于以下原因，<xref:System.Xml.XmlDictionaryReaderQuotas.MaxDepth%2A> 非常重要：

- `MaxDepth` 与 `MaxBytesPerRead`交互：读取器始终在内存中保留当前元素以及它的所有上级的数据，因此读取器的最大内存消耗与这两个设置的积成比例。

- 当反序列化深度嵌套的对象图时，会迫使反序列化程序访问整个堆栈并引发一个不可恢复的 <xref:System.StackOverflowException>。 对于 <xref:System.Runtime.Serialization.DataContractSerializer> 和 <xref:System.Xml.Serialization.XmlSerializer>，XML 嵌套和对象嵌套之间存在直接关联。 使用 `MaxDepth` 可缓解这一威胁。

#### <a name="maxnametablecharcount"></a>MaxNameTableCharCount

此配额限制读取器的名称表  的大小。 名称表包含在处理 XML 文档时遇到的一些字符串（例如，命名空间和前缀）。 因为这些字符串缓冲在内存中，所以设置此配额可防止在预计使用流模式时进行过度缓冲。

#### <a name="maxstringcontentlength"></a>MaxStringContentLength

此配额限制 XML 读取器返回的最大字符串大小。 此配额不限制 XML 读取器本身的内存消耗，但限制使用该读取器的组件中的内存消耗。 例如，当 <xref:System.Runtime.Serialization.DataContractSerializer> 使用一个以 <xref:System.Xml.XmlDictionaryReaderQuotas.MaxStringContentLength%2A>保护的读取器时，将不会反序列化大于此配额的字符串。 当直接使用 <xref:System.Xml.XmlDictionaryReader> 类时，并不是所有的方法都将受此配额的约束，而只有专门设计用于读取字符串的方法（如 <xref:System.Xml.XmlDictionaryReader.ReadContentAsString%2A> 方法）才受此配额的约束。 读取器的 <xref:System.Xml.XmlReader.Value%2A> 属性不受此配额的影响，因此当需要此配额所提供的保护时，不应使用该属性。

#### <a name="maxarraylength"></a>MaxArrayLength

此配额限制 XML 读取器返回的基元数组（包括字节数组）的最大大小。 此配额不限制 XML 读取器本身的内存消耗，但限制使用该读取器的所有组件的内存消耗。 例如，当 <xref:System.Runtime.Serialization.DataContractSerializer> 使用一个以 <xref:System.Xml.XmlDictionaryReaderQuotas.MaxArrayLength%2A>保护的读取器时，将不会反序列化大于此配额的字节数组。 当尝试在一个协定中混合流式编程模型和缓冲编程模型时，设置此配额非常重要。 请记住，当直接使用 <xref:System.Xml.XmlDictionaryReader> 类时，只有专门设计用于读取某些基元类型的任意大小数组的方法（如 <xref:System.Xml.XmlDictionaryReader.ReadInt32Array%2A>）才受此配额的约束。

## <a name="threats-specific-to-the-binary-encoding"></a>二进制编码所特有的威胁

二进制 XML 编码 WCF 支持包含 *字典字符串* 功能。 可以仅仅使用几个字节对一个大型字符串进行编码。 这会实现显著的性能改进，但也引入了必须缓解的新型拒绝服务威胁。

有两种类型的字典： *静态* 字典和 *动态* 字典。 静态字典是可以使用二进制编码中的短代码表示的长字符串的内置列表。 当读取器已创建且无法修改时，这一字符串列表是固定的。 默认情况下，WCF 使用的静态字典中的任何字符串都足以产生严重的拒绝服务威胁，但它们仍可能在字典扩展攻击中使用。 在您提供自己的静态字典的复杂情况下，在引入大型字典字符串时应谨慎。

动态字典功能使得消息可以定义它们自己的字符串，并将它们与短代码关联。 字符串与代码的这些映射在整个通信会话期间一直保留在内存中，这样后续消息就不必重新发送字符串，并且可以利用已经定义的代码。 这些字符串可以是任意长度，因此造成了比静态字典中的字符串更严重的威胁。

必须缓解的首要威胁是动态字典（字符串与代码映射表）变得过大的可能性。 此类字典可以在几个消息的过程中展开，因此 `MaxReceivedMessageSize` 配额无法提供保护，因为该配额仅适用于各个单独的消息。 因此，在 <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement.MaxSessionSize%2A> 上存在一个限制字典大小的单独的 <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement> 属性。

与其他大多数配额不同，此配额在编写消息时也适用。 如果在读取消息时超过此配额，将像通常一样引发 `QuotaExceededException` 。 如果在编写消息时超过此配额，那么导致超过此配额的任何字符串都将按原样编写，而不使用动态字典功能。

### <a name="dictionary-expansion-threats"></a>字典展开威胁

二进制所特有的一类重要的攻击是因字典展开而引起的。 一个小的二进制形式的消息如果大量使用了字符串字典功能，那么它的完全展开的文本形式可能是一个非常大的消息。 动态字典字符串的展开系数受 <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement.MaxSessionSize%2A> 配额限制，因为不会有任何动态字典字符串超过整个字典的最大大小。

<xref:System.Xml.XmlDictionaryReaderQuotas.MaxNameTableCharCount%2A>、 `MaxStringContentLength`和 `MaxArrayLength` 属性仅限制内存消耗。 在非流式使用模式下，通常不需要它们来缓解任何威胁，因为内存使用已受 `MaxReceivedMessageSize`限制。 但是， `MaxReceivedMessageSize` 计算的是展开前的字节数。 当使用的是二进制编码时，内存消耗可以超过 `MaxReceivedMessageSize`，而仅受 <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement.MaxSessionSize%2A>这一个因素的限制。 为此，当使用二进制编码时应总是设置所有的读取器配额（尤其是 <xref:System.Xml.XmlDictionaryReaderQuotas.MaxStringContentLength%2A>），这一点非常重要。

当同时使用二进制编码和 <xref:System.Runtime.Serialization.DataContractSerializer>时， `IExtensibleDataObject` 接口可能被误用来发起字典展开攻击。 此接口实质上是为并非属于协定一部分的任意数据提供不受限制的存储。 如果配额不能设置得足够低以使 `MaxSessionSize` 与 `MaxReceivedMessageSize` 的乘积不会造成问题，那么当使用二进制编码时应禁用 `IExtensibleDataObject` 功能。 为此，应当将 `IgnoreExtensionDataObject` 属性 (Attribute) 的 `true` 属性 (Property) 设置为 `ServiceBehaviorAttribute` 。 或者，不实现 `IExtensibleDataObject` 接口。 有关详细信息，请参阅[向前兼容的数据协定](forward-compatible-data-contracts.md)。

### <a name="quotas-summary"></a>配额概述

下表概括了关于配额的指导信息。

|条件|要设置的重要配额|
|---------------|-----------------------------|
|非流式或流式小型消息、文本或 MTOM 编码|`MaxReceivedMessageSize`、`MaxBytesPerRead` 和 `MaxDepth`|
|非流式或流式小型消息或二进制编码|`MaxReceivedMessageSize`、 `MaxSessionSize`以及所有 `ReaderQuotas`|
|流式大型消息、文本或 MTOM 编码|`MaxBufferSize` 和所有 `ReaderQuotas`|
|流式大型消息或二进制编码|`MaxBufferSize`、 `MaxSessionSize`以及所有 `ReaderQuotas`|

- 必须总是设置传输层超时值，并且当使用流模式时切勿使用同步读/写，无论您是对大消息还是小消息进行流式处理，都是如此。

- 当对配额有疑问时，应将它设置为一个安全值，而不是将它留在那里不进行设置。

## <a name="preventing-malicious-code-execution"></a>防止恶意代码执行

下面的常规威胁类可以执行代码并且具有意外的影响：

- 反序列化程序加载恶意的、不安全的或者安全性敏感类型。

- 传入消息导致反序列化程序以一种具有意外后果的方式构造一个通常而言很安全的类型的实例。

下面几节进一步讨论这些威胁类。

## <a name="datacontractserializer"></a>DataContractSerializer

 (上的安全信息 <xref:System.Xml.Serialization.XmlSerializer> ，请参阅相关文档。 ) 的安全模型与的安全模型 <xref:System.Xml.Serialization.XmlSerializer> 类似 <xref:System.Runtime.Serialization.DataContractSerializer> ，并且主要在详细信息中有所不同。 例如，用于类型包括的是 <xref:System.Xml.Serialization.XmlIncludeAttribute> 属性，而不是 <xref:System.Runtime.Serialization.KnownTypeAttribute> 属性。 但是， <xref:System.Xml.Serialization.XmlSerializer> 特有的一些威胁将在本主题的后面部分讨论。

### <a name="preventing-unintended-types-from-being-loaded"></a>防止加载意外类型

加载意外类型可能具有非常严重的后果，无论该类型是恶意的，还是仅仅具有安全性敏感的负面影响，都有可能如此。 一个类型可能包含可利用的安全漏洞，在它的构造函数或类构造函数中执行安全性敏感操作，易于形成拒绝服务攻击的大内存需求量，或者可能引发不可恢复的异常。 类型可能具有在类型加载完成后、但在创建任何实例之前运行的类构造函数。 为此，必须控制反序列化程序可能加载的一组类型，这一点很重要。

<xref:System.Runtime.Serialization.DataContractSerializer> 以一种松散耦合的方式进行反序列化。 它绝不从传入的数据中读取公共语言运行库 (CLR) 类型和程序集名称。 这与 <xref:System.Xml.Serialization.XmlSerializer>的行为类似，但与 <xref:System.Runtime.Serialization.NetDataContractSerializer>、 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter>和 <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter>的行为不同。 松散耦合会引入一定程度的安全性，因为远程攻击者无法通过在消息中命名某个类型来指示首先加载该类型。

始终允许 <xref:System.Runtime.Serialization.DataContractSerializer> 加载按照协定当前预计加载的类型。 例如，如果数据协定具有一个 `Customer`类型的数据成员，则允许 <xref:System.Runtime.Serialization.DataContractSerializer> 在反序列化该数据成员时加载 `Customer` 类型。

此外， <xref:System.Runtime.Serialization.DataContractSerializer> 还支持多态性。 一个数据成员可能声明为 <xref:System.Object>，但传入的数据可能包含一个 `Customer` 实例。 只有当已通过下面某种机制使反序列化程序“知晓” `Customer` 类型时，这种情况才是可能的。

- 应用于类型的<xref:System.Runtime.Serialization.KnownTypeAttribute> 属性。

- `KnownTypeAttribute` 属性，用于指定一个返回类型列表的方法。

- `ServiceKnownTypeAttribute` 属性。

- `KnownTypes` 配置节。

- 在构造期间向 <xref:System.Runtime.Serialization.DataContractSerializer> 显式传递的已知类型列表（如果直接使用序列化程序）。

上述每个机制都引入了反序列化程序可加载的更多类型，从而增加了外围应用。 应控制其中的每个机制，确保不会有任何恶意类型或意外的类型添加到已知类型列表中。

一旦已知类型在范围之内，那么即使协定实际禁止使用它，也可以随时加载它，并且可以创建该类型的实例。 例如，假定使用上述某个机制将类型“MyDangerousType”添加到了已知类型列表中。 这表示：

- `MyDangerousType` 已加载并且它的类构造函数已运行。

- 即使当反序列化具有一个字符串数据成员的数据协定时，恶意消息也可能导致创建 `MyDangerousType` 的一个实例。 `MyDangerousType`中的代码（例如，属性 setter）可以运行。 完成此操作之后，反序列化程序会尝试将该实例指定给字符串数据成员，结果失败，并出现异常。

当编写返回已知类型列表的方法时，或者将一个列表直接传递给 <xref:System.Runtime.Serialization.DataContractSerializer> 构造函数时，应确保准备该列表的代码是安全的，并仅仅作用于受信任的数据。

如果在配置中指定已知类型，应确保配置文件是安全的。 应始终在配置中使用强名称（通过指定该类型所在的已签名程序集的公钥），但不要指定要加载的类型的版本。 类型加载程序在可能的情况下会自动选取最新版本。 如果您在配置中指定特定版本，将面临以下风险：类型可能具有一个可在将来的版本中修复的安全漏洞，但仍然加载有漏洞的版本，因为在配置中显式指定了它。

具有过多的已知类型有另一个后果： <xref:System.Runtime.Serialization.DataContractSerializer> 将在应用程序域中创建序列化/反序列化代码的缓存，其中对于每个必须序列化和反序列化的类型，都有一个相应的项。 只要应用程序域在运行，就绝不会清除这一缓存。 因此，知道应用程序使用许多已知类型的攻击者可能导致所有这些类型都反序列化，从而导致缓存消耗了极大的内存量。

### <a name="preventing-types-from-being-in-an-unintended-state"></a>防止类型处于意外状态

类型可能具有必须强制执行的内部一致性约束。 在反序列化期间，必须小心，以避免破坏这些约束。

下面的类型示例代表太空飞船上的密封过渡仓的状态，并强制执行不能同时打开内门和外门的约束。

[!code-csharp[DataContractAttribute#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/datacontractattribute/cs/overview.cs#3)]
[!code-vb[DataContractAttribute#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/datacontractattribute/vb/overview.vb#3)]

攻击者可能发送一个类似下面这样的恶意消息，以绕过此约束，而使对象进入无效状态，这可能造成意外的且不可预测的后果。

```xml
<SpaceStationAirlock>
    <innerDoorOpen>true</innerDoorOpen>
    <outerDoorOpen>true</outerDoorOpen>
</SpaceStationAirlock>
```

注意到下面几点就可以避免这种情形：

- 当 <xref:System.Runtime.Serialization.DataContractSerializer> 反序列化大多数类时，构造函数都不会运行。 因此，不要依赖在构造函数中执行的任何状态管理。

- 使用回调可确保对象处于有效状态。 标记有 <xref:System.Runtime.Serialization.OnDeserializedAttribute> 属性的回调尤其有用，因为它在反序列化完成后运行，有可能检查并更正整体状态。 有关详细信息，请参阅 [版本容错序列化回调](version-tolerant-serialization-callbacks.md)。

- 在设计数据协定类型时，不要使它依赖项属性 setter 的任何特定调用顺序。

- 使用标有 <xref:System.SerializableAttribute> 属性的旧式类型时应小心。 其中许多功能都设计为仅与受信任的数据一起使用 .NET Framework 远程处理。 标有此属性的现有类型在设计时可能并未考虑状态安全性。

- 考虑到状态安全性，不要依赖 <xref:System.Runtime.Serialization.DataMemberAttribute.IsRequired%2A> 属性 (Attribute) 的 <xref:System.Runtime.Serialization.DataMemberAttribute> 属性 (Property) 来保证数据的存在。 数据可能总是 `null`、 `zero`或 `invalid`。

- 在未首先验证的情况下，绝不要信任从不受信任的数据源反序列化的对象图。 每个单独的对象可能都处于一致状态，但对象图整体有可能处于不一致状态。 此外，即使禁用对象图保存模式，反序列化的对象图也可能具有对同一对象的多个引用或者具有循环引用。 有关详细信息，请参阅 [序列化和反序列](serialization-and-deserialization.md)化。

### <a name="using-the-netdatacontractserializer-securely"></a>安全地使用 NetDataContractSerializer

<xref:System.Runtime.Serialization.NetDataContractSerializer> 是一个序列化引擎，它使用类型的紧密耦合。 这类似于 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> 和 <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter>。 也就是说，它通过从传入数据中读取 .NET Framework 程序集和类型名来确定要实例化的类型。 尽管它是 WCF 的一部分，但没有提供的方法来插入此序列化引擎;必须编写自定义代码。 `NetDataContractSerializer`提供主要是为了简化从 .NET Framework 远程处理到 WCF 的迁移。 有关详细信息，请参阅 [序列化和反序列](serialization-and-deserialization.md)化中的相关部分。

因为消息本身可能指示可以加载的任何类型，所以 <xref:System.Runtime.Serialization.NetDataContractSerializer> 机制本质上是不安全的，应当仅与受信任的数据一起使用。 有关详细信息，请参阅 [BinaryFormatter security guide （安全指南](../../../standard/serialization/binaryformatter-security-guide.md)）。

即使与受信任的数据一起使用时，传入数据也可能不足以指定要加载的类型，尤其当 <xref:System.Runtime.Serialization.NetDataContractSerializer.AssemblyFormat%2A> 属性设置为 <xref:System.Runtime.Serialization.Formatters.FormatterAssemblyStyle.Simple>时更有可能如此。 可访问应用程序的目录或全局程序集缓存的任何人都可以用恶意类型来代替预计要加载的类型。 应始终通过正确地设置权限来确保应用程序的目录和全局程序集缓存的安全性。

通常，如果您允许部分受信任的代码访问您的 `NetDataContractSerializer` 实例，或以其他方式控制代理项选择器 (<xref:System.Runtime.Serialization.ISurrogateSelector>) 或序列化联编程序 (<xref:System.Runtime.Serialization.SerializationBinder>)，则代码可能会对序列化/反序列化过程进行大量控制。 例如，它可能会插入任意类型、导致信息泄漏、篡改生成的对象图或序列化数据，或使产生的序列化流溢出。

关于 `NetDataContractSerializer` 的另外一个安全问题是拒绝服务，而不是恶意代码执行威胁。 当使用 `NetDataContractSerializer`时，应始终将 <xref:System.Runtime.Serialization.NetDataContractSerializer.MaxItemsInObjectGraph%2A> 配额设置为一个安全值。 很容易构造这样一个小的恶意消息：分配一个其大小仅受此配额限制的对象数组。

### <a name="xmlserializer-specific-threats"></a>XmlSerializer 特有的威胁

<xref:System.Xml.Serialization.XmlSerializer> 安全模型与 <xref:System.Runtime.Serialization.DataContractSerializer>的安全模型类似。 但是，有几个威胁是 <xref:System.Xml.Serialization.XmlSerializer>特有的。

<xref:System.Xml.Serialization.XmlSerializer> 在运行时生成序列化程序集  ，它们包含实际执行序列化和反序列化的代码；这些程序集是在临时文件目录中创建的。 如果另外某个进程或用户对该目录拥有访问权限，可能用任意代码来覆盖序列化/反序列化代码。 之后 <xref:System.Xml.Serialization.XmlSerializer> 将使用它的安全上下文来运行这些代码，而不是运行序列化/反序列化代码。 请确保在临时文件目录上正确地设置了权限，以防止这种情况的发生。

<xref:System.Xml.Serialization.XmlSerializer> 还具有这样一种模式，即：使用预先生成的序列化程序集，而不是在运行时生成这些程序集。 只要 <xref:System.Xml.Serialization.XmlSerializer> 可以找到一个合适的序列化程序集，就会触发这一模式。 <xref:System.Xml.Serialization.XmlSerializer> 检查对序列化程序集进行签名的密钥是否就是对包含所序列化的类型的程序集进行签名的密钥。 这有助于防止恶意程序集伪装成序列化程序集。 但是，如果包含可序列化类型的程序集未签名，则 <xref:System.Xml.Serialization.XmlSerializer> 将无法执行此检查，而使用具有正确名称的任何程序集。 这就使得运行恶意代码成为可能。 应始终对包含可序列化类型的程序集进行签名，或者严格控制对应用程序的目录和全局程序集缓存的访问，以防止引入恶意程序集。

<xref:System.Xml.Serialization.XmlSerializer> 可能遭到拒绝服务攻击。 <xref:System.Xml.Serialization.XmlSerializer> 没有 `MaxItemsInObjectGraph` 所具有的 <xref:System.Runtime.Serialization.DataContractSerializer>配额。 这样，在消息大小所允许的范围内，它可以反序列化任意多的对象。

### <a name="partial-trust-threats"></a>部分信任威胁

关于与以部分信任运行的代码有关的威胁，请注意下面几点。 这些威胁包括恶意的部分受信任代码以及恶意的部分受信任代码与其他攻击情况（例如，构造特定字符串、然后对它进行反序列化的部分受信任代码）的组合。

- 使用任何序列化组件时，切勿在进行这样的使用之前断言任何权限，即使整个序列化方案是在断言的范围之内，并且您未处理任何不受信任的数据或对象，也不要进行这样的断言。 这样的使用可能导致安全漏洞。

- 在部分受信任的代码可以通过扩展点（代理项）、要序列化的类型或其他方法控制序列化过程的情况下，部分受信任的代码可能会导致序列化程序将大量数据输出到序列化流中，这将导致此流的接收方会受到拒绝服务 (DoS) 攻击。 如果要序列化数据，而这些数据专用于易于遭到 DoS 威胁的目标，则不序列化部分受信任的类型，或以其他方式使部分受信任的代码控制序列化。

- 如果允许部分受信任的代码访问 <xref:System.Runtime.Serialization.DataContractSerializer> 实例或控制 [数据协定代理](../extending/data-contract-surrogates.md)项，则可能会对序列化/反序列化过程进行大量控制。 例如，它可能会插入任意类型、导致信息泄漏、篡改生成的对象图或序列化数据，或使产生的序列化流溢出。 “安全地使用 NetDataContractSerializer”一节中介绍了等效的 <xref:System.Runtime.Serialization.NetDataContractSerializer> 威胁。

- 如果对类型应用了 <xref:System.Runtime.Serialization.DataContractAttribute> 属性（或者类型标记为 <xref:System.SerializableAttribute> ，但不是 <xref:System.Runtime.Serialization.ISerializable>），即使所有构造函数都不是公共的或者受需求保护，反序列化程序也可以创建这样一个类型的实例。

- 绝不要信任反序列化的结果，除非要反序列化的数据是受信任的，并且您确定所有已知类型都是您信任的类型。 请注意，在部分信任情况下运行时，已知类型不是从应用程序配置文件加载的（而是从计算机配置文件加载的）。

- 如果您使用添加到部分受信任代码的代理项来传递 <xref:System.Runtime.Serialization.DataContractSerializer> 实例，则代码可以更改该代理项的任何可修改设置。

- 对于已反序列化的对象，如果 XML 读取器（或者其中的数据）来自部分受信任的代码，则会将所生成的反序列化对象视为不受信任的数据。

- <xref:System.Runtime.Serialization.ExtensionDataObject> 类型没有公共成员，并不意味着它其中的数据是安全的。 例如，如果从特权数据源反序列化为其中存在一些数据的对象，然后将该对象传递给部分受信任的代码，则这些部分受信任的代码可以通过序列化 `ExtensionDataObject` 来读取该对象中的数据。 当从具有特权的数据源反序列化为之后将传递给部分受信任的代码的对象时，应考虑将 <xref:System.Runtime.Serialization.DataContractSerializer.IgnoreExtensionDataObject%2A> 设置为 `true` 。

- <xref:System.Runtime.Serialization.DataContractSerializer> 和 <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> 以完全信任的方式支持对私有成员、受保护成员、内部成员和公共成员进行序列化。 但在部分信任的情况下，只可序列化公共成员。 如果应用程序尝试序列化非公共成员，则会引发 <xref:System.Security.SecurityException> 。

    若要允许在部分信任的情况下序列化内部成员或受保护成员，则请使用 <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> 程序集属性。 此属性允许程序集声明其内部成员对其他程序集可见。 在此情况下，需要序列化其内部成员的程序集可声明其内部成员对 System.Runtime.Serialization.dll 可见。

    此方法的优点是，不需要提升的代码生成路径。

    不过，此方法还有两大缺点。

    第一个缺点是， <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> 特性的选择加入的属性是程序集范围的。 也就是说，无法指定仅某个类可具有序列化的内部成员。 当然，仍可以通过不将 <xref:System.Runtime.Serialization.DataMemberAttribute> 属性添加到某个特定的内部成员来选择不对该成员进行序列化。 类似地，出于可见性方面的考虑，开发人员也可选择使某个成员成为内部成员而不是私有成员或受保护成员。

    第二个缺点是，此方法仍不支持私有成员或受保护成员。

    若要演示如何在部分信任的情况下使用 <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> 属性，请考虑以下程序：

    [!code-csharp[CDF_WCF_SecurityConsiderationsForData#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/cdf_wcf_securityconsiderationsfordata/cs/program.cs#1)]

    在上面的示例中， `PermissionsHelper.InternetZone` 对应于部分信任情况下的 <xref:System.Security.PermissionSet> 。 现在，如果没有 <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> 属性，应用程序将失败，并引发， <xref:System.Security.SecurityException> 指示非公共成员不能在部分信任中序列化。

    但如果将以下行添加到源文件，则程序将成功运行。

    [!code-csharp[CDF_WCF_SecurityConsiderationsForData#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/cdf_wcf_securityconsiderationsfordata/cs/program.cs#2)]

## <a name="other-state-management-concerns"></a>其他状态管理问题

还有其他几个与对象状态管理的问题值得注意：

- 当将基于流的编程模型与流传输一起使用时，消息的处理会随着消息的到达而发生。 消息的发送方可能在流的传输过程中中止发送操作，从而在接收方期待更多内容的时候将代码置于了不可预测的状态。 通常，不要等待流完成，并且不要在基于流的操作中执行在流传输中止时无法回滚的任何工作。 这同样适用于在流正文之后消息格式有误的情况（例如，SOAP 信封缺少一个结束标记，或者有第二个消息正文）。

- 使用 `IExtensibleDataObject` 功能可能导致发送敏感数据。 如果您将来自不受信任源的数据放入具有 `IExtensibleObjectData` 的数据协定中，之后在对消息进行签名的安全通道上重新发送这些数据，则可能表明您对这些数据一无所知。 此外，如果您同时考虑已知数据块和未知数据块，则所发送的整体状态可能是无效的。 可以通过选择将扩展数据属性设置为 `null` 或者选择禁用 `IExtensibleObjectData` 功能来避免这种情况。

## <a name="schema-import"></a>架构导入

通常，导入架构以生成类型的过程只会在设计时发生，例如，当在 Web 服务上使用 [ServiceModel Metadata Utility Tool (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) 来生成客户端类时就会发生此过程。 但是，在更复杂的情况下，也可能在运行时处理架构。 请注意，这样做可能会有遭到拒绝服务攻击的风险。 有些架构可能需要很长时间才能导入。 如果架构可能来自不受信任的源，那么在这种情况下绝不要使用 <xref:System.Xml.Serialization.XmlSerializer> 架构导入组件。

## <a name="threats-specific-to-aspnet-ajax-integration"></a>特定于 ASP.NET AJAX 集成的威胁

当用户实现 <xref:System.ServiceModel.Description.WebScriptEnablingBehavior> 或时 <xref:System.ServiceModel.Description.WebHttpBehavior> ，WCF 公开可同时接受 XML 和 JSON 消息的终结点。 但是，只有一组读取器配额，供 XML 读取器和 JSON 读取器同时使用。 某些配额设置可能适合于一种读取器，但对另一种读取器而言太大。

实现 `WebScriptEnablingBehavior`时，用户可选择公开位于终结点的 JavaScript 代理。 必须考虑以下安全问题：

- 可以通过检查 JavaScript 代理来获取有关服务的信息（操作名称和参数名称等）。

- 使用 JavaScript 终结点时，可能会在客户端的 Web 浏览器缓存中保留敏感信息和私有信息。

## <a name="a-note-on-components"></a>关于组件的说明

WCF 是一个灵活且可自定义的系统。 本主题中的大部分内容侧重于最常见的 WCF 使用方案。 但是，可以通过多种不同的方式撰写 WCF 提供的组件。 必须了解使用每个组件的安全含义。 具体而言：

- 当您必须使用 XML 读取器时，应使用 <xref:System.Xml.XmlDictionaryReader> 类所提供的读取器，而不要使用其他任何读取器。 安全读取器是使用 <xref:System.Xml.XmlDictionaryReader.CreateTextReader%2A>、 <xref:System.Xml.XmlDictionaryReader.CreateBinaryReader%2A>或 <xref:System.Xml.XmlDictionaryReader.CreateMtomReader%2A> 方法创建的。 不要使用 <xref:System.Xml.XmlReader.Create%2A> 方法。 应始终为读取器配置安全配额。 WCF 中的序列化引擎仅在与 WCF 中的安全 XML 读取器一起使用时才是安全的。

- 当使用 <xref:System.Runtime.Serialization.DataContractSerializer> 来反序列化可能不受信任的数据时，应总是设置 <xref:System.Runtime.Serialization.DataContractSerializer.MaxItemsInObjectGraph%2A> 属性。

- 创建消息时，如果 `maxSizeOfHeaders` 不能提供足够的保护，应设置 `MaxReceivedMessageSize` 参数。

- 创建编码器时，应始终配额相关的配额，例如 `MaxSessionSize` 和 `MaxBufferSize`。

- 使用 XPath 消息筛选器时，应设置 <xref:System.ServiceModel.Dispatcher.XPathMessageFilter.NodeQuota%2A> 以限制筛选器访问的 XML 节点数量。 不要使用 XPath 表达式，此类表达式即使不访问许多节点，可能也需要很长时间来计算。

- 通常，当使用接受配额的任何组件时，都应了解它的安全含义并将它设置为一个安全值。

## <a name="see-also"></a>请参阅

- <xref:System.Runtime.Serialization.DataContractSerializer>
- <xref:System.Xml.XmlDictionaryReader>
- <xref:System.Xml.Serialization.XmlSerializer>
- [数据协定已知类型](data-contract-known-types.md)
