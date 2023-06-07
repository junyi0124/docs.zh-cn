---
title: 安全协议
ms.date: 03/30/2017
helpviewer_keywords:
- security [WCF], protocols
ms.assetid: 57ffcbea-807c-4e43-a41c-44b3db8ed2af
ms.openlocfilehash: 1455aeeeb759f8eb2cc09c8649a5cbd6843d950a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96254007"
---
# <a name="security-protocols"></a>安全协议

Web 服务安全协议提供 Web 服务安全机制，这些机制可满足所有现有企业的消息传递安全要求。 本部分介绍 (WCF) 详细信息 (在 <xref:System.ServiceModel.Channels.SecurityBindingElement>) 中为以下 Web 服务安全协议实现的 Windows Communication Foundation。  
  
|规范/文档|链接|  
|-|-|  
|WSS：SOAP 消息安全 1.0|<http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-soap-message-security-1.0.pdf>|  
|WSS：用户名令牌配置文件 1.0|<http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-username-token-profile-1.0.pdf>|  
|WSS：X509 令牌配置文件 1.0|<http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-x509-token-profile-1.0.pdf>|  
|WSS：SAML 1.1 令牌配置文件 1.0|<http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.0.pdf>|  
|WSS：SOAP 消息安全 1.1|<http://www.oasis-open.org/committees/download.php/16790/wss-v1.1-spec-os-SOAPMessageSecurity.pdf>|  
|WSS 用户名令牌配置文件 1.1|<http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-username-token-profile-1.0.pdf>|  
|WSS：X.509 令牌配置文件 1.1|<http://www.oasis-open.org/committees/download.php/16785/wss-v1.1-spec-os-x509TokenProfile.pdf>|  
|WSS：Kerberos 令牌配置文件 1.1|<http://www.oasis-open.org/committees/download.php/16788/wss-v1.1-spec-os-KerberosTokenProfile.pdf>|  
|WSS：SAML 1.1 令牌配置文件 1.1|<http://www.oasis-open.org/committees/download.php/16768/wss-v1.1-spec-os-SAMLTokenProfile.pdf>|  
|WS-Secure Conversation 1.3|<http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512/ws-secureconversation-1.3-os.pdf>|  
|WS-Trust 1.3|<http://docs.oasis-open.org/ws-sx/ws-trust/200512/ws-trust-1.3-os.pdf>|  
|应用说明：<br /><br /> 将 WS-Trust 用于 TLS 握手|即将发布|  
|应用说明：<br /><br /> 将 WS-Trust 用于 SPNEGO|即将发布|  
|应用说明：<br /><br /> Web 服务寻址终结点引用和标识|即将发布|  
|WS-SecurityPolicy 1.2 (2007/04)|<http://www.oasis-open.org/committees/download.php/23821/ws-securitypolicy-1.2-spec-cs.pdf>|  
  
 WCF，版本1提供17个身份验证模式，可用作 Web 服务安全配置的基础。 每一种模式都针对一组常用部署需求进行了优化，如：  
  
- 用于对客户端和服务进行身份验证的凭据。  
  
- 消息或传输安全保护机制。  
  
- 消息交换模式。  
  
|身份验证模式|客户端身份验证|服务器身份验证|“模式”|  
|-------------------------|---------------------------|---------------------------|----------|  
|UserNameOverTransport|用户名/密码|X509|Transport|  
|CertificateOverTransport|X509|X509|Transport|  
|KerberosOverTransport|Windows|X509|Transport|  
|IssuedTokenOverTransport|联合|X509|Transport|  
|SspiNegotiatedOverTransport|协商的 Windows Sspi|协商的 Windows Sspi|Transport|  
|AnonymousForCertificate|无|X509|消息|  
|UserNameForCertificate|用户名/密码|X509|消息|  
|MutualCertificate|X509|X509|消息|  
|MutualCertificateDuplex|X509|X509|消息|  
|IssuedTokenForCertificate|联合|X509|消息|  
|Kerberos|Windows|Windows|消息|  
|IssuedToken|联合|联合|消息|  
|SspiNegotiated|协商的 Windows Sspi|协商的 Windows Sspi|消息|  
|AnonymousForSslNegotiated|无|X509、TLS-Nego|消息|  
|UserNameForSslNegotiated|用户名/密码|X509、TLS-Nego|消息|  
|MutualSslNegotiated|X509|X509、TLS-Nego|消息|  
|IssuedTokenForSslNegotiated|联合|X509、TLS-Nego|消息|  
  
 使用此类身份验证模式的终结点可以使用 WS-SecurityPolicy (WS-SP) 表示其安全要求。 本文档介绍每种身份验证模式的安全标头和基础结构消息的结构，并提供策略和消息的示例。  
  
 WCF 利用 WS-SecureConversation 提供安全会话支持来保护应用程序之间的多消息交换。  请参见下面的“安全会话”了解实现细节。  
  
 除了身份验证模式之外，WCF 还提供一些设置，用于控制适用于大多数基于消息安全的身份验证模式的常见保护机制，例如：签名和加密操作的顺序、算法套件、密钥派生和签名确认。  
  
 本文档使用以下前缀和命名空间。  
  
|前缀|命名空间|  
|------------|---------------|  
|s|`http://www.w3.org/2003/05/soap-envelope`|  
|sp|`http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702`|  
|a|`http://www.w3.org/2005/08/addressing`|  
|wsse|TBD – OASIS WSS 1.0 URI|  
|wsse11|TBD – OASIS WSS 1.1 URI|  
|wsu|`http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd`|  
|ds|TBD – W3C XMLDSig URI|  
|wst|TBD – WS-Trust 2005/02 URI|  
|wssc|TBD – WS-SecureConversation 2005/02 URI|  
|wsaw|`http://www.w3.org/2006/05/addressing/wsdl`|  
|wsp|`http://schemas.xmlsoap.org/ws/2004/09/policy`|  
|mssp|`http://schemas.microsoft.com/ws/2005/07/securitypolicy`|  
  
## <a name="1-token-profiles"></a>1. 令牌配置文件  

 Web 服务安全规范将凭据表示为安全令牌。 WCF 支持以下令牌类型：  
  
### <a name="11-usernametoken"></a>1.1 UsernameToken  

 WCF 遵循 UsernameToken10 和 UsernameToken11 配置文件，其约束如下：  
  
 R1101 UsernameToken\Password 元素的 PasswordType 属性必须省略或者值为 #PasswordText（默认值）。  
  
 可以使用可扩展性实现 #PasswordDigest。 人们已经发现，#PasswordDigest 经常被误认为是足够安全的密码保护机制。 但实际上，#PasswordDigest 不可取代 UsernameToken 加密。 #PasswordDigest 的主要目的是防止重放攻击。 在 WCF 身份验证模式下，通过使用消息签名缓解重播攻击威胁。  
  
 B1102 WCF 从不发出 Nonce 并创建 UsernameToken 的子元素。  
  
 这些子元素旨在帮助重放检测。 WCF 使用消息签名。  
  
 OASIS WSS SOAP Message Security UsernameToken Profile 1.1 (UsernameToken11) 引入了从密码派生密钥的功能。  
  
 B1103 UsernameToken 密码不得用于密钥派生，因此也不得用于加密操作。  
  
 根本原因：密码通常被视为过于脆弱，不适合用于加密操作。  
  
### <a name="12-x509-token"></a>1.2 X509 令牌  

 WCF 支持 X509v3 证书作为凭据类型，并遵循 X509tokenprofile1.1 1.0 和 X509tokenprofile1.1 1.1 的限制：  
  
 R1201 在包含 X509v3 证书时，BinarySecurityToken 元素的 ValueType 属性必须值为 #X509v3。  
  
 WSS X509 Token Profile 1.0 和 1.1 还定义了 #X509PKIPathv1 和 #PKCS7 作为值类型。 WCF 不支持这些类型。  
  
 R1202 如果 SubjectKeyIdentifier (SKI) 扩展在 X509 证书中存在，wsse:KeyIdentifier 应该用于对该令牌的外部引用，并且 ValueType 属性为 #X509SubjectKeyIdentifier 且其内容为证书的 SKI 扩展的 base64 编码值。  
  
 SKI 引用已广泛实现，已证明是高度可互操作的外部引用类型。  
  
 R1203 对 X509 安全令牌的外部引用不应使用 ds:X509IssuerSerial。  
  
 R1204 如果使用 X509TokenProfile1.1，则对 X509 安全令牌的外部引用应该使用 WS-Security 1.1 引入的指纹。  
  
 WCF 支持 X509IssuerSerial。 但是，X509IssuerSerial 存在互操作性问题： WCF 使用字符串来比较 X509IssuerSerial 的两个值。 因此，如果一次重命名使用者名称的组件并将其发送到 WCF 服务，则可能找不到该证书。  
  
### <a name="13-kerberos-token"></a>1.3 Kerberos 令牌  

 为了实现 Windows 身份验证，WCF 支持 KerberosTokenProfile 1.1，但具有以下限制：  
  
 R1301 Kerberos 令牌必须携带 GSS_API 和 Kerberos 规范中定义的 GSS 包装的 Kerberos v4 AP_REQ 的值，并且必须有值为 #GSS_Kerberosv5_AP_REQ 的 ValueType 属性。  
  
 WCF 使用 GSS 包装的 Kerberos AP 要求，而不是要求空的 AP 要求。 这是一种安全最佳做法。  
  
### <a name="14-saml-v11-token"></a>1.4 SAML v1.1 令牌  

 对于 SAML v1.1 令牌，WCF 支持 WSS SAML 令牌配置文件1.0 和1.1。 可以实现其他版本的 SAML 令牌格式。  
  
### <a name="15-security-context-token"></a>1.5 安全上下文令牌  

 WCF 支持 Ws-secureconversation 中引入 (SCT) 安全上下文令牌。 SCT 用于表示在 SecureConversation 中建立的安全上下文以及下面所述的二进制协商协议 TLS 和 SSPI。  
  
## <a name="2-common-message-security-parameters"></a>2. 公用消息安全参数  
  
### <a name="21-timestamp"></a>2.1 时间戳  

 时间戳存在与否是使用 <xref:System.ServiceModel.Channels.SecurityBindingElement.IncludeTimestamp%2A> 类的 <xref:System.ServiceModel.Channels.SecurityBindingElement> 属性控制的。 WCF 始终序列化 wsse： TimeStamp with wsse：已创建和 wsse： Expires 字段。 使用签名时，总会对 wsse:TimeStamp 进行签名。  
  
