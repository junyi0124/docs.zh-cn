---
title: <security> 的 <customBinding>
ms.date: 03/30/2017
ms.assetid: 243a5148-bbd1-447f-a8a5-6e7792c0a3f1
ms.openlocfilehash: eb661864e2540a9b9f5f217402d37515043c862a
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91170111"
---
# <a name="security-of-custombinding"></a>\<security> 的 \<customBinding>

指定自定义绑定的安全选项。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<customBinding>**](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<security>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<security allowSerializedSigningTokenOnReply="Boolean"
          authenticationMode="AuthenticationMode"
          defaultAlgorithmSuite="SecurityAlgorithmSuite"
          includeTimestamp="Boolean"
          requireDerivedKeys="Boolean"
          keyEntropyMode="ClientEntropy/ServerEntropy/CombinedEntropy"
          messageProtectionOrder="SignBeforeEncrypt/SignBeforeEncryptAndEncryptSignature/EncryptBeforeSign"
          messageSecurityVersion="WSSecurityJan2004/WSSecurityXXX2005"
          requireDerivedKeys="Boolean"
          requireSecurityContextCancellation="Boolean"
          requireSignatureConfirmation="Boolean"
          securityHeaderLayout="Strict/Lax/LaxTimestampFirst/LaxTimestampLast">
   <issuedTokenParameters />
   <localClientSettings />
   <localServiceSettings />
   <secureConversationBootstrap />
</security>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 以下几节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|allowSerializedSigningTokenOnReply|可选。 一个布尔值，指定是否可以在答复时使用序列化令牌。 默认值是 `false`。 使用双向绑定时，默认设置为 `true` 并忽略所做的任何设置。|  
|authenticationMode|可选。 指定在发起方和响应方之间使用的身份验证模式。 请参见下面所有的值。<br /><br /> 默认为 `sspiNegotiated`。|  
|defaultAlgorithmSuite|可选。 设置消息加密和密钥包装算法。 算法和密钥大小由 <xref:System.ServiceModel.Security.SecurityAlgorithmSuite> 类确定。 这些算法与“安全策略语言”(WS-SecurityPolicy) 规范中指定的算法一致。<br /><br /> 以下显示了可能的值。 默认值是 `Basic256`。<br /><br /> 此属性与选取不同于默认算法的算法集的其他平台一起使用。 在对此设置进行修改时，应该注意相关算法的优缺点。 此属性的类型为 <xref:System.ServiceModel.Security.SecurityAlgorithmSuite>。|  
|includeTimestamp|一个布尔值，指定是否每个消息都包含时间戳。 默认为 `true`。|  
|keyEntropyMode|指定用于保护消息的密钥的计算方法。 密钥只能基于客户端密钥材料、服务密钥材料或两者的组合。 有效值为<br /><br /> -   `ClientEntropy`：会话密钥基于客户端提供的密钥数据。<br />-   `ServerEntropy`：会话密钥基于服务器提供的密钥数据。<br />-   `CombinedEntropy`：会话密钥基于客户端和服务提供的密钥数据。<br /><br /> 默认为 `CombinedEntropy`。<br /><br /> 此属性的类型为 <xref:System.ServiceModel.Security.SecurityKeyEntropyMode>。|  
|messageProtectionOrder|设置对消息应用消息级安全算法的顺序。 有效值包括以下值：<br /><br /> -   `SignBeforeEncrypt`：先签名，然后加密。<br />-   `SignBeforeEncryptAndEncryptSignature`：先签名，然后加密签名。<br />-   `EncryptBeforeSign`：先加密，然后签名。<br /><br /> 默认值取决于所使用的 WS-Security 版本。 使用 WS-Security 1.1 时，默认值为 `SignBeforeEncryptAndEncryptSignature`。 使用 WS-Security 1.0 时，默认值为 `SignBeforeEncrypt`。<br /><br /> 此属性的类型为 <xref:System.ServiceModel.Security.MessageProtectionOrder>。|  
|messageSecurityVersion|可选。 设置所使用的 WS-Security 的版本。 有效值包括以下值：<br /><br /> - WSSecurity11WSTrustFebruary2005WSSecureConversationFebruary2005WSSecurityPolicy11<br />- WSSecurity10WSTrustFebruary2005WSSecureConversationFebruary2005WSSecurityPolicy11BasicSecurityProfile10<br />- WSSecurity11WSTrustFebruary2005WSSecureConversationFebruary2005WSSecurityPolicy11BasicSecurityProfile10<br /><br /> 默认值是 WSSecurity11WSTrustFebruary2005WSSecureConversationFebruary2005WSSecurityPolicy11，且该默认值在 XML 中可以简单地表示为 `Default`。 此属性的类型为 <xref:System.ServiceModel.MessageSecurityVersion>。|  
|requireDerivedKeys|一个布尔值，指定是否可以从原始校验密钥中派生密钥。 默认为 `true`。|  
|requireSecurityContextCancellation|可选。 一个布尔值，指定当不再需要安全上下文时是否应将其取消和终止。 默认为 `true`。|  
|requireSignatureConfirmation|可选。 一个布尔值，指定是否启用 WS-Security 签名确认。 当设置为 `true` 时，消息签名由响应方进行确认。  为相互证书配置自定义绑定时或将自定义绑定配置为使用所颁发的令牌（WSS 1.1 绑定）时，此特性默认为 `true`。 否则默认值为 `false`。<br /><br /> 签名确认用于确认服务正在完全知晓请求的情况下做出响应。|  
|securityHeaderLayout|可选。 指定安全头中元素的排序。 有效值为<br /><br /> -   `Strict`：根据 "使用之前声明" 的一般原则，将项添加到安全标头中。<br />-   `Lax`：项以任何可确认为 WSS： SOAP 消息安全的顺序添加到安全标头中。<br />-   `LaxWithTimestampFirst`：项以任何符合 WSS： SOAP 消息安全的顺序添加到安全标头中，但安全标头中的第一个元素必须是 wsse： Timestamp 元素。<br />-   `LaxWithTimestampLast`：项以任何符合 WSS： SOAP 消息安全的顺序添加到安全标头中，但安全头中的最后一个元素必须是 wsse： Timestamp 元素。<br /><br /> 默认为 `Strict`。<br /><br /> 此元素的类型为 <xref:System.ServiceModel.Channels.SecurityHeaderLayout>。|  
  
## <a name="authenticationmode-attribute"></a>authenticationMode 特性  
  
|值|说明|  
|-----------|-----------------|  
|字符串|`AnonymousForCertificate`<br /><br /> `AnonymousForSslNegotiated`<br /><br /> `CertificateOverTransport`<br /><br /> `IssuedToken`<br /><br /> `IssuedTokenForCertificate`<br /><br /> `IssuedTokenForSslNegotiated`<br /><br /> `IssuedTokenOverTransport`<br /><br /> `Kerberos`<br /><br /> `KerberosOverTransport`<br /><br /> `MutualCertificate`<br /><br /> `MutualCertificateDuplex`<br /><br /> `MutualSslNegotiated`<br /><br /> `SecureConversation`<br /><br /> `SspiNegotiated`<br /><br /> `UserNameForCertificate`<br /><br /> `UserNameForSslNegotiated`<br /><br /> `UserNameOverTransport`<br /><br /> `SspiNegotiatedOverTransport`|  
  
## <a name="defaultalgorithm-attribute"></a>defaultAlgorithm 特性  
  
|值|描述|  
|-----------|-----------------|  
|Basic128|使用 Aes128 加密，对消息摘要使用 Sha1，对密钥包装使用 Rsa-oaep-mgf1p。|  
|Basic192|使用 Aes192 加密，对消息摘要使用 Sha1，对密钥包装使用 Rsa-oaep-mgf1p。|  
|Basic256|使用 Aes256 加密，对消息摘要使用 Sha1，对密钥包装使用 Rsa-oaep-mgf1p。|  
|Basic256Rsa15|对消息加密使用 Aes256，对消息摘要使用 Sha1，对密钥包装使用 Rsa15。|  
|Basic192Rsa15|对消息加密使用 Aes192，对消息摘要使用 Sha1，对密钥包装使用 Rsa15。|  
|TripleDes|使用 TripleDes 加密，对消息摘要使用 Sha1，对密钥包装使用 Rsa-oaep-mgf1p。|  
|Basic128Rsa15|对消息加密使用 Aes128，对消息摘要使用 Sha1，对密钥包装使用 Rsa15。|  
|TripleDesRsa15|使用 TripleDes 加密，对消息摘要使用 Sha1，对密钥包装使用 Rsa15。|  
|Basic128Sha256|对消息加密使用 Aes256，对消息摘要使用 Sha256，对密钥包装使用 Rsa-oaep-mgf1p。|  
|Basic192Sha256|对消息加密使用 Aes192，对消息摘要使用 Sha256，对密钥包装使用 Rsa-oaep-mgf1p。|  
|Basic256Sha256|对消息加密使用 Aes256，对消息摘要使用 Sha256，对密钥包装使用 Rsa-oaep-mgf1p。|  
|TripleDesSha256|对消息加密使用 TripleDes，对消息摘要使用 Sha256，对密钥包装使用 Rsa-oaep-mgf1p。|  
|Basic128Sha256Rsa15|对消息加密使用 Aes128，对消息摘要使用 Sha256，对密钥包装使用 Rsa15。|  
|Basic192Sha256Rsa15|对消息加密使用 Aes192，对消息摘要使用 Sha256，对密钥包装使用 Rsa15。|  
|Basic256Sha256Rsa15|对消息加密使用 Aes256，对消息摘要使用 Sha256，对密钥包装使用 Rsa15。|  
|TripleDesSha256Rsa15|对消息加密使用 TripleDes，对消息摘要使用 Sha256，对密钥包装使用 Rsa15。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<issuedTokenParameters>](issuedtokenparameters.md)|指定一个当前颁发的令牌。 此元素的类型为 <xref:System.ServiceModel.Configuration.IssuedTokenParametersElement>。|  
|[\<localClientSettings>](localclientsettings-element.md)|指定此绑定的本地客户端安全设置。 此元素的类型为 <xref:System.ServiceModel.Configuration.LocalClientSecuritySettingsElement>。|  
|[\<localServiceSettings>](localservicesettings-element.md)|指定此绑定的本地服务安全设置。 此元素的类型为 <xref:System.ServiceModel.Configuration.LocalServiceSecuritySettingsElement>。|  
|[\<secureConversationBootstrap>](secureconversationbootstrap.md)|指定用于启动安全对话服务的默认值。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<binding>](bindings.md)|定义自定义绑定的所有绑定功能。|  
  
