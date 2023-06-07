---
title: 可靠消息传送协议版本 1.1
ms.date: 03/30/2017
ms.assetid: 0da47b82-f8eb-42da-8bfe-e56ce7ba6f59
ms.openlocfilehash: ad0a77842c10965749eab4e76bb123938e07e9d5
ms.sourcegitcommit: ee5b798427f81237a3c23d1fd81fff7fdc21e8d3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84144716"
---
# <a name="reliable-messaging-protocol-version-11"></a>可靠消息传送协议版本 1.1

本主题介绍了使用 HTTP 传输进行互操作所需的 WS-RELIABLEMESSAGING 2007 （版本1.1）协议的 Windows Communication Foundation （WCF）实现的详细信息。 WCF 遵循了 WS-RELIABLEMESSAGING 规范以及本主题中所述的约束和说明。 请注意，从 .NET Framework 3.5 开始实现 WS-RELIABLEMESSAGING 版本1.1 协议。

Ws-reliablemessaging 2007 年2月协议是在 WCF 中由实现的 <xref:System.ServiceModel.Channels.ReliableSessionBindingElement> 。

为方便起见，本主题使用以下角色：

- 发起方：启动 WS-Reliable Message 序列创建的客户端。

- 响应方：接收发起方请求的服务。

 本文档使用下表中的前缀和命名空间。

|前缀|命名空间|
|-|-|
|wsrm|`http://docs.oasis-open.org/ws-rx/wsrm/200702`|
|netrm|`http://schemas.microsoft.com/ws/2006/05/rm`|
|s|`http://www.w3.org/2003/05/soap-envelope`|
|wsa|`http://schemas.xmlsoap.org/ws/2005/08/addressing`|
|wsse|`http://docs.oasis-open.org/wss/2004/01/oasis-200401-wssecurity-secext-1.0.xsd`|
|wsrmp|`http://docs.oasis-open.org/ws-rx/wsrmp/200702`|
|netrmp|`http://schemas.microsoft.com/ws-rx/wsrmp/200702`|
|wsp|（WS-Policy 1.2 或 WS-Policy 1.5）|

## <a name="messaging"></a>消息传递

### <a name="sequence-creation"></a>序列创建

WCF 实现 `CreateSequence` 和 `CreateSequenceResponse` 消息以建立可靠的消息传送顺序。 适用以下约束：

- B1101： WCF 发起方使用与 `CreateSequence` 消息的、和相同的终结点引用 `ReplyTo` `AcksTo` `Offer/Endpoint` 。

- R1102：`AcksTo` 消息中的 `ReplyTo`、`Offer/Endpoint` 和 `CreateSequence` 终结点引用必须具有包含相同字符串表示的地址值，以便它们与八进制形式匹配。

  - 在创建序列之前，WCF 响应程序将验证的 URI 部分 `AcksTo` `ReplyTo` 和 `Endpoint` 终结点引用是否相同。

- R1103：`AcksTo` 消息中的 `ReplyTo`、`Offer/Endpoint` 和 `CreateSequence` 终结点引用应该具有一组相同的引用参数。

  - WCF 不强制，但假设上的引用参数 `AcksTo` `ReplyTo` 和 `Offer/Endpoint` 上的终结点引用相同， `CreateSequence` 并使用来自 `ReplyTo` 终结点引用的引用参数进行确认和反向序列消息。

- B1104： WCF 发起方不会 `Expires` 在消息中生成可选的或 `Offer/Expires` 元素 `CreateSequence` 。

- B1105：访问消息时 `CreateSequence` ，WCF 响应程序使用 `Expires` 元素中的值 `CreateSequence` 作为 `Expires` 元素中的值 `CreateSequenceResponse` 。 否则，WCF 响应程序读取并忽略 `Expires` 和 `Offer/Expires` 值。

- B1106：访问消息时 `CreateSequenceResponse` ，WCF 发起方读取可选 `Expires` 值但不使用该值。

- B1107： WCF 发起方和响应方始终 `IncompleteSequenceBehavior` 在和元素中生成可选的元素 `CreateSequence/Offer` `CreateSequenceResponse` 。

- B1108： WCF 只使用 `DiscardFollowingFirstGap` `NoDiscard` 元素中的和值 `IncompleteSequenceBehavior` 。

  - WS-ReliableMessaging 利用 `Offer` 机制建立构成会话的两个相反的相关序列。