### <a name="22-protection-order"></a>2.2 保护顺序  

 WCF 支持消息保护顺序 "加密前签名" 和 "签名前加密" (安全策略 1.2) 。 建议使用“加密前签名”，其原因包括：除非使用 WS-Security 1.1 SignatureConfirmation 机制，否则使用“签名前加密”进行保护的消息易受签名替换攻击，并且对加密内容进行签名使得审核更加困难。  
  
### <a name="23-signature-protection"></a>2.3 签名保护  

 在使用“签名前加密”时，建议保护签名以防止对加密内容或签名密钥进行猜测的蛮力攻击（尤其是在自定义令牌与弱密钥材料一起使用时）。  
  
### <a name="24-algorithm-suite"></a>2.4 算法组  

 WCF 支持安全策略1.2 中列出的所有算法套件。  
  
### <a name="25-key-derivation"></a>2.5 密钥派生  

 WCF 使用 "对称密钥的密钥派生"，如 Ws-secureconversation 中所述。  
  
### <a name="26-signature-confirmation"></a>2.6 签名确认  

 签名确认可用于防止中间人攻击以保护签名集。  
  
### <a name="27-security-header-layout"></a>2.7 安全标头布局  

 每种身份验证模式都描述一种特定的安全标头布局。 安全标头内的元素为半有序。 为了定义安全标头子元素的顺序，WS-Security Policy 定义了以下安全标头布局模式：  
  
|||  
|-|-|  
|严格|根据“使用前先声明”的一般原则，各项按照 Security Policy 第 7.7.1 节中所述的编号布局规则添加到安全标头中。|  
|Lax|各项以任何符合“WSS: SOAP 消息安全”的顺序添加到安全标头中。|  
|LaxTimestampFirst|与 Lax 相同，只是安全标头中的第一项必须为 wsse:Timestamp|  
|LaxTimestampLast|与 lax 相同，只是安全标头中的最后一项必须为 wsse:Timestamp|  
  
 WCF 支持安全标头布局的所有四种模式。 以下针对身份验证模式的安全标头结构和消息示例遵循“Strict”模式。  
  
## <a name="3-common-message-security-parameters"></a>3. 常见消息安全参数  

 本节介绍每种身份验证模式的示例策略以及演示客户端和服务所交换的消息中的安全标头结构的示例。  
  
### <a name="31-transport-protection"></a>3.1 传输保护  

 WCF 提供五种使用安全传输来保护消息的身份验证模式。UserNameOverTransport、CertificateOverTransport、KerberosOverTransport、IssuedTokenOverTransport 和 SspiNegotiatedOverTransport。  
  
 这些身份验证模式是使用 SecurityPolicy 中描述的传输绑定构造的。 对于 UserNameOverTransport 身份验证模式，UsernameToken 是签名支持令牌。 对于其他身份验证模式，令牌作为签名认可令牌出现。 SecurityPolicy 的附录 C.1.2 和 C.1.3 详细介绍了安全标头布局。 下面的示例安全标头演示给定身份验证模式的 Strict 布局。  
  
 任何情况下，令牌的“Derived Keys”属性的值总是为“false”。  
  
 传输绑定的各个属性的值如下：  
  
 时间戳：true  
  
 安全标头布局：Strict  
  
 算法组：Basic256  
  
#### <a name="311-usernameovertransport"></a>3.1.1 UsernameOverTransport  

 在此身份验证模式下，客户端使用“用户名令牌”进行身份验证，该令牌作为签名支持令牌（总是从发起方发送到接收方）出现在 SOAP 层上。 在传输层，服务是用 X.509 证书进行身份验证的。 所用绑定为传输绑定。  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="UserNameOverTransport_policy"><wsp:ExactlyOne><wsp:All><sp:TransportBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:TransportToken><wsp:Policy><sp:HttpsToken/></wsp:Policy></sp:TransportToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/></wsp:Policy></sp:TransportBinding><sp:SignedEncryptedSupportingTokens xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:UsernameToken sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient"><wsp:Policy><sp:WssUsernameToken10/></wsp:Policy></sp:UsernameToken></wsp:Policy></sp:SignedEncryptedSupportingTokens><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
 安全标头布局  
  
 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="_0"> ... </u:Timestamp><o:UsernameToken u:Id="uuid-b96fbb3a-e646-4403-9473-2e5ffc733ff8-1"> ... </o:UsernameToken></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="_0"> ... </u:Timestamp></o:Security>  
```  
  
#### <a name="312-certificateovertransport"></a>3.1.2 CertificateOverTransport  

 在此身份验证模式下，客户端使用 X.509 证书进行身份验证，该证书作为认可支持令牌（总是从发起方发送到接收方）出现在 SOAP 层上。 在传输层，服务是用 X.509 证书进行身份验证的。 所用绑定为传输绑定。 CertificateOverTransport 仅对 SOAP 标头而非 SOAP 正文进行签名。 这是由 TransportWithMessageCredentials 安全模式使用的身份验证模式。 仅对 SOAP 标头进行签名，因为身份验证是通过使用消息凭据进行的。  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="CertificateOverTransport_policy"><wsp:ExactlyOne><wsp:All><sp:TransportBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:TransportToken><wsp:Policy><sp:HttpsToken/></wsp:Policy></sp:TransportToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/></wsp:Policy></sp:TransportBinding><sp:EndorsingSupportingTokens xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:X509Token sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient"><wsp:Policy><sp:RequireThumbprintReference/><sp:WssX509V3Token10/></wsp:Policy></sp:X509Token><sp:SignedParts><sp:Header Name="To" Namespace="http://www.w3.org/2005/08/addressing"/></sp:SignedParts></wsp:Policy></sp:EndorsingSupportingTokens><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
 安全标头布局  
  
 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="_0"> ... </u:Timestamp><o:BinarySecurityToken> ... </o:BinarySecurityToken><Signature xmlns="http://www.w3.org/2000/09/xmldsig#"> ... </Signature></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="_0"> ... </u:Timestamp></o:Security>  
```  
  
#### <a name="313-issuedtokenovertransport"></a>3.1.3 IssuedTokenOverTransport  

 在此身份验证模式下，客户端不向服务进行身份验证，而是出示一个由安全令牌服务 (STS) 颁发的令牌，并证明掌握了共享密钥。 颁发的令牌作为认可支持令牌（总是从发起方发送到接收方）出现在 SOAP 层上。 在传输层，服务是用 X.509 证书进行身份验证的。 绑定为传输绑定。  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="IssuedTokenOverTransport_policy">
 <wsp:ExactlyOne>
  <wsp:All>
   <sp:TransportBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702">
    <wsp:Policy>
     <sp:TransportToken>
      <wsp:Policy>
       <sp:HttpsToken />
      </wsp:Policy>
     </sp:TransportToken>
     <sp:AlgorithmSuite>
      <wsp:Policy>
       <sp:Basic256 />
      </wsp:Policy>
     </sp:AlgorithmSuite>
     <sp:Layout>
      <wsp:Policy>
       <sp:Strict/>
      </wsp:Policy>
     </sp:Layout>
     <sp:IncludeTimestamp/>
    </wsp:Policy>
   </sp:TransportBinding>
   <sp:EndorsingSupportingTokens xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702">
    <wsp:Policy>
     <sp:IssuedToken sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient">
      <Issuer xmlns="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702">
       <Address xmlns="http://www.w3.org/2005/08/addressing">http://www.w3.org/2005/08/addressing/anonymous</Address>
       <Metadata xmlns="http://www.w3.org/2005/08/addressing">
        <Metadata xmlns="http://schemas.xmlsoap.org/ws/2004/09/mex" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
         <wsx:MetadataSection xmlns="">
          <wsx:MetadataReference>
           <Address xmlns="http://www.w3.org/2005/08/addressing"> ... </Address>
           <Identity xmlns="http://schemas.xmlsoap.org/ws/2006/02/addressingidentity">
            <Dns> ...  </Dns>
           </Identity>
          </wsx:MetadataReference>
         </wsx:MetadataSection>
        </Metadata>
       </Metadata>
      </Issuer>
      <sp:RequestSecurityTokenTemplate>
       <trust:KeyType xmlns:trust="http://docs.oasis-open.org/ws-sx/ws-trust/200512">http://docs.oasis-open.org/ws-sx/ws-trust/200512/SymmetricKey</trust:KeyType>
      </sp:RequestSecurityTokenTemplate>
      <wsp:Policy>
       <sp:RequireInternalReference/>
      </wsp:Policy>
     </sp:IssuedToken>
     <sp:SignedParts>
      <sp:Header Name="To" Namespace="http://www.w3.org/2005/08/addressing"/>
     </sp:SignedParts>
    </wsp:Policy>
   </sp:EndorsingSupportingTokens>
   <sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702">
    <wsp:Policy>
     <sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/>
     <sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/>
    </wsp:Policy>
   </sp:Wss11>
   <sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702">
    <wsp:Policy>
     <sp:MustSupportIssuedTokens/>
     <sp:RequireClientEntropy/>
     <sp:RequireServerEntropy/>
    </wsp:Policy>
   </sp:Trust13>
   <wsaw:UsingAddressing/>
  </wsp:All>
 </wsp:ExactlyOne>
</wsp:Policy>
```  
  
 安全标头布局  
  
 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-67692bb6-85b7-4299-8587-3ce60086b0d2-5"> ... </u:Timestamp><c:SecurityContextToken u:Id="uuid-fab7e1b2-8dc4-412b-bda9-b95a4f836815-16" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:SecurityContextToken><c:DerivedKeyToken u:Id="_0" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><c:DerivedKeyToken u:Id="_1" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_8" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-fab7e1b2-8dc4-412b-bda9-b95a4f836815-21"> ... </u:Timestamp><c:DerivedKeyToken u:Id="_0" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><c:DerivedKeyToken u:Id="_1" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_6" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
#### <a name="314-kerberosovertransport"></a>3.1.4 KerberosOverTransport  

 在此身份验证模式下，客户端使用 Kerberos 票证向服务进行身份验证。 Kerberos 令牌作为认可支持令牌出现在 SOAP 层上。 在传输层，服务是用 X.509 证书进行身份验证的。 绑定为传输绑定。  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="KerberosOverTransport_policy"><wsp:ExactlyOne><wsp:All><sp:TransportBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:TransportToken><wsp:Policy><sp:HttpsToken/></wsp:Policy></sp:TransportToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic128/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/></wsp:Policy></sp:TransportBinding><sp:EndorsingSupportingTokens xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:KerberosToken sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/Once"><wsp:Policy><sp:WssGssKerberosV5ApReqToken11/></wsp:Policy></sp:KerberosToken><sp:SignedParts><sp:Header Name="To" Namespace="http://www.w3.org/2005/08/addressing"/></sp:SignedParts></wsp:Policy></sp:EndorsingSupportingTokens><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
 安全标头布局  
  
 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="_0"> ... </u:Timestamp><o:BinarySecurityToken> ... </o:BinarySecurityToken><Signature xmlns="http://www.w3.org/2000/09/xmldsig#"> ... </Signature></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="_0"> ... </u:Timestamp></o:Security>  
```  
  
