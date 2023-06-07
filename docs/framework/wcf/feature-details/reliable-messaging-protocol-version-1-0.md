---
title: 可靠消息传送协议版本 1.0
ms.date: 03/30/2017
ms.assetid: a5509a5c-de24-4bc2-9a48-19138055dcce
ms.openlocfilehash: 8d192afcffca52136d6d71de49770c5a5ad13895
ms.sourcegitcommit: 71b8f5a2108a0f1a4ef1d8d75c5b3e129ec5ca1e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2020
ms.locfileid: "84202306"
---
# <a name="reliable-messaging-protocol-version-10"></a>可靠消息传送协议版本 1.0

本主题介绍了使用 HTTP 传输进行互操作所需的 WS-TRUST 2005 年2月（版本1.0）协议的 Windows Communication Foundation （WCF）实现细节。 WCF 遵循与本主题中所述的约束和说明相关的 WS 可靠消息传送规范。 请注意，ws-reliablemessaging 版本1.0 协议是从 WinFX 开始实现的。

WS-可靠消息2月2005协议在 WCF 中由实现 <xref:System.ServiceModel.Channels.ReliableSessionBindingElement> 。

为方便起见，本主题使用以下角色：

- 发起方：启动 WS-Reliable Message 序列创建的客户端

- 响应方：接收发起方的请求的服务

本文档使用下表中的前缀和命名空间。

|前缀|命名空间|
|------------|---------------|
|wsrm|`http://schemas.xmlsoap.org/ws/2005/02/rm`|
|netrm|`http://schemas.microsoft.com/ws/2006/05/rm`|
|s|`http://www.w3.org/2003/05/soap-envelope`|
|wsa|`http://schemas.xmlsoap.org/ws/2005/08/addressing`|
|wsse|`http://docs.oasis-open.org/wss/2004/01/oasis-200401-wssecurity-secext-1.0.xsd`|

## <a name="messaging"></a>消息传递

### <a name="sequence-establishment-messages"></a>序列建立消息

WCF 实现 `CreateSequence` 和 `CreateSequenceResponse` 消息以建立可靠的消息序列。 适用以下约束：

- B1101： WCF 发起方不会在消息中生成可选的 Expires 元素，也不会 `CreateSequence` 在 `CreateSequence` 消息包含元素的情况下，在 `Offer` `Expires` 元素中包含可选元素 `Offer` 。

- B1102：访问消息时 `CreateSequence` ，WCF `Responder` 将发送和接收两个 `Expires` 元素（如果存在），但不使用它们的值。

WS-Reliable Messaging 引入了 `Offer` 机制来建立两个形成会话的反向相关的序列。

- R1103：如果 `CreateSequence` 包含一个 `Offer` 元素，则可靠消息响应方要么必须接受序列并使用包含形成两个相关反向序列的 `CreateSequenceResponse` 元素的 `wsrm:Accept` 进行响应，要么必须拒绝 `CreateSequence` 请求。

- R1104：在反向序列上流动的 `SequenceAcknowledgement` 和应用程序消息必须发送到 `ReplyTo` 的 `CreateSequence` 终结点引用。

- R1105：`AcksTo` 中的 `ReplyTo` 和 `CreateSequence` 终结点引用必须具有与八进制识别匹配的地址值。

  在创建序列之前，WCF 响应程序验证和 epr 的 URI 部分 `AcksTo` `ReplyTo` 是否相同。

- R1106：`AcksTo` 中的 `ReplyTo` 和 `CreateSequence` 终结点引用应具有相同的引用参数集。

  WCF 不强制执行，但假定的 [引用参数] `AcksTo` `ReplyTo` `CreateSequence` 相同，并使用 `ReplyTo` 确认和反向序列消息的终结点引用中的 [引用参数]。

- R1107：在使用 `Offer` 机制建立两个反向序列时，在反向序列上流动的 `SequenceAcknowledgement` 和应用程序消息必须发送到 `ReplyTo` 的 `CreateSequence` 终结点引用。

- R1108：使用 Offer 机制建立两个反向序列时，`[address]` 的 `wsrm:AcksTo` 元素的 `wsrm:Accept` 终结点引用子元素的 `CreateSequenceResponse` 属性必须与 `CreateSequence` 的识别字节的目标 URI 相匹配。