- B1109：如果 `CreateSequence` 包含一个 `Offer` 元素，则通过使用不带元素的进行响应，WCF 响应程序会拒绝所提供的序列 `CreateSequenceResponse` `Accept` 。

- B1110：如果可靠消息响应程序拒绝所提供的序列，WCF 发起方会错误地创建新的序列。

- B1111：如果不 `CreateSequence` 包含 `Offer` 元素，则双向 WCF 响应程序会通过响应错误拒绝所提供的序列 `CreateSequenceRefused` 。

- R1112：使用 `Offer` 机制建立两个相反序列时，`[address]` 终结点引用的 `CreateSequenceResponse/Accept/AcksTo` 属性必须与 `CreateSequence` 消息的目标 URI 逐字节匹配。

- R1113：使用 `Offer` 机制建立两个相反序列时，从发起方流向响应方的两个序列上的所有消息都必须发送到同一终结点引用。

WCF 使用 ws-reliablemessaging 在发起方和响应方之间建立可靠会话。 WCF Ws-reliablemessaging 实现为单向、请求-答复和全双工消息传递模式提供可靠的会话。 `Offer` 和 `CreateSequence` 上的 WS-ReliableMessaging `CreateSequenceResponse` 机制允许您建立两个相关的相反序列，并提供适合于所有消息终结点的会话协议。 因为 WCF 为此类会话提供安全保证，包括会话完整性的端到端保护，所以确保用于同一参与方的消息到达同一目标是不切实际的。 这还允许应用程序消息上序列确认的“非法携带”。 因此，约束 R1102、R1112 和 R1113 适用于 WCF。

`CreateSequence` 消息的一个示例。

```xml
<s:Envelope>
  <s:Header>
    <wsa:Action s:mustUnderstand="1">http://docs.oasis-open.org/ws-rx/wsrm/200702/CreateSequence</wsa:Action>
    <wsa:MessageID>urn:uuid:949cca61-8813-42ff-ab33-18d9e3fa82fa</wsa:MessageID>
    <wsa:ReplyTo>
        <wsa:Address>http://Business456.com/clientA</wsa:Address>
    </wsa:ReplyTo>
    <wsa:To s:mustUnderstand="1">http://BusinessABC.com/serviceA</wsa:To>
  </s:Header>
  <s:Body>
    <wsrm:CreateSequence>
      <wsrm:AcksTo>
        <wsa:Address>http://Business456.com/clientA</wsa:Address>
      </wsrm:AcksTo>
      <wsrm:Offer>
        <wsrm:Identifier>urn:uuid:066b4730-fc82-458a-a5c1-210be4fb4e4e</wsrm:Identifier>
        <wsrm:Endpoint>
          <wsa:Address>http://Business456.com/clientA</wsa:Address>
        </wsrm:Endpoint>
        <wsrm:IncompleteSequenceBehavior>DiscardFollowingFirstGap</wsrm:IncompleteSequenceBehavior>
      </wsrm:Offer>
    </wsrm:CreateSequence>
  </s:Body>
</s:Envelope>
```

`CreateSequenceResponse` 消息的一个示例。

```xml
<s:Envelope>
  <s:Header>
    <wsa:Action s:mustUnderstand="1">http://docs.oasis-open.org/ws-rx/wsrm/200702/CreateSequenceResponse</wsa:Action>
    <wsa:RelatesTo>urn:uuid:949cca61-8813-42ff-ab33-18d9e3fa82fa</wsa:RelatesTo>
    <wsa:To s:mustUnderstand="1">http://Business456.com/clientA</wsa:To>
  </s:Header>
  <s:Body>
    <wsrm:CreateSequenceResponse>
      <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
      <wsrm:IncompleteSequenceBehavior>DiscardFollowingFirstGap</wsrm:IncompleteSequenceBehavior>
      <wsrm:Accept>
        <wsrm:AcksTo>
          <wsa:Address>http://BusinessABC.com/serviceA</wsa:Address>
        </wsrm:AcksTo>
      </wsrm:Accept>
    </wsrm:CreateSequenceResponse>
  </s:Body>
</s:Envelope>
```

### <a name="closing-a-sequence"></a>关闭序列

WCF 将 `CloseSequence` 和 `CloseSequenceResponse` 消息用于可靠消息源启动的关闭。 WCF 可靠消息传送目标不会启动关闭，并且 WCF 可靠消息源不支持可靠消息目标启动的关闭。 适用以下约束：