#### <a name="315-sspinegotiatedovertransport"></a>3.1.5 SspiNegotiatedOverTransport  

 在此模式下，将使用协商协议来执行客户端和服务器身份验证。 如果可能，就使用 Kerberos，否则使用 NTLM。 产生的 SCT 作为认可支持令牌（总是从发起方发送到接收方）出现在 SOAP 层上。 在传输层，服务还是由 X.509 证书另外进行身份验证。 所用绑定为传输绑定。 "SPNEGO" (协商) 介绍 WCF 如何将 SSPI 二进制协商协议用于 WS-TRUST。 在通过 SPNEGO 握手建立 SCT 之后，本节将介绍安全标头示例。  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="SspiNegotiatedOverTransport_policy"><wsp:ExactlyOne><wsp:All><sp:TransportBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:TransportToken><wsp:Policy><sp:HttpsToken/></wsp:Policy></sp:TransportToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/></wsp:Policy></sp:TransportBinding><sp:EndorsingSupportingTokens xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:SpnegoContextToken sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient"><wsp:Policy><sp:MustNotSendCancel/><sp:MustNotSendAmend/><sp:MustNotSendRenew/></wsp:Policy></sp:SpnegoContextToken><sp:SignedParts><sp:Header Name="To" Namespace="http://www.w3.org/2005/08/addressing"/></sp:SignedParts></wsp:Policy></sp:EndorsingSupportingTokens><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
### <a name="security-header-examples"></a>安全标头示例  

 在使用 WS-Trust 二进制协商通过 SPNEGO 握手建立安全上下文令牌之后，应用程序消息将具有如下结构的安全标头。  
  
 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="_0"> ... </u:Timestamp><sc:SecurityContextToken u:Id="uuid-9a29d087-5dae-4d40-bf86-5746d9d30eca-1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:SecurityContextToken><Signature xmlns="http://www.w3.org/2000/09/xmldsig#"> ... </Signature></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="_0"> ... </u:Timestamp></o:Security>  
```  
  
### <a name="32-using-x509-certificates-for-service-authentication"></a>3.2 使用 x.509 证书进行服务身份验证  

 本节介绍以下身份验证模式：MutualCertificate WSS1.0、Mutual CertificateDuplex、MutualCertificate WSS1.1、AnonymousForCertificate、UserNameForCertificate 和 IssuedTokenForCertificate。  
  
#### <a name="321-mutualcertificate-wss10"></a>3.2.1 MutualCertificate WSS1.0  

 在此身份验证模式下，客户端使用 X.509 证书进行身份验证，该证书作为发起方令牌出现在 SOAP 层上。 同样使用 X.509 证书对服务进行身份验证。 同时对 SOAP 标头和 SOAP 正文进行签名。 创建对称密钥并使用收件人的传输证书对该密钥进行加密。  
  
 所用绑定为带有以下属性值的非对称绑定：  
  
 发起方令牌：客户端的 X.509 证书，包含模式设置为 .../IncludeToken/AlwaysToRecipient  
  
 接收方令牌：服务器的 X.509 证书，包含模式设置为 .../IncludeToken/Never  
  
 令牌保护：False  
  
 整个标头和正文签名：True  
  
 保护顺序：SignBeforeEncrypt  
  
 加密签名：True  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="MutualCertificate_policy"><wsp:ExactlyOne><wsp:All><sp:AsymmetricBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:InitiatorToken><wsp:Policy><sp:X509Token sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient"><wsp:Policy><sp:WssX509V3Token10/></wsp:Policy></sp:X509Token></wsp:Policy></sp:InitiatorToken><sp:RecipientToken><wsp:Policy><sp:X509Token sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/Never"><wsp:Policy><sp:WssX509V3Token10/></wsp:Policy></sp:X509Token></wsp:Policy></sp:RecipientToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptSignature/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:AsymmetricBinding><sp:Wss10 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/></wsp:Policy></sp:Wss10><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
### <a name="security-header-examples-signbeforeencrypt-encryptsignature"></a>安全标头示例：SignBeforeEncrypt、EncryptSignature  

 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-39cb393e-9da8-4d5d-b273-540ef614569b-1"> ... </u:Timestamp><o:BinarySecurityToken> ... </o:BinarySecurityToken><e:EncryptedKey Id="_0" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedKey><e:EncryptedData Id="_7" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"> <u:Timestamp u:Id="uuid-3d742930-70d3-4d7e-aa0a-8721128dc115-8"> ... </u:Timestamp><e:EncryptedKey Id="_0" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedKey><e:EncryptedData Id="_5" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="MutualCertificate_policy"><wsp:ExactlyOne><wsp:All><sp:AsymmetricBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:InitiatorToken><wsp:Policy><sp:X509Token sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient"><wsp:Policy><sp:WssX509V3Token10/></wsp:Policy></sp:X509Token></wsp:Policy></sp:InitiatorToken><sp:RecipientToken><wsp:Policy><sp:X509Token sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/Never"><wsp:Policy><sp:WssX509V3Token10/></wsp:Policy></sp:X509Token></wsp:Policy></sp:RecipientToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptBeforeSigning/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:AsymmetricBinding><sp:Wss10 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/></wsp:Policy></sp:Wss10><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
### <a name="security-header-examples-encryptbeforesign"></a>安全标头示例：EncryptBeforeSign  

 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-73da3e21-abff-4294-a910-e75303d280cc-1"> ... </u:Timestamp><o:BinarySecurityToken> ... </o:BinarySecurityToken><e:EncryptedKey Id="_0" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedKey><Signature xmlns="http://www.w3.org/2000/09/xmldsig#"> ... </Signature><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-02f276b6-804f-480d-99e9-2e90fc76ab27-3"> ... </u:Timestamp><e:EncryptedKey Id="_0" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedKey><Signature xmlns="http://www.w3.org/2000/09/xmldsig#"> ... </Signature><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList></o:Security>  
```  
  
#### <a name="322-mutualcertificateduplex"></a>3.2.2 MutualCertificateDuplex  

 在此身份验证模式下，客户端使用 X.509 证书进行身份验证，该证书作为发起方令牌出现在 SOAP 层上。 同样使用 X.509 证书对服务进行身份验证。  
  
 所用绑定为带有以下属性值的非对称绑定：  
  
 发起方令牌：客户端的 X509 证书，包含模式设置为 .../IncludeToken/AlwaysToRecipient  
  
 接收方令牌：服务器的 X509 证书，包含模式设置为 .../IncludeToken/AlwaysToInitiator  
  
 令牌保护：False  
  
 整个标头和正文签名：True  
  
 保护顺序：SignBeforeEncrypt  
  
 加密签名：True  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="MutualCertificateDuplex_policy"><wsp:ExactlyOne><wsp:All><sp:AsymmetricBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:InitiatorToken><wsp:Policy><sp:X509Token sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient"><wsp:Policy><sp:RequireThumbprintReference/><sp:WssX509V3Token10/></wsp:Policy></sp:X509Token></wsp:Policy></sp:InitiatorToken><sp:RecipientToken><wsp:Policy><sp:X509Token sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToInitiator"><wsp:Policy><sp:RequireThumbprintReference/><sp:WssX509V3Token10/></wsp:Policy></sp:X509Token></wsp:Policy></sp:RecipientToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptSignature/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:AsymmetricBinding><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><cdp:CompositeDuplex xmlns:cdp="http://schemas.microsoft.com/net/2006/06/duplex"/><ow:OneWay xmlns:ow="http://schemas.microsoft.com/ws/2005/05/routing/policy"/><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
### <a name="security-header-examples-signbeforeencrypt-encryptsignature"></a>安全标头示例：SignBeforeEncrypt、EncryptSignature  

 请求和响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-4dec3da4-b572-4654-ba4d-4a2f84a87510-1"> ... </u:Timestamp><o:BinarySecurityToken> ... </o:BinarySecurityToken><e:EncryptedKey Id="_0" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedKey><e:EncryptedData Id="_7" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="MutualCertificateDuplex_policy"><wsp:ExactlyOne><wsp:All><sp:AsymmetricBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:InitiatorToken><wsp:Policy><sp:X509Token sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient"><wsp:Policy><sp:RequireThumbprintReference/><sp:WssX509V3Token10/></wsp:Policy></sp:X509Token></wsp:Policy></sp:InitiatorToken><sp:RecipientToken><wsp:Policy><sp:X509Token sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToInitiator"><wsp:Policy><sp:RequireThumbprintReference/><sp:WssX509V3Token10/></wsp:Policy></sp:X509Token></wsp:Policy></sp:RecipientToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptBeforeSigning/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:AsymmetricBinding><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><cdp:CompositeDuplex xmlns:cdp="http://schemas.microsoft.com/net/2006/06/duplex"/><ow:OneWay xmlns:ow="http://schemas.microsoft.com/ws/2005/05/routing/policy"/><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
### <a name="security-header-examples-encryptbeforesign"></a>安全标头示例：EncryptBeforeSign  

 请求和响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-b0e23feb-cd2d-4dc1-bad9-284bc45f3be3-1"> ... </u:Timestamp><o:BinarySecurityToken> ... </o:BinarySecurityToken><e:EncryptedKey Id="_0" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedKey><Signature xmlns="http://www.w3.org/2000/09/xmldsig#"> ... </Signature><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList></o:Security>  
