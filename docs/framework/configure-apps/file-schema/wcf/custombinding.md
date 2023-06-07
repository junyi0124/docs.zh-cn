---
title: <customBinding>
ms.date: 03/30/2017
ms.assetid: 9da4f960-f64e-4d8a-894d-2b09eba5ce4b
ms.openlocfilehash: cdaaacf0dfa75209d001f6e8d6ac7175816048aa
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "74140792"
---
# \<customBinding>

提供了对用户消息堆栈的完全控制。

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<customBinding>**  

## <a name="syntax"></a>语法

```xml
<customBinding>
  <binding name="String"
           closeTimeout="TimeSpan"
           openTimeout="TimeSpan"
           receiveTimeout="TimeSpan"
           sendTimeout="TimeSpan">
    <compositeDuplex clientBaseAddress="Uri" />
    <reliableSession acknowledgementInterval="TimeSpan"
                     advancedFlowControl="Boolean"
                     bufferedMessagesQuota="Integer"
                     inactivityTimeout="TimeSpan"
                     maxPendingChannels="Integer"
                     maxRetryCount="Integer"
                     ordered="Boolean" />
    <pnrpPeerResolver />
    <windowsStreamSecurity protectionLevel="None/Sign/EncryptAndSign" />
    <sslStreamSecurity requireClientCertificate="Boolean" />
    <transactionFlow transactionProtocol="OleTransactions/WSAtomicTransactionOctober2004" />
    <security defaultAlgorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
              authenticationMode="UserNameForAnonymous"
              contextMode="Cookie"
              defaultProtectionLevel="Sign"
              enableKeyDerivation="false"
              keyEntropyMode="ClientEntropy"
              messageProtectionOrder="SignBeforeEncryptAndEncryptSignature"
              securityVersion="WSSecurityXXX2005">
      <localClientSettings cacheCookies="false"
                           detectReplays="false"
                           maxCookieCachingTime="00:07:24" />
      <localServiceSettings replayCacheSize="9"
                            maxClockSkew="00:00:03"
                            replayWindow="00:07:22.2190000" />
    </security>
    <binaryMessageEncoding maxReadPoolSize="Integer"
                           maxWritePoolSize="Integer"
                           maxSessionSize="Integer" />
    <httpsTransport manualAddressing="Boolean"
                    maxMessageSize="Integer"
                    authenticationScheme="Negotiate"
                    bypassProxyOnLocal="Boolean"
                    hostNameComparisonMode="Exact"
                    mapAddressingHeadersToHttpHeaders="Boolean"
                    proxyaddress="Uri"
                    realm="String"
                    requireClientCertificate="Boolean" />
    <peerTransport manualAddressing="false"
                   maxMessageSize="20002"
                   listenIPAddress="202.10.1.9"
                   messageAuthentication="false"
                   peerNodeAuthenticationMode="None"
                   port="1000" />
    <security defaultAlgorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
              authenticationMode="UserNameForAnonymous"
              bootstrapBindingConfiguration="String"
              bootstrapBindingSectionName="String"
              defaultProtectionLevel="None/Sign/EncryptAndSign"
              requireDerivedKeys="Boolean"
              securityHeaderLayout="Strict/Lax/LaxTimestampFirst/LaxTimestampLast"
              includeTimestamp="Boolean"
              keyEntropyMode="ClientEntropy/ServerEntropy/CombinedEntropy"
              messageProtectionOrder="SignBeforeEncrypt/SignBeforeEncryptAndEncryptSignature/EncryptBeforeSign"
              protectTokens="Boolean"
              requireSecurityContextCancellation="Boolean"
              securityVersion=" WSSecurityJan2004/WSSecurityXXX2005"
              requireSignatureConfirmation="Boolean">
      <localClientSettings cacheCookies="Boolean"
                           detectReplays="Boolean"
                           replayCacheSize="Integer"
                           maxClockSkew="TimeSpan"
                           maxCookieCachingTime="TimeSpan"
                           replayWindow="TimeSpan"
                           sessionKeyRenewalInterval="TimeSpan"
                           sessionKeyRolloverInterval="TimeSpan"
                           reconnectOnTransportFailure="Boolean"
                           timestampValidityDuration="TimeSpan"
                           cookieRenewalThresholdPercentage="Integer" />
      <localServiceSettings detectReplays="Boolean"
                            issuedCookieLifeTime="TimeSpan"
                            maxStatefulNegotiations="Integer"
                            replayCacheSize="Integer"
                            maxClockSkew="TimeSpan"
                            negotiationTimeout="TimeSpan"
                            replayWindow="TimeSpan"
                            inactivityTimeout="TimeSpan"
                            sessionKeyRenewalInterval="TimeSpan"
                            sessionKeyRolloverInterval="TimeSpan"
                            reconnectOnTransportFailure="Boolean"
                            maxConcurrentSessions="Integer"
                            timestampValidityDuration="TimeSpan" />
      <federationParameters trustVersion="WSTrustApr2004/WSTrustFeb2005" />
    </security>
    <security defaultAlgorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
              authenticationMode="UserNameForAnonymous"
              bootstrapBindingConfiguration="String"
              bootstrapBindingSectionName="String"
              defaultProtectionLevel="None/Sign/EncryptAndSign"
              requireDerivedKeys="Boolean"
              securityHeaderLayout="Strict/Lax/LaxTimestampFirst/LaxTimestampLast"
              includeTimestamp="Boolean"
              keyEntropyMode="ClientEntropy/ServerEntropy/CombinedEntropy"
              messageProtectionOrder="SignBeforeEncrypt/SignBeforeEncryptAndEncryptSignature/EncryptBeforeSign"
              protectTokens="Boolean"
              requireSecurityContextCancellation="Boolean"
              securityVersion=" WSSecurityJan2004/WSSecurityXXX2005"
              requireSignatureConfirmation="Boolean" >
      <localClientSettings cacheCookies="Boolean"
                           detectReplays="Boolean"
                           replayCacheSize="Integer"
                           maxClockSkew="TimeSpan"
                           maxCookieCachingTime="TimeSpan"
                           replayWindow="TimeSpan"
                           sessionKeyRenewalInterval="TimeSpan"
                           sessionKeyRolloverInterval="TimeSpan"
                           reconnectOnTransportFailure="Boolean"
                           timestampValidityDuration="TimeSpan"
                           cookieRenewalThresholdPercentage="Integer" />
      <localServiceSettings detectReplays="Boolean"
                           issuedCookieLifeTime="TimeSpan"
                           maxStatefulNegotiations="Integer"
                           replayCacheSize="Integer"
                           maxClockSkew="TimeSpan"
                           negotiationTimeout="TimeSpan"
                           replayWindow="TimeSpan"
                           inactivityTimeout="TimeSpan"
                           sessionKeyRenewalInterval="TimeSpan"
                           sessionKeyRolloverInterval="TimeSpan"
                           reconnectOnTransportFailure="Boolean"
                           maxConcurrentSessions="Integer"
                           timestampValidityDuration="TimeSpan" />
      <federationParameters trustVersion="WSTrustApr2004/WSTrustFeb2005" />
      <genericIssuedTokenParameters>
        <localIssuerIssuedTokenParameters keyType="SymmetricKey/PublicKey"
                                          keySize="Integer"
                                          tokenType="String" />
        <issuedTokenParametersEndpointAddress address="URI"
                                              bindingConfiguration="String"
                                              binding="String" />
        <issuedTokenClient localIssuerChannelBehaviors="String"
                           cacheIssuedTokens="Boolean"
                           maxIssuedTokenCachingTime="TimeSpan"
                           keyEntropyMode="ClientEntropy/ServerEntropy/CombinedEntropy" />
        <issuedTokenClientBehavior issuerAddress="String"
                                   behaviorConfiguration="String" />
        <issuedTokenClientBehavior address="URI"
                                   bindingConfiguration="String"
                                   binding="String" />
      </genericIssuedTokenParameters>
    </security>
  </binding>
</customBinding>
```