- B1201： WCF 可靠消息源始终发送一 `CloseSequence` 条消息以关闭序列。

- B1202：可靠消息源在发送 `CloseSequence` 消息之前，等待确认全部的序列消息。

- B1203：除非序列不包含消息，否则可靠消息源始终包括可选的 `LastMsgNumber` 元素。

- R1204：可靠消息目标不得通过发送 `CloseSequence` 消息启动关闭。

- B1205：收到消息后 `CloseSequence` ，WCF 可靠消息源会将序列视为不完整并发送错误。

 `CloseSequence` 消息的一个示例。

```xml
<s:Envelope>
  <s:Header>
    <wsa:Action s:mustUnderstand="1">http://docs.oasis-open.org/ws-rx/wsrm/200702/CloseSequence</wsa:Action>
    <wsa:MessageID>urn:uuid:6ce1d4c3-e1c1-474f-a8c9-4210e37f7877</wsa:MessageID>
    <wsa:ReplyTo>
      <wsa:Address>http://Business456.com/clientA</wsa:Address>
    </wsa:ReplyTo>
    <wsa:To s:mustUnderstand="1">http://BusinessABC.com/serviceA</wsa:To>
  </s:Header>
  <s:Body>
    <wsrm:CloseSequence>
      <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
      <wsrm:LastMsgNumber>30</wsrm:LastMsgNumber>
    </wsrm:CloseSequence>
  </s:Body>
</s:Envelope>
```

示例 `CloseSequenceResponse` 消息：

```xml
<s:Envelope>
  <s:Header>
    <wsrm:SequenceAcknowledgement>
      <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
      <wsrm:AcknowledgementRange Lower="1" Upper="30"></wsrm:AcknowledgementRange>
      <wsrm:Final></wsrm:Final>
      <netrm:BufferRemaining>8</netrm:BufferRemaining>
    </wsrm:SequenceAcknowledgement>
    <wsa:Action s:mustUnderstand="1">http://docs.oasis-open.org/ws-rx/wsrm/200702/CloseSequenceResponse</wsa:Action>
    <wsa:RelatesTo>urn:uuid:6ce1d4c3-e1c1-474f-a8c9-4210e37f7877</wsa:RelatesTo>
    <wsa:To s:mustUnderstand="1">http://Business456.com/clientA</wsa:To>
  </s:Header>
  <s:Body>
    <wsrm:CloseSequenceResponse>
      <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
    </wsrm:CloseSequenceResponse>
  </s:Body>
</s:Envelope>
```

### <a name="sequence-termination"></a>序列终止

WCF 主要在 `TerminateSequence/TerminateSequenceResponse` 完成握手后使用握手 `CloseSequence/CloseSequenceResponse` 。 WCF 可靠消息传送目标不启动终止，可靠消息源不支持可靠消息目标启动的终止。 适用以下约束：

- B1301： WCF 发起方仅在 `TerminateSequence` 成功完成握手后发送消息 `CloseSequence/CloseSequenceResponse` 。

- R1302： WCF 验证元素在 `LastMsgNumber` 给定序列的所有和消息中是否一致 `CloseSequence` `TerminateSequence` 。 这意味着 `LastMsgNumber` 并不存在于所有 `CloseSequence` 和 `TerminateSequence` 消息上，或者它存在于所有 `CloseSequence` 和 `TerminateSequence` 消息上且完全相同。

- B1303：在 `TerminateSequence` 握手后收到 `CloseSequence/CloseSequenceResponse` 消息时，可靠消息目标通过 `TerminateSequenceResponse` 消息进行响应。 由于可靠消息源在发送 `TerminateSequence` 消息之前具有最终确认，因此可靠消息目标可毫无疑问地知道序列将结束，并立即回收资源。

- B1304：在 `TerminateSequence` 握手之前接收消息时 `CloseSequence/CloseSequenceResponse` ，WCF 可靠消息目标将使用消息进行响应 `TerminateSequenceResponse` 。 如果可靠消息目标确定序列中没有不一致之处，则可靠消息目标将在回收资源之前等待应用程序目标指定的时间，以便客户端有机会接收最终确认。 否则，可靠消息目标立即回收资源，并通过引发 `Faulted` 事件向应用程序目标指出序列结束但存有疑问。

`TerminateSequence` 消息的一个示例。

