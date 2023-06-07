---
title: SecurityBindingElement 身份验证模式
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 12300bf4-c730-4405-9f65-d286f68b5a43
ms.openlocfilehash: bf1b8103714c174fc2746bc864a7d7e0e5ea5ff1
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96253981"
---
# <a name="securitybindingelement-authentication-modes"></a>SecurityBindingElement 身份验证模式

Windows Communication Foundation (WCF) 提供多种模式，客户端和服务通过这些模式进行身份验证。 你可以通过使用 <xref:System.ServiceModel.Channels.SecurityBindingElement> 类上的静态方法或通过配置为这些身份验证模式创建安全绑定元素。 本主题简要说明 18 种身份验证模式。  
  
 有关将元素用于其中一种身份验证模式的示例，请参阅 [如何：为指定的身份验证模式创建 SecurityBindingElement](how-to-create-a-securitybindingelement-for-a-specified-authentication-mode.md)。  
  
## <a name="basic-configuration-programming"></a>基本配置编程  

 下面的过程说明如何在配置文件中设置身份验证模式。  
  
#### <a name="to-set-the-authentication-mode-in-configuration"></a>在配置中设置身份验证模式  
  
1. 向 [\<bindings>](../../configure-apps/file-schema/wcf/bindings.md) 元素添加一个 [\<customBinding>](../../configure-apps/file-schema/wcf/custombinding.md) 。  
  
2. 作为子元素，将元素添加 [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) 到 `<customBinding>` 元素。  
  
3. 向 `<security>` 元素中添加一个 `<binding>` 元素。  
  
4. 将 `authenticationMode` 属性设置为下述值之一。 例如，下面的代码将模式设置为 `AnonymousForCertificate`。  
  
    ```xml  
    <bindings>  
      <customBinding>  
        <binding name="SecureCustomBinding">  
         <security authenticationMode ="AnonymousForCertificate" />  
        </binding>  
      </customBinding>  
    </bindings>  
    ```  
  
#### <a name="to-set-the-mode-programmatically"></a>以编程方式设置模式  
  
1. 确定返回类型，可以是以下类型之一：<xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement>、<xref:System.ServiceModel.Channels.TransportSecurityBindingElement>、<xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement> 或 <xref:System.ServiceModel.Channels.SecurityBindingElement>。  
  