## <a name="attributes-and-elements"></a>特性和元素

以下几节描述了特性、子元素和父元素。

### <a name="attributes"></a>特性

|属性|说明|
|---------------|-----------------|
|closeTimeout|一个 <xref:System.TimeSpan> 值，指定为完成关闭操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|
|NAME|一个包含绑定的配置名称的字符串。 此值是用户定义的一个字符串，可充当自定义绑定的标识字符串。 从 .NET Framework 4 开始，绑定和行为不需要具有名称。 有关默认配置和无值绑定和行为的详细信息，请参阅[WCF 服务的](../../../wcf/samples/simplified-configuration-for-wcf-services.md)[简化配置](../../../wcf/simplified-configuration.md)和简化配置。|
|openTimeout|一个 <xref:System.TimeSpan> 值，指定为完成打开操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|
|receiveTimeout|一个 <xref:System.TimeSpan> 值，指定为完成接收操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|
|sendTimeout|一个 <xref:System.TimeSpan> 值，指定为完成发送操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|

### <a name="child-elements"></a>子元素

|元素|描述|
|-------------|-----------------|
|[\<compositeDuplex>](compositeduplex.md)|对自定义绑定指定双向消息处理。 它与本身不允许进行双工通信的传输（例如，HTTP）一起使用。 与此相反，TCP 本身允许进行双工通信，并且不要求服务在将消息发送回客户端时使用此绑定元素。<br /><br /> 客户端必须公开一个地址，以便服务进行联系和建立连接。 此客户端地址由 `ClientBaseAddress` 属性提供。<br /><br /> 此元素的类型为 <xref:System.ServiceModel.Configuration.CompositeDuplexElement>。|
|[\<pnrpPeerResolver>](pnrppeerresolver.md)|指定对等名称解析协议 (PNRP) 对等名称解析程序。 此元素的类型为 <xref:System.ServiceModel.Configuration.PnrpPeerResolverElement>。|
|[\<reliableSession>](reliablesession.md)|指定 WS-ReliableMessaging 的设置。 如果将该元素添加到自定义绑定，则生成的通道可支持一次性传递保证。 此元素的类型为 <xref:System.ServiceModel.Configuration.ReliableSessionElement>。|
|[\<security>](security-of-custombinding.md)|指定自定义绑定的安全选项。 此元素的类型为 <xref:System.ServiceModel.Configuration.SecurityElement>。|
|[\<sslStreamSecurity>](sslstreamsecurity.md)|指定 SSL 流绑定的安全设置。 此元素的类型为 <xref:System.ServiceModel.Configuration.SslStreamSecurityElement>。|
|[\<transactionFlow>](transactionflow.md)|指定绑定支持事务流以及 `transactionProtocol` 属性将要使用的协议。 此元素的类型为 <xref:System.ServiceModel.Configuration.TransactionFlowElement>。|
|[\<windowsStreamSecurity>](windowsstreamsecurity.md)|指定自定义绑定的流安全选项。 此元素的类型为 <xref:System.ServiceModel.Configuration.WindowsStreamSecurityElement>。|