```xml
<s:Envelope>
  <s:Header>
    <wsa:Action s:mustUnderstand="1">http://docs.oasis-open.org/ws-rx/wsrm/200702/TerminateSequence</wsa:Action>
    <wsa:MessageID>urn:uuid:3597a398-4f3c-40f4-9335-8f1515572fdf</wsa:MessageID>
    <wsa:ReplyTo>
      <wsa:Address>http://Business456.com/clientA</wsa:Address>
    </wsa:ReplyTo>
    <wsa:To s:mustUnderstand="1">http://BusinessABC.com/serviceA</wsa:To>
  </s:Header>
  <s:Body>
    <wsrm:TerminateSequence>
      <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
      <wsrm:LastMsgNumber>30</wsrm:LastMsgNumber>
      </wsrm:TerminateSequence>
  </s:Body>
</s:Envelope>
```

示例 `TerminateSequenceResponse` 消息：

```xml
<s:Envelope>
  <s:Header>
    <wsrm:SequenceAcknowledgement>
      <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
      <wsrm:AcknowledgementRange Lower="1" Upper="30"></wsrm:AcknowledgementRange>
      <wsrm:Final></wsrm:Final>
      <netrm:BufferRemaining>8</netrm:BufferRemaining>
    </wsrm:SequenceAcknowledgement>
    <wsa:Action s:mustUnderstand="1">http://docs.oasis-open.org/ws-rx/wsrm/200702/TerminateSequenceResponse</wsa:Action>
    <wsa:RelatesTo>urn:uuid:3597a398-4f3c-40f4-9335-8f1515572fdf</wsa:RelatesTo>
    <wsa:To s:mustUnderstand="1">://Business456.com/clientA</wsa:To>
  </s:Header>
  <s:Body>
    <wsrm:TerminateSequenceResponse>
      <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
    </wsrm:TerminateSequenceResponse>
  </s:Body>
</s:Envelope>
```

### <a name="sequences"></a>序列

以下是适用于序列的约束的列表：

- B1401： WCF 生成并访问的序列号 `xs:long` 的最大非独占值9223372036854775807不超过最大值。

`Sequence` 标头的一个示例。

```xml
<wsrm:Sequence s:mustUnderstand="1">
  <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
  <wsrm:MessageNumber>1</wsrm:MessageNumber>
</wsrm:Sequence>
```

### <a name="request-acknowledgement"></a>请求确认

WCF 使用 `AckRequested` 标头作为 keep-alive 机制。

`AckRequested` 标头的一个示例。

```xml
<wsrm:AckRequested>
  <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
</wsrm:AckRequested>
```

### <a name="sequenceacknowledgement"></a>SequenceAcknowledgement

WCF 将 "非法携带" 机制用于在 WS-TRUST 消息中提供的序列确认。 适用以下约束：

- R1601：使用机制建立两个反向序列时 `Offer` ， `SequenceAcknowledgement` 标头可能包含在传输给目标接收方的任何应用程序消息中。 远程终结点必须能够访问非法携带的 `SequenceAcknowledgement` 标头。

- B1602： WCF 不生成 `SequenceAcknowledgement` 包含元素的标头 `Nack` 。 WCF 将验证每个 `Nack` 元素是否包含序列号，否则将忽略 `Nack` 元素和值。

 `SequenceAcknowledgement` 标头的一个示例。

```xml
<wsrm:SequenceAcknowledgement>
  <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
  <wsrm:AcknowledgementRange Lower="1" Upper="1"></wsrm:AcknowledgementRange>
</wsrm:SequenceAcknowledgement>
```

### <a name="ws-reliablemessaging-faults"></a>WS-ReliableMessaging 错误

下面是适用于 WS-RELIABLEMESSAGING 错误的 WCF 实现的约束列表。 适用以下约束：

- B1701： WCF 不会生成 `MessageNumberRollover` 错误。

- B1702：通过 SOAP 1.2，当服务终结点达到其连接限制并且无法处理新连接时，WCF 将生成嵌套的 `CreateSequenceRefused` 错误子代码， `netrm:ConnectionLimitReached` 如下面的示例中所示。

```xml
<s:Envelope>
  <s:Header>
    <wsa:Action>http://docs.oasis-open.org/ws-rx/wsrm/200702/fault</wsa:Action>
  </s:Header>
  <s:Body>
    <s:Fault>
      <s:Code>
        <s:Value>s:Receiver</s:Value>
        <s:Subcode>
          <s:Value>wsrm:CreateSequenceRefused</s:Value>
          <s:Subcode>
            <s:Value>netrm:ConnectionLimitReached</s:Value>
          </s:Subcode>
        </s:Subcode>
      </s:Code>
      <s:Reason>
        <s:Text xml:lang="en">Server 'http://BusinessABC.com/serviceA' is too busy to process this request. Try again later.</s:Text>
      </s:Reason>
    </s:Fault>
  </s:Body>
</s:Envelope>
```