```  
  
#### <a name="323-using-symmetricbinding-with-x509-service-authentication"></a>3.2.3 将 SymmetricBinding 用于 X.509 服务身份验证  

 “WSS10”对 X509 令牌方案提供有限支持。 例如，如果消息仅使用服务 X509 令牌，则无法为其提供签名和加密保护。 “WSS11”将 EncryptedKey 用作对称令牌。 现在，为服务的 X.509 证书加密的临时密钥可同时用于请求和响应消息保护。 下面3.4 部分中描述的身份验证模式使用此模式。  
  
 WS-SecurityPolicy 描述了此模式，即将 SymmetricBinding 用于服务 X509 令牌作为保护令牌。  
  
 身份验证模式 AnonymousForCertificate、UsernameForCertificate、MutualCertificate WSS11 和 IssuedTokenForCertificate 都使用具有以下属性值的类似的 sp:SymmetricBinding 实例：  
  
 保护令牌：服务器的 X509 证书，包含模式设置为 …/IncludeToken/Never  
令牌保护：False  
  
 整个标头和正文签名：True  
  
 保护顺序：SignBeforeEncrypt  
  
 加密签名：True  
  
 上述身份验证模式的区别仅在于它们所使用的支持令牌。 AnonymousForCertificate 没有任何支持令牌，MutualCertificate WSS 1.1 将客户端的 X509 证书作为认可支持令牌，UserNameForCertificate 将“用户名令牌”作为签名支持令牌，而 IssuedTokenForCertificate 将颁发的令牌作为认可支持令牌。  
  
#### <a name="324-anonymousforcertificate"></a>3.2.4 AnonymousForCertificate  

 在此身份验证模式下，客户端是匿名的，而使用 X.509 证书对服务进行身份验证。 所用绑定是对称绑定的一个实例，如上面 3.4.2 中所述。  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="AnonymousforCertificate_policy"><wsp:ExactlyOne><wsp:All><sp:SymmetricBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:ProtectionToken><wsp:Policy><sp:X509Token sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/Never"><wsp:Policy><sp:RequireDerivedKeys/><sp:RequireThumbprintReference/><sp:WssX509V3Token10/></wsp:Policy></sp:X509Token></wsp:Policy></sp:ProtectionToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptSignature/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:SymmetricBinding><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/><sp:RequireSignatureConfirmation/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
### <a name="security-header-examples-signbeforeencrypt-encryptsignature"></a>安全标头示例：SignBeforeEncrypt、EncryptSignature  

 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-4de2d2a1-6266-4a02-93e6-242a1ac2aeb3-2"> ... </u:Timestamp><e:EncryptedKey Id="uuid-4de2d2a1-6266-4a02-93e6-242a1ac2aeb3-1" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedKey><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_8" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-b06cb481-3176-4c56-af35-c38252bb6c78-4"> ... </u:Timestamp><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_2" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_8" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData><e:EncryptedData Id="_7" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="AnonymousforCertificate_policy"><wsp:ExactlyOne><wsp:All><sp:SymmetricBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:ProtectionToken><wsp:Policy><sp:X509Token sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/Never"><wsp:Policy><sp:RequireDerivedKeys/><sp:RequireThumbprintReference/><sp:WssX509V3Token10/></wsp:Policy></sp:X509Token></wsp:Policy></sp:ProtectionToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptBeforeSigning/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:SymmetricBinding><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/><sp:RequireSignatureConfirmation/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
### <a name="security-header-examples-encryptbeforesign"></a>安全标头示例：EncryptBeforeSign  

 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-562aac68-8cdd-45d5-bc03-df662e6ed048-2"> ... </u:Timestamp><e:EncryptedKey Id="uuid-562aac68-8cdd-45d5-bc03-df662e6ed048-1" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedKey><sc:DerivedKeyToken u:Id="_1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><Signature xmlns="http://www.w3.org/2000/09/xmldsig#"> ... </Signature><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-15b48260-23da-424d-8dc4-8f4e150fb8cf-3"> ... </u:Timestamp><sc:DerivedKeyToken u:Id="_1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><k:SignatureConfirmation u:Id="_2" Value="ALF+QNGmWn2k3LpWEDIzSBgTkvo=" xmlns:k="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd"></k:SignatureConfirmation><Signature xmlns="http://www.w3.org/2000/09/xmldsig#"> ... </Signature><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList></o:Security>  
```  
  
#### <a name="325-usernameforcertificate"></a>3.2.5 UserNameForCertificate  

 在此身份验证模式下，客户端使用“用户名令牌”向服务进行身份验证，该令牌作为签名支持令牌出现在 SOAP 层上。 服务使用 X.509 证书对客户端进行身份验证。 所用绑定为对称绑定，其保护令牌是由客户端生成的密钥，用服务的公钥进行加密。  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="UserNameForCertificate_policy"><wsp:ExactlyOne><wsp:All><sp:SymmetricBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:ProtectionToken><wsp:Policy><sp:X509Token sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/Never"><wsp:Policy><sp:RequireDerivedKeys/><sp:RequireThumbprintReference/><sp:WssX509V3Token10/></wsp:Policy></sp:X509Token></wsp:Policy></sp:ProtectionToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptSignature/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:SymmetricBinding><sp:SignedEncryptedSupportingTokens xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:UsernameToken sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient"><wsp:Policy><sp:WssUsernameToken10/></wsp:Policy></sp:UsernameToken></wsp:Policy></sp:SignedEncryptedSupportingTokens><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><http:BasicAuthentication xmlns:http="http://schemas.microsoft.com/ws/06/2004/policy/http"/><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
### <a name="security-header-examples-signbeforeencrypt-encryptsignature"></a>安全标头示例：SignBeforeEncrypt、EncryptSignature  

 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-27196139-acb9-410f-a2c6-51d20d24278b-2"> ... </u:Timestamp><e:EncryptedKey Id="uuid-27196139-acb9-410f-a2c6-51d20d24278b-1" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedKey><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_9" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData><e:EncryptedData Id="_8" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-681226f7-126a-4806-b732-fcca097cd7a8-5"> ... </u:Timestamp><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_6" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="UserNameForCertificate_policy"><wsp:ExactlyOne><wsp:All><sp:SymmetricBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:ProtectionToken><wsp:Policy><sp:X509Token sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/Never"><wsp:Policy><sp:RequireDerivedKeys/><sp:RequireThumbprintReference/><sp:WssX509V3Token10/></wsp:Policy></sp:X509Token></wsp:Policy></sp:ProtectionToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptBeforeSigning/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:SymmetricBinding><sp:SignedEncryptedSupportingTokens xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:UsernameToken sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient"><wsp:Policy><sp:WssUsernameToken10/></wsp:Policy></sp:UsernameToken></wsp:Policy></sp:SignedEncryptedSupportingTokens><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><http:BasicAuthentication xmlns:http="http://schemas.microsoft.com/ws/06/2004/policy/http"/><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
### <a name="security-header-examples-encryptbeforesign"></a>安全标头示例：EncryptBeforeSign  

 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-8276d8b7-74a0-4257-b8a5-e25350e7c2d4-2"> ... </u:Timestamp><e:EncryptedKey Id="uuid-8276d8b7-74a0-4257-b8a5-e25350e7c2d4-1" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedKey><sc:DerivedKeyToken u:Id="_1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><e:EncryptedData Id="_8" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData><Signature xmlns="http://www.w3.org/2000/09/xmldsig#"> ... </Signature><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-8a7ad353-f071-49dc-90dd-5ad2e9abd40a-4"> ... </u:Timestamp><sc:DerivedKeyToken u:Id="_1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><Signature xmlns="http://www.w3.org/2000/09/xmldsig#"> ... </Signature><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList></o:Security>  
```  
  
#### <a name="326-mutualcertificate-wss-11"></a>3.2.6 MutualCertificate (WSS 1.1)  

 在此身份验证模式下，客户端使用 X.509 证书进行身份验证，该证书作为认可支持令牌出现在 SOAP 层上。 同样使用 X.509 证书对服务进行身份验证。 所用绑定为对称绑定，其保护令牌是由客户端生成的密钥，用服务的公钥进行加密。  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="MutualCertificate_policy"><wsp:ExactlyOne><wsp:All><sp:SymmetricBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:ProtectionToken><wsp:Policy><sp:X509Token sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/Never"><wsp:Policy><sp:RequireDerivedKeys/><sp:RequireThumbprintReference/><sp:WssX509V3Token10/></wsp:Policy></sp:X509Token></wsp:Policy></sp:ProtectionToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptSignature/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:SymmetricBinding><sp:EndorsingSupportingTokens xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:X509Token sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient"><wsp:Policy><sp:RequireThumbprintReference/><sp:WssX509V3Token10/></wsp:Policy></sp:X509Token></wsp:Policy></sp:EndorsingSupportingTokens><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/><sp:RequireSignatureConfirmation/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
### <a name="security-header-examples-signbeforeencrypt-encryptsignature"></a>安全标头示例：SignBeforeEncrypt、EncryptSignature  

 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-75305d4e-f54f-4e36-9de9-45b6d2053c80-2"> ... </u:Timestamp><e:EncryptedKey Id="uuid-75305d4e-f54f-4e36-9de9-45b6d2053c80-1" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedKey><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_2" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><o:BinarySecurityToken> ...</o:BinarySecurityToken><e:EncryptedData Id="_9" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData><e:EncryptedData Id="_10" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-8c73fa91-f95b-40ff-b088-48118e6fadcf-5"> ... </u:Timestamp><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_3" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_9" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData><e:EncryptedData Id="_10" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData><e:EncryptedData Id="_8" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="MutualCertificate_policy"><wsp:ExactlyOne><wsp:All><sp:SymmetricBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:ProtectionToken><wsp:Policy><sp:X509Token sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/Never"><wsp:Policy><sp:RequireDerivedKeys/><sp:RequireThumbprintReference/><sp:WssX509V3Token10/></wsp:Policy></sp:X509Token></wsp:Policy></sp:ProtectionToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptBeforeSigning/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:SymmetricBinding><sp:EndorsingSupportingTokens xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:X509Token sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient"><wsp:Policy><sp:RequireThumbprintReference/><sp:WssX509V3Token10/></wsp:Policy></sp:X509Token></wsp:Policy></sp:EndorsingSupportingTokens><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/><sp:RequireSignatureConfirmation/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
### <a name="security-header-examples-encryptbeforesign"></a>安全标头示例：EncryptBeforeSign  

 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-0b940a9e-606f-43b9-b05d-a162043529bc-2"> ... </u:Timestamp><e:EncryptedKey Id="uuid-0b940a9e-606f-43b9-b05d-a162043529bc-1" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedKey><sc:DerivedKeyToken u:Id="_1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><o:BinarySecurityToken> ... </o:BinarySecurityToken><Signature Id="_2" xmlns="http://www.w3.org/2000/09/xmldsig#"> ... </Signature><Signature xmlns="http://www.w3.org/2000/09/xmldsig#"> ... </Signature><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-67dacc31-4a50-4866-b673-ccc03e156337-3"> ... </u:Timestamp><sc:DerivedKeyToken u:Id="_1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><k:SignatureConfirmation u:Id="_2" Value="mYyksUQKkK27Fd6hmgOiqFwvudk=" xmlns:k="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd"></k:SignatureConfirmation><k:SignatureConfirmation u:Id="_3" Value="SreOZ4Rr2BcXjFQFvgN55ERypI/1/86hdWThE5lav0eYIxF1OCzQgZF+y7cQ82t+g3CRnLbE3c52DqMpY/HXlrdMct3m3rnpDH+fqdhNY4fE+M2v4zUMFR7uxDKWcEm9zZpmUvJCDfJRfKRaKjy5cTbccRKqSxw7HAqOYnqibA4=" xmlns:k="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd"></k:SignatureConfirmation><Signature xmlns="http://www.w3.org/2000/09/xmldsig#"> ... </Signature><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList></o:Security>  
```  
  