- R1109：在使用 `Offer` 机制建立两个反向序列时，发起方发送的消息和响应方的消息确认必须发送到同一个终结点引用。

  WCF 使用 WS 可靠的消息传送在发起方和响应方之间建立可靠会话。 WCF 的 WS 可靠消息传递实现为单向、请求-答复和全双工消息传递模式提供可靠的会话。 上的 WS 可靠消息传送 `Offer` 机制 `CreateSequence` / `CreateSequenceResponse` 允许你建立两个相关的反向序列，并提供适用于所有消息终结点的会话协议。 因为 WCF 为此类会话提供了安全保证，包括会话完整性的端到端保护，所以确保向同一方发送的消息到达同一目标是不切实际的。 这也允许在应用程序消息上捎带序列确认消息。 因此，约束 R1104、R1105 和 R1108 适用于 WCF。

`CreateSequence` 消息的一个示例。

```xml
<s:Envelope>
  <s:Header>
    <a:Action s:mustUnderstand="1">
      http://schemas.xmlsoap.org/ws/2005/02/rm/CreateSequence
    </a:Action>
    <a:ReplyTo>
      <a:Address>
         http://Business456.com/clientA
      </a:Address>
    </a:ReplyTo>
    <a:MessageID>
      urn:uuid:addabbbf-60cb-44d3-8c5b-9e0841629a36
    </a:MessageID>
    <a:To s:mustUnderstand="1">
      http://Business456.com/clientA
    </a:To>
  </s:Header>
  <s:Body>
    <wsrm:CreateSequence>
      <wsrm:AcksTo>
       <wsa:Address>
         http://Business456.com/clientA
       </wsa:Address>
     </wsrm:AcksTo>
     <wsrm:Offer>
      <wsrm:Identifier>
        urn:uuid:0afb8d36-bf26-4776-b8cf-8c91fddb5496
      </wsrm:Identifier>
     </wsrm:Offer>
   </wsrm:CreateSequence>
  </s:Body>
</s:Envelope>
```

 `CreateSequenceResponse` 消息的一个示例。

```xml
<s:Envelope>
  <s:Header>
    <a:Action s:mustUnderstand="1">
      http://schemas.xmlsoap.org/ws/2005/02/rm/CreateSequenceResponse
    </a:Action>
    <a:RelatesTo>
      urn:uuid:addabbbf-60cb-44d3-8c5b-9e0841629a36
    </a:RelatesTo>
    <a:To s:mustUnderstand="1">
      http://Business456.com/clientA
    </a:To>
  </s:Header>
  <s:Body>
   <wsrm:CreateSequenceResponse>
    <Identifier>
     urn:uuid:eea0a36c-b38a-43e8-8c76-2fabe2d76386
    </Identifier>
    <Accept>
    <AcksTo>
      <a:Address>
        http://BusinessABC.com/serviceA
      </a:Address>
    </AcksTo>
    </Accept>
   </wsrm:CreateSequenceResponse>
  </s:Body>
</s:Envelope>
```

### <a name="sequence"></a>序列

以下是适用于序列的约束的列表：

- B1201： WCF 生成并访问的序列号 `xs:long` 的最大非独占值9223372036854775807不超过最大值。

- B1202： WCF 始终使用操作 URI 生成一个空的 expression-bodied 最后一条消息 `http://schemas.xmlsoap.org/ws/2005/02/rm/LastMessage` 。

- B1203： `LastMessage` 只要操作 URI 不为，WCF 就会接收并传递包含一个元素的序列标头的消息 `http://schemas.xmlsoap.org/ws/2005/02/rm/LastMessage` 。

Sequence 标头的一个示例。

```xml
<wsrm:Sequence>
  <wsrm:Identifier>
    urn:uuid:addabbbf-60cb-44d3-8c5b-9e0841629a36
  </wsrm:Identifier>
  <wsrm:MessageNumber>
    10
  </wsrm:MessageNumber>
  <wsrm:LastMessage/>
 </wsrm:Sequence>
```

### <a name="ackrequested-header"></a>AckRequested 标头

WCF 使用 `AckRequested` 标头作为 keep-alive 机制。 WCF 不会生成可选的 `MessageNumber` 元素。 接收到带有 `AckRequested` 包含元素的标头的消息时 `MessageNumber` ，WCF 将忽略该 `MessageNumber` 元素的值，如下面的示例中所示。

