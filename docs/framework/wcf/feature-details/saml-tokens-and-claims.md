---
title: SAML 令牌和声明
description: 了解 WFC 如何使用 SAML 令牌来携带语句，这些语句是一组与其他实体相关的声明。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF, federation
- federation
- issued tokens
- SAML token
ms.assetid: 930b6e34-9eab-4e95-826c-4e06659bb977
ms.openlocfilehash: a1de58ae28041b8e89822120c9369612217eb3b7
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96288510"
---
# <a name="saml-tokens-and-claims"></a>SAML 令牌和声明

安全断言标记语言 (SAML) *令牌* 是声明的 XML 表示形式。 默认情况下，在联合安全方案中 Windows Communication Foundation (WCF) 使用的 SAML 令牌是 *颁发的令牌*。  
  
 SAML 令牌包含由一个实体所生成的关于另一实体的多组声明的语句。 例如，在联合安全方案中，语句是由安全令牌服务针对系统中的某一用户所生成的。 安全令牌服务将对 SAML 令牌进行签名，以指示令牌中所包含语句的真实性。 此外，SAML 令牌与 SAML 令牌用户确实了解的加密密钥材料关联。 这就向依赖方证明了 SAML 令牌实际上是颁发给该用户的。 例如，在典型的方案中：  
  
1. 客户端向安全令牌服务请求 SAML 令牌，并通过使用 Windows 证书对该安全令牌服务进行身份验证。  
  
2. 安全令牌服务向客户端颁发 SAML 令牌。 SAML 令牌使用与安全令牌服务关联的证书进行签名，并包含针对目标服务所加密的校验密钥。  
  
3. 客户端还收到 *证明密钥* 的副本。 然后，客户端向应用程序服务提供 SAML 令牌 (*信赖方*) 并用该证明密钥对消息进行签名。  
  
4. 依赖方可通过 SAML 令牌上的签名了解到，该令牌是由安全令牌服务颁发的； 依赖方还可通过使用校验密钥创建的消息签名了解到，该令牌是颁发给客户端的。  
  
## <a name="from-claims-to-samlattributes"></a>从 Claim 到 SamlAttribute  

 在 WCF 中，SAML 令牌中的语句作为对象进行建模，前提是对象 <xref:System.IdentityModel.Tokens.SamlAttribute> <xref:System.IdentityModel.Claims.Claim> <xref:System.IdentityModel.Claims.Claim> 具有 <xref:System.IdentityModel.Claims.Claim.Right%2A> 属性 <xref:System.IdentityModel.Claims.Rights.PossessProperty%2A> 且 <xref:System.IdentityModel.Claims.Claim.Resource%2A> 属性的类型为 <xref:System.String> 。 例如：  
  
 [!code-csharp[c_CreateSTS#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#8)]
 [!code-vb[c_CreateSTS#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#8)]  
  
> [!NOTE]
> 如果 SAML 令牌在消息中序列化，无论是在安全令牌服务颁发这些令牌时，还是在客户端将其作为身份验证的一部分提交到服务中时，最大消息大小配额都必须足够大，以便能够容纳 SAML 令牌和其他消息部分。 正常情况下，默认消息大小配额足够使用。 但是，如果 SAML 令牌由于包含数以百计的声明而过于庞大，您可能需要提高配额，以便容纳序列化的令牌。 有关详细信息，请参阅 [数据的安全注意事项](security-considerations-for-data.md)。  
  
## <a name="from-samlattributes-to-claims"></a>从 SamlAttribute 到 Claim  

 在消息中接收到 SAML 令牌时，SAML 令牌中的各种语句将转换为 <xref:System.IdentityModel.Policy.IAuthorizationPolicy> 对象，并放置在 <xref:System.IdentityModel.Policy.AuthorizationContext> 中。 来自每个 SAML 语句的声明将由 <xref:System.IdentityModel.Policy.AuthorizationContext.ClaimSets%2A> 的 <xref:System.IdentityModel.Policy.AuthorizationContext> 属性返回，并可对这些声明进行检查，以确定是否对该用户进行身份验证和授权。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.IdentityModel.Policy.AuthorizationContext>
- <xref:System.IdentityModel.Policy.IAuthorizationPolicy>
- <xref:System.IdentityModel.Claims.ClaimSet>
- [联合](federation.md)
- [如何：创建联合客户端](how-to-create-a-federated-client.md)
- [如何：在联合身份验证服务上配置凭据](how-to-configure-credentials-on-a-federation-service.md)
- [使用标识模型管理声明和授权](managing-claims-and-authorization-with-the-identity-model.md)
- [声明和令牌](claims-and-tokens.md)
- [声明创建和资源值](claim-creation-and-resource-values.md)
- [如何：创建自定义声明](../extending/how-to-create-a-custom-claim.md)