#### <a name="327-issuedtokenforcertificate"></a>3.2.7 IssuedTokenForCertificate  

 在此身份验证模式下，客户端不向服务进行身份验证，而是出示一个由 STS 颁发的令牌，并证明掌握了共享密钥。 颁发的令牌作为认可支持令牌出现在 SOAP 层上。 服务使用 X.509 证书对客户端进行身份验证。 所用绑定为对称绑定，其保护令牌是由客户端生成的密钥，用服务的公钥进行加密。  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="IssuedTokenForCertificate_policy"><wsp:ExactlyOne><wsp:All><sp:SymmetricBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:ProtectionToken><wsp:Policy><sp:X509Token sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/Never"><wsp:Policy><sp:RequireDerivedKeys/><sp:RequireThumbprintReference/><sp:WssX509V3Token10/></wsp:Policy></sp:X509Token></wsp:Policy></sp:ProtectionToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptSignature/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:SymmetricBinding><sp:EndorsingSupportingTokens xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:IssuedToken sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient"><Issuer xmlns="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><Address xmlns="http://www.w3.org/2005/08/addressing">http://www.w3.org/2005/08/addressing/anonymous</Address><Metadata xmlns="http://www.w3.org/2005/08/addressing"><Metadata xmlns="http://schemas.xmlsoap.org/ws/2004/09/mex" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><wsx:MetadataSection xmlns=""><wsx:MetadataReference><Address xmlns="http://www.w3.org/2005/08/addressing"> ... </Address><Identity xmlns="http://schemas.xmlsoap.org/ws/2006/02/addressingidentity"><Dns> ...  </Dns></Identity></wsx:MetadataReference></wsx:MetadataSection></Metadata></Metadata></Issuer><sp:RequestSecurityTokenTemplate><trust:KeyType xmlns:trust="http://docs.oasis-open.org/ws-sx/ws-trust/200512">http://docs.oasis-open.org/ws-sx/ws-trust/200512/SymmetricKey</trust:KeyType></sp:RequestSecurityTokenTemplate><wsp:Policy><sp:RequireDerivedKeys/><sp:RequireInternalReference/></wsp:Policy></sp:IssuedToken></wsp:Policy></sp:EndorsingSupportingTokens><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/><sp:RequireSignatureConfirmation/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
### <a name="security-header-examples-signbeforeencrypt-encryptsignature"></a>安全标头示例：SignBeforeEncrypt、EncryptSignature  

 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-1d2c03e6-0b69-4483-903a-6ef9b9d286ed-5"> ... </u:Timestamp><c:SecurityContextToken u:Id="uuid-3f0f57fa-046d-40c0-919f-d0d7aa640b9f-1" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:SecurityContextToken><c:DerivedKeyToken u:Id="_0" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><c:DerivedKeyToken u:Id="_1" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_8" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-3f0f57fa-046d-40c0-919f-d0d7aa640b9f-6"> ... </u:Timestamp><c:DerivedKeyToken u:Id="_0" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><c:DerivedKeyToken u:Id="_1" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_6" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
### <a name="security-header-examples-encryptbeforesign"></a>安全标头示例：EncryptBeforeSign  

 策略  
  
```xml  
<wsp:Policy wsu:Id="IssuedTokenForCertificate_policy"><wsp:ExactlyOne><wsp:All><sp:SymmetricBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:ProtectionToken><wsp:Policy><sp:X509Token sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/Never"><wsp:Policy><sp:RequireDerivedKeys/><sp:RequireThumbprintReference/><sp:WssX509V3Token10/></wsp:Policy></sp:X509Token></wsp:Policy></sp:ProtectionToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptBeforeSigning/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:SymmetricBinding><sp:EndorsingSupportingTokens xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:IssuedToken sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient"><Issuer xmlns="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><Address xmlns="http://www.w3.org/2005/08/addressing">http://www.w3.org/2005/08/addressing/anonymous</Address><Metadata xmlns="http://www.w3.org/2005/08/addressing"><Metadata xmlns="http://schemas.xmlsoap.org/ws/2004/09/mex" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><wsx:MetadataSection xmlns=""><wsx:MetadataReference><Address xmlns="http://www.w3.org/2005/08/addressing"> ... </Address><Identity xmlns="http://schemas.xmlsoap.org/ws/2006/02/addressingidentity"><Dns> ...  </Dns></Identity></wsx:MetadataReference></wsx:MetadataSection></Metadata></Metadata></Issuer><sp:RequestSecurityTokenTemplate><trust:KeyType xmlns:trust="http://docs.oasis-open.org/ws-sx/ws-trust/200512">http://docs.oasis-open.org/ws-sx/ws-trust/200512/SymmetricKey</trust:KeyType></sp:RequestSecurityTokenTemplate><wsp:Policy><sp:RequireDerivedKeys/><sp:RequireInternalReference/></wsp:Policy></sp:IssuedToken></wsp:Policy></sp:EndorsingSupportingTokens><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/><sp:RequireSignatureConfirmation/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-de1c51aa-2ecc-4e70-b6bd-9dca58331fa7-5"> ... </u:Timestamp><c:SecurityContextToken u:Id="uuid-96c5e80a-9b87-4c6f-af77-752ca65cf607-16" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:SecurityContextToken><c:DerivedKeyToken u:Id="_0" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><c:DerivedKeyToken u:Id="_1" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_8" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-96c5e80a-9b87-4c6f-af77-752ca65cf607-21"> ... </u:Timestamp><c:DerivedKeyToken u:Id="_0" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><c:DerivedKeyToken u:Id="_1" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_6" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
## <a name="33-kerberos"></a>3.3 Kerberos  

 在此身份验证模式下，客户端使用 Kerberos 票证向服务进行身份验证。 该票证还提供服务器身份验证。 所用绑定为对称绑定，具有以下属性：  
  
 保护令牌：Kerberos 票证，包含模式设置为 .../IncludeToken/Once  
令牌保护：False  
  
 整个标头和正文签名：True  
  
 保护顺序：SignBeforeEncrypt  
  
 加密签名：True  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="Kerberos_policy"><wsp:ExactlyOne><wsp:All><sp:SymmetricBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:ProtectionToken><wsp:Policy><sp:KerberosToken sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/Once"><wsp:Policy><sp:RequireDerivedKeys/><sp:WssGssKerberosV5ApReqToken11/></wsp:Policy></sp:KerberosToken></wsp:Policy></sp:ProtectionToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic128/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptSignature/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:SymmetricBinding><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
### <a name="security-header-examples-signbeforeencrypt-encryptsignature"></a>安全标头示例：SignBeforeEncrypt、EncryptSignature  

 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-e8f6dc3b-407d-4387-bd33-97aedfd8bf13-2"> ... </u:Timestamp><o:BinarySecurityToken> ... </o:BinarySecurityToken><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_8" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-7b03568e-66ae-49da-82ee-5d12d372876e-3"> ... </u:Timestamp><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_6" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="Kerberos_policy"><wsp:ExactlyOne><wsp:All><sp:SymmetricBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:ProtectionToken><wsp:Policy><sp:KerberosToken sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/Once"><wsp:Policy><sp:RequireDerivedKeys/><sp:WssGssKerberosV5ApReqToken11/></wsp:Policy></sp:KerberosToken></wsp:Policy></sp:ProtectionToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic128/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptBeforeSigning/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:SymmetricBinding><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
### <a name="security-header-examples-encryptbeforesign"></a>安全标头示例：EncryptBeforeSign  

 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-d29247f0-f220-4e81-9a8d-a15f5ac31072-2"> ... </u:Timestamp><o:BinarySecurityToken> ... </o:BinarySecurityToken><sc:DerivedKeyToken u:Id="_1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><Signature xmlns="http://www.w3.org/2000/09/xmldsig#"> ... </Signature><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-9025b930-4f15-42fe-8e78-35d3a3480177-2"> ... </u:Timestamp><sc:DerivedKeyToken u:Id="_1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><Signature xmlns="http://www.w3.org/2000/09/xmldsig#"> ... </Signature><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList></o:Security>  
```  
  
#### <a name="34-issuedtoken"></a>3.4 IssuedToken  

 在此身份验证模式下，客户端不向服务进行身份验证，而是提供一个由 STS 颁发的令牌，并证明掌握了共享密钥。 服务也不向客户端进行身份验证，而是由 STS 将共享密钥作为颁发的令牌的一部分进行加密，这样，只有服务才能解密该密钥。 所用绑定为对称绑定，具有以下属性：  
  
 保护令牌：颁发的令牌，包含模式设置为 .../IncludeToken/AlwaysToRecipient  
