---
title: <ws2007FederationHttpBinding>
ms.date: 03/30/2017
ms.assetid: 9af4ec79-cdef-457e-9dca-09d5eb821594
ms.openlocfilehash: fe9ab2e19706a5d295b5916aeb818621d1132c11
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90558766"
---
# \<ws2007FederationHttpBinding>

一个安全且可互操作的绑定，它派生自 [\<wsFederationHttpBinding>](wsfederationhttpbinding.md) 并支持联合安全性。

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<ws2007FederationHttpBinding>**  
  
## <a name="syntax"></a>语法

```xml
<ws2007FederationHttpBinding>
  <binding bypassProxyOnLocal="Boolean"
           closeTimeout="TimeSpan"
           hostNameComparisonMode="StrongWildcard/Exact/WeakWildcard"
           maxBufferPoolSize="integer"
           maxReceivedMessageSize="integer"
           messageEncoding="Text/Mtom"
           name="string"
           openTimeout="TimeSpan"
           privacyNoticeAt="Uri"
           privacyNoticeVersion="Integer"
           proxyAddress="Uri"
           receiveTimeout="TimeSpan"
           sendTimeout="TimeSpan"
           textEncoding="UnicodeFffeTextEncoding/Utf16TextEncoding/Utf8TextEncoding"
           transactionFlow="Boolean"
           useDefaultWebProxy="Boolean">
    <security mode="None/Message/TransportWithMessageCredential">
      <message negotiateServiceCredential="Boolean"
               algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
               issuedTokenType="string"
               issuedKeyType="SymmetricKey/PublicKey">
      </message>
    </security>
    <reliableSession ordered="Boolean"
                     inactivityTimeout="TimeSpan"
                     enabled="Boolean" />
    <readerQuotas maxArrayLength="Integer"
                  maxBytesPerRead="Integer"
                  maxDepth="Integer"
                  maxNameTableCharCount="Integer"
                  maxStringContentLength="Integer" />
  </binding>
</ws2007FederationHttpBinding>
```

## <a name="attributes-and-elements"></a>特性和元素

下列各节描述了特性、子元素和父元素。

### <a name="attributes"></a>特性

|属性|说明|
|---------------|-----------------|
|`bypassProxyOnLocal`|一个值，指示是否对本地地址不使用代理服务器。 默认值为 `false`。|
|`closeTimeout`|一个 <xref:System.TimeSpan> 值，指定为完成关闭操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|
|`hostNameComparisonMode`|指定用于分析 URI 的 HTTP 主机名比较模式。 此属性的类型为 <xref:System.ServiceModel.HostNameComparisonMode>，指示在对 URI 进行匹配时，是否使用主机名来访问服务。 默认值为 <xref:System.ServiceModel.HostNameComparisonMode.StrongWildcard>，表示忽略匹配项中的主机名。|
|`maxBufferPoolSize`|此绑定的最大缓冲池大小。 默认值为 524,288 字节 (512 * 1024)。 Windows Communication Foundation (WCF) 的许多部件使用缓冲区。 每次使用缓冲区时，创建和销毁它们都将占用大量资源，而缓冲区的垃圾回收过程也是如此。 利用缓冲池，可以从缓冲池中获得缓冲区，使用缓冲区，然后在完成工作后将其返回给缓冲池。 这样就避免了创建和销毁缓冲区的系统开销。|
|`maxReceivedMessageSize`|在采用此绑定配置的通道上可以接收的最大消息大小（包括标头），单位为字节。 如果消息超出此限制，则发送方将收到 SOAP 错误。 接收方将删除该消息，并在跟踪日志中创建事件项。 默认值为 65536。|
|`messageEncoding`|定义用于对消息进行编码的编码器。 有效值包括以下值：<br /><br /> -Text：使用文本消息编码器。<br />-Mtom：使用消息传输组织机制 1.0 (MTOM) 编码器。<br /><br /> 默认值为 Text。<br /><br /> 此属性的类型为 <xref:System.ServiceModel.WSMessageEncoding>。|
|`name`|绑定的配置名称。 因为此值用作绑定的标识，所以它应该是唯一的。 从 .NET Framework 4 开始，绑定和行为不需要具有名称。 有关默认配置和无值绑定和行为的详细信息，请参阅[WCF 服务的](../../../wcf/samples/simplified-configuration-for-wcf-services.md)[简化配置](../../../wcf/simplified-configuration.md)和简化配置。|
|`openTimeout`|一个 <xref:System.TimeSpan> 值，指定为完成打开操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|
|`privacyNoticeAt`|隐私声明所在的 URI。|
|`privacyNoticeVersion`|当前隐私声明的版本。|
|`proxyAddress`|一个指定 HTTP 代理的地址的 URI。 如果 `useDefaultWebProxy` 为 `true`，则此设置必须为 `null`。 默认值为 `null`。|
|`receiveTimeout`|一个 <xref:System.TimeSpan> 值，指定为完成接收操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:10:00。|
|`sendTimeout`|一个 <xref:System.TimeSpan> 值，指定为完成发送操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|
|`textEncoding`|设置要用来在绑定上发出消息的字符集编码。 有效值包括以下值：<br /><br /> -BigEndianUnicode： Unicode Big Endian 编码。<br />-Unicode：16位编码。<br />-UTF8：8位编码。<br /><br /> 默认编码为 UTF8。 此属性的类型为 <xref:System.Text.Encoding>。|
|`transactionFlow`|一个值，指定绑定是否支持流动 WS-Transactions。 默认值为 `false`。|
|`useDefaultWebProxy`|一个值，指示是否使用系统的自动配置 HTTP 代理。 如果此属性为 `null`，则代理地址必须为 `true`（即，不设置代理地址）。 默认值为 `true`。|