### <a name="ws-addressing-faults"></a>WS-Addressing 错误

由于 ws-reliablemessaging 使用 WS-ADDRESSING，因此 WCF ws-reliablemessaging 实现可以生成和传输 WS-ADDRESSING 错误。 本部分介绍 WCF 在 WS-RELIABLEMESSAGING 层显式生成和传输的 WS-ADDRESSING 错误：

- B1801：当满足以下任一条件时，WCF 将生成并传输 `Message Addressing Header Required` 错误：

  - `CreateSequence``CloseSequence` 或 `TerminateSequence` 消息缺少 `MessageId` 头。

  - `CreateSequence``CloseSequence` 或 `TerminateSequence` 消息缺少 `ReplyTo` 头。

  - `CreateSequenceResponse`、`CloseSequenceResponse` 或 `TerminateSequenceResponse` 消息缺少 `RelatesTo` 头。

- B1802： WCF 生成并传输 `Endpoint Unavailable` 错误，以指示没有终结点侦听，可以根据消息中的寻址标头检查来处理序列 `CreateSequence` 。

## <a name="protocol-composition"></a>协议组合

### <a name="composition-with-ws-addressing"></a>与 WS-Addressing 组合

WCF 支持两个版本的 WS-ADDRESSING： WS-ADDRESSING 2004/08 [WS-ADDRESSING] 和 W3C WS 寻址1.0 建议 [WS-ATOMICTRANSACTION] 和 [WS-ADDRESSING-SOAP]。

尽管 WS-ReliableMessaging 规范仅提及 WS-Addressing 2004/08，但是它不限制要使用的 WS-Addressing 版本。 下面列出了适用于 WCF 的约束：

- R2101：WS-Addressing 2004/08 和 WS-Addressing 1.0 都可以与 WS-Reliable Messaging 一起使用。

- R2102：在整个的给定 WS-ReliableMessaging 序列或通过使用 `Offer` 机制关联的一对相反序列中，必须使用 WS-Addressing 的单个版本。

### <a name="composition-with-soap"></a>与 SOAP 组合

WCF 支持将 SOAP 1.1 和 SOAP 1.2 同时用于 WS-TRUST 消息。

### <a name="composition-with-ws-security-and-ws-secureconversation"></a>与 WS-Security 和 WS-SecureConversation 组合

WCF 通过使用安全传输（HTTPS）、使用 WS-MANAGEMENT 组合和结合 WS 安全会话来为 ws-reliablemessaging 序列提供保护。 WS-ReliableMessaging 1.1 协议、WS-Security 1.1 和 WS-Secure Conversation 1.3 协议应该一起使用。 下面列出了适用于 WCF 的约束：

- R2301：若要保护 WS-RELIABLEMESSAGING 序列的完整性，以及单个消息的完整性和保密性，WCF 需要使用 WS 安全对话。

- R2302：必须在建立 WS-ReliableMessaging 序列之前建立 WS-Secure Conversation 会话。

- R2303：如果 WS-ReliableMessaging 序列生存期超过了 WS-Secure Conversation 会话的生存期，则必须通过使用对应的 WS-Secure Conversation 续订绑定来续订通过使用 WS-Secure Conversation 建立的 `SecurityContextToken`。

- B2304：WS-ReliableMessaging 序列或一对相关的相反序列始终绑定到单个 WS-SecureConversation 会话。

- R2305：用 WS 安全会话撰写时，WCF 响应程序要求 `CreateSequence` 消息包含 `wsse:SecurityTokenReference` 元素和 `wsrm:UsesSequenceSTR` 标头。

 `UsesSequenceSTR` 标头的一个示例。

```xml
<wsrm:UsesSequenceSTR></wsrm:UsesSequenceSTR>
```

### <a name="composition-with-ssltls-sessions"></a>与 SSL/TLS 会话组合

WCF 不支持与 SSL/TLS 会话组合：

- B2401： WCF 不生成 `wsrm:UsesSequenceSSL` 标头。