令牌保护：False  
  
 整个标头和正文签名：True  
  
 保护顺序：SignBeforeEncrypt  
  
 加密签名：True  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="IssuedToken_policy"><wsp:ExactlyOne><wsp:All><sp:SymmetricBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:ProtectionToken><wsp:Policy><sp:IssuedToken sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient"><Issuer xmlns="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><Address xmlns="http://www.w3.org/2005/08/addressing">http://www.w3.org/2005/08/addressing/anonymous</Address><Metadata xmlns="http://www.w3.org/2005/08/addressing"><Metadata xmlns="http://schemas.xmlsoap.org/ws/2004/09/mex" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><wsx:MetadataSection xmlns=""><wsx:MetadataReference><Address xmlns="http://www.w3.org/2005/08/addressing"> ... </Address><Identity xmlns="http://schemas.xmlsoap.org/ws/2006/02/addressingidentity"><Dns> ...  </Dns></Identity></wsx:MetadataReference></wsx:MetadataSection></Metadata></Metadata></Issuer><sp:RequestSecurityTokenTemplate><trust:KeyType xmlns:trust="http://docs.oasis-open.org/ws-sx/ws-trust/200512">http://docs.oasis-open.org/ws-sx/ws-trust/200512/SymmetricKey</trust:KeyType></sp:RequestSecurityTokenTemplate><wsp:Policy><sp:RequireDerivedKeys/><sp:RequireInternalReference/></wsp:Policy></sp:IssuedToken></wsp:Policy></sp:ProtectionToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptSignature/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:SymmetricBinding><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
### <a name="security-header-examples-signbeforeencrypt-encryptsignature"></a>安全标头示例：SignBeforeEncrypt、EncryptSignature  

 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-61ce3989-bc38-4d67-8262-6232c9d49a26-5"> ... </u:Timestamp><c:SecurityContextToken u:Id="uuid-7e2d2617-1c28-465a-be30-de4a78cfc0e2-1" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:SecurityContextToken><c:DerivedKeyToken u:Id="_0" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><c:DerivedKeyToken u:Id="_1" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_8" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-7e2d2617-1c28-465a-be30-de4a78cfc0e2-6"> ... </u:Timestamp><c:DerivedKeyToken u:Id="_0" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><c:DerivedKeyToken u:Id="_1" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_6" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>
```  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="IssuedToken_policy"><wsp:ExactlyOne><wsp:All><sp:SymmetricBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:ProtectionToken><wsp:Policy><sp:IssuedToken sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient"><Issuer xmlns="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><Address xmlns="http://www.w3.org/2005/08/addressing">http://www.w3.org/2005/08/addressing/anonymous</Address><Metadata xmlns="http://www.w3.org/2005/08/addressing"><Metadata xmlns="http://schemas.xmlsoap.org/ws/2004/09/mex" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><wsx:MetadataSection xmlns=""><wsx:MetadataReference><Address xmlns="http://www.w3.org/2005/08/addressing"> ... </Address><Identity xmlns="http://schemas.xmlsoap.org/ws/2006/02/addressingidentity"><Dns> ...  </Dns></Identity></wsx:MetadataReference></wsx:MetadataSection></Metadata></Metadata></Issuer><sp:RequestSecurityTokenTemplate><trust:KeyType xmlns:trust="http://docs.oasis-open.org/ws-sx/ws-trust/200512">http://docs.oasis-open.org/ws-sx/ws-trust/200512/SymmetricKey</trust:KeyType></sp:RequestSecurityTokenTemplate><wsp:Policy><sp:RequireDerivedKeys/><sp:RequireInternalReference/></wsp:Policy></sp:IssuedToken></wsp:Policy></sp:ProtectionToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptBeforeSigning/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:SymmetricBinding><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
### <a name="security-header-examples-encryptbeforesign"></a>安全标头示例：EncryptBeforeSign  

 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-1dc8bdef-4202-4e08-8a5e-ab94da579dec-5"> ... </u:Timestamp><c:SecurityContextToken u:Id="uuid-7e004f51-63a3-4069-9b03-6a1a311a3181-1" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:SecurityContextToken><c:DerivedKeyToken u:Id="_0" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><c:DerivedKeyToken u:Id="_1" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_8" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-7e004f51-63a3-4069-9b03-6a1a311a3181-6"> ... </u:Timestamp><c:DerivedKeyToken u:Id="_0" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> </c:DerivedKeyToken> ... <c:DerivedKeyToken u:Id="_1" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_6" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
### <a name="35-using-sslnegotiated-for-service-authentication"></a>3.5 使用 SslNegotiated 进行服务身份验证  

 本节介绍的一组身份验证模式将对称绑定用于保护令牌，作为安全上下文令牌（符合 WS-SecureConversation (WS-SC)），其键值是通过对 WS-Trust (WS-T) RST/RSTR 消息执行 TLS 协议进行协商的。 有关使用 WS-Trust 实现 TLS 握手的详细信息，请参见 TLSNEGO 中的内容。 此处的消息示例中，我们假设已通过握手建立了带有关联安全上下文的 SCT。  
  
 所用绑定为对称绑定，具有以下属性：  
  
 保护令牌：SslContextToken，包含模式设置为 .../IncludeToken/Never  
令牌保护：False  
  
 整个标头和正文签名：True  
  
 保护顺序：SignBeforeEncrypt  
  
 加密签名：True  
  
#### <a name="351-policy-for-sslnegotiated-service-authentication"></a>3.5.1 SslNegotiated 服务身份验证的策略  

 本节介绍的所有身份验证模式的策略都相似，唯一的区别在于，所用的特定签名支持令牌或认可令牌不相同。  
  
#### <a name="352-anonymousforsslnegotiated"></a>3.5.2 AnonymousForSslNegotiated  

 在此身份验证模式下，客户端是匿名的，而使用 X.509 证书对服务进行身份验证。 所用绑定是对称绑定的一个实例，如上面 3.5.1 中所述。  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="AnonymousForSslNegotiated_policy"><wsp:ExactlyOne><wsp:All><sp:SymmetricBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:ProtectionToken><wsp:Policy><mssp:SslContextToken sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient" xmlns:mssp="http://schemas.microsoft.com/ws/2005/07/securitypolicy"><wsp:Policy><sp:RequireDerivedKeys/><sp:MustNotSendCancel/><sp:MustNotSendAmend/><sp:MustNotSendRenew/></wsp:Policy></mssp:SslContextToken></wsp:Policy></sp:ProtectionToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptSignature/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:SymmetricBinding><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
### <a name="security-header-examples-signbeforeencrypt-encryptsignature"></a>安全标头示例：SignBeforeEncrypt、EncryptSignature  

 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-f4b86ce2-4ba6-4c55-bac1-2e920fc6d5db-4"> ... </u:Timestamp><sc:SecurityContextToken u:Id="uuid-d21ec2b8-99f5-443c-a4c6-a4d40619187e-1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:SecurityContextToken><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_8" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-d21ec2b8-99f5-443c-a4c6-a4d40619187e-4"> ... </u:Timestamp><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_6" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="AnonymousForSslNegotiated_policy"><wsp:ExactlyOne><wsp:All><sp:SymmetricBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:ProtectionToken><wsp:Policy><mssp:SslContextToken sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient" xmlns:mssp="http://schemas.microsoft.com/ws/2005/07/securitypolicy"><wsp:Policy><sp:RequireDerivedKeys/><sp:MustNotSendCancel/><sp:MustNotSendAmend/><sp:MustNotSendRenew/></wsp:Policy></mssp:SslContextToken></wsp:Policy></sp:ProtectionToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptBeforeSigning/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:SymmetricBinding><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
### <a name="security-header-examples-encryptbeforesign"></a>安全标头示例：EncryptBeforeSign  

 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-c84b24b9-39e0-4cc3-99e2-cec088f1b9eb-4"> ... </u:Timestamp><sc:SecurityContextToken u:Id="uuid-df206ad9-1ee2-46d7-9fb4-6e4631c9762f-1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:SecurityContextToken><sc:DerivedKeyToken u:Id="_1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><Signature xmlns="http://www.w3.org/2000/09/xmldsig#"> ... </Signature><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-df206ad9-1ee2-46d7-9fb4-6e4631c9762f-3"> ... </u:Timestamp><sc:DerivedKeyToken u:Id="_1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><Signature xmlns="http://www.w3.org/2000/09/xmldsig#"> ... </Signature><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList></o:Security>  
```  
  
#### <a name="353-usernameforsslnegotiated"></a>3.5.3 UserNameForSslNegotiated  

 在此身份验证模式下，客户端使用“用户名令牌”进行身份验证，该令牌作为签名支持令牌出现在 SOAP 层上。 使用 X.509 证书对服务进行身份验证。 所用绑定是对称绑定的一个实例，如上面 3.5.1 中所述。  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="UserNameForSslNegotiated_policy"><wsp:ExactlyOne><wsp:All><sp:SymmetricBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:ProtectionToken><wsp:Policy><mssp:SslContextToken sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient" xmlns:mssp="http://schemas.microsoft.com/ws/2005/07/securitypolicy"><wsp:Policy><sp:RequireDerivedKeys/><sp:MustNotSendCancel/><sp:MustNotSendAmend/><sp:MustNotSendRenew/></wsp:Policy></mssp:SslContextToken></wsp:Policy></sp:ProtectionToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptSignature/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:SymmetricBinding><sp:SignedEncryptedSupportingTokens xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:UsernameToken sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient"><wsp:Policy><sp:WssUsernameToken10/></wsp:Policy></sp:UsernameToken></wsp:Policy></sp:SignedEncryptedSupportingTokens><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
### <a name="security-header-examples-signbeforeencrypt-encryptsignature"></a>安全标头示例：SignBeforeEncrypt、EncryptSignature  

 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-3d1a12c3-e690-474a-a223-a346fc0329a9-4"> ... </u:Timestamp><sc:SecurityContextToken u:Id="uuid-137ea768-7d49-404b-87eb-f11d9c7154aa-1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:SecurityContextToken><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_9" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData><e:EncryptedData Id="_8" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-137ea768-7d49-404b-87eb-f11d9c7154aa-5"> ... </u:Timestamp><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_6" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="UserNameForSslNegotiated_policy"><wsp:ExactlyOne><wsp:All><sp:SymmetricBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:ProtectionToken><wsp:Policy><mssp:SslContextToken sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient" xmlns:mssp="http://schemas.microsoft.com/ws/2005/07/securitypolicy"><wsp:Policy><sp:RequireDerivedKeys/><sp:MustNotSendCancel/><sp:MustNotSendAmend/><sp:MustNotSendRenew/></wsp:Policy></mssp:SslContextToken></wsp:Policy></sp:ProtectionToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptBeforeSigning/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:SymmetricBinding><sp:SignedEncryptedSupportingTokens xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:UsernameToken sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient"><wsp:Policy><sp:WssUsernameToken10/></wsp:Policy></sp:UsernameToken></wsp:Policy></sp:SignedEncryptedSupportingTokens><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
