---
title: 消息协议
ms.date: 03/30/2017
ms.assetid: 5b20bca7-87b3-4c8f-811b-f215b5987104
ms.openlocfilehash: 871297abb0ccc46e079ab85b098705602d14a161
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96248313"
---
# <a name="messaging-protocols"></a>消息协议

Windows Communication Foundation (WCF) 通道堆栈采用编码和传输通道，将内部消息表示形式转换为其网络格式，并使用特定传输来发送。 用于 Web 服务互操作性的最常见传输是 HTTP，Web 服务使用的最常见编码是基于 XML 的 SOAP 1.1、SOAP 1.2 和消息传输优化机制 (MTOM)。

本主题介绍了使用的以下协议的 WCF 实现细节 <xref:System.ServiceModel.Channels.HttpTransportBindingElement> 。

规范/文档：

- [HTTP 1.1](https://www.ietf.org/rfc/rfc2616.txt)
- [SOAP 1.1 HTTP 绑定](https://www.w3.org/TR/2000/NOTE-SOAP-20000508)，第7部分
- [SOAP 1.2 HTTP 绑定](https://www.w3.org/TR/soap12-part2) 第7部分

本主题介绍了和使用的以下协议的 WCF 实现细节 <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement> <xref:System.ServiceModel.Channels.MtomMessageEncodingBindingElement> 。

规范/文档：

- [XML](https://www.w3.org/TR/REC-xml)
- [SOAP 1.1](https://www.w3.org/TR/2000/NOTE-SOAP-20000508/)
- [SOAP 1.2 核心](https://www.w3.org/TR/soap12-part1/)
- [WS-Addressing 2004/08](https://www.w3.org/Submission/2004/SUBM-ws-addressing-20040810/)
- [W3C Web 服务寻址 1.0 - 核心](https://www.w3.org/TR/2006/REC-ws-addr-core-20060509)
- [W3C Web 服务寻址 1.0 - SOAP 绑定](https://www.w3.org/TR/2006/REC-ws-addr-soap-20060509)
- [W3C Web 服务寻址 1.0-WSDL 绑定](https://www.w3.org/TR/2006/CR-ws-addr-wsdl-20060529/)
- [W3C Web 服务寻址 1.0 - 元数据](https://www.w3.org/TR/ws-addr-metadata/)
- [WSDL SOAP1.1 绑定](https://www.w3.org/TR/wsdl/)
- [WSDL SOAP1.2 绑定](https://www.w3.org/Submission/wsdl11soap12/)

本主题介绍适用于的以下协议的 WCF 实现细节 <xref:System.ServiceModel.Channels.MtomMessageEncodingBindingElement> 。

规范/文档：

- [XOP](https://www.w3.org/TR/xop10/)
- [MTOM + SOAP 1.2 绑定](https://www.w3.org/TR/soap12-mtom/)
- [MTOM SOAP 1.1 绑定](https://www.w3.org/Submission/soap11mtom10/)
- [MTOM WS-Policy 断言](https://www.w3.org/Submission/2006/SUBM-WS-MTOMPolicy-20061101/)

本主题中使用以下 XML 命名空间和关联的前缀：

| 前缀 | 命名空间统一资源标识符 (URI) |
|------------|---------------------------------------------------|
| s11 | `http://schemas.xmlsoap.org/soap/envelope` |
| s12 |`http://www.w3.org/2003/05/soap-envelope` |
| wsa |`http://www.w3.org/2004/08/addressing` |
| wsam |`http://www.w3.org/2007/05/addressing/metadata` |
| wsap |`http://schemas.xmlsoap.org/ws/2004/09/policy/addressing` |
| wsa10 |`http://www.w3.org/2005/08/addressing` |
| wsaw10 |`http://www.w3.org/2006/05/addressing/wsdl` |
| xop |`http://www.w3.org/2004/08/xop/include` |
| xmime |`http://www.w3.org/2004/06/xmlmime`<br /><br /> `http://www.w3.org/2005/05/xmlmime` |
| dp |`http://schemas.microsoft.com/net/2006/06/duplex` |

## <a name="soap-11-and-soap-12"></a>SOAP 1.1 和 SOAP 1.2

### <a name="envelope-and-processing-model"></a>信封和处理模型

WCF 实现 SOAP 1.1 信封处理基本配置文件 1.1 (BP11) 和基本配置文件 1.0 (SSBP10) 。 SOAP 1.2 信封处理是遵循 SOAP12-Part1 实现的。

本部分介绍 WCF 与 BP11 和 SOAP12-Part1 有关的某些实现选择。

#### <a name="mandatory-header-processing"></a>强制标头处理

WCF 遵循用于处理 `mustUnderstand` soap 1.1 和 soap 1.2 规范中所述标头的规则，具有以下变化形式。

通过关联的绑定元素配置的单个通道（例如，文本消息编码、安全性、可靠消息传递和事务）处理进入 WCF 信道堆栈的消息。 每个通道都识别来自关联命名空间的标头，并将其标记为已理解。 消息进入调度程序后，操作格式化程序读取相应消息/操作协定所期望的标头，并将其标记为已理解。 然后，调度程序验证是否还有任何标头未被理解，但仍标记为 `mustUnderstand`，如果是这样，则引发异常。 包含 `mustUnderstand` 标头并且目标为接收方的消息不会由接收方应用程序代码进行处理。

这样的分层处理实现了 SOAP 节点的基础结构层和应用层之间的分离。

- B1111：在 WCF 基础结构通道堆栈处理消息之后但在应用程序处理之前，检测不到的标头

     在 SOAP 1.1 和 SOAP 1.2 中，`mustUnderstand` 标头值不相同。 Basic Profile 1.1 要求，在 SOAP 1.1 消息中，`mustUnderstand` 值为 0 或 1。 SOAP 1.2 可以使用 0、1、`false` 和 `true` 作为值，但建议使用 `xs:boolean` 值的规范表示形式（`false` 和 `true`）。

- B1112：对于 soap `mustUnderstand` 1.1 和 soap 1.2 版本的 soap 信封，WCF 将发出值0和1。 WCF 将接受标头的整个值空间 `xs:boolean` `mustUnderstand` (0、1、 `false` `true`) 

#### <a name="soap-faults"></a>SOAP 错误

下面是特定于 WCF 的 SOAP 错误实现的列表。

- B2121： WCF 返回以下 SOAP 1.1 错误代码： `s11:mustUnderstand` 、 `s11:Client` 和 `s11:Server` 。

- B2122： WCF 返回以下 SOAP 1.2 错误代码： `s12:MustUnderstand` 、 `s12:Sender` 和 `s12:Receiver` 。

### <a name="http-binding"></a>HTTP 绑定

#### <a name="soap-11-http-binding"></a>SOAP 1.1 HTTP 绑定

WCF 在基本配置文件1.1 规范第3.4 节后实现 SOAP 1.1 HTTP 绑定，并提供以下说明：

- B2211： WCF 服务不实现 HTTP POST 请求的重定向。

- B2212： WCF 客户端根据3.4.8 支持 HTTP Cookie。

#### <a name="soap-12-http-binding"></a>SOAP 1.2 HTTP 绑定

WCF 实现 SOAP 1.2 HTTP 绑定，如 SOAP 1.2-第2部分 (SOAP12Part2) 规范中所述，其中包含以下说明。

SOAP 1.2 为 `application/soap+xml` 媒体类型提供了一个可选的操作参数。 在未使用 WS-Addressing 时，采用此参数优化消息调度很有用，这种情况下，不需要分析 SOAP 消息的正文。

- R2221：`application/soap+xml` 操作参数出现在 SOAP 1.2 请求中时，必须与相应 WSDL 绑定内的 `soapAction` 元素的 `wsoap12:operation` 属性匹配。

- R2222：如果使用 WS-Addressing 2004/08 或 WS-Addressing 1.0，`application/soap+xml` 操作参数出现在 SOAP 1.2 消息中时，必须与 `wsa:Action` 匹配。

如果 WS-Addressing 禁用，并且传入的请求不包含操作参数，则视为未指定消息 `Action`。

## <a name="ws-addressing"></a>WS-Addressing

WCF 实现了 WS-ADDRESSING 的3个版本：

- WS-Addressing 2004/08

- W3C Web 服务寻址 1.0 核心 (ADDR10-CORE) 和 SOAP 绑定 (ADDR10-SOAP)

- WS-Addressing 1.0 - 元数据

### <a name="endpoint-references"></a>终结点引用

WCF 实现的所有 WS-Addressing 版本都使用终结点引用来描述终结点。

#### <a name="endpoint-references-and-ws-addressing-versions"></a>终结点引用和 WS-Addressing 版本

WCF 实现了许多基础结构协议，它们使用 WS-Addressing，特别是 `EndpointReference` 元素和 `W3C.WsAddressing.EndpointReferenceType` 类 (例如，Ws-reliablemessaging、WS-SECURECONVERSATION 和 ws-trust) 。 WCF 支持将 WS-Addressing 的任一版本与其他基础结构协议一起使用。 WCF 端点支持每个端点 WS-Addressing 的一种版本。

对于 R3111， `EndpointReference` 与 WCF 终结点交换的消息中使用的元素或类型的命名空间必须与此终结点实现的 WS-Addressing 的版本相匹配。

例如，如果 WCF 终结点实现了 WS-RELIABLEMESSAGING，则中的 `AcksTo` 此类终结点返回的标头将 `CreateSequenceResponse` 使用 `EncodingBinding` 元素为此终结点指定 WS-Addressing 版本。

#### <a name="endpoint-references-and-metadata"></a>终结点引用和元数据

很多情况下，需要对给定终结点进行元数据或元数据引用通信。

B3121： WCF 采用 WS-MetadataExchange (MEX) 规范第6部分中描述的机制，按值或按引用包含终结点引用的元数据。

请考虑这样一种情况： WCF 服务需要使用安全断言标记语言进行身份验证， (SAML) 令牌颁发者颁发的令牌颁发者 `http://sts.fabrikam123.com` 。 WCF 终结点通过 `sp:IssuedToken` 将断言与 `sp:Issuer` 指向令牌颁发者的嵌套断言结合使用来描述此身份验证要求。 访问该 `sp:Issuer` 断言的客户端应用程序需要知道如何与令牌颁发机构终结点进行通信。 客户端需要知道令牌颁发机构的有关元数据。 使用在 MEX 中定义的终结点引用元数据扩展，WCF 提供对令牌颁发者元数据的引用。

```xml
<sp:IssuedToken>
  <sp:Issuer>
    <wsa10:Address>
      http://sts.fabrikam123.com
    </wsa10:Address>
    <wsa10:Metadata>
      <mex:Metadata>
        <mex:MetadataSection>
          <mex:MetadataReference>
            <wsa10:Address>
              http://sts.fabrikam123.com/mex
            </wsa10:Address>
          </mex:MetadataReference>
        </mex:MetadataSection>
      </mex:Metadata>
    </wsa10:Metadata>
  </sp:Issuer>
</sp:IssuedToken>
```

### <a name="message-addressing-headers"></a>消息寻址标头

#### <a name="message-headers"></a>消息头

对于这两个 WS-Addressing 版本，WCF 使用规范、、、和指定的以下消息标头 `wsa:To` `wsa:ReplyTo` `wsa:Action` `wsa:MessageID` `wsa:RelatesTo` 。

B3211：对于所有 WS-Addressing 版本，WCF 都接受，但不会生成现成的 WS-Addressing 消息标头 `wsa:FaultTo` 和 `wsa:From` 。

与 WCF 应用程序交互的应用程序可以添加这些消息标头，WCF 将相应地对其进行处理。

#### <a name="reference-parameters-and-properties"></a>引用参数和属性

WCF 根据相应规范实现终结点引用参数和引用属性的处理。

B3221：配置为使用 WS-Addressing 2004/08 时，WCF 终结点不区分处理引用属性和引用参数。

### <a name="message-exchange-patterns"></a>消息交换模式

Web 服务操作调用所涉及的消息序列称为 " *消息交换模式*"。 WCF 支持单向、请求-答复和双工消息交换模式。 本节根据所用的消息交换模式说明 WS-Addressing 对消息处理的需求。

本节通篇都是由请求方发送第一条消息，响应方接收第一条消息。

#### <a name="one-way-message"></a>单向消息

当 WCF 终结点配置为支持具有给定的消息 `Action` 遵循单向模式时，wcf 终结点将遵循以下行为和要求。 除非另行指定，否则行为和规则适用于 WCF 中支持的两个版本的 WS-Addressing：

- R3311：请求方必须包含 `wsa:To`、`wsa:Action` 以及表示终结点引用所指定的所有引用参数的标头。 如果使用 WS-Addressing 2004/08 并且终结点引用指定了 [reference properties]，则对应的标头必须也添加到消息中。

- B3312：请求方可以包含 `MessageID`、`ReplyTo` 和 `FaultTo` 标头。 接收方基础结构将忽略这些标头，并且这些标头将传给应用程序。

- R3313：在使用 HTTP 并且没有通过 HTTP 响应发送任何消息时，响应方必须发送一个正文为空且 HTTP 状态码为 202 的 HTTP 响应。

     如果使用的是 HTTP 传输，并且操作协定声明消息是单向的，则 HTTP 响应仍可用于发送基础结构消息 — 例如，可靠消息传递可通过 HTTP 响应发送 `SequenceAcknowledgement` 消息。

- B3314： WCF 响应方不会发送错误消息来响应单向消息。

#### <a name="request-reply"></a>请求-答复

如果为具有给定的消息配置了 WCF 终结点 `Action` 以遵循请求-答复模式，则 wcf 终结点将遵循下面的行为和要求。 除非另外指定，否则行为和规则适用于 WCF 中支持的两个版本的 WS-Addressing：

- R3321：请求者必须在请求中包含 `wsa:To` `wsa:Action` `wsa:MessageID` 所有引用参数或引用属性的请求、、和标头，或由终结点引用所指定的)  (。

- R3322：使用 WS-Addressing 2004/08 时，还必须在请求中包含 `ReplyTo`。

- R3323：使用 WS-Addressing 1.0 时，如果 `ReplyTo` 请求中不存在，则使用具有等于的 [address] 属性的默认终结点引用 `http://www.w3.org/2005/08/addressing/anonymous` 。

- R3324：请求 `wsa:To` `wsa:Action` 者必须 `wsa:RelatesTo` 在答复消息中包含、和标头，以及所有引用参数或引用属性的标头 (或 `ReplyTo` 请求中的终结点引用所指定的) 。

### <a name="web-services-addressing-faults"></a>Web 服务寻址错误

R3411： WCF 产生 WS-Addressing 2004/08 定义的以下错误。

| 代码 | 原因 |
|----------|-----------|
| `wsa:DestinationUnreachable` | 到达的消息所带有的 `ReplyTo` 不同于为此通道建立的答复地址；在 To 标头中指定的地址上，没有终结点在进行侦听。 |
| `wsa:ActionNotSupported` | 与终结点关联的基础结构通道或调度程序不能识别在 `Action` 标头中指定的操作。 |

R3412： WCF 产生 WS-Addressing 1.0 定义的以下错误。

| 代码 | 原因 |
|----------|-----------|
| `wsa10:InvalidAddressingHeader` | 重复 `wsa:To` 的 `wsa:ReplyTo` 、 `wsa:From` 或 `wsa:MessageID` 。 `wsa:RelatesTo`用相同的重复 `RelationshipType` 。 |
| `wsa10:MessageAddressingHeaderRequired` | 缺少必需的 Addressing 标头。 |
| `wsa10:DestinationUnreachable` | 到达的消息所带有的 `ReplyTo` 不同于为此通道建立的答复地址。 在 To 标头中指定的地址上没有终结点在进行侦听。 |
| `wsa10:ActionNotSupported` | 与终结点关联的基础结构通道或调度程序不能识别在 `Action` 标头中指定的操作。 |
| `wsa10:EndpointUnavailable` | RM 通道发送回此错误，指示终结点将不会根据对 `CreateSequence` 消息的寻址标头的检查结果来处理序列。 |

上表中的代码对应于 SOAP 1.1 中的 `FaultCode` 和 SOAP 1.2 中的 `SubCode` (Code=Sender)。

### <a name="wsdl-11-binding-and-ws-policy-assertions"></a>WSDL 1.1 绑定和 WS-Policy 断言

#### <a name="indicating-use-of-ws-addressing"></a>指示使用 WS-Addressing

WCF 使用策略断言来指示终结点对特定 WS-Addressing 版本的支持。

下面的策略断言具有终结点策略主题 [WS-PA]，并指示从终结点收发的消息必须使用 WS-Addressing 2004/08。

```xml
<wsap:UsingAddressing />
```

此策略断言扩充了 WS-Addressing 2004/08 规范。

下面的策略断言指示发送/接收的消息必须使用 WS-Addressing 1.0。

```xml
<wsam:Addressing/>
```

下面的策略断言具有终结点策略主题 [WS-PA]，并指示从终结点收发的消息必须使用 WS-Addressing 2004/08。

```xml
<wsaw10:UsingAddressing />
```

`wsaw10:UsingAddressing` 元素借自于 [WS-Addressing-WSDL]，并在符合该规范第 3.1.2 节的 WS-Policy 的上下文中使用。

使用 Addressing 不会更改 WSDL 1.1、SOAP 1.1 和 SOAP 1.2 HTTP 绑定的语义。 例如，如果某个请求发送到使用 Addressing 和 WSDL SOAP 1.x HTTP 绑定的终结点，并且该请求期望得到一个答复，则该答复必须用 HTTP 响应发送。

对于通过 http 响应发送的答复，WS-AM 断言是：

```xml
<wsam:AnonymousResponses/>
```

完整的策略断言可能类似于：

```xml
<wsam:Addressing>
    <wsp:Policy>
        <wsam:AnonymousResponses />
    </wsp:Policy>
</wsam:Addressing>
```

但是，有些消息交换模式可以从在请求方和响应方之间建立两个独立的反向 HTTP 连接中获益，例如，由响应方发送的主动单向消息。

WCF 提供一项功能，通过该功能，两个基础传输通道可构成一个复合双工通道，其中一个通道用于输入消息，另一个用于输出消息。 对于 HTTP 传输，复合双工提供了两个反向 HTTP 连接。 请求方使用一个连接将消息发送到响应方，而响应方使用另一个连接将消息发送回请求方。

对于通过单独的 http 请求发送的答复，ws-am 断言是：

```xml
<wsam:NonAnonymousResponses/>
```

完整的策略断言可能类似于：

```xml
<wsam:Addressing>
    <wsp:Policy>
        <wsam:NonAnonymousResponses />
    </wsp:Policy>
</wsam:Addressing>
```

在使用 WSDL 1.1 SOAP 1.x HTTP 绑定的终结点上，如果要使用以下具有终结点策略主题 [WS-PA] 的断言，要求将两个单独的反向 HTTP 连接分别用于从请求方到响应方以及从响应方到请求方传送消息。

```xml
<cdp:CompositeDuplex/>
```

上面的陈述对于请求消息的 `wsa:ReplyTo` 标头提出了以下要求：

- R3514： `ReplyTo` `[address]` `http://www.w3.org/2005/08/addressing/anonymous` 如果终结点使用 WSDL 1.1 SOAP 1.x HTTP 绑定，并且具有与 `wsap10:UsingAddressing` 附加的 or 断言耦合的策略替代 `wsap:UsingAddressing` `cdp:CompositeDuplex` 项，则发送到终结点的请求消息必须有一个标头，其属性不等于。

- R3515：发送到终结点的请求消息必须有一个 `ReplyTo` 标头 `[address]` ，其属性等于 `http://www.w3.org/2005/08/addressing/anonymous` ，或者根本没有 `ReplyTo` 标头，前提是该终结点使用 WSDL 1.1 SOAP 1.x HTTP 绑定，并且具有具有断言的策略替代项 `wsap10:UsingAddressing` 并且未 `cdp:CompositeDuplex` 附加断言。

- R3516：发送到终结点的请求消息必须有一个 `ReplyTo` 标头，其 `[address]` 属性等于， `http://www.w3.org/2005/08/addressing/anonymous` 如果终结点使用 WSDL 1.1 SOAP 1.x HTTP 绑定，并且有一个带有断言的策略替代项 `wsap:UsingAddressing` ，且未 `cdp:CompositeDuplex` 附加断言。

WS-addressing WSDL 规范力图描述类似协议绑定，它引入了一个带有 3 个文本值（required、optional 和 prohibited）的 `<wsaw:Anonymous/>` 元素来指示对 `wsa:ReplyTo` 标头的需求（第 3.2 节）。 遗憾的是，在 WS-Policy 的上下文中，这样的元素定义不特别适合用作断言，因为它要求使用特定于域的扩展来支持将此类元素用作断言的备选项的交集。 这样的元素定义还指示 `ReplyTo` 标头的值在传输时与终结点行为相反，这使它只能特定于 HTTP 传输。

#### <a name="action-definition"></a>操作定义

WS-Addressing 2004/08 为 `wsa:Action` 元素定义了一个 `wsdl:portType/wsdl:operation/[wsdl:input | wsdl:output | wsdl:fault]` 属性。 WS-Addressing 1.0 WSDL 绑定 (WS-ADDR10-WSDL) 定义了一个类似属性 `wsaw10:Action`。

两者之间唯一的区别在于默认 Action 模式语义（分别在 WS-ADDR 第 3.3.2 节和 WS-ADDR10-WSDL 第 4.4.4 节描述）。

`portType`在 WCF 术语) 但使用不同版本的 ws-addressing 时，有两个终结点共享相同的 (或协定是合理的。 但是，如果 Action 是由 `portType` 定义的，并且在实现该 `portType` 的终结点之间不应更改，则不可能同时支持这两种默认操作模式。

为了解决此矛盾，WCF 支持单一版本的 `Action` 属性。

B3521： WCF 使用 `wsaw10:Action` `wsdl:portType/wsdl:operation/[wsdl:input | wsdl:output | wsdl:fault]` WS-ADDR10-WSDL-WSDL 中定义的元素上的属性来确定 `Action` 相应消息的 URI，而不考虑终结点使用的 WS-Addressing 版本。

#### <a name="use-endpoint-reference-inside-wsdl-port"></a>在 WSDL 端口内使用终结点引用

WS-ADDR10-WSDL 4.1 节扩展了 `wsdl:port` 元素以包含 `<wsa10:EndpointReference…/>` 子元素，从而以 WS-Addressing 方式描述终结点。 WCF 将此实用工具扩展 WS-Addressing 2004/08 上，允许 `<wsa:EndpointReference…/>` 显示为的子元素 `wsdl:port` 。

- R3531：如果终结点有一个带有 `<wsaw10:UsingAddressing/>``wsdl:port`。

- R3532：如果 `wsdl:port` 包含一个子元素 `<wsa10:EndpointReference …/>` ，则 `wsa10:EndpointReference/wsa10:Address` 子元素的值必须与同级元素的属性的值匹配 `@address` `wsdl:port` / `wsdl:location` 。

- R3533：如果终结点有一个带有 `<wsap:UsingAddressing/>``wsdl:port`。

- R3534：如果 `wsdl:port` 包含一个子元素 `<wsa:EndpointReference …/>` ，则 `wsa:EndpointReference/wsa:Address` 子元素的值必须与同级元素的属性的值匹配 `@address` `wsdl:port` / `wsdl:location` 。

### <a name="composition-with-ws-security"></a>与 WS-Security 的结合

根据 WS-ADDR 和 WS-ADDR10 中的安全注意事项章节，建议将所有寻址消息头和消息正文一起签名，以便将它们绑定在一起。

当 WS-Security 用于消息完整性保护时，WS-Addressing 消息头和从引用参数或/和属性产生的标头必须与消息正文一起签名。

### <a name="examples"></a>示例

#### <a name="one-way-message"></a>单向消息

在此方案中，发送方向接收方发出单向消息。 使用 SOAP 1.2、HTTP 1.1 和 W3C WS-Addressing 1.0。

请求消息结构：消息头包含 `wsa10:To` 和 `wsa10:Action` 元素。 消息正文包含来自应用程序命名空间的一个特定 `<app:Ping>` 元素。

HTTP 标头： POST 中的目标与元素中的 URI 匹配 `wsa10:To` 。

按照 SOAP 1.2 的要求，Content-Type 标头的值为 `application/soap+xml`。 此外，还包含参数 `charset` 和 `action`。 Content-Type 标头的 `action` 参数与 `wsa10:Action` 消息头的值匹配。

```http
POST http://fabrikam123.com/Service HTTP/1.1
Content-Type: application/soap+xml; charset=utf-8;  
              action="http://fabrikam123.com/Service/OneWay"
Host: 131.107.72.15
Content-Length: 1501
Expect: 100-continue
Proxy-Connection: Keep-Alive
<s12:Envelope>
  <s12:Header>
    <wsa10:To s12:mustUnderstand="1">
        http://fabrikam123.com/Service
    </wsa10:To>
    <wsa10:Action s12:mustUnderstand="1">
        http://fabrikam123.com/Service/OneWay
    </wsa10:Action>
  </s12:Header>
  <s12:Body>
    <Ping xmlns="http://fabrikam123.com/Service/">
      <Text>Hello World</Text>
    </Ping>
  </s12:Body>
</s12:Envelope>
```

接收方以一个状态为 202 的空 HTTP 响应进行回应。 HTTP 响应的一个示例为：

```http
HTTP/1.1 202 Accepted
Date: Fri, 15 Jul 2005 08:56:07 GMT
Server: Microsoft-IIS/6.0
MicrosoftOfficeWebServer: 5.0_Pub
X-Powered-By: ASP.NET
X-AspNet-Version: 2.0.50215
Cache-Control: private
Content-Length: 0
```

## <a name="soap-message-transmission-optimization-mechanism"></a>SOAP Message Transmission Optimization Mechanism（SOAP 消息传输优化机制）

本部分介绍 HTTP SOAP MTOM 的 WCF 实现细节。 MTOM 技术是与传统文本/XML 编码或 WCF 二进制编码相同的类的 SOAP 消息编码机制。 MTOM 包括以下内容：

- [XOP] 描述的 XML 编码和打包机制，用于将包含 base64 编码的二进制数据的 XML 信息项优化为多个独立的二进制部分。

- XOP 包的 MIME 封装，用于将 XML Infoset 和 XOP 包的每一个二进制部分序列化为单独的 MIME 部分。

- 应用于 SOAP 1.x 信封的 MIME XOP 编码。

- HTTP 传输绑定。

可以通过 WCF 将 MTOM 与非 HTTP 传输一起使用。 不过，在本主题中，我们将着重讨论 HTTP 上的应用。

MTOM 格式利用了包含 MTOM 本身、XOP 以及 MIME 在内的大型规范集。 在某种程度上，此规范集的模块性使得重新构造格式和处理语义的确切需求变得有点困难。 本节介绍 MTOM HTTP 绑定的格式和处理需求。

### <a name="mtom-message-encoding"></a>MTOM 消息编码

#### <a name="generating-mtom-messages"></a>生成 MTOM 消息

[XOP] 第 3.1 节描述了将具有元素信息项（包含 base64 值）的 XML 编码为抽象定义的 XOP 包的过程。

下面的一系列步骤描述了特定于 MTOM 的编码过程：

1. 确保要编码的 SOAP 信封不包含具有的和的元素信息项 `[namespace name]` `http://www.w3.org/2004/08/xop/include` `[local name]` `Include` 。

2. 创建一个空 MIME 包。

3. 在原始 XML Infoset 中标识要优化的元素信息项。 对于要优化的项，组成 `[children]` 元素信息项的字符必须采用规范形式 `xs:base64Binary` (请参阅 3.2.16 base64Binary) ，并不能包含前面、以内联的任何空白字符，或遵循非空白的内容。）

4. 创建一个是原始 SOAP 信封副本的 XOP SOAP 信封，但是，在上一步中标识的每个元素信息项的 children 都替换为如下构造的 `xop:Include` 元素信息项：

    1. 通过将被替换字符处理为 base64 编码的数据，将其转换为二进制数据。

    2. 生成一个符合 R3133 和 R3134 需求的唯一 Content-ID 标头值。

    3. 生成一个具有二进制值的 Content-Transfer-Encoding MIME 标头。

    4. 如果要优化的元素信息项（新插入的 `xop:Include` 元素信息项的 [parent]）有一个 `xmime:contentType` 属性信息项，则生成一个带有 `xmime:contentType` 属性的值的 Content-Type MIME 标头。

    5. 生成一个新的二进制 MIME 部分，其内容由以下几项组成：从处理为 base64 的被替换数据解码的二进制数据、4b 中的 Content-ID、4c 中的 Content- Transfer-Encoding 标头，以及 Content-Type 标头（如果在步骤 4d 中生成）。

    6. 将一个 `href` 属性添加到 `xop:Include` 元素，其值为从 Content-ID 标头值（在步骤 4b 中生成）派生的 cid: uri。 删除包含 " \<" and "> " 字符，对剩余的字符串进行 URL 转义，然后添加前缀 `cid:` 。 以下最小字符集需要根据 RFC1738 和 RFC2396 进行转义。 其他字符也可转义。

        ```
        Hexadecimal 00-1F , 7F, 20, "<" | ">" | "#" | "%" | <">
        "{" | "}" | "|" | "\" | "^" | "[" | "]" | "`" | "~" | "^"
        ```

5. 用步骤 4 中的 XOP SOAP 信封创建一个根 MIME 部分。

6. 写入 HTTP 标头，包括 HTTP Content-Type 标头。

7. 写入 MIME 包。

#### <a name="processing-mtom-messages"></a>处理 MTOM 消息

处理 MTOM 消息的过程与前面“生成 MTOM 消息”一节中所述的过程完全相反：

1. 确保根 MIME 部分有 Content-Type `application/xop+xml`。

2. 通过将包的根 MIME 部分作为 XML 文档进行分析，构造一个 SOAP 信封。 字符编码由根 MIME 部分的 Content-Type 的 `charset` 参数确定。

3. 对于构造的 SOAP 信封中的每个元素信息项（有一个 `xop:Include` 元素信息项作为其 [children] 属性的唯一成员）：

    1. 移除 `cid:` 前缀，取消转义 `@href` 元素的 `xop:Include` 属性的值中所有 URI 转义序列 (RFC 2396)。 将结果字符串括在 " \<", "> " 中。

    2. 找到 Content-ID 标头值与步骤 3a 中派生的字符串匹配的 MIME 部分。

    3. 将出现在每一项的 `xop:Include` 属性中的 `children` 元素信息项替换为字符信息项，这些字符信息项表示的是 MIME 部分（在步骤 3b 中标识）全部正文的规范 base64 编码（请参见 XSD-2，3.2.16 base64Binary）（实际是将 `xop:Include` 元素信息项替换为从包部分重新构造的数据）。

#### <a name="http-content-type-header"></a>HTTP Content-Type 标头

下面是一个 WCF 说明，其中列出了从 MTOM 规范本身所述的要求派生的 SOAP 1. x MTOM 编码消息的 HTTP Content-type 标头格式，并派生自 MTOM 和 RFC 2387。

- R4131：HTTP Content-Type 标头必须具有值 multipart/related（不区分大小写）及其参数。 参数名不区分大小写。 参数顺序并不重要。

- RFC 2045 第 5.1 节中列出了 MIME 消息 Content-Type 标头的完全 Backus-Naur 形式 (BNF)。

- R4132：HTTP Content-Type 标头必须具有一个类型参数，并且值 `application/xop+xml` 括在双引号中。

虽然 RFC 2387 中不明确要求使用双引号，但文本观察到所有多部分/相关的媒体类型参数最有可能包含 " \@ " 或 "/" 之类的保留字符，因此需要双引号。

- R4133：HTTP Content-Type 标头应该有一个 start 参数，其值为包含 SOAP 1.x 信封的 MIME 部分的 Content-ID 标头，并且括在双引号中。 如果省略 start 参数，则第一个 MIME 部分必须包含 SOAP 1.x 信封。

- R4134：SOAP 1.1 MTOM 编码消息的 HTTP Content-Type 标头必须包含值为 text/xml（括在双引号中）的 start-info 参数。

- R4135：SOAP 1.2 MTOM 编码消息的 HTTP Content-Type 标头必须包含值为 `application/soap+xml`（括在双引号中）的 start-info 参数。

- R4136：SOAP 1.x MTOM 编码消息的 HTTP Content-Type 标头必须具有边界参数，其值（括在双引号中）与 RFC 2046 第 5.1.1 节中定义的 MIME 边界 BNF 匹配。

    ```
    boundary := 0*69<bchars> bcharsnospace
    bchars := bcharsnospace / " "
    bcharsnospace :=    DIGIT / ALPHA / "'" / "(" / ")" / "+"
                        / "_" / "," / "-" / "." / "/" / ":" / "=" / "?"
    ```

     示例：

     正确

    ```http
    Content-Type: multipart/related; type="application/xop+xml";start=" <part0@tempuri.org>";boundary="uuid:0ca0e16e-feb1-426c-97d8-c4508ada5e82+id=1";start-info="text/xml"
    ```

     正确

    ```http
    Content-Type: Multipart/Related; type="application/xop+xml";start-info="text/xml";boundary="uuid:0ca0e16e-feb1-426c-97d8-c4508ada5e82+id=1"
    ```

     不正确

    ```http
    Content-Type: Multipart/Related; type=application/xop+xml;start=" <part0@tempuri.org>";start-info="text/xml";boundary="uuid:0ca0e16e-feb1-426c-97d8-c4508ada5e82+id=1"
    ```

#### <a name="infoset-mime-part"></a>Infoset MIME 部分

SOAP 1.x 信封封装为 XOP MIME 包的根部分，通常称为 `infoset` 部分。

- R4141：SOAP 1.x 信封必须封装为 XOP MIME 包的根部分，称为 `infoset` 部分，并从 HTTP Content-Type 引用。

- R4142：SOAP `Infoset` 部分必须包含以下 MIME 标头：`Content-ID`、`Content-Transfer-Encoding` 和 `Content-Type`。

Content-ID 标头的格式根据 RFC 2045 定义为

```
"Content-ID" ":" msg-id
```

其中 `msg-id` 在 RFC 2822（取代 RFC 822，在 RFC 2045 中引用）定义：

```
msg-id    =       [CFWS] "<" id-left "@" id-right ">" [CFWS]
```

和实际上是包含在 "" 中的电子邮件地址 \<" and  "> 。 RFC 2822 中增加了 `[CFWS]` 前缀和后缀，用于携带注释，不应用于保持互操作性。

R4143：Infoset MIME 部分的 Content-ID 标头的值必须遵循 RFC 2822 中的 `msg-id` 结果，并省略 `[CFWS]` 前缀和后缀部分。

许多 MIME 实现会将 "" 中包含的值的要求宽松 \<" and "> 为电子邮件地址，并 `absoluteURI` \<" , "> 在电子邮件地址之外使用。 此版本的 WCF 使用以下形式的内容 ID MIME 标头值：

```
Content-ID: <http://tempuri.org/0>
```

R4144：MTOM 处理程序应接受符合以下宽松 `msg-id` 的 Content-ID 标头值。

```
msg-id-relaxed =     [CFWS] "<" (absoluteURI | mail-address) ">" [CFWS]
mail-address   =     id-left "@" id-right
```

MIME (RFC 2045) 提供 Content-Transfer-Encoding 标头来传递 MIME 部分内容的编码。 为 Content-Transfer-Encoding 定义的默认值为 7-bit，它不适合于大多数 SOAP 消息，因此，需要使用 Content-Transfer-Encoding 标头以获得更好的互操作性：

- R4145：SOAP Infoset 部分必须包含 Content-Transfer-Encoding 标头。

- R4146：如果 SOAP 信封字符编码为 UTF-8，则 Content-Transfer-Encoding 标头的值必须为 8-bit。

- R4147：如果 SOAP 信封字符编码为 UTF-16，则 Content-Transfer-Encoding 标头的值必须为 binary。

- 根据 [XOP] 第 5 节，

- R4148： SOAP 1.1 信息集部分必须包含媒体类型为 application/xop + xml 且参数类型 = "text/xml" 和字符集的 Content-type 标头

    ```http
    Content-Type: application/xop+xml;
                  charset=utf-8;type="text/xml"
    ```

- R4149： SOAP 1.2 信息集部分必须包含 media type `application/xop+xml` 和 parameters 类型为 "" 和的 content-type 标头 `application/soap+xml` `charset` 。

    ```http
    Content-Type: application/xop+xml;
                  charset=utf-8;type="application/soap+xml"
    ```

     虽然 XOP 定义 `charset` 参数对于 `application/xop+xml` 是可选的，但是对于类似于 BP 1.1 对 `charset` 媒体类型要求有 `text/xml` 参数的互操作性，该参数仍然是需要的。

- R41410：`type` 和 `charset` 参数必须出现在 SOAP 1.x Infoset 部分的 Content-Type 标头中。

#### <a name="wcf-endpoint-support-for-mtom"></a>WCF 对 MTOM 的终结点支持

MTOM 的用途是对 SOAP 消息进行编码，以优化 base64 编码数据。 下面列出了相关约束：

- R4151：任何包含 base64 编码数据的元素信息项都可以优化。

- B4152： WCF 优化包含 base64 编码数据的元素信息项，长度超过1024个字节。

配置为使用 MTOM 的 WCF 终结点将始终发送 MTOM 编码的消息。 即使没有一个部分符合必需的标准，消息仍然是 MTOM 编码消息（序列化为一个 MIME 包，具有包含 SOAP 信封的单个 MIME 部分）。

### <a name="ws-policy-assertion-for-mtom"></a>MTOM 的 WS-Policy 断言

WCF 使用以下策略断言来指示终结点的 MTOM 使用：

```xml
<wsoma:OptimizedMimeSerialization />
```

- R4211：上面的策略断言有一个终结点策略主题，并指定终结点收发的所有消息都必须使用 MTOM 进行优化。

- B4212：配置为使用 MTOM 优化时，WCF 终结点会向附加到相应的策略添加 MTOM 策略断言 `wsdl:binding` 。

### <a name="composition-with-ws-security"></a>与 WS-Security 的结合

MTOM 是一种类似于 `text/xml` 和 WCF 二进制 XML 的编码机制。 MTOM 可以与 WS-Security 及其他 WS-* 协议自然结合：使用 WS-Security 进行保护的消息可使用 MTOM 优化。

### <a name="examples"></a>示例

#### <a name="wcf-soap-11-message-encoded-using-mtom"></a>使用 MTOM 编码的 WCF SOAP 1.1 消息

```http
POST http://131.107.72.15/Mtom/svc/service.svc/Soap11MtomUTF8 HTTP/1.1
SOAPAction: "http://xmlsoap.org/echoBinaryAsString"
Content-Type: multipart/related;type="application/xop+xml";
              start="<http://tempuri.org/0>";start-info="text/xml";
       boundary="uuid:0ca0e16e-feb1-426c-97d8-c4508ada5e82+id=1"
Host: 131.107.72.15
Content-Length: 1501
--uuid:0ca0e16e-feb1-426c-97d8-c4508ada5e82+id=1
Content-ID: <http://tempuri.org/0>
Content-Transfer-Encoding: 8bit
Content-Type: application/xop+xml;charset=utf-8;type="text/xml"
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <s:Body>
    <EchoBinaryAsString xmlns="http://xmlsoap.org/Ping">
      <array>
        <xop:Include
         href="cid:http%3A%2F%2Ftempuri.org%2F1%2F632618206521093670"
         xmlns:xop="http://www.w3.org/2004/08/xop/include"/>
      </array>
    </EchoBinaryAsString>
  </s:Body>
</s:Envelope>
--uuid:0ca0e16e-feb1-426c-97d8-c4508ada5e82+id=1
Content-ID: <http://tempuri.org/1/632618206521093670>
Content-Transfer-Encoding: binary
Content-Type: application/octet-stream
…Binary Content..
--uuid:0ca0e16e-feb1-426c-97d8-c4508ada5e82+id=1
```

#### <a name="wcf-secure-soap-12-message-encoded-using-mtom"></a>使用 MTOM 编码的 WCF 安全 SOAP 1.2 消息

此示例中，消息是使用 MTOM 以及采用 WS-Security 进行保护的 SOAP 1.2 编码的。 标识来进行编码的二进制部分为 `BinarySecurityToken` 的内容、对应于加密签名的 `CipherValue` 的 `EncryptedData`，以及经过加密的正文。 请注意， `CipherValue` WCF 的 `EncryptedKey` 无法进行优化标识，因为其长度小于1024个字节。

```http
POST http://131.107.72.15/Mtom/service.svc/Soap12MtomSecureSignEncrypt HTTP/1.1
Content-Type: multipart/related; type="application/xop+xml";
              start="<http://tempuri.org/0>";
            boundary="uuid:0ca0e16e-feb1-426c-97d8-c4508ada5e82+id=3";
              start-info="application/soap+xml";
              action="http://xmlsoap.org/echoBinaryAsString"
Host: 131.107.72.15
Content-Length: 1941
--uuid:0ca0e16e-feb1-426c-97d8-c4508ada5e82+id=3
Content-ID: <http://tempuri.org/0>
Content-Transfer-Encoding: 8bit
Content-Type: application/xop+xml;charset=utf-8;type="application/soap+xml"
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/" xmlns:u="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">
  <s:Header>
    <o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd">
      <u:Timestamp u:Id="uuid-4d4ee765-5717-4d53-9ac9-99bddc07df6c-5">
        <u:Created>2005-09-09T06:57:32.488Z</u:Created>
        <u:Expires>2005-09-09T07:02:32.488Z</u:Expires>
      </u:Timestamp>
      <o:BinarySecurityToken u:Id="uuid-4d4ee765-5717-4d53-9ac9-99bddc07df6c-2" ValueType="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-x509-token-profile-1.0#X509v3" EncodingType="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-soap-message-security-1.0#Base64Binary">
        <xop:Include href="cid:http%3A%2F%2Ftempuri.org%2F1%2F632618206525089430" xmlns:xop="http://www.w3.org/2004/08/xop/include"/>
      </o:BinarySecurityToken>
      <e:EncryptedKey Id="_1" xmlns:e="http://www.w3.org/2001/04/xmlenc#">
        <e:EncryptionMethod Algorithm="http://www.w3.org/2001/04/xmlenc#rsa-oaep-mgf1p"/>
        <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
          <o:SecurityTokenReference>
            <o:KeyIdentifier ValueType="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-x509-token-profile-1.0#X509SubjectKeyIdentifier">Xeg55vRyK3ZhAEhEf+YT0z986L0=</o:KeyIdentifier>
          </o:SecurityTokenReference>
        </KeyInfo>
        <e:CipherData>          <e:CipherValue>oQfpxwT8/SAGyZQzKE2b4yO6dXuQj7pwJ+5CGL3Rf7C06bQ5ttMoQ9GLJcQYkXTzin+WwHEgs5bj5ml9HKTW9QAU5JJ6lksdymmQvWP5ZtGPBVchO4sofEGoCKmBiZL/DYS/cnbzgnc/3a6NYnc10y2fWGaGLiqa00zijAw7o0Y=</e:CipherValue>
        </e:CipherData>
      </e:EncryptedKey>
      <c:DerivedKeyToken u:Id="_2" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc">
        <o:SecurityTokenReference>
          <o:Reference URI="#_1"/>
        </o:SecurityTokenReference>
        <c:Nonce>OrEPRX7fISIS4sXYWPMv3g==</c:Nonce>
      </c:DerivedKeyToken>
      <e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#">
        <e:DataReference URI="#_3"/>
        <e:DataReference URI="#_4"/>
      </e:ReferenceList>
      <e:EncryptedData Id="_4" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#">
        <e:EncryptionMethod Algorithm="http://www.w3.org/2001/04/xmlenc#aes256-cbc"/>
        <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
          <o:SecurityTokenReference>
            <o:Reference URI="#_2"/>
          </o:SecurityTokenReference>
        </KeyInfo>
        <e:CipherData>
          <e:CipherValue>
            <xop:Include href="cid:http%3A%2F%2Ftempuri.org%2F2%2F632618206525089430" xmlns:xop="http://www.w3.org/2004/08/xop/include"/>
          </e:CipherValue>
        </e:CipherData>
      </e:EncryptedData>
    </o:Security>
  </s:Header>
  <s:Body u:Id="_0">
    <e:EncryptedData Id="_3" Type="http://www.w3.org/2001/04/xmlenc#Content" xmlns:e="http://www.w3.org/2001/04/xmlenc#">
      <e:EncryptionMethod Algorithm="http://www.w3.org/2001/04/xmlenc#aes256-cbc"/>
      <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
        <o:SecurityTokenReference xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd">
          <o:Reference URI="#_2"/>
        </o:SecurityTokenReference>
      </KeyInfo>
      <e:CipherData>
        <e:CipherValue>
          <xop:Include href="cid:http%3A%2F%2Ftempuri.org%2F3%2F632618206525089430" xmlns:xop="http://www.w3.org/2004/08/xop/include"/>
        </e:CipherValue>
      </e:CipherData>
    </e:EncryptedData>
  </s:Body>
</s:Envelope>
--uuid:0ca0e16e-feb1-426c-97d8-c4508ada5e82+id=3
Content-ID: <http://tempuri.org/1/632618206525089430>
Content-Transfer-Encoding: binary
Content-Type: application/octet-stream
...Binary content of BinarySecurityToken - X509 Certificate...
--uuid:0ca0e16e-feb1-426c-97d8-c4508ada5e82+id=3
Content-ID: <http://tempuri.org/2/632618206525089430>
Content-Transfer-Encoding: binary
Content-Type: application/octet-stream
...Binary serialization of the encrypted primary signature...
--uuid:0ca0e16e-feb1-426c-97d8-c4508ada5e82+id=3
Content-ID: <http://tempuri.org/3/632618206525089430>
Content-Transfer-Encoding: binary
Content-Type: application/octet-stream
...Binary serialization of the encrypted Body...
--uuid:0ca0e16e-feb1-426c-97d8-c4508ada5e82+id=3--
```
