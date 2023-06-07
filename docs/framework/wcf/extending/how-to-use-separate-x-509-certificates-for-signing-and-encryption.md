---
title: 如何：使用独立的 X.509 证书进行签名和加密
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF, extensibility
- ClientCredentials class
- ClientCredentialsSecurityTokenManager class
ms.assetid: 0b06ce4e-7835-4d82-8baf-d525c71a0e49
ms.openlocfilehash: e464aff46f311ede1cd629fb459ade9a6e627d59
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/07/2019
ms.locfileid: "70796961"
---
# <a name="how-to-use-separate-x509-certificates-for-signing-and-encryption"></a>如何：使用独立的 X.509 证书进行签名和加密

本主题演示如何将 Windows Communication Foundation （WCF）配置为在客户端和服务上使用不同的消息签名和加密证书。

若要启用单独的证书来进行签名和加密，则必须创建自定义客户端或服务凭据（或两者），因为 WCF 不提供 API 来设置多个客户端或服务证书。 此外，还必须提供安全令牌管理器，以利用多个证书的信息并为指定的密钥用法和消息方向创建相应的安全令牌提供程序。

下图演示所用的主类、它们从其继承的类（由向上箭头指示）以及某些方法和属性的返回类型。

- `MyClientCredentials` 是 <xref:System.ServiceModel.Description.ClientCredentials> 的自定义实现。

  - 图中显示的其属性都返回 <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> 的实例。

  - 其方法 <xref:System.ServiceModel.Description.ClientCredentials.CreateSecurityTokenManager%2A> 返回 `MyClientCredentialsSecurityTokenManager` 的实例。

- `MyClientCredentialsSecurityTokenManager` 是 <xref:System.ServiceModel.ClientCredentialsSecurityTokenManager> 的自定义实现。

  - 其方法 <xref:System.ServiceModel.ClientCredentialsSecurityTokenManager.CreateSecurityTokenProvider%2A> 返回 <xref:System.IdentityModel.Selectors.X509SecurityTokenProvider> 的实例。

![显示如何使用客户端凭据的图表](./media/e4971edd-a59f-4571-b36f-7e6b2f0d610f.gif "e4971edd-a59f-4571-b36f-7e6b2f0d610f")

有关自定义凭据的详细信息， [请参阅演练：创建自定义客户端和](walkthrough-creating-custom-client-and-service-credentials.md)服务凭据。

此外，必须创建自定义标识验证程序，并将它链接到自定义绑定中的安全绑定元素。 还必须使用自定义凭据而不是默认凭据。

下图演示在自定义绑定中涉及的类，以及如何链接自定义标识验证程序。 涉及到几个绑定元素，它们都继承自 <xref:System.ServiceModel.Channels.BindingElement>。 <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement> 具有 <xref:System.ServiceModel.Channels.LocalClientSecuritySettings> 属性，该属性返回 <xref:System.ServiceModel.Security.IdentityVerifier> 的实例（从其自定义 `MyIdentityVerifier`）。

![显示自定义绑定元素的图表](./media/dddea4a2-0bb4-4921-9bf4-20d4d82c3da5.gif "dddea4a2-0bb4-4921-9bf4-20d4d82c3da5")

有关创建自定义标识验证程序的详细信息，请参阅如何：[如何：创建自定义客户端标识](how-to-create-a-custom-client-identity-verifier.md)验证程序。

### <a name="to-use-separate-certificates-for-signing-and-encryption"></a>使用独立的证书进行签名和加密