- R2402：可靠消息发起方不得将 `CreateSequence` 带有标头的消息发送 `wsrm:UsesSequenceSSL` 到 WCF 响应方。

### <a name="composition-with-ws-policy"></a>与 WS-Policy 组合

WCF 支持两个版本的 WS 策略： WS 策略1.2 和 WS 策略1.5。

## <a name="ws-reliablemessaging-ws-policy-assertion"></a>WS-ReliableMessaging WS-Policy 断言

WCF 使用 ws-reliablemessaging WS 策略断言 `wsrm:RMAssertion` 来描述终结点功能。 下面列出了适用于 WCF 的约束：

- B3001： WCF `wsrmn:RMAssertion` 将 WS 策略断言附加到 `wsdl:binding` 元素。 WCF 同时支持 `wsdl:binding` 和元素的附件 `wsdl:port` 。

- B3002： WCF 从不生成 `wsp:Optional` 标记。

- B3003：在访问 `wsrmp:RMAssertion` WS 策略断言时，WCF 将忽略 `wsp:Optional` 标记并将 ws-rm 策略视为强制。

- R3004：因为 WCF 不与 SSL/TLS 会话组合，WCF 不接受指定的策略 `wsrmp:SequenceTransportSecurity` 。

- B3005： WCF 始终生成 `wsrmp:DeliveryAssurance` 元素。

- B3006： WCF 始终指定 `wsrmp:ExactlyOnce` 传送保证。

- B3007： WCF 生成并读取 WS-RELIABLEMESSAGING 断言的以下属性，并在 WCF 上提供对这些属性的控制 `ReliableSessionBindingElement` ：

  - `netrmp:InactivityTimeout`

  - `netrmp:AcknowledgementInterval`

  `RMAssertion` 的一个示例。

  ```xml
  <wsrmp:RMAssertion>
    <wsp:Policy>
      <wsrmp:SequenceSTR/>
      <wsrmp:DeliveryAssurance>
        <wsp:Policy>
          <wsrmp:ExactlyOnce/>
          <wsrmp:InOrder/>
        </wsp:Policy>
      </wsrmp:DeliveryAssurance>
    </wsp:Policy>
    <netrmp:InactivityTimeout Milliseconds="600000"/>
    <netrmp:AcknowledgementInterval Milliseconds="200"/>
  </wsrmp:RMAssertion>
  ```

## <a name="flow-control-ws-reliablemessaging-extension"></a>流控制 WS-ReliableMessaging 扩展

WCF 使用 WS-RELIABLEMESSAGING 扩展性提供对序列消息流的其他更严格的控制。

通过将属性设置为，启用流控制 <xref:System.ServiceModel.Channels.ReliableSessionBindingElement.FlowControlEnabled?displayProperty=nameWithType> `true` 。 下面列出了适用于 WCF 的约束：

- B4001：启用可靠消息流控制时，WCF 会 `netrm:BufferRemaining` 在标头的元素扩展性中生成元素 `SequenceAcknowledgement` ，如下面的示例中所示。

  ```xml
  <wsrm:SequenceAcknowledgement>
    <wsrm:Identifier>urn:uuid:656652b8-9af2-4e94-9d07-2dc21c05ed27</wsrm:Identifier>
    <wsrm:AcknowledgementRange Upper="1" Lower="1"/>
    <netrm:BufferRemaining>8</netrm:BufferRemaining>
  </wsrm:SequenceAcknowledgement>
  ```

- B4002：即使在启用可靠消息流控制时，WCF 也不需要 `netrm:BufferRemaining` 标头中的 `SequenceAcknowledgement` 元素。

- B4003： WCF 可靠消息传送目标用 `netrm:BufferRemaining` 来指示它可以缓冲多少条新消息。

- B4004：启用可靠消息流控制时，WCF 可靠消息源将使用的值 `netrm:BufferRemaining` 来阻止消息传输。

- B4005： WCF 生成 `netrm:BufferRemaining` 0 到4096之间的整数值，并读取0到0之间的整数值 `xs:int` `maxInclusive` 214748364。

## <a name="message-exchange-patterns"></a>消息交换模式

本节介绍当 WS-RELIABLEMESSAGING 用于不同消息交换模式时，WCF 的行为。 对于每个消息交换模式，可以考虑下面的两个部署方案：

- 不可寻址的发起方：发起方位于防火墙背后；响应方只能在 HTTP 响应上将消息传递到发起方。