```xml
<wsrm:AckRequested>
  <wsrm:Identifier>
    urn:uuid:addabbbf-60cb-44d3-8c5b-9e0841629a36
  </wsrm:Identifier>
</wsrm:AckRequested>
```

### <a name="sequenceacknowledgement-header"></a>SequenceAcknowledgement 标头

WCF 对 WS-TRUST 消息中提供的序列确认使用非法携带机制。

- R1401：使用 `Offer` 机制建立两个相反序列时，`SequenceAcknowledgement` 头可能包括在传输到预期接收方的任何应用程序消息中。

- B1402：在接收任何序列消息（例如，为了满足消息）之前，WCF 必须生成确认时 `AckRequested` ，wcf 将生成 `SequenceAcknowledgement` 包含范围0-0 的标头，如以下示例中所示。

  ```xml
  <wsrm:SequenceAcknowledgement>
    <wsrm:Identifier>
      urn:uuid:addabbbf-60cb-44d3-8c5b-9e0841629a36
    </wsrm:Identifier>
    <wsrm:AcknowledgementRange Upper="0" Lower="0"/>
  </wsrm:SequenceAcknowledgement>
  ```

- B1403： WCF 不生成 `SequenceAcknowledgement` 包含 `Nack` 元素但支持元素的标头 `Nack` 。

### <a name="ws-reliablemessaging-faults"></a>WS-ReliableMessaging 错误

下面列出了适用于 WS-TRUST 消息错误的 WCF 实现的约束：

- B1501： WCF 不会生成 `MessageNumberRollover` 错误。

- B1502： WCF 终结点可能生成 `CreateSequenceRefused` 错误，如规范中所述。

- B1503：当服务终结点达到其连接限制并且无法处理新连接时，WCF 将生成附加的 `CreateSequenceRefused` 错误子代码， `netrm:ConnectionLimitReached` 如下面的示例中所示。

  ```xml
  <s:Envelope>
    <s:Header>
      <wsa:Action>
        http://schemas.xmlsoap.org/ws/2005/08/addressing/fault
      </wsa:Action>
    </s:Header>
    <s:Body>
      <s:Fault>
        <s:Code>
          <s:Value>
            s:Receiver
          </s:Value>
          <s:Subcode>
            <s:Value>
              wsrm:CreateSequenceRefused
            </s:Value>
            <s:Subcode>
              <s:Value>
                netrm:ConnectionLimitReached
              </s:Value>
            </s:Subcode>
          </s:Subcode>
        </s:Code>
        <s:Reason>
          <s:Text xml:lang="en">
            [Reason]
          </s:Text>
        </s:Reason>
      </s:Fault>
    </s:Body>
  </s:Envelope>
  ```

### <a name="ws-addressing-faults"></a>WS-Addressing 错误

由于 WS 可靠消息传送使用 WS-ADDRESSING，WCF WS-TRUST 消息的实现可能会产生 WS-ADDRESSING 错误。 本部分介绍 WCF 在 WS-TRUST 消息层上显式生成的 WS 寻址错误：

- B1601：在以下条件之一成立时，WCF 将生成所需的错误消息寻址标头：

  - 消息缺少 `Sequence` 标头和 `Action` 标头。

  - `CreateSequence` 消息缺少 `MessageId` 标头。

  - `CreateSequence` 消息缺少 `ReplyTo` 标头。

- B1602： WCF 在答复缺少标头的消息时生成不支持的错误操作 `Sequence` ，并且具有不能 `Action` 在 ws-trust 消息规范中识别的标头。

- B1603： WCF 生成错误终结点不可用，以指示终结点不会根据 `CreateSequence` 消息的寻址标头的检查来处理序列。

## <a name="protocol-composition"></a>协议组合

### <a name="composition-with-ws-addressing"></a>与 WS-Addressing 组合

WCF 支持两个版本的 WS-ADDRESSING： WS-ADDRESSING 2004/08 [WS-ADDRESSING] 和 W3C WS 寻址1.0 建议 [WS-ATOMICTRANSACTION] 和 [WS-ADDRESSING-SOAP]。

尽管 WS-Reliable Messaging 规范只提及 WS-Addressing 2004/08，它并未限制要使用的 WS-Addressing 版本。 下面列出了适用于 WCF 的约束：