### <a name="parent-elements"></a>父元素

|元素|描述|
|-------------|-----------------|
|绑定|包含 Windows Communication Foundation 应用程序的所有绑定。|

## <a name="remarks"></a>注解

自定义绑定提供了对 WCF 消息堆栈的完全控制。 通过添加特定实体的配置元素，可以创建特别定制的绑定。 例如，用户可以合并 `httpsTransport` 节、`reliableSession` 节和 `security` 节以创建一个安全可靠的基于 https 的绑定。

单个绑定按照堆栈元素在堆栈上出现的顺序来指定它们的配置元素，从而定义消息堆栈。 每个元素都定义和配置了该堆栈的一个元素。 在每个自定义绑定中，必须有且只能有一个传输元素。 如果没有该元素，消息堆栈将是不完整的。

元素在堆栈中出现的顺序非常重要，因为在将操作应用于消息时会采用该顺序。 建议的堆栈元素顺序如下：

1. 事务（可选）

2. 可靠消息（可选）

3. 安全（可选）

4. Transport

5. 编码器（可选）

当系统提供的某个绑定不符合服务需求时，应使用自定义绑定。 例如，自定义绑定可用于实现在服务终结点使用新的传输或新的编码器。

自定义绑定是使用以特定顺序“堆叠”的绑定元素集合中的某个 <xref:System.ServiceModel.Channels.CustomBinding.%23ctor%2A> 构造的，顺序如下：