- 可寻址的发起方：发起方和响应方都可以发送 HTTP 请求；换句话说，可以建立两个相反的 HTTP 连接。

### <a name="one-way-non-addressable-initiator"></a>单向、不可寻址的发起方

#### <a name="binding"></a>绑定

WCF 通过一个 HTTP 通道使用一个序列来提供单向消息交换模式。 WCF 使用 HTTP 请求将所有消息从发起方传输到响应方，并使用 HTTP 响应将所有消息从响应方传输到发起方。

#### <a name="createsequence-exchange"></a>CreateSequence 交换

WCF 发起方 `CreateSequence` `Offer` 在 http 请求上传输没有元素的消息，并在 `CreateSequenceResponse` http 响应中预期消息。 WCF 响应程序创建一个序列，并 `CreateSequenceResponse` `Accept` 在 HTTP 响应上传输没有元素的消息。

#### <a name="sequenceacknowledgement"></a>SequenceAcknowledgement

WCF 发起方处理除 `CreateSequence` 消息和错误消息之外的所有消息的回复确认。 WCF 响应方始终将对 HTTP 响应的独立确认传输到所有序列和 `AckRequested` 消息。

#### <a name="closesequence-exchange"></a>CloseSequence 交换

WCF 发起方在 `CloseSequence` http 请求上传输消息，并在 `CreateSequenceResponse` http 响应上期望消息。 WCF 响应程序 `CloseSequenceResponse` 在 HTTP 响应上传输消息。

#### <a name="terminatesequence-exchange"></a>TerminateSequence 交换

WCF 发起方在 `TerminateSequence` http 请求上传输消息，并在 `TerminateSequenceResponse` http 响应上期望消息。 WCF 响应程序 `TerminateSequenceResponse` 在 HTTP 响应上传输消息。

### <a name="one-way-addressable-initiator"></a>单向、可寻址的发起方

#### <a name="binding"></a>绑定

WCF 通过一个入站和一个出站 HTTP 通道使用一个序列来提供单向消息交换模式。 WCF 使用 HTTP 请求传输所有消息。 所有 HTTP 响应的正文都为空，并且具有 HTTP 202 状态代码。

#### <a name="createsequence-exchange"></a>CreateSequence 交换

WCF 发起方 `CreateSequence` `Offer` 在 HTTP 请求上传输没有元素的消息。 WCF 响应程序创建一个序列，并 `CreateSequenceResponse` `Accept` 在 HTTP 请求上传输没有元素的消息。

### <a name="duplex-addressable-initiator"></a>双工、可寻址的发起方

#### <a name="binding"></a>绑定

WCF 通过一个入站和一个出站 HTTP 通道提供使用两个序列的完全异步的双向消息交换模式。 此消息交换模式可以与 `Request/Reply`、`Addressable` 发起方消息交换模式以有限方式混合使用。 WCF 使用 HTTP 请求传输所有消息。 所有 HTTP 响应的正文都为空，并且具有 HTTP 202 状态代码。

#### <a name="createsequence-exchange"></a>CreateSequence 交换

WCF 发起方在 `CreateSequence` `Offer` HTTP 请求上传输具有元素的消息。 WCF 响应程序确保 `CreateSequence` 具有 `Offer` 元素，然后创建一个序列，并 `CreateSequenceResponse` 使用一个元素传输消息 `Accept` 。

#### <a name="sequence-lifetime"></a>序列生存期

WCF 将两个序列视为一个全双工会话。

生成出错的错误时，WCF 要求远程终结点对这两个序列进行故障排除。 读取出错一序列的错误时，WCF 会对这两个序列进行故障排除。

WCF 可以关闭其出站序列并继续处理其入站序列上的消息。 相反，WCF 可以处理入站序列的关闭，并继续按其出站序列发送消息。

### <a name="request-reply-and-one-way-non-addressable-initiator"></a>请求-答复和单向、不可寻址的发起方

#### <a name="binding"></a>绑定

WCF 提供在一个 HTTP 通道上使用两个序列的单向和请求-答复消息交换模式。 WCF 使用 HTTP 请求将所有消息从发起方传输到响应方，并使用 HTTP 响应将所有消息从响应方传输到发起方。

#### <a name="createsequence-exchange"></a>CreateSequence 交换

WCF 发起方在 `CreateSequence` `Offer` http 请求上传输具有元素的消息，并 `CreateSequenceResponse` 在 http 响应中预期消息。 WCF 响应方创建一个序列，并在 `CreateSequenceResponse` HTTP 响应中使用一个元素传输消息 `Accept` 。