- R2101：WS-Addressing 2004/08 和 WS-Addressing 1.0 都可以与 WS-Reliable Messaging 一起使用。

- R2102：必须在给定的 WS-TRUST 消息传递序列或使用机制关联的一对反向序列中使用一对 WS-ADDRESSING 的单版本 `wsrm:Offer` 。

### <a name="composition-with-soap"></a>与 SOAP 组合

WCF 支持将 SOAP 1.1 和 SOAP 1.2 同时用于 WS-TRUST 消息。

### <a name="composition-with-ws-security-and-ws-secureconversation"></a>与 WS-Security 和 WS-SecureConversation 组合

WCF 通过使用安全传输（HTTPS）、使用 WS 安全机制和结合 WS 安全会话撰写，为 WS-MANAGEMENT 消息传递序列提供保护。 下面列出了适用于 WCF 的约束：

- R2301：若要保护 WS-TRUST 消息序列的完整性，以及单个消息的完整性和保密性，WCF 需要使用 WS 安全对话。

- R2302：WS-Secure Conversation 会话必须在建立 WS-Reliable Messaging 序列之前建立。

- R2303：如果 WS-Reliable Messaging 序列生存期超过了 WS-Secure Conversation 会话的生存期，则通过使用 WS-Secure Conversation 建立的 `SecurityContextToken` 必须通过使用相应的 WS-Secure Conversation 续订绑定来进行续订。

- B2304：WS-Reliable Messaging 序列或一对相关的反向序列总是绑定到单一的 WS-SecureConversation 会话。

  WCF 源在 `wsse:SecurityTokenReference` 消息的元素可扩展性部分生成元素 `CreateSequence` 。

- R2305：使用 WS 安全会话撰写时， `CreateSequence` 消息必须包含 `wsse:SecurityTokenReference` 元素。

## <a name="ws-reliable-messaging-ws-policy-assertion"></a>WS-Reliable Messaging WS-Policy 断言

WCF 使用 WS 可靠的消息传递 WS 策略断言 `wsrm:RMAssertion` 来描述终结点功能。 下面列出了适用于 WCF 的约束：

- B3001： WCF `wsrm:RMAssertion` 将 WS 策略断言附加到 `wsdl:binding` 元素。 WCF 同时支持 `wsdl:binding` 和元素的附件 `wsdl:port` 。

- B3002： WCF 支持 WS 可靠消息传递断言的以下可选属性，并在 WCF 上提供对这些属性的控制 `ReliableMessagingBindingElement` ：

  - `wsrm:InactivityTimeout`

  - `wsrm:AcknowledgementInterval`

  下面是一个示例。

  ```xml
  <wsrm:RMAssertion>
    <wsrm:InactivityTimeout Milliseconds="600000" />
    <wsrm:AcknowledgementInterval Milliseconds="200" />
  </wsrm:RMAssertion>
  ```

## <a name="flow-control-ws-reliable-messaging-extension"></a>流控制 WS-Reliable Messaging 扩展

WCF 使用 WS 可靠的消息传递扩展性提供对序列消息流的可选更严格的控制。

通过将属性设置为，启用流控制 <xref:System.ServiceModel.Channels.ReliableSessionBindingElement.FlowControlEnabled?displayProperty=nameWithType> `true` 。 下面列出了适用于 WCF 的约束：

- B4001：启用可靠消息流控制时，WCF 会 `netrm:BufferRemaining` 在标头的元素扩展性中生成元素 `SequenceAcknowledgement` 。

- B4002：启用可靠消息流控制时，WCF 不需要 `netrm:BufferRemaining` 元素出现在 `SequenceAcknowledgement` 标头中，如下面的示例中所示。

  ```xml
  <wsrm:SequenceAcknowledgement>
    <wsrm:Identifier>
      http://fabrikam123.com/abc
    </wsrm:Identifier>
    <wsrm:AcknowledgementRange Upper="1" Lower="1"/>
    <netrm:BufferRemaining>
      8
    </netrm:BufferRemaining>
  </wsrm:SequenceAcknowledgement>
  ```

- B4003： WCF 使用 `netrm:BufferRemaining` 来指示可靠消息传送目标可以缓冲多少条新消息。

