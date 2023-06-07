---
title: 如何：更改 X.509 证书私钥的加密提供程序
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- cryptographic provider [WCF], changing
- cryptographic provider [WCF]
ms.assetid: b4254406-272e-4774-bd61-27e39bbb6c12
ms.openlocfilehash: 33d42f26407787b26e1447f8b8f619dd6fc15229
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96262107"
---
# <a name="how-to-change-the-cryptographic-provider-for-an-x509-certificates-private-key"></a>如何：更改 X.509 证书私钥的加密提供程序

本主题说明如何更改用于提供 x.509 证书私钥的加密提供程序，以及如何将该提供程序集成到 Windows Communication Foundation (WCF) 安全框架中。 有关使用证书的详细信息，请参阅使用 [证书](../feature-details/working-with-certificates.md)。  
  
 WCF 安全框架提供了一种方法来引入新的安全令牌类型，如 [如何：创建自定义令牌](how-to-create-a-custom-token.md)中所述。 还可以使用自定义令牌替换系统提供的现有令牌类型。  
  
 在本主题中，系统提供的 X.509 安全令牌替换为自定义 X.509 令牌，此自定义令牌提供了一种不同的证书私钥的实现。 当实际私钥是由默认 Windows 加密提供程序之外的其他加密提供程序提供时，这是非常有用的。 可选的加密提供程序的一个示例是硬件安全模块，该模块执行所有私钥相关的加密操作并且没有将私钥存储在内存中，从而提高了系统的安全性。  
  
 下面的示例仅供演示使用。 它不会替换默认的 Windows 加密提供程序，而只是演示在哪种情况下可以集成这样一个提供程序。  
  
## <a name="procedures"></a>过程  

 每个具有一个或多个关联的安全密钥的安全令牌都必须实现 <xref:System.IdentityModel.Tokens.SecurityToken.SecurityKeys%2A> 属性，以便从此安全令牌实例返回一个密钥集合。 如果此令牌为 X.509 安全令牌，则集合包含一个 <xref:System.IdentityModel.Tokens.X509AsymmetricSecurityKey> 类的实例，此类表示与证书关联的公钥和私钥。 若要替换用于提供证书密钥的默认的加密提供程序，请创建此类的一个新实现。  
  
#### <a name="to-create-a-custom-x509-asymmetric-key"></a>创建自定义 X.509 非对称密钥  
  
1. 定义一个从 <xref:System.IdentityModel.Tokens.X509AsymmetricSecurityKey> 类派生的新类。  
  
2. 重写 <xref:System.IdentityModel.Tokens.SecurityKey.KeySize%2A> 只读属性。 此属性返回证书的公钥/私钥对的实际密钥大小。  
  
3. 重写 <xref:System.IdentityModel.Tokens.SecurityKey.DecryptKey%2A> 方法。 WCF 安全框架调用此方法以使用证书的私钥对对称密钥进行解密。 （此密钥之前使用证书的公钥进行加密。）  
  
4. 重写 <xref:System.IdentityModel.Tokens.AsymmetricSecurityKey.GetAsymmetricAlgorithm%2A> 方法。 WCF 安全框架调用此方法来获取 <xref:System.Security.Cryptography.AsymmetricAlgorithm> 类的实例，该类表示证书的私有或公钥的加密提供程序，具体取决于传递给该方法的参数。  
  
5. 可选。 重写 <xref:System.IdentityModel.Tokens.AsymmetricSecurityKey.GetHashAlgorithmForSignature%2A> 方法。 如果需要 <xref:System.Security.Cryptography.HashAlgorithm> 类的另一个实现，请重写此方法。  
  
6. 重写 <xref:System.IdentityModel.Tokens.AsymmetricSecurityKey.GetSignatureFormatter%2A> 方法。 此方法返回与证书私钥关联的 <xref:System.Security.Cryptography.AsymmetricSignatureFormatter> 类的实例。  
  
