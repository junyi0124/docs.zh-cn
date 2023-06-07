---
title: 如何：创建联合客户端
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF, federation
- federation
ms.assetid: 56ece47e-98bf-4346-b92b-fda1fc3b4d9c
ms.openlocfilehash: a03d388f2773e312a149b5caf1747627d1c17864
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96286618"
---
# <a name="how-to-create-a-federated-client"></a>如何：创建联合客户端

在 Windows Communication Foundation (WCF) 中，为 *联合服务* 创建客户端包括三个主要步骤：  
  
1. 配置 [\<wsFederationHttpBinding>](../../configure-apps/file-schema/wcf/wsfederationhttpbinding.md) 或类似的自定义绑定。 有关创建适当绑定的详细信息，请参阅 [如何：创建 WSFederationHttpBinding](how-to-create-a-wsfederationhttpbinding.md)。 另外，还可以对联合服务的元数据终结点运行 " [)  ( ](../servicemodel-metadata-utility-tool-svcutil-exe.md) " "配置文件" 实用工具，以生成配置文件，以便与联合服务以及一个或多个安全令牌服务进行通信。  
  
2. 设置 <xref:System.ServiceModel.Security.IssuedTokenClientCredential> 的属性，它可以控制客户端与安全令牌服务之间交互的各个方面。  
  
3. 设置 <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential> 的属性，它允许使用所需的证书来与给定终结点（例如安全令牌服务）进行安全通信。  
  
> [!NOTE]
> 当客户端使用模拟凭据、<xref:System.Security.Cryptography.CryptographicException> 绑定或自定义颁发令牌以及非对称密钥时，可能会引发 <xref:System.ServiceModel.WSFederationHttpBinding>。 当将 <xref:System.ServiceModel.WSFederationHttpBinding> 和 <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.IssuedKeyType%2A> 属性分别设置为 <xref:System.ServiceModel.Security.Tokens.IssuedSecurityTokenParameters.KeyType%2A> 时，将非对称密钥与 <xref:System.IdentityModel.Tokens.SecurityKeyType.AsymmetricKey> 绑定和自定义颁发令牌一起使用。 当客户端尝试发送消息，而客户端模拟的标识不存在对应的用户配置文件时，将引发 <xref:System.Security.Cryptography.CryptographicException>。 若要缓解此问题，请在发送消息之前登录客户端计算机或调用 `LoadUserProfile`。  
  
 本主题介绍有关这些过程的详细信息。 有关创建适当绑定的详细信息，请参阅 [如何：创建 WSFederationHttpBinding](how-to-create-a-wsfederationhttpbinding.md)。 有关联合 [身份验证](federation.md)服务工作原理的详细信息，请参阅联合。  
  
### <a name="to-generate-and-examine-the-configuration-for-a-federated-service"></a>生成并检查联合服务的配置  
  
1. 使用服务的元数据 URL 的地址作为命令行参数，运行 " [)  ( ](../servicemodel-metadata-utility-tool-svcutil-exe.md) "。  
  
2. 在适当的编辑器中打开生成的配置文件。  
  
3. 检查任何生成的和元素的特性和内容 [\<issuer>](../../configure-apps/file-schema/wcf/issuer.md) [\<issuerMetadata>](../../configure-apps/file-schema/wcf/issuermetadata.md) 。 它们位于 [\<security>](../../configure-apps/file-schema/wcf/security-of-wsfederationhttpbinding.md) [\<wsFederationHttpBinding>](../../configure-apps/file-schema/wcf/wsfederationhttpbinding.md) 或自定义绑定元素的元素内。 确保地址包含所需的域名或其他地址信息。 一定要检查此信息，因为客户端将对这些地址进行身份验证，可能会泄露信息（例如用户名/密码对）。 如果地址不是预期的地址，则会导致将信息泄漏给非目标接收方。  
  
4. 检查 [\<issuedTokenParameters>](../../configure-apps/file-schema/wcf/issuedtokenparameters.md) 注释掉 <> 元素内的任何其他元素 `alternativeIssuedTokenParameters` 。 当使用 Svcutil.exe 工具生成联合服务的配置时，如果联合服务或任何中间安全令牌服务没有指定颁发者地址，而是指定公开了多个终结点的安全令牌服务的元数据地址，则生成的配置文件将引用第一个终结点。 其他终结点在配置文件中作为注释掉 <`alternativeIssuedTokenParameters`> 元素。  
  
     确定这些 <> 中是否有一个更适合 `issuedTokenParameters` 于配置中已存在的那个。 例如，客户端可能更倾向于使用 Windows CardSpace 令牌而不是用户名/密码对 security token service 进行身份验证。  
  
    > [!NOTE]
    > 对于在与服务进行通信之前必须遍历多个安全令牌服务的情况，中间安全令牌服务有可能会将客户端引导到不正确的安全令牌服务。 因此，请确保中 security token service 的终结点 [\<issuedTokenParameters>](../../configure-apps/file-schema/wcf/issuedtokenparameters.md) 是预期的 security token service 而不是未知的 security token service。  
  