- B4004：当可靠消息目标应用程序无法快速接收消息时，WCF 可靠消息传递服务会限制传输的消息数。 可靠消息传递目标将缓冲消息，而该元素的值将下降为 0。

- B4005： WCF 生成 `netrm:BufferRemaining` 0 到4096之间的整数值，并读取0到0之间的整数值 `xs:int` `maxInclusive` 214748364。

## <a name="message-exchange-patterns"></a>消息交换模式

本部分介绍在将 WS-TRUST 消息用于不同消息交换模式时，WCF 的行为。 对于每个消息交换模式，可以考虑下面的两个部署方案：

- 不可寻址发起方：发起方位于防火墙后面；响应方仅可以通过 HTTP 响应向发起方传递消息。

- 可寻址的发起方：发起方和响应方都可以发送 HTTP 请求；换句话说，可以建立两个相反的 HTTP 连接。

### <a name="one-way-non-addressable-initiator"></a>单向、不可寻址的发起方

#### <a name="binding"></a>绑定

WCF 通过一个 HTTP 通道使用一个序列来提供单向消息交换模式。 WCF 使用 HTTP 请求将所有消息从 RMS 传输到 RMD，并使用 HTTP 响应将所有消息从 RMD 传输到 RMS。

#### <a name="createsequence-exchange"></a>CreateSequence 交换

WCF 发起方将生成一 `CreateSequence` 条不含产品/服务的消息。 WCF 响应方确保在 `CreateSequence` 创建序列之前没有任何提议。 WCF 响应方 `CreateSequence` 使用消息答复请求 `CreateSequenceResponse` 。

#### <a name="sequenceacknowledgement"></a>SequenceAcknowledgement

WCF 发起方处理除 `CreateSequence` 消息和错误消息之外的所有消息的回复确认。 WCF 响应方始终在对序列和消息的响应中生成独立确认 `AckRequested` 。

#### <a name="terminatesequence-message"></a>TerminateSequence 消息

WCF 将视为单向 `TerminateSequence` 操作，这意味着 http 响应的正文和 http 202 状态代码为空。

### <a name="one-way-addressable-initiator"></a>单向、可寻址的发起方

#### <a name="binding"></a>绑定

WCF 提供了一种单向消息交换模式，它在入站和出站 Http 通道上使用一个序列。 WCF 使用 HTTP 请求传输所有消息。 所有 HTTP 响应的正文都为空，并且具有 HTTP 202 状态代码。

#### <a name="createsequence-exchange"></a>CreateSequence 交换

WCF 发起方将生成一 `CreateSequence` 条不含产品/服务的消息。 在创建序列之前，WCF 响应程序确保 `CreateSequence` 没有任何提议。 WCF 响应 `CreateSequenceResponse` 程序通过 `ReplyTo` 终结点引用发送的 HTTP 请求传输消息。

### <a name="duplex-addressable-initiator"></a>双工、可寻址的发起方

#### <a name="binding"></a>绑定

WCF 提供了一种完全异步的双向消息交换模式，它在入站和出站 HTTP 通道上使用两个序列。 WCF 使用 HTTP 请求传输所有消息。 所有 HTTP 响应的正文都为空，并且具有 HTTP 202 状态代码。

#### <a name="createsequence-exchange"></a>CreateSequence 交换

WCF 发起方会生成一 `CreateSequence` 条包含产品/服务的消息。 WCF 响应方确保在 `CreateSequence` 创建序列之前具有提议。 WCF 将发送的 `CreateSequenceResponse` HTTP 请求发送到 `CreateSequence` 的 `ReplyTo` 终结点引用。

#### <a name="sequence-lifetime"></a>序列生存期

WCF 将两个序列视为一个全双工会话。

生成出错的错误时，WCF 要求远程终结点对这两个序列进行故障排除。 读取出错一序列的错误时，WCF 会对这两个序列进行故障排除。

WCF 可以关闭其出站序列并继续处理其入站序列上的消息。 相反，WCF 可以处理入站序列的关闭，并继续按其出站序列发送消息。

### <a name="request-reply-non-addressable-initiator"></a>请求-答复、不可寻址的发起方

#### <a name="binding"></a>绑定

WCF 提供在一个 HTTP 通道上使用两个序列的单向和请求-答复消息交换模式。 WCF 使用 HTTP 请求来传输请求序列的消息，并使用 HTTP 响应传输答复序列的消息。