### <a name="child-elements"></a>子元素

|元素|说明|
|-------------|-----------------|
|[\<security>](security-of-wsfederationhttpbinding.md)|定义消息的安全设置。 此元素的类型为 <xref:System.ServiceModel.Configuration.WSFederationHttpSecurityElement>。|
|[\<readerQuotas>](/previous-versions/dotnet/netframework-4.0/ms731325(v=vs.100))|定义可由采用此绑定配置的终结点进行处理的 SOAP 消息的复杂性约束。 此元素的类型为 <xref:System.ServiceModel.Configuration.XmlDictionaryReaderQuotasElement>。|
|[\<reliableSession>](/previous-versions/ms731375(v=vs.90))|指定是否在通道终结点之间建立可靠会话。|

### <a name="parent-elements"></a>父元素

|元素|说明|
|-------------|-----------------|
|[\<bindings>](bindings.md)|此元素包含标准绑定和自定义绑定的集合。|

## <a name="remarks"></a>备注

联合是一种可以在多个企业或信任域通过共享标识进行身份验证和授权的功能。 它使用 WS-Trust 协议将标识的表示形式从一个信任域映射到另一个信任域。 联合 HTTP 绑定支持 SOAP 安全以及混合模式安全，但不支持传输安全。 配置了此绑定的服务必须使用 HTTP 传输。 有关详细信息，请参阅 [\<wsFederationHttpBinding>](wsfederationhttpbinding.md)。

## <a name="example"></a>示例

```xml
<configuration>
  <system.ServiceModel>
    <bindings>
      <ws2007FederationHttpBinding>
        <binding bypassProxyOnLocal="false"
                 transactionFlow="false"
                 hostNameComparisonMode="WeakWildcard"
                 maxReceivedMessageSize="1000"
                 messageEncoding="Mtom"
                 proxyAddress="http://www.contoso.com"
                 textEncoding="Utf16TextEncoding"
                 useDefaultWebProxy="false">
          <reliableSession ordered="false"
                           inactivityTimeout="00:02:00"
                           enabled="true" />
          <security mode="None">
            <message negotiateServiceCredential="false"
                     algorithmSuite="Aes128"
                     issuedTokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1"
                     issuedKeyType="PublicKey">
              <issuer address="http://localhost/Sts" />
            </message>
          </security>
        </binding>
      </ws2007FederationBinding>
    </bindings>
  </system.ServiceModel>
</configuration>
```

## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.WS2007FederationHttpBinding>
- <xref:System.ServiceModel.Configuration.WS2007FederationHttpBindingElement>
- [\<wsFederationHttpBinding>](wsfederationhttpbinding.md)
- [绑定](../../../wcf/bindings.md)
- [配置系统提供的绑定](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [使用绑定配置服务和客户端](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