#### <a name="one-way-message"></a>单向消息

若要成功完成单向消息交换，WCF 发起方会在 HTTP 请求上传输请求序列消息，并 `SequenceAcknowledgement` 在 http 响应上接收独立的消息。 `SequenceAcknowledgement` 必须确认已传输的消息。

WCF 响应方可以使用确认、错误或具有空正文和 HTTP 202 状态代码的响应来答复请求。

#### <a name="two-way-messages"></a>双向消息

若要成功完成双向消息交换协议，WCF 发起方会在 http 请求上传输请求序列消息，并在 HTTP 响应上接收答复序列消息。 响应必须包含一个确认已传输的请求序列消息的 `SequenceAcknowledgement`。

WCF 响应方可以使用应用程序答复、错误或正文为空的响应和 HTTP 202 状态代码来回复请求。

由于存在单向消息和应用程序答复的计时，因此请求序列消息的序列号与响应消息的序列号不相关联。

#### <a name="retrying-replies"></a>重试答复

WCF 依赖 HTTP 请求-答复相关性进行双向消息交换协议关联。 因此，当确认请求序列消息时，WCF 发起方不会停止重试请求序列消息，而是在 HTTP 响应携带 `SequenceAcknowledgement` 、应用程序答复或错误时停止。 WCF 响应方在答复关联到的请求的 HTTP 响应上重试响应。

#### <a name="closesequence-exchange"></a>CloseSequence 交换

接收所有单向请求序列消息的所有答复序列消息和确认之后，WCF 发起程序会针对 `CloseSequence` http 请求传输请求序列的消息，并在 `CloseSequenceResponse` http 响应中使用。

关闭请求序列将隐式关闭答复序列。 这意味着 WCF 发起方在消息上包括答复序列的最终 `SequenceAcknowledgement` `CloseSequence` ，且答复序列没有 `CloseSequence` 交换。

WCF 响应程序确保所有答复都得到确认，并 `CloseSequenceResponse` 在 HTTP 响应中传输消息。

#### <a name="terminatesequence-exchange"></a>TerminateSequence 交换

接收到 `CloseSequenceResponse` 消息后，WCF 发起程序会针对 `TerminateSequence` http 请求传输请求序列的消息，并在 `TerminateSequenceResponse` http 响应中使用。

与 `CloseSequence` 交换一样，终止请求序列将隐式终止答复序列。 这意味着 WCF 发起方在消息上包括答复序列的最终 `SequenceAcknowledgement` `TerminateSequence` ，且答复序列没有 `TerminateSequence` 交换。

WCF 响应程序 `TerminateSequenceResponse` 在 HTTP 响应上传输消息。

### <a name="requestreply-addressable-initiator"></a>请求/答复、可寻址的发起方

#### <a name="binding"></a>绑定

WCF 通过一个入站和一个出站 HTTP 通道使用两个序列提供请求-答复消息交换模式。 此消息交换模式可以与 `Duplex, Addressable` 发起方消息交换模式以有限方式混合使用。 WCF 使用 HTTP 请求传输所有消息。 所有 HTTP 响应的正文都为空，并且具有 HTTP 202 状态代码。

#### <a name="createsequence-exchange"></a>CreateSequence 交换

WCF 发起方在 `CreateSequence` `Offer` HTTP 请求上传输具有元素的消息。 WCF 响应方确保 `CreateSequence` 具有 `Offer` 元素，然后创建一个序列，并 `CreateSequenceResponse` 使用一个元素传输消息 `Accept` 。

#### <a name="requestreply-correlation"></a>请求/答复关联

以下内容适用于所有关联的请求和答复：

- WCF 确保所有应用程序请求消息都具有 `ReplyTo` 终结点引用和 `MessageId` 。

- WCF 将本地终结点引用应用于每个应用程序请求消息的 `ReplyTo` 。 本地终结点引用是发起方的 `CreateSequence` 消息的 `ReplyTo` 和响应方的 `CreateSequence` 消息的 `To`。

- WCF 可确保传入的请求消息具有 `MessageId` 和 `ReplyTo` 。

- WCF 确保 `ReplyTo` 所有应用程序请求消息的终结点引用的 URI 与前面定义的本地终结点引用匹配。

- WCF 确保所有答复都在 `RelatesTo` `To` `wsa` 请求/答复相关规则后具有正确的标头和标头。