### <a name="to-configure-an-issuedtokenclientcredential-in-code"></a>在代码中配置 IssuedTokenClientCredential  
  
1. 通过 <xref:System.ServiceModel.Security.IssuedTokenClientCredential> 类的 <xref:System.ServiceModel.Description.ClientCredentials.IssuedToken%2A> 属性（通过 <xref:System.ServiceModel.Description.ClientCredentials> 类的 <xref:System.ServiceModel.ClientBase%601.ClientCredentials%2A> 属性或通过 <xref:System.ServiceModel.ClientBase%601> 类返回）访问 <xref:System.ServiceModel.ChannelFactory>，如以下示例代码中所示。  
  
     [!code-csharp[c_CreateSTS#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#9)]
     [!code-vb[c_CreateSTS#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#9)]  
  
2. 如果不需要缓存令牌，则将 <xref:System.ServiceModel.Security.IssuedTokenClientCredential.CacheIssuedTokens%2A> 属性设置为 `false`。 <xref:System.ServiceModel.Security.IssuedTokenClientCredential.CacheIssuedTokens%2A> 属性控制是否缓存安全令牌服务中的这类令牌。 如果将此属性设置为 `false`，则只要客户端必须向联合服务对自己重新进行身份验证，客户端就会从安全令牌服务请求一个新令牌，而不管上一个令牌是否仍然有效。 如果将此属性设置为 `true`，则只要客户端必须向联合服务对自己重新进行身份验证，客户端就会重用现有的令牌（只要令牌未过期）。 默认值为 `true`。  
  
3. 如果缓存令牌需要一个时间限制，则将 <xref:System.ServiceModel.Security.IssuedTokenClientCredential.MaxIssuedTokenCachingTime%2A> 属性设置为 <xref:System.TimeSpan> 值。 该属性指定可以缓存令牌的时间。 在指定的时间间隔结束之后，将从客户端缓存中移除令牌。 默认情况下，缓存令牌的时间将不限。 下面的示例将时间间隔设置为 10 分钟。  
  
     [!code-csharp[c_CreateSTS#15](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#15)]
     [!code-vb[c_CreateSTS#15](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#15)]  
  
4. 可选。 将 <xref:System.ServiceModel.Security.IssuedTokenClientCredential.IssuedTokenRenewalThresholdPercentage%2A> 设置为一个百分比。 默认值为 60（百分比）。 该属性指定令牌有效期的百分比。 例如，如果颁发的令牌在 10 个小时内有效，并将 <xref:System.ServiceModel.Security.IssuedTokenClientCredential.IssuedTokenRenewalThresholdPercentage%2A> 设置为 80，则在 8 小时后续订令牌。 下面的示例将值设置为 80％。  
  
     [!code-csharp[c_CreateSTS#16](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#16)]
     [!code-vb[c_CreateSTS#16](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#16)]  
  
     在缓存时间少于续订阈值时间的情况下，`IssuedTokenRenewalThresholdPercentage` 值将重写通过令牌有效期和 `MaxIssuedTokenCachingTime` 值确定的续订时间间隔。 例如，如果 `IssuedTokenRenewalThresholdPercentage` 与令牌持续时间的乘积是 8 个小时，而 `MaxIssuedTokenCachingTime` 值为 10 分钟，则客户端每 10 分钟就会联系安全令牌服务以获取更新的令牌。  
  
5. 如果在一个绑定上需要除 <xref:System.ServiceModel.Security.SecurityKeyEntropyMode.CombinedEntropy> 之外的密钥平均信息量模式，并且该绑定对消息凭据不使用消息安全或传输安全（例如， 该绑定没有 <xref:System.ServiceModel.Channels.SecurityBindingElement>），则将 <xref:System.ServiceModel.Security.IssuedTokenClientCredential.DefaultKeyEntropyMode%2A> 属性设置为适当的值。 *熵* 模式确定是否可以使用属性控制对称密钥 <xref:System.ServiceModel.Security.IssuedTokenClientCredential.DefaultKeyEntropyMode%2A> 。 此默认值为 <xref:System.ServiceModel.Security.SecurityKeyEntropyMode.CombinedEntropy>，其中，客户端和令牌颁发者都提供数据，并将这些数据组合在一起以生成实际密钥。 其他值为 <xref:System.ServiceModel.Security.SecurityKeyEntropyMode.ClientEntropy> 和 <xref:System.ServiceModel.Security.SecurityKeyEntropyMode.ServerEntropy>，这意味着整个密钥由客户端或服务器分别指定。 下面的示例将该属性设置为只使用服务器数据来生成密钥。  
  
     [!code-csharp[c_CreateSTS#17](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#17)]
     [!code-vb[c_CreateSTS#17](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#17)]  
  
    > [!NOTE]
    > 如果在安全令牌服务或服务绑定中存在 <xref:System.ServiceModel.Channels.SecurityBindingElement>，则通过 <xref:System.ServiceModel.Security.IssuedTokenClientCredential.DefaultKeyEntropyMode%2A> 的 <xref:System.ServiceModel.Security.IssuedTokenClientCredential> 属性来重写 <xref:System.ServiceModel.Channels.SecurityBindingElement.KeyEntropyMode%2A> 上的 `SecurityBindingElement` 集。  
  
6. 配置任何特定于颁发者的终结点行为，方法是将这些行为添加到由 <xref:System.ServiceModel.Security.IssuedTokenClientCredential.IssuerChannelBehaviors%2A> 属性返回的集合中。  
  
     [!code-csharp[c_CreateSTS#14](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#14)]
     [!code-vb[c_CreateSTS#14](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#14)]  
  
### <a name="to-configure-the-issuedtokenclientcredential-in-configuration"></a>在配置中配置 IssuedTokenClientCredential  
  
1. [\<issuedToken>](../../configure-apps/file-schema/wcf/issuedtoken.md) [\<issuedToken>](../../configure-apps/file-schema/wcf/issuedtoken.md) 在终结点行为中创建元素作为元素的子元素。  
  
2. 如果不需要标记缓存，请将 `cacheIssuedTokens` <> 元素) 的特性 (设置 `issuedToken` 为 `false` 。  
  
3. 如果缓存的令牌需要时间限制，请将 `maxIssuedTokenCachingTime` <> 元素上的属性设置 `issuedToken` 为合适的值。 例如：  
    `<issuedToken maxIssuedTokenCachingTime='00:10:00' />`  
  
4. 如果首选值不是默认值，请将 `issuedTokenRenewalThresholdPercentage` <> 元素上的属性设置 `issuedToken` 为适当的值，例如：  
  
    ```xml  
    <issuedToken issuedTokenRenewalThresholdPercentage = "80" />  
    ```  
  
5. 如果在一个绑定上需要除 `CombinedEntropy` 之外的密钥平均信息量模式，并且该绑定对消息凭据不使用消息安全或传输安全（例如，该绑定没有 `SecurityBindingElement`），则根据需要将 `defaultKeyEntropyMode` 元素上的 `<issuedToken>` 属性设置为 `ServerEntropy` 或 `ClientEntropy`。  
  
    ```xml  
    <issuedToken defaultKeyEntropyMode = "ServerEntropy" />  
    ```  
  
6. 可选。 通过创建一个 <`issuerChannelBehaviors`> 元素作为 <> 元素的子元素来配置任何特定于颁发者的自定义终结点行为 `issuedToken` 。 对于每个行为，请创建一个 <`add`> 元素作为 <> 元素的子元素 `issuerChannelBehaviors` 。 通过在 `issuerAddress` <> 元素上设置属性，指定行为的颁发者地址 `add` 。 通过在 `behaviorConfiguration` <> 元素上设置特性来指定行为本身 `add` 。  
  
    ```xml  
    <issuerChannelBehaviors>  
    <add issuerAddress="http://fabrikam.org/sts" behaviorConfiguration="FabrikamSTS" />  
    </issuerChannelBehaviors>  
    ```  
  
### <a name="to-configure-an-x509certificaterecipientclientcredential-in-code"></a>在代码中配置 X509CertificateRecipientClientCredential  
  
1. 通过 <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential> 类的 <xref:System.ServiceModel.Description.ClientCredentials.ServiceCertificate%2A> 属性的 <xref:System.ServiceModel.ClientBase%601.ClientCredentials%2A> 属性或 <xref:System.ServiceModel.ClientBase%601> 属性来访问 <xref:System.ServiceModel.ChannelFactory>。  
  
     [!code-csharp[c_CreateSTS#18](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#18)]
     [!code-vb[c_CreateSTS#18](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#18)]  
  
2. 如果 <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> 实例可用于给定终结点的证书，则使用通过 <xref:System.Collections.Generic.ICollection%601.Add%2A> 属性返回的集合的 <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential.ScopedCertificates%2A> 方法。  
  
     [!code-csharp[c_CreateSTS#19](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#19)]
     [!code-vb[c_CreateSTS#19](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#19)]  
  
3. 如果 <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> 实例不可用，则使用 <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential.SetScopedCertificate%2A> 类的 <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential> 方法，如以下示例中所示。  
  
     [!code-csharp[c_CreateSTS#20](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#20)]
     [!code-vb[c_CreateSTS#20](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#20)]  
  
### <a name="to-configure-an-x509certificaterecipientclientcredential-in-configuration"></a>在配置中配置 X509CertificateRecipientClientCredential  
  
1. 创建一个 [\<scopedCertificates>](../../configure-apps/file-schema/wcf/scopedcertificates-element.md) 元素 [\<serviceCertificate>](../../configure-apps/file-schema/wcf/servicecertificate-of-clientcredentials-element.md) ，作为元素的子元素，该元素本身是 [\<clientCredentials>](../../configure-apps/file-schema/wcf/clientcredentials.md) 终结点行为中的元素的子元素。  
  
2. 创建一个 `<add>` 元素，作为 `<scopedCertificates>` 元素的子元素。 指定 `storeLocation`、`storeName`、`x509FindType` 和 `findValue` 属性的值以引用适当的证书。 将 `targetUri` 属性设置为一个值，该值提供证书所要用于的终结点的地址，如以下示例中所示。  
  
    ```xml  
    <scopedCertificates>  
     <add targetUri="http://fabrikam.com/sts"
          storeLocation="CurrentUser"  
          storeName="TrustedPeople"  
          x509FindType="FindBySubjectName"  
          findValue="FabrikamSTS" />  
    </scopedCertificates>  
    ```  
  
## <a name="example"></a>示例  

 下面的代码示例在代码中配置 <xref:System.ServiceModel.Security.IssuedTokenClientCredential> 类的实例。  
  
 [!code-csharp[c_FederatedClient#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_federatedclient/cs/source.cs#2)]
 [!code-vb[c_FederatedClient#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_federatedclient/vb/source.vb#2)]  
  
## <a name="net-framework-security"></a>.NET Framework 安全性  

 要防止可能的信息泄漏，运行 Svcutil.exe 工具以处理联合终结点中的元数据的客户端应确保生成的安全令牌服务地址是预期的地址。 在安全令牌服务公开多个终结点时，这一点尤为重要，因为 Svcutil.exe 工具产生的最终配置文件会使用第一个此类终结点，而该终结点有可能不是客户端应在使用的那个终结点。  
  
## <a name="localissuer-required"></a>必需的 LocalIssuer  

 如果客户端始终应使用一个本地颁发者，则请注意以下事项：如果链中的倒数第二个安全令牌服务指定了一个颁发者地址或颁发者元数据地址，则 Svcutil.exe 的默认输出会导致使用的不是本地颁发者。  
  
 有关设置 <xref:System.ServiceModel.Security.IssuedTokenClientCredential.LocalIssuerAddress%2A> 类的、 <xref:System.ServiceModel.Security.IssuedTokenClientCredential.LocalIssuerBinding%2A> 和 <xref:System.ServiceModel.Security.IssuedTokenClientCredential.LocalIssuerChannelBehaviors%2A> 属性的详细信息 <xref:System.ServiceModel.Security.IssuedTokenClientCredential> ，请参阅 [如何：配置本地颁发者](how-to-configure-a-local-issuer.md)。  
  
## <a name="scoped-certificates"></a>作用域证书  

 如果必须指定服务证书才能与任何安全令牌服务进行通信，则通常是因为没有使用证书协商，可以使用 <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential.ScopedCertificates%2A> 类的 <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential> 属性来指定证书。 <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential.SetDefaultCertificate%2A> 方法采用 <xref:System.Uri> 和 <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> 作为参数。 在与指定的 URI 处的终结点进行通信时将使用指定的证书。 或者，可以使用 <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential.SetScopedCertificate%2A> 方法将证书添加到由 <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential.ScopedCertificates%2A> 属性返回的集合中。  
  
> [!NOTE]
> 作用域限定为给定的 URI 的证书的客户端想法仅适用于对服务（这些服务公开这些 URI 处的终结点）进行出站调用的应用程序。 它不适用于对颁发的令牌进行签名的证书，例如在服务器上的服务器中配置的、由类的返回的证书 <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.KnownCertificates%2A> <xref:System.ServiceModel.Security.IssuedTokenServiceCredential> 。 有关详细信息，请参阅 [如何：在联合身份验证服务上配置凭据](how-to-configure-credentials-on-a-federation-service.md)。  
  
## <a name="see-also"></a>另请参阅

- [联合示例](../samples/federation-sample.md)
- [如何：在 WSFederationHttpBinding 上禁用安全会话](how-to-disable-secure-sessions-on-a-wsfederationhttpbinding.md)
- [如何：创建 WSFederationHttpBinding](how-to-create-a-wsfederationhttpbinding.md)
- [如何：在联合身份验证服务上配置凭据](how-to-configure-credentials-on-a-federation-service.md)
- [如何：配置本地颁发者](how-to-configure-a-local-issuer.md)
- [元数据的安全注意事项](security-considerations-with-metadata.md)
- [如何：保护元数据终结点](how-to-secure-metadata-endpoints.md)