### <a name="security-header-examples-encryptbeforesign"></a>安全标头示例：EncryptBeforeSign  

 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-56e931e8-20c2-457f-a83e-8fcd6b92c258-4"> ... </u:Timestamp><sc:SecurityContextToken u:Id="uuid-83d053cb-03a0-4461-9616-86475cf083c4-1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:SecurityContextToken><sc:DerivedKeyToken u:Id="_1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><e:EncryptedData Id="_8" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData><Signature xmlns="http://www.w3.org/2000/09/xmldsig#"> ... </Signature><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-83d053cb-03a0-4461-9616-86475cf083c4-4"> ... </u:Timestamp><sc:DerivedKeyToken u:Id="_1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><Signature xmlns="http://www.w3.org/2000/09/xmldsig#"> ... </Signature><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList></o:Security>  
```  
  
#### <a name="354-issuedtokenforsslnegotiated"></a>3.5.4 IssuedTokenForSslNegotiated  

 在此身份验证模式下，客户端不向服务进行身份验证，而是出示一个由 STS 颁发的令牌，并证明掌握了共享密钥。 颁发的令牌作为认可支持令牌出现在 SOAP 层上。 使用 X.509 证书对服务进行身份验证。 所用绑定是对称绑定的一个实例，如上面 3.5.1 中所述。  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="IssuedTokenForSslNegotiated_policy"><wsp:ExactlyOne><wsp:All><sp:SymmetricBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:ProtectionToken><wsp:Policy><mssp:SslContextToken sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient" xmlns:mssp="http://schemas.microsoft.com/ws/2005/07/securitypolicy"><wsp:Policy><sp:RequireDerivedKeys/><sp:MustNotSendCancel/><sp:MustNotSendAmend/><sp:MustNotSendRenew/></wsp:Policy></mssp:SslContextToken></wsp:Policy></sp:ProtectionToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptSignature/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:SymmetricBinding><sp:EndorsingSupportingTokens xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:IssuedToken sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient"><Issuer xmlns="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><Address xmlns="http://www.w3.org/2005/08/addressing">http://www.w3.org/2005/08/addressing/anonymous</Address><Metadata xmlns="http://www.w3.org/2005/08/addressing"><Metadata xmlns="http://schemas.xmlsoap.org/ws/2004/09/mex" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><wsx:MetadataSection xmlns=""><wsx:MetadataReference><Address xmlns="http://www.w3.org/2005/08/addressing"> ... </Address><Identity xmlns="http://schemas.xmlsoap.org/ws/2006/02/addressingidentity"><Dns> ... </Dns></Identity></wsx:MetadataReference></wsx:MetadataSection></Metadata></Metadata></Issuer><sp:RequestSecurityTokenTemplate><trust:KeyType xmlns:trust="http://docs.oasis-open.org/ws-sx/ws-trust/200512">http://docs.oasis-open.org/ws-sx/ws-trust/200512/SymmetricKey</trust:KeyType></sp:RequestSecurityTokenTemplate><wsp:Policy><sp:RequireDerivedKeys/><sp:RequireInternalReference/></wsp:Policy></sp:IssuedToken></wsp:Policy></sp:EndorsingSupportingTokens><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/><sp:RequireSignatureConfirmation/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
### <a name="security-header-examples-signbeforeencrypt-encryptsignature"></a>安全标头示例：SignBeforeEncrypt、EncryptSignature  

 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-b19fb2e7-8f0c-45c1-b62c-45d6ff6d57e7-5"> ... </u:Timestamp><c:SecurityContextToken u:Id="uuid-199509f9-7963-42b7-b340-7598ee261d5a-1" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:SecurityContextToken><c:DerivedKeyToken u:Id="_0" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><c:DerivedKeyToken u:Id="_1" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_8" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-199509f9-7963-42b7-b340-7598ee261d5a-6"> ... </u:Timestamp><c:DerivedKeyToken u:Id="_0" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><c:DerivedKeyToken u:Id="_1" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_6" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="IssuedTokenForSslNegotiated_policy"><wsp:ExactlyOne><wsp:All><sp:SymmetricBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:ProtectionToken><wsp:Policy><mssp:SslContextToken sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient" xmlns:mssp="http://schemas.microsoft.com/ws/2005/07/securitypolicy"><wsp:Policy><sp:RequireDerivedKeys/><sp:MustNotSendCancel/><sp:MustNotSendAmend/><sp:MustNotSendRenew/></wsp:Policy></mssp:SslContextToken></wsp:Policy></sp:ProtectionToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptBeforeSigning/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:SymmetricBinding><sp:EndorsingSupportingTokens xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:IssuedToken sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient"><Issuer xmlns="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><Address xmlns="http://www.w3.org/2005/08/addressing">http://www.w3.org/2005/08/addressing/anonymous</Address><Metadata xmlns="http://www.w3.org/2005/08/addressing"><Metadata xmlns="http://schemas.xmlsoap.org/ws/2004/09/mex" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><wsx:MetadataSection xmlns=""><wsx:MetadataReference><Address xmlns="http://www.w3.org/2005/08/addressing"> ... </Address><Identity xmlns="http://schemas.xmlsoap.org/ws/2006/02/addressingidentity"><Dns> ... </Dns></Identity></wsx:MetadataReference></wsx:MetadataSection></Metadata></Metadata></Issuer><sp:RequestSecurityTokenTemplate><trust:KeyType xmlns:trust="http://docs.oasis-open.org/ws-sx/ws-trust/200512">http://docs.oasis-open.org/ws-sx/ws-trust/200512/SymmetricKey</trust:KeyType></sp:RequestSecurityTokenTemplate><wsp:Policy><sp:RequireDerivedKeys/><sp:RequireInternalReference/></wsp:Policy></sp:IssuedToken></wsp:Policy></sp:EndorsingSupportingTokens><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/><sp:RequireSignatureConfirmation/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>
```  
  
### <a name="security-header-examples-encryptbeforesign"></a>安全标头示例：EncryptBeforeSign  

 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-d75104d5-313e-440f-b112-db8aff57a5fe-5"> ... </u:Timestamp><c:SecurityContextToken u:Id="uuid-e668caab-b7e4-4056-ac42-4015ae2a67a6-1" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:SecurityContextToken><c:DerivedKeyToken u:Id="_0" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><c:DerivedKeyToken u:Id="_1" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_8" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-e668caab-b7e4-4056-ac42-4015ae2a67a6-6"> ... </u:Timestamp><c:DerivedKeyToken u:Id="_0" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><c:DerivedKeyToken u:Id="_1" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_6" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
#### <a name="355-mutualsslnegotiated"></a>3.5.5 MutualSslNegotiated  

 在此身份验证模式下，客户端和服务都使用 X.509 证书进行身份验证。 所用绑定是对称绑定的一个实例，如上面 3.5.1 中所述。  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="MutualSslNegotiated_policy"><wsp:ExactlyOne><wsp:All><sp:SymmetricBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:ProtectionToken><wsp:Policy><mssp:SslContextToken sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient" xmlns:mssp="http://schemas.microsoft.com/ws/2005/07/securitypolicy"><wsp:Policy><sp:RequireDerivedKeys/><sp:MustNotSendCancel/><mssp:RequireClientCertificate/><sp:MustNotSendAmend/><sp:MustNotSendRenew/></wsp:Policy></mssp:SslContextToken></wsp:Policy></sp:ProtectionToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptSignature/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:SymmetricBinding><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
### <a name="security-header-examples-signbeforeencrypt-encryptsignature"></a>安全标头示例：SignBeforeEncrypt、EncryptSignature  

 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-d1e6037e-8a64-494f-9447-07d3125b81b5-4"> ... </u:Timestamp><sc:SecurityContextToken u:Id="uuid-e4b73625-3011-4f6d-a6f9-4d682e860801-1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:SecurityContextToken><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_8" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-e4b73625-3011-4f6d-a6f9-4d682e860801-4"> ... </u:Timestamp><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_6" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="MutualSslNegotiated_policy"><wsp:ExactlyOne><wsp:All><sp:SymmetricBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:ProtectionToken><wsp:Policy><mssp:SslContextToken sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient" xmlns:mssp="http://schemas.microsoft.com/ws/2005/07/securitypolicy"><wsp:Policy><sp:RequireDerivedKeys/><sp:MustNotSendCancel/><mssp:RequireClientCertificate/><sp:MustNotSendAmend/><sp:MustNotSendRenew/></wsp:Policy></mssp:SslContextToken></wsp:Policy></sp:ProtectionToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptBeforeSigning/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:SymmetricBinding><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>
```  
  
### <a name="security-header-examples-encryptbeforesign"></a>安全标头示例：EncryptBeforeSign  

 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-c2a6ab10-889a-4ee1-871d-05410c90fc10-4"> ... </u:Timestamp><sc:SecurityContextToken u:Id="uuid-ede0bd89-1f7e-4453-96ed-13e58c7ba8fe-1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:SecurityContextToken><sc:DerivedKeyToken u:Id="_1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><Signature xmlns="http://www.w3.org/2000/09/xmldsig#"> ... </Signature><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-ede0bd89-1f7e-4453-96ed-13e58c7ba8fe-3"> ... </u:Timestamp><sc:DerivedKeyToken u:Id="_1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><Signature xmlns="http://www.w3.org/2000/09/xmldsig#"> ... </Signature><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList></o:Security>  
```  
  
### <a name="36-sspinegotiated"></a>3.6 SspiNegotiated  

 在此身份验证模式下，将使用协商协议来执行客户端和服务器身份验证。 如果可能，就使用 Kerberos，否则使用 NTLM。 所用绑定为对称绑定，具有以下属性：  
  
 保护令牌：SpnegoContextToken，包含模式设置为 .../IncludeToken/AlwaysToRecipient  
令牌保护：False  
  
 整个标头和正文签名：True  
  
 保护顺序：SignBeforeEncrypt  
  
 加密签名：True  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="SspiNegotiated_policy"><wsp:ExactlyOne><wsp:All><sp:SymmetricBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:ProtectionToken><wsp:Policy><sp:SpnegoContextToken sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient"><wsp:Policy><sp:RequireDerivedKeys/><sp:MustNotSendCancel/><sp:MustNotSendAmend/><sp:MustNotSendRenew/></wsp:Policy></sp:SpnegoContextToken></wsp:Policy></sp:ProtectionToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptSignature/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:SymmetricBinding><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
