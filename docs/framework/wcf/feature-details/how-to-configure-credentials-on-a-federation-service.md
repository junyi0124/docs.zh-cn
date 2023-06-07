---
title: 如何：在联合身份验证服务上配置凭据
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF, federation
- federation
ms.assetid: 149ab165-0ef3-490a-83a9-4322a07bd98a
ms.openlocfilehash: 692ccc0c39ca7ed40601551ea6bbcdd840fa03af
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96257582"
---
# <a name="how-to-configure-credentials-on-a-federation-service"></a>如何：在联合身份验证服务上配置凭据

在 Windows Communication Foundation (WCF) ，创建联合服务包含以下主要过程：  
  
1. 配置 <xref:System.ServiceModel.WSFederationHttpBinding> 或类似的自定义绑定。 有关创建适当绑定的详细信息，请参阅 [如何：创建 WSFederationHttpBinding](how-to-create-a-wsfederationhttpbinding.md)。  
  
2. 配置 <xref:System.ServiceModel.Security.IssuedTokenServiceCredential>，它控制如何对提供给服务的已颁发令牌进行身份验证。  
  
 本主题提供有关第二个步骤的详细信息。 有关联合 [身份验证](federation.md)服务工作原理的详细信息，请参阅联合。  
  
### <a name="to-set-the-properties-of-issuedtokenservicecredential-in-code"></a>在代码中设置 IssuedTokenServiceCredential 的属性  
  
1. 使用 <xref:System.ServiceModel.Description.ServiceCredentials.IssuedTokenAuthentication%2A> 类的 <xref:System.ServiceModel.Description.ServiceCredentials> 属性返回对 <xref:System.ServiceModel.Security.IssuedTokenServiceCredential> 实例的引用。 该属性可从 <xref:System.ServiceModel.ServiceHostBase.Credentials%2A> 类的 <xref:System.ServiceModel.ServiceHostBase> 属性访问。  
  
2. <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.AllowUntrustedRsaIssuers%2A> `true` 如果要对自行颁发的令牌（如 CardSpace 卡）进行身份验证，请将属性设置为。 默认值为 `false`。  
  
3. 用 <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.KnownCertificates%2A> 类的实例填充由 <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> 属性返回的集合。 每个实例表示一个颁发者，服务将从该颁发者对令牌进行身份验证。  
  
    > [!NOTE]
    > 与 <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential.ScopedCertificates%2A> 属性返回的客户端集合不同，已知证书集合不是键控集合。 不管发送包含已颁发令牌的消息的客户端地址是什么，服务都接受指定证书颁发的令牌（还受其他限制，这一点将在本主题的后面说明）。  
  
4. 将 <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.CertificateValidationMode%2A> 属性设置为 <xref:System.ServiceModel.Security.X509CertificateValidationMode> 枚举值之一。 这只能在代码中完成。 默认值为 <xref:System.IdentityModel.Selectors.X509CertificateValidator.ChainTrust%2A>。  
  
5. 如果 <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.CertificateValidationMode%2A> 属性设置为 <xref:System.ServiceModel.Security.X509CertificateValidationMode.Custom>，则将自定义 <xref:System.IdentityModel.Selectors.X509CertificateValidator> 类的一个实例分配给 <xref:System.ServiceModel.Security.X509ServiceCertificateAuthentication.CustomCertificateValidator%2A> 属性。  
  
6. 如果 <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.CertificateValidationMode%2A> 设置为 `ChainTrust` 或 `PeerOrChainTrust`，则将 <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.RevocationMode%2A> 属性设置为 <xref:System.Security.Cryptography.X509Certificates.X509RevocationMode> 枚举中的适当值。 请注意，在 `PeerTrust` 或 `Custom` 验证模式中不使用吊销模式。  
  
7. 如果需要，将自定义 <xref:System.IdentityModel.Tokens.SamlSerializer> 类的一个实例分配给 <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.SamlSerializer%2A> 属性。 例如，如果要分析自定义 SAML 断言，则需要自定义安全断言标记语言 (SAML) 序列化程序。  
  
### <a name="to-set-the-properties-of-issuedtokenservicecredential-in-configuration"></a>在配置中设置 IssuedTokenServiceCredential 的属性  
  