## <a name="remarks"></a>备注  

 有关使用此元素的详细信息，请参阅 [SecurityBindingElement Authentication 模式](../../../wcf/feature-details/securitybindingelement-authentication-modes.md) 和 [如何：使用 SecurityBindingElement 创建自定义绑定](../../../wcf/feature-details/how-to-create-a-custom-binding-using-the-securitybindingelement.md)。  
  
## <a name="example"></a>示例  

 下面的示例演示如何使用自定义绑定配置安全性。 并演示如何使用自定义绑定实现消息级安全性和安全传输。 如果在客户端和服务之间传输消息时需要进行安全的传输，同时消息必须在消息级别上保持安全，这非常有用。 系统提供的绑定不支持此配置。  
  
 服务配置定义了一个自定义绑定，该绑定支持使用 TLS/SSL 协议和 Windows 消息安全性保护的 TCP 通信。 此自定义绑定使用服务证书在传输级别对服务进行身份验证，并且在客户端与服务之间传输消息时保护消息。 这是通过 [\<sslStreamSecurity>](sslstreamsecurity.md) 绑定元素实现的。 服务的证书是使用服务行为配置的。  
  
 此外，此自定义绑定将消息安全性与 Windows 凭据类型（默认凭据类型）一起使用。 这是由 [安全](security-of-custombinding.md) 绑定元素实现的。 如果 Kerberos 身份验证机制可用，则将使用消息级安全性对客户端和服务进行身份验证。 如果 Kerberos 身份验证机制不可用，则使用 NTLM 身份验证。 NTLM 向服务对客户端进行身份验证，但不向客户端对服务进行身份验证。 [安全](security-of-custombinding.md)绑定元素配置为使用 `SecureConversation` authenticationType，这将导致在客户端和服务上创建安全会话。 为了使服务的双工协定起作用，需要这么做。 有关运行此示例的详细信息，请参阅 [自定义绑定安全性](../../../wcf/samples/custom-binding-security.md)。  
  