### <a name="security-header-examples-signbeforeencrypt-encryptsignature"></a>安全标头示例：SignBeforeEncrypt、EncryptSignature  

 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-9a954fcb-4df2-4610-9800-f542f2b5130a-4"> ... </u:Timestamp><sc:SecurityContextToken u:Id="uuid-554d8cfc-e956-43db-9abb-afcafd024347-1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:SecurityContextToken><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_8" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-554d8cfc-e956-43db-9abb-afcafd024347-4"> ... </u:Timestamp><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_6" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>
```  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="SspiNegotiated_policy"><wsp:ExactlyOne><wsp:All><sp:SymmetricBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:ProtectionToken><wsp:Policy><sp:SpnegoContextToken sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient"><wsp:Policy><sp:RequireDerivedKeys/><sp:MustNotSendCancel/><sp:MustNotSendAmend/><sp:MustNotSendRenew/></wsp:Policy></sp:SpnegoContextToken></wsp:Policy></sp:ProtectionToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptBeforeSigning/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:SymmetricBinding><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
### <a name="security-header-examples-encryptbeforesign"></a>安全标头示例：EncryptBeforeSign  

 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-f1673773-f9a7-4b43-b13b-405e7dd4a6e3-4"> ... </u:Timestamp><sc:SecurityContextToken u:Id="uuid-e0aabc81-6942-4fe6-81bc-9def184565ea-1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:SecurityContextToken><sc:DerivedKeyToken u:Id="_1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><Signature xmlns="http://www.w3.org/2000/09/xmldsig#"> ... </Signature><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-e0aabc81-6942-4fe6-81bc-9def184565ea-3"> ... </u:Timestamp><sc:DerivedKeyToken u:Id="_1" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><sc:DerivedKeyToken u:Id="_0" xmlns:sc="http://docs.oasis-open.org/ws-sx/ws-secureconversation/200512"> ... </sc:DerivedKeyToken><Signature xmlns="http://www.w3.org/2000/09/xmldsig#"> ... </Signature><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList></o:Security>  
```  
  
### <a name="37-secureconversation"></a>3.7 Ws-secureconversation  

 所用绑定为对称绑定，保护令牌为符合 WS-SecureConversation (WS-SC) 的 SCT。 该 SCT 是根据嵌套的绑定使用 WS-Trust (WS-Trust) 或 WS-SecureConversation (WS-SC) 协商的，该嵌套绑定本身是使用协商协议的对称绑定。 如果可能，协商协议将使用 Kerberos 来执行客户端和服务器身份验证。 如果无法使用 Kerberos，则退而使用 NTLM。  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="SecureConversation_policy"><wsp:ExactlyOne><wsp:All><sp:SymmetricBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:ProtectionToken><wsp:Policy><sp:SecureConversationToken sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient"><wsp:Policy><sp:RequireDerivedKeys/><sp:BootstrapPolicy><wsp:Policy><sp:SignedParts xmlns:sp="http://schemas.xmlsoap.org/ws/2005/07/securitypolicy"><sp:Body/><sp:Header Name="To" Namespace="http://www.w3.org/2005/08/addressing"/><sp:Header Name="From" Namespace="http://www.w3.org/2005/08/addressing"/><sp:Header Name="FaultTo" Namespace="http://www.w3.org/2005/08/addressing"/><sp:Header Name="ReplyTo" Namespace="http://www.w3.org/2005/08/addressing"/><sp:Header Name="MessageID" Namespace="http://www.w3.org/2005/08/addressing"/><sp:Header Name="RelatesTo" Namespace="http://www.w3.org/2005/08/addressing"/><sp:Header Name="Action" Namespace="http://www.w3.org/2005/08/addressing"/></sp:SignedParts><sp:EncryptedParts xmlns:sp="http://schemas.xmlsoap.org/ws/2005/07/securitypolicy"><sp:Body/></sp:EncryptedParts><sp:SymmetricBinding xmlns:sp="http://schemas.xmlsoap.org/ws/2005/07/securitypolicy"><wsp:Policy><sp:ProtectionToken><wsp:Policy><sp:SpnegoContextToken sp:IncludeToken="http://schemas.xmlsoap.org/ws/2005/07/securitypolicy/IncludeToken/AlwaysToRecipient"><wsp:Policy><sp:RequireDerivedKeys/></wsp:Policy></sp:SpnegoContextToken></wsp:Policy></sp:ProtectionToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptSignature/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:SymmetricBinding><sp:Wss11 xmlns:sp="http://schemas.xmlsoap.org/ws/2005/07/securitypolicy"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/></wsp:Policy></sp:Wss11><sp:Trust10 xmlns:sp="http://schemas.xmlsoap.org/ws/2005/07/securitypolicy"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust10></wsp:Policy></sp:BootstrapPolicy><sp:MustNotSendAmend/></wsp:Policy></sp:SecureConversationToken></wsp:Policy></sp:ProtectionToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptSignature/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:SymmetricBinding><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
### <a name="security-header-examples-signbeforeencrypt-encryptsignature"></a>安全标头示例：SignBeforeEncrypt、EncryptSignature  

 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-f01c6159-f159-454d-bd97-bbcc9b8e25d3-5"> ... </u:Timestamp><c:SecurityContextToken u:Id="uuid-582920d7-14a7-4adc-8091-e1f92d7d8055-1" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:SecurityContextToken><c:DerivedKeyToken u:Id="_0" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><c:DerivedKeyToken u:Id="_1" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_8" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-582920d7-14a7-4adc-8091-e1f92d7d8055-6"> ... </u:Timestamp><c:DerivedKeyToken u:Id="_0" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><c:DerivedKeyToken u:Id="_1" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_6" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
 策略  
  
```xml  
<wsp:Policy wsu:Id="SecureConversation_policy"><wsp:ExactlyOne><wsp:All><sp:SymmetricBinding xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:ProtectionToken><wsp:Policy><sp:SecureConversationToken sp:IncludeToken="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/IncludeToken/AlwaysToRecipient"><wsp:Policy><sp:RequireDerivedKeys/><sp:BootstrapPolicy><wsp:Policy><sp:SignedParts xmlns:sp="http://schemas.xmlsoap.org/ws/2005/07/securitypolicy"><sp:Body/><sp:Header Name="To" Namespace="http://www.w3.org/2005/08/addressing"/><sp:Header Name="From" Namespace="http://www.w3.org/2005/08/addressing"/><sp:Header Name="FaultTo" Namespace="http://www.w3.org/2005/08/addressing"/><sp:Header Name="ReplyTo" Namespace="http://www.w3.org/2005/08/addressing"/><sp:Header Name="MessageID" Namespace="http://www.w3.org/2005/08/addressing"/><sp:Header Name="RelatesTo" Namespace="http://www.w3.org/2005/08/addressing"/><sp:Header Name="Action" Namespace="http://www.w3.org/2005/08/addressing"/></sp:SignedParts><sp:EncryptedParts xmlns:sp="http://schemas.xmlsoap.org/ws/2005/07/securitypolicy"><sp:Body/></sp:EncryptedParts><sp:SymmetricBinding xmlns:sp="http://schemas.xmlsoap.org/ws/2005/07/securitypolicy"><wsp:Policy><sp:ProtectionToken><wsp:Policy><sp:SpnegoContextToken sp:IncludeToken="http://schemas.xmlsoap.org/ws/2005/07/securitypolicy/IncludeToken/AlwaysToRecipient"><wsp:Policy><sp:RequireDerivedKeys/></wsp:Policy></sp:SpnegoContextToken></wsp:Policy></sp:ProtectionToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptSignature/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:SymmetricBinding><sp:Wss11 xmlns:sp="http://schemas.xmlsoap.org/ws/2005/07/securitypolicy"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/></wsp:Policy></sp:Wss11><sp:Trust10 xmlns:sp="http://schemas.xmlsoap.org/ws/2005/07/securitypolicy"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust10></wsp:Policy></sp:BootstrapPolicy><sp:MustNotSendAmend/></wsp:Policy></sp:SecureConversationToken></wsp:Policy></sp:ProtectionToken><sp:AlgorithmSuite><wsp:Policy><sp:Basic256/></wsp:Policy></sp:AlgorithmSuite><sp:Layout><wsp:Policy><sp:Strict/></wsp:Policy></sp:Layout><sp:IncludeTimestamp/><sp:EncryptBeforeSigning/><sp:OnlySignEntireHeadersAndBody/></wsp:Policy></sp:SymmetricBinding><sp:Wss11 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportRefKeyIdentifier/><sp:MustSupportRefIssuerSerial/><sp:MustSupportRefThumbprint/><sp:MustSupportRefEncryptedKey/></wsp:Policy></sp:Wss11><sp:Trust13 xmlns:sp="http://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702"><wsp:Policy><sp:MustSupportIssuedTokens/><sp:RequireClientEntropy/><sp:RequireServerEntropy/></wsp:Policy></sp:Trust13><wsaw:UsingAddressing/></wsp:All></wsp:ExactlyOne></wsp:Policy>  
```  
  
### <a name="security-header-examples-encryptbeforesign"></a>安全标头示例：EncryptBeforeSign  

 请求  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-d57e6342-1c68-4095-a0c1-41979088a944-5"> ... </u:Timestamp><c:SecurityContextToken u:Id="uuid-9b22407d-b914-4c41-9105-1cf8cf7c3fe5-1" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:SecurityContextToken><c:DerivedKeyToken u:Id="_0" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><c:DerivedKeyToken u:Id="_1" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_8" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```  
  
 响应  
  
```xml  
<o:Security s:mustUnderstand="1" xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd"><u:Timestamp u:Id="uuid-9b22407d-b914-4c41-9105-1cf8cf7c3fe5-6"> ... </u:Timestamp><c:DerivedKeyToken u:Id="_0" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><c:DerivedKeyToken u:Id="_1" xmlns:c="http://schemas.xmlsoap.org/ws/2005/02/sc"> ... </c:DerivedKeyToken><e:ReferenceList xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:ReferenceList><e:EncryptedData Id="_6" Type="http://www.w3.org/2001/04/xmlenc#Element" xmlns:e="http://www.w3.org/2001/04/xmlenc#"> ... </e:EncryptedData></o:Security>  
```