2. 调用 <xref:System.ServiceModel.Channels.SecurityBindingElement> 类的适当静态方法。 例如，下面的代码调用 <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateAnonymousForCertificateBindingElement%2A> 方法。  
  
     [!code-csharp[c_CustomBindingsAuthMode#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_custombindingsauthmode/cs/source.cs#3)]
     [!code-vb[c_CustomBindingsAuthMode#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_custombindingsauthmode/vb/source.vb#3)]  
  
3. 使用绑定元素创建自定义绑定。 有关详细信息，请参阅 [自定义绑定](../extending/custom-bindings.md)。  
  
## <a name="mode-descriptions"></a>模式说明  
  
### <a name="anonymousforcertificate"></a>AnonymousForCertificate  

 在此身份验证模式中，客户端是匿名的，因此使用 X.509 证书对服务进行身份验证。 安全绑定元素是由 <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> 方法返回的 <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateAnonymousForCertificateBindingElement%2A>。 或者，将 `authenticationMode` <> 元素的特性设置 `security` 为 `AnonymousForCertificate` 。  
  
### <a name="anonymousforsslnegotiated"></a>AnonymousForSslNegotiated  

 在此身份验证模式中，客户端是匿名的，因此使用在运行时进行协商的 X.509 证书对服务进行身份验证。 安全绑定元素是在为第一个参数传递 <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> 值时由 <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateSslNegotiationBindingElement%2A> 方法返回的 `false`。 或者，将 `authenticationMode` 属性设置为 `AnonymousForSslNegotiated`。  
  
### <a name="certificateovertransport"></a>CertificateOverTransport  

 在此身份验证模式中，客户端使用 X.509 证书进行身份验证，此证书作为认可的支持令牌（即签署消息签名的令牌）显示在 SOAP 层上。 在传输层，服务是用 X.509 证书进行身份验证的。 安全绑定元素是由 <xref:System.ServiceModel.Channels.TransportSecurityBindingElement> 方法返回的 <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateCertificateOverTransportBindingElement%2A>。 或者，将 `authenticationMode` 属性设置为 `CertificateOverTransport`。  
  
### <a name="issuedtoken"></a>IssuedToken  

 在此身份验证模式中，客户端不向服务进行身份验证，而是向安全令牌服务进行身份验证并接收一个 SAML 令牌，然后将其提供给服务器，以证明它知道共享密钥。 服务也不向客户端进行身份验证，但安全令牌服务会将共享密钥作为已颁发令牌的一部分进行加密，只有服务才能解密该密钥。 安全绑定元素是由 <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> 方法返回的 <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateIssuedTokenBindingElement%2A>。 或者，将 `authenticationMode` 属性设置为 `IssuedToken`。  
  
### <a name="issuedtokenforcertificate"></a>IssuedTokenForCertificate  

 在此身份验证模式中，客户端不向服务进行身份验证，而是向安全令牌服务进行身份验证并接收一个 SAML 令牌，然后将其提供给服务器，以证明它知道共享密钥。 颁发的令牌作为认可的支持令牌或持有者令牌（即签署消息签名的令牌）显示在 SOAP 层上。 服务使用 X.509 证书对客户端进行身份验证。 安全绑定元素是由 <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> 方法返回的 <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateIssuedTokenForCertificateBindingElement%2A>。 或者，将 `authenticationMode` 属性设置为 `IssuedTokenForCertificate`。  
  
### <a name="issuedtokenforsslnegotiated"></a>IssuedTokenForSslNegotiated  

 在此身份验证模式中，客户端不向服务进行身份验证，而是向安全令牌服务进行身份验证并接收一个 SAML 令牌，然后将其提供给服务器，以证明它知道共享密钥。 颁发的令牌作为认可的支持令牌或持有者令牌（即签署消息签名的令牌）显示在 SOAP 层上。 使用 X.509 证书对服务进行身份验证。 安全绑定元素是由 <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> 方法返回的 <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateIssuedTokenForSslBindingElement%2A>。 或者，将 `authenticationMode` 属性设置为 `IssuedTokenForSslnegotiated`。  
  
### <a name="issuedtokenovertransport"></a>IssuedTokenOverTransport  

 在此身份验证模式中，客户端不向服务进行身份验证，而是向安全令牌服务进行身份验证并接收一个 SAML 令牌，然后将其提供给服务器，以证明它知道共享密钥。 颁发的令牌作为认可的支持令牌或持有者令牌（即签署消息签名的令牌）显示在 SOAP 层上。 在传输层，服务是用 X.509 证书进行身份验证的。 安全绑定元素是由 `TransportSecurityBindingElement` 方法返回的 <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateIssuedTokenOverTransportBindingElement%2A>。 或者，将 `authenticationMode` 属性设置为 `IssuedTokenOverTransport`。  
  
### <a name="kerberos"></a>Kerberos  

 在此身份验证模式下，客户端使用 Kerberos 票证向服务进行身份验证。 该票证还提供服务器身份验证。 安全绑定元素是由 `SymmetricSecurityBindingElement` 方法返回的 <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateKerberosBindingElement%2A>。 或者，将 `authenticationMode` 属性设置为 `Kerberos`。  
  
> [!NOTE]
> 若要使用此身份验证模式，服务帐户必须与服务主体名称 (SPN) 关联。 为此，请在 NETWORK SERVICE 帐户或 LOCAL SYSTEM 帐户下运行服务。 也可以使用 SetSpn.exe 工具为服务帐户创建一个 SPN。 在任一情况下，客户端都必须在元素中使用正确的 SPN [\<servicePrincipalName>](../../configure-apps/file-schema/wcf/serviceprincipalname.md) ，或者使用 <xref:System.ServiceModel.EndpointAddress> 构造函数。 有关详细信息，请参阅 [服务标识和身份验证](service-identity-and-authentication.md)。  
  
> [!NOTE]
> 当使用 `Kerberos` 身份验证模式时，不支持 <xref:System.Security.Principal.TokenImpersonationLevel.Anonymous> 和 <xref:System.Security.Principal.TokenImpersonationLevel.Delegation> 模拟级别。  
  
### <a name="kerberosovertransport"></a>KerberosOverTransport  

 在此身份验证模式下，客户端使用 Kerberos 票证向服务进行身份验证。 Kerberos 令牌作为认可的支持令牌（即签署消息签名的令牌）显示在 SOAP 层上。 在传输层，服务是用 X.509 证书进行身份验证的。 安全绑定元素是由 `TransportSecurityBindingElement` 方法返回的 <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateKerberosOverTransportBindingElement%2A>。 或者，将 `authenticationMode` 属性设置为 `KerberosOverTransport`。  
  
> [!NOTE]
> 若要使用此身份验证模式，服务帐户必须与 SPN 关联。 为此，请在 NETWORK SERVICE 帐户或 LOCAL SYSTEM 帐户下运行服务。 也可以使用 SetSpn.exe 工具为服务帐户创建一个 SPN。 在任一情况下，客户端都必须在元素中使用正确的 SPN [\<servicePrincipalName>](../../configure-apps/file-schema/wcf/serviceprincipalname.md) ，或者使用 <xref:System.ServiceModel.EndpointAddress> 构造函数。 有关详细信息，请参阅 [服务标识和身份验证](service-identity-and-authentication.md)。  
  
### <a name="mutualcertificate"></a>MutualCertificate  

 在此身份验证模式中，客户端使用 X.509 证书进行身份验证，此证书作为认可的支持令牌（即签署消息签名的令牌）显示在 SOAP 层上。 同样使用 X.509 证书对服务进行身份验证。 安全绑定元素是由 `SymmetricSecurityBindingElement` 方法返回的 <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateMutualCertificateBindingElement%2A>。 或者，将 `authenticationMode` 属性设置为 `MutualCertificate`。  
  
### <a name="mutualcertificateduplex"></a>MutualCertificateDuplex  

 在此身份验证模式中，客户端使用 X.509 证书进行身份验证，此证书作为认可的支持令牌（即签署消息签名的令牌）显示在 SOAP 层上。 同样使用 X.509 证书对服务进行身份验证。 此绑定是由 `AsymmetricSecurityBindingElement` 方法返回的 <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateMutualCertificateDuplexBindingElement%2A>。 或者，将 `authenticationMode` 属性设置为 `MutualCertificateDuplex`。  
  
### <a name="mutualsslnegotiated"></a>MutualSslNegotiated  

 在此身份验证模式中，客户端和服务都使用 X.509 证书进行身份验证。 安全绑定元素是在为第一个参数传递 `SymmetricSecurityBindingElement` 值时由 <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateSslNegotiationBindingElement%2A> 方法返回的 `true`。 或者，将 `authenticationMode` 属性设置为 `MutualSslNegotiated`。  
  
### <a name="secureconversation"></a>SecureConversation  

 安全绑定元素是由 `SymmetricSecurityBindingElement` 方法返回的 <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateSecureConversationBindingElement%2A>。 此方法将 <xref:System.ServiceModel.Channels.SecurityBindingElement> 作为一个参数，在初始化过程中使用该参数建立安全会话。 或者，将 `authenticationMode` 属性设置为 `SecureConversation`。  
  
 如果未指定启动绑定，则会对启动使用 `SspiNegotiated` 身份验证模式。  
  
### <a name="sspinegotiation"></a>SspiNegotiation  

 在此身份验证模式中，使用协商协议来执行客户端和服务器身份验证。 可能时使用 Kerberos；否则使用 NT LanMan (NTLM)。 安全绑定元素是由 `SymmetricSecurityBindingElement` 方法返回的 <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateSspiNegotiationBindingElement%2A>。 或者，将 `authenticationMode` 属性设置为 `SspiNegotiated`。  
  
### <a name="sspinegotiatedovertransport"></a>SspiNegotiatedOverTransport  

 在此身份验证模式中，使用协商协议来执行客户端和服务器身份验证。 可能时使用 Kerberos 协议；否则使用 NTLM。 生成的令牌作为认可的支持令牌（即签署消息签名的令牌）显示在 SOAP 层上。 在传输层，服务还是由 X.509 证书另外进行身份验证。 安全绑定元素是由 `TransportSecurityBindingElement` 方法返回的 <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateSspiNegotiationOverTransportBindingElement%2A>。 或者，将 `authenticationMode` 属性设置为 `SspiNegotiatedOverTransport`。  
  
### <a name="usernameforcertificate"></a>UserNameForCertificate  

 在此身份验证模式中，客户端使用用户名令牌向服务进行身份验证，此证书作为经过签名的支持令牌（即签署消息签名的令牌）显示在 SOAP 层上。 服务使用 X.509 证书对客户端进行身份验证。 安全绑定元素是由 `SymmetricSecurityBindingElement` 方法返回的 <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateUserNameForCertificateBindingElement%2A>。 或者，将 `authenticationMode` 属性设置为 `UserNameForCertificate`。  
  
 对于 `UserNameForCertificate` 身份验证模式，客户端和服务都必须使用 WS-Security 1.1。  
  
### <a name="usernameforsslnegotiated"></a>UserNameForSslNegotiated  

 在此身份验证模式中，客户端使用用户名令牌进行身份验证，此证书作为经过签名的支持令牌（即签署消息签名的令牌）显示在 SOAP 层上。 使用 X.509 证书对服务进行身份验证。 安全绑定元素是由 `SymmetricSecurityBindingElement` 方法返回的 <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateUserNameForSslBindingElement%2A>。 或者，将 `authenticationMode` 属性设置为 `UserNameForSslNegotiated`。  
  
### <a name="usernameovertransport"></a>UserNameOverTransport  

 在此身份验证模式中，客户端使用用户名令牌进行身份验证，此证书作为经过签名的支持令牌（即签署消息签名的令牌）显示在 SOAP 层上。 在传输层，服务是用 X.509 证书进行身份验证的。 安全绑定元素是由 `TransportSecurityBindingElement` 方法返回的 <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateUserNameOverTransportBindingElement%2A>。 或者，将 `authenticationMode` 属性设置为 `UserNameOverTransport`。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Channels.SecurityBindingElement>
- [如何：为指定的身份验证模式创建 SecurityBindingElement](how-to-create-a-securitybindingelement-for-a-specified-authentication-mode.md)