#### <a name="createsequence-exchange"></a>CreateSequence 交换

WCF 发起方会生成一 `CreateSequence` 条包含产品/服务的消息。 WCF 响应方确保在 `CreateSequence` 创建序列之前具有提议。 WCF 响应方 `CreateSequence` 使用消息答复请求 `CreateSequenceResponse` 。

#### <a name="one-way-message"></a>单向消息

若要成功完成单向消息交换协议，WCF 发起方会在 http 请求上传输请求序列消息，并 `SequenceAcknowledgement` 在 http 响应上接收独立的消息。 `SequenceAcknowledgement` 必须确认已传输的消息。

WCF 响应程序可以使用确认、错误或具有空正文和 HTTP 202 状态代码的响应来答复请求。

#### <a name="two-way-messages"></a>双向消息

若要成功完成双向消息交换协议，WCF 发起方会在 http 请求上传输请求序列消息，并在 HTTP 响应上接收答复序列消息。 响应必须包含一个确认已传输的请求序列消息的 `SequenceAcknowledgement`。

WCF 响应程序可以使用应用程序答复、错误或正文为空的响应和 HTTP 202 状态代码来回复请求。

由于存在单向消息和应用程序答复的计时，因此请求序列消息的序列号与响应消息的序列号不相关联。

#### <a name="retrying-replies"></a>重试答复

WCF 依赖 HTTP 请求-答复相关性进行双向消息交换协议关联。 因此，当确认请求序列消息时，WCF 发起方不会停止重试请求序列消息，而是在 HTTP 响应携带确认、用户消息或错误时停止。 WCF 响应方重试答复关联到的请求的 HTTP 请求阶段的答复。

#### <a name="lastmessage-exchange"></a>LastMessage 交换

WCF 发起方在 HTTP 请求腿上生成并传输空的 expression-bodied 最后一条消息。 WCF 需要一个响应，但忽略实际响应消息。 WCF 响应方答复请求序列的 expression-bodied 最后一条消息，该消息具有答复序列的 expression-bodied 最后一条消息。

如果 WCF 响应程序收到操作 URI 不是的最后一条消息 `http://schemas.xmlsoap.org/ws/2005/02/rm/LastMessage` ，则 wcf 将使用最后一条消息进行答复。 在双向消息交换协议中，最后一条消息携带应用程序消息；在单向消息交换协议中，最后一条消息为空。

WCF 响应程序不需要确认答复序列的 expression-bodied 最后一条消息。

#### <a name="terminatesequence-exchange"></a>TerminateSequence 交换

当所有请求都收到有效答复时，WCF 发起方会生成请求序列的消息并将其传输到 `TerminateSequence` HTTP 请求腿。 WCF 需要一个响应，但忽略实际响应消息。 WCF 响应方 `TerminateSequence` 用答复序列的消息答复请求序列的消息 `TerminateSequence` 。

在正常的关闭序列中，两个 `TerminateSequence` 消息都携带一个完整范围的 `SequenceAcknowledgement`。

### <a name="requestreply-addressable-initiator"></a>请求/答复、可寻址的发起方

#### <a name="binding"></a>绑定

WCF 在入站和出站 HTTP 通道上使用两个序列提供请求-答复消息交换模式。 WCF 使用 HTTP 请求传输所有消息。 所有 HTTP 响应的正文都为空，并且具有 HTTP 202 状态代码。

#### <a name="createsequence-exchange"></a>CreateSequence 交换

WCF 发起方会生成一 `CreateSequence` 条包含产品/服务的消息。 WCF 响应方确保在 `CreateSequence` 创建序列之前具有提议。 WCF 将发送的 `CreateSequenceResponse` HTTP 请求发送到 `CreateSequence` 的 `ReplyTo` 终结点引用。

#### <a name="requestreply-correlation"></a>请求/答复关联

WCF 发起方确保所有应用程序请求消息都具有 `MessageId` 和 `ReplyTo` 终结点引用。 WCF 发起方 `CreateSequence` `ReplyTo` 在每个应用程序请求消息上应用消息的终结点引用。 WCF 响应程序要求传入请求消息具有 `MessageId` 和 `ReplyTo` 。 WCF 响应程序确保终结点引用的 `CreateSequence` 和所有应用程序请求消息的 URI 相同。