```xml  
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceModel.Samples.CalculatorService"
               behaviorConfiguration="CalculatorServiceBehavior">
        <host>
          <baseAddresses>
            <!-- use following base address -->
            <add baseAddress="net.tcp://localhost:8000/ServiceModelSamples/Service"/>
          </baseAddresses>
        </host>
        <endpoint address=""
                  binding="customBinding"
                  bindingConfiguration="Binding1"
                  contract="Microsoft.ServiceModel.Samples.ICalculatorDuplex" />
        <!-- the mex endpoint is exposed at net.tcp://localhost:8000/ServiceModelSamples/service/mex -->
        <endpoint address="mex"
                  binding="mexTcpBinding"
                  contract="IMetadataExchange" />
      </service>
    </services>
    <bindings>
      <!-- configure a custom binding -->
      <customBinding>
        <binding name="Binding1">
          <security authenticationMode="SecureConversation"
                    requireSecurityContextCancellation="true">
          </security>
          <textMessageEncoding messageVersion="Soap12WSAddressing10"
                               writeEncoding="utf-8" />
          <sslStreamSecurity requireClientCertificate="false" />
          <tcpTransport />
        </binding>
      </customBinding>
    </bindings>
    <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->
    <behaviors>
      <serviceBehaviors>
        <behavior name="CalculatorServiceBehavior">
          <serviceMetadata />
          <serviceDebug includeExceptionDetailInFaults="False" />
          <serviceCredentials>
            <serviceCertificate findValue="localhost"
                                storeLocation="LocalMachine"
                                storeName="My"
                                x509FindType="FindBySubjectName" />
          </serviceCredentials>
        </behavior>
      </serviceBehaviors>
    </behaviors>
  </system.serviceModel>
</configuration>
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.SecurityElement>
- <xref:System.ServiceModel.Channels.SecurityBindingElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [绑定](../../../wcf/bindings.md)
- [扩展绑定](../../../wcf/extending/extending-bindings.md)
- [自定义绑定](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
- [如何：使用 SecurityBindingElement 创建自定义绑定](../../../wcf/feature-details/how-to-create-a-custom-binding-using-the-securitybindingelement.md)
- [自定义绑定安全性](../../../wcf/samples/custom-binding-security.md)