- 最顶层是一个允许流事务的可选 <xref:System.ServiceModel.Channels.TransactionFlowBindingElement>。

- 接下来是一个可选的 <xref:System.ServiceModel.Channels.ReliableSessionBindingElement>，它提供了 WS-ReliableMessaging 规范中定义的会话和排序机制。 此会话概念可跨 SOAP 和传输中介。

- 接下来是一个可选的安全绑定元素，它提供了授权、身份验证、保护和机密性之类的安全功能。 Windows Communication Foundation （WCF）提供以下安全绑定元素：

  - <xref:System.ServiceModel.Channels.SecurityBindingElement>

  - <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement>

  - <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement>

  - <xref:System.ServiceModel.Channels.TransportSecurityBindingElement>

- 接着是由下面的绑定元素指定的多个可选消息模式：

- <xref:System.ServiceModel.Channels.CompositeDuplexBindingElement>

- 再下面是以下可选的传输升级/帮助器绑定元素：

  - <xref:System.ServiceModel.Channels.PnrpPeerResolverBindingElement>

  - <xref:System.ServiceModel.Channels.SslStreamSecurityBindingElement>

  - <xref:System.ServiceModel.Channels.WindowsStreamSecurityBindingElement>

- 再接下来是一个必需的消息编码绑定元素。 可以使用自己的传输或者使用以下消息编码绑定之一：

  - <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement>

  - <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement>

  - <xref:System.ServiceModel.Channels.MtomMessageEncodingBindingElement>

- 底层是一个必需的传输元素。 你可以使用自己的传输，或者使用 Windows Communication Foundation （WCF）提供的传输绑定元素之一：

  - <xref:System.ServiceModel.Channels.TcpTransportBindingElement>

  - <xref:System.ServiceModel.Channels.NamedPipeTransportBindingElement>

  - <xref:System.ServiceModel.Channels.HttpTransportBindingElement>

  - <xref:System.ServiceModel.Channels.HttpsTransportBindingElement>

  - <xref:System.ServiceModel.Channels.MsmqTransportBindingElement>

  - <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBindingElement>

  - <xref:System.ServiceModel.Channels.PeerTransportBindingElement>

下表总结了每层的选项。

|层|选项|必选|
|-----------|-------------|--------------|
|事务流|<xref:System.ServiceModel.Channels.TransactionFlowBindingElement>|否|
|可靠性|<xref:System.ServiceModel.Channels.ReliableSessionBindingElement>|否|
|安全性|对称、非对称、传输级|否|
|形状更改|<xref:System.ServiceModel.Channels.CompositeDuplexBindingElement>|否|
|传输升级|SSL 流、Windows 流、对等解析程序|否|
|编码|文本、二进制、MTOM、自定义|是|
|Transport|TCP、命名管道、HTTP、HTTPS、MSMQ 风格、自定义|是|

此外，可以定义自己的绑定元素，并将它们插在前面定义的任何层之间。

有关如何使用自定义绑定来修改系统提供的绑定的讨论，请参阅[如何：自定义系统提供的绑定](../../../wcf/extending/how-to-customize-a-system-provided-binding.md)。

## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Channels.Binding>
- <xref:System.ServiceModel.Channels.BindingElement>
- <xref:System.ServiceModel.Configuration.BindingsSection>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [\<binding>](bindings.md)
- [绑定](../../../wcf/bindings.md)
- [扩展绑定](../../../wcf/extending/extending-bindings.md)
- [自定义绑定](../../../wcf/extending/custom-bindings.md)
- [customBinding 元素](custombinding.md)
- [绑定](../../../wcf/bindings.md)
- [配置系统提供的绑定](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [使用绑定配置服务和客户端](../../../wcf/using-bindings-to-configure-services-and-clients.md)