1. 创建一个 `<issuedTokenAuthentication>` 元素作为 <> 元素的子元素 `serviceCredentials` 。  
  
2. `allowUntrustedRsaIssuers` `<issuedTokenAuthentication>` `true` 如果对自行颁发的令牌（如 CardSpace 卡）进行身份验证，请将元素的属性设置为。  
  
3. 创建一个 `<knownCertificates>` 元素，作为 `<issuedTokenAuthentication>` 元素的子元素。  
  
4. 创建零个或多个 `<add>` 元素作为 `<knownCertificates>` 元素的子项，并指定如何使用 `storeLocation`、`storeName`、`x509FindType` 和 `findValue` 属性来定位证书。  
  
5. 如有必要，请将 `samlSerializer` <> 元素的属性设置 `issuedTokenAuthentication` 为自定义类的类型名称 <xref:System.IdentityModel.Tokens.SamlSerializer> 。  
  
## <a name="example"></a>示例  

 下面的示例在代码中设置 <xref:System.ServiceModel.Security.IssuedTokenServiceCredential> 的属性。  
  
 [!code-csharp[C_FederatedService#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_federatedservice/cs/source.cs#2)]
 [!code-vb[C_FederatedService#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_federatedservice/vb/source.vb#2)]  
  
 为了使联合服务能够对客户端进行身份验证，必须满足有关已颁发令牌的下列各项条件：  
  
- 如果已颁发令牌的数字签名使用 RSA 安全密钥标识符，则 <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.AllowUntrustedRsaIssuers%2A> 属性必须为 `true`。  
  
- 如果已颁发令牌的签名使用 X.509 颁发者序列号、X.509 主题密钥标识符或 X.509 指纹安全标识符，则已颁发的令牌必须由 <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.KnownCertificates%2A> 类的 <xref:System.ServiceModel.Security.IssuedTokenServiceCredential> 属性返回的集合中的证书进行签名。  
  
- 如果使用 X.509 证书对已颁发的令牌进行签名，该证书必须按照由 <xref:System.ServiceModel.Security.X509ServiceCertificateAuthentication.CertificateValidationMode%2A> 属性的值指定的语义进行验证，无论该证书是作为 <xref:System.IdentityModel.Tokens.X509RawDataKeyIdentifierClause> 发送给依赖方还是从 <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.KnownCertificates%2A> 属性获取。 有关 x.509 证书验证的详细信息，请参阅使用 [证书](working-with-certificates.md)。  
  
 例如，将 <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.CertificateValidationMode%2A> 设置为 <xref:System.ServiceModel.Security.X509CertificateValidationMode.PeerTrust> 将对签名证书位于 `TrustedPeople` 证书存储中的任何已颁发令牌进行身份验证。 在这种情况下，请将 <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.TrustedStoreLocation%2A> 属性设置为 <xref:System.Security.Cryptography.X509Certificates.StoreLocation.CurrentUser> 或 <xref:System.Security.Cryptography.X509Certificates.StoreLocation.LocalMachine>。 您可以选择其他模式，包括 <xref:System.ServiceModel.Security.X509CertificateValidationMode.Custom>。 如果选择了 `Custom`，则必须将 <xref:System.IdentityModel.Selectors.X509CertificateValidator> 类的一个实例分配给 <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.CustomCertificateValidator%2A> 属性。 自定义验证程序可以使用它喜欢的任何条件来验证证书。 有关详细信息，请参阅 [如何：创建使用自定义证书验证程序的服务](../extending/how-to-create-a-service-that-employs-a-custom-certificate-validator.md)。  
  
## <a name="see-also"></a>另请参阅

- [联合](federation.md)
- [联合与信任](federation-and-trust.md)
- [联合示例](../samples/federation-sample.md)
- [如何：在 WSFederationHttpBinding 上禁用安全会话](how-to-disable-secure-sessions-on-a-wsfederationhttpbinding.md)
- [如何：创建 WSFederationHttpBinding](how-to-create-a-wsfederationhttpbinding.md)
- [如何：创建联合客户端](how-to-create-a-federated-client.md)
- [使用证书](working-with-certificates.md)
- [SecurityBindingElement 身份验证模式](securitybindingelement-authentication-modes.md)