1. 定义从 <xref:System.ServiceModel.Description.ClientCredentials> 类继承的新客户端凭据类。 实现四个新属性以允许指定多个证书：`ClientSigningCertificate`、`ClientEncryptingCertificate`、`ServiceSigningCertificate` 和 `ServiceEncryptingCertificate`。 还重写 <xref:System.ServiceModel.Description.ClientCredentials.CreateSecurityTokenManager%2A> 方法以返回在下一步中定义的自定义 <xref:System.ServiceModel.ClientCredentialsSecurityTokenManager> 类的实例。

     [!code-csharp[c_FourCerts#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_fourcerts/cs/source.cs#1)]
     [!code-vb[c_FourCerts#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_fourcerts/vb/source.vb#1)]

2. 定义从 <xref:System.ServiceModel.ClientCredentialsSecurityTokenManager> 类继承的新客户端安全令牌管理器。 重写 <xref:System.ServiceModel.ClientCredentialsSecurityTokenManager.CreateSecurityTokenProvider%2A> 方法以创建相应的安全令牌提供程序。 `requirement` 参数（一个 <xref:System.IdentityModel.Selectors.SecurityTokenRequirement>）提供消息方向和密钥用法。

     [!code-csharp[c_FourCerts#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_fourcerts/cs/source.cs#2)]
     [!code-vb[c_FourCerts#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_fourcerts/vb/source.vb#2)]

3. 定义从 <xref:System.ServiceModel.Description.ServiceCredentials> 类继承的新服务凭据类。 实现四个新属性以允许指定多个证书：`ClientSigningCertificate`、`ClientEncryptingCertificate`、`ServiceSigningCertificate` 和 `ServiceEncryptingCertificate`。 还重写 <xref:System.ServiceModel.Description.ServiceCredentials.CreateSecurityTokenManager%2A> 方法以返回在下一步中定义的自定义 <xref:System.ServiceModel.Security.ServiceCredentialsSecurityTokenManager> 类的实例。

     [!code-csharp[c_FourCerts#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_fourcerts/cs/source.cs#3)]
     [!code-vb[c_FourCerts#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_fourcerts/vb/source.vb#3)]

4. 定义从 <xref:System.ServiceModel.Security.ServiceCredentialsSecurityTokenManager> 类继承的新服务安全令牌管理器。 如果给定传入消息的方向和密钥用法，则重写 <xref:System.ServiceModel.Security.ServiceCredentialsSecurityTokenManager.CreateSecurityTokenProvider%2A> 方法以创建相应的安全令牌提供程序。

     [!code-csharp[c_FourCerts#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_fourcerts/cs/source.cs#4)]
     [!code-vb[c_FourCerts#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_fourcerts/vb/source.vb#4)]

### <a name="to-use-multiple-certificates-on-the-client"></a>在客户端上使用多个证书

1. 创建自定义绑定。 安全绑定元素必须以双工模式运行，以允许为请求和响应采用不同的安全令牌提供程序。 执行此操作的一种方法是使用具有双工能力的传输，或使用下面的代码所示的 <xref:System.ServiceModel.Channels.CompositeDuplexBindingElement>。 将在下一步中定义的自定义 <xref:System.ServiceModel.Security.IdentityVerifier> 链接到安全绑定元素。 将默认客户端凭据替换为以前创建的自定义客户端凭据。

     [!code-csharp[c_FourCerts#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_fourcerts/cs/source.cs#5)]
     [!code-vb[c_FourCerts#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_fourcerts/vb/source.vb#5)]

2. 定义一个自定义的 <xref:System.ServiceModel.Security.IdentityVerifier>。 因为使用不同的证书来加密请求和对响应进行签名，所以服务具有多个标识。

    > [!NOTE]
    > 在下面的示例中，出于演示的目的，提供的自定义标识验证程序不执行任何终结点标识检查。 建议在生产代码中不要这样做。

     [!code-csharp[c_FourCerts#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_fourcerts/cs/source.cs#6)]
     [!code-vb[c_FourCerts#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_fourcerts/vb/source.vb#6)]

### <a name="to-use-multiple-certificates-on-the-service"></a>在服务上使用多个证书

1. 创建自定义绑定。 安全绑定元素必须以双工模式运行，以允许为请求和响应采用不同的安全令牌提供程序。 像客户端一样使用具有双工能力的传输，或使用下面的代码所示的 <xref:System.ServiceModel.Channels.CompositeDuplexBindingElement>。 将默认服务凭据替换为以前创建的自定义服务凭据。

     [!code-csharp[c_FourCerts#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_fourcerts/cs/source.cs#7)]
     [!code-vb[c_FourCerts#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_fourcerts/vb/source.vb#7)]

## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Description.ClientCredentials>
- <xref:System.ServiceModel.Description.ServiceCredentials>
- <xref:System.ServiceModel.ClientCredentialsSecurityTokenManager>
- <xref:System.ServiceModel.Security.ServiceCredentialsSecurityTokenManager>
- <xref:System.ServiceModel.Security.IdentityVerifier>
- [演练：创建自定义客户端和服务凭据](walkthrough-creating-custom-client-and-service-credentials.md)