7. 重写 <xref:System.IdentityModel.Tokens.SecurityKey.IsSupportedAlgorithm%2A> 方法。 此方法用于指示安全密钥实现是否支持某个特定的加密算法。  
  
     [!code-csharp[c_CustomX509Token#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customx509token/cs/source.cs#1)]
     [!code-vb[c_CustomX509Token#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customx509token/vb/source.vb#1)]  
  
 下面的过程演示如何将前面的过程中创建的自定义 x.509 非对称安全密钥实现与 WCF 安全框架相集成，以替换系统提供的 x.509 安全令牌。  
  
#### <a name="to-replace-the-system-provided-x509-security-token-with-a-custom-x509-asymmetric-security-key-token"></a>将系统提供的 X.509 安全令牌替换为自定义 X.509 非对称安全密钥令牌  
  
1. 创建一个自定义 X.509 安全令牌，此令牌返回自定义 X.509 非对称安全密钥而不是系统提供的安全密钥，如下面的示例所示。 有关自定义安全令牌的详细信息，请参阅 [如何：创建自定义令牌](how-to-create-a-custom-token.md)。  
  
     [!code-csharp[c_CustomX509Token#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customx509token/cs/source.cs#2)]
     [!code-vb[c_CustomX509Token#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customx509token/vb/source.vb#2)]  
  
2. 创建一个自定义安全令牌提供程序以返回一个自定义 X.509 安全令牌，如下面的示例所示。 有关自定义安全令牌提供程序的详细信息，请参阅 [如何：创建自定义安全令牌提供程序](how-to-create-a-custom-security-token-provider.md)。  
  
     [!code-csharp[c_CustomX509Token#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customx509token/cs/source.cs#3)]
     [!code-vb[c_CustomX509Token#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customx509token/vb/source.vb#3)]  
  
3. 如果自定义安全密钥需要在发起方端使用，请创建一个自定义客户端安全令牌管理器和若干自定义客户端凭据类，如下面的示例所示。 有关自定义客户端凭据和客户端安全令牌管理器的详细信息，请参阅 [演练：创建自定义客户端和服务凭据](walkthrough-creating-custom-client-and-service-credentials.md)。  
  
     [!code-csharp[c_CustomX509Token#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customx509token/cs/source.cs#4)]
     [!code-vb[c_CustomX509Token#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customx509token/vb/source.vb#4)]  
  
     [!code-csharp[c_CustomX509Token#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customx509token/cs/source.cs#6)]
     [!code-vb[c_CustomX509Token#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customx509token/vb/source.vb#6)]  
  
4. 如果自定义安全密钥需要在接收方端使用，请创建一个自定义服务安全令牌管理器和若干自定义服务凭据，如下面的示例所示。 有关自定义服务凭据和服务安全令牌管理器的详细信息，请参阅 [演练：创建自定义客户端和服务凭据](walkthrough-creating-custom-client-and-service-credentials.md)。  
  
     [!code-csharp[c_CustomX509Token#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customx509token/cs/source.cs#5)]
     [!code-vb[c_CustomX509Token#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customx509token/vb/source.vb#5)]  
  
     [!code-csharp[c_CustomX509Token#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customx509token/cs/source.cs#7)]
     [!code-vb[c_CustomX509Token#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customx509token/vb/source.vb#7)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.IdentityModel.Tokens.X509AsymmetricSecurityKey>
- <xref:System.IdentityModel.Tokens.AsymmetricSecurityKey>
- <xref:System.IdentityModel.Tokens.SecurityKey>
- <xref:System.Security.Cryptography.AsymmetricAlgorithm>
- <xref:System.Security.Cryptography.HashAlgorithm>
- <xref:System.Security.Cryptography.AsymmetricSignatureFormatter>
- [演练：创建自定义客户端和服务凭据](walkthrough-creating-custom-client-and-service-credentials.md)
- [如何：创建自定义安全令牌身份验证器](how-to-create-a-custom-security-token-authenticator.md)
- [如何：创建自定义安全令牌提供程序](how-to-create-a-custom-security-token-provider.md)
- [如何：创建自定义令牌](how-to-create-a-custom-token.md)
