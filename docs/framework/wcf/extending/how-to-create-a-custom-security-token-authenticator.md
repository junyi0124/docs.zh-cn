---
title: 如何：创建自定义安全令牌身份验证器
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF, authentication
ms.assetid: 10e245f7-d31e-42e7-82a2-d5780325d372
ms.openlocfilehash: 667dc82502ba881b067b5588271947f7c18c8810
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96249249"
---
# <a name="how-to-create-a-custom-security-token-authenticator"></a>如何：创建自定义安全令牌身份验证器

本主题演示如何创建自定义安全令牌身份验证器以及如何将其与自定义安全令牌管理器相集成。 安全令牌身份验证器可验证随传入消息一起提供的安全令牌的内容。 如果验证成功，身份验证器将返回 <xref:System.IdentityModel.Policy.IAuthorizationPolicy> 实例的集合，该集合经过计算后可返回一组声明。  
  
 若要在 Windows Communication Foundation 中使用自定义安全令牌身份验证器 (WCF) ，必须首先创建自定义凭据和安全令牌管理器实现。 有关创建自定义凭据和安全令牌管理器的详细信息，请参阅 [演练：创建自定义客户端和服务凭据](walkthrough-creating-custom-client-and-service-credentials.md)。
  
## <a name="procedures"></a>过程  
  
#### <a name="to-create-a-custom-security-token-authenticator"></a>创建自定义安全令牌身份验证器  
  
1. 定义一个从 <xref:System.IdentityModel.Selectors.SecurityTokenAuthenticator> 类派生的新类。  
  
2. 重写 <xref:System.IdentityModel.Selectors.SecurityTokenAuthenticator.CanValidateTokenCore%2A> 方法。 此方法返回 `true` 或 `false`，具体取决于自定义身份验证器是否可以验证传入的令牌类型。  
  
3. 重写 <xref:System.IdentityModel.Selectors.SecurityTokenAuthenticator.ValidateTokenCore%2A> 方法。 此方法需要适当地验证令牌内容。 如果此令牌通过验证步骤，它将返回 <xref:System.IdentityModel.Policy.IAuthorizationPolicy> 实例的集合。 下面的示例使用将在后面的过程中创建的自定义授权策略实现。  
  
     [!code-csharp[C_CustomTokenAuthenticator#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customtokenauthenticator/cs/source.cs#1)]
     [!code-vb[C_CustomTokenAuthenticator#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customtokenauthenticator/vb/source.vb#1)]  
  
 前面的代码在 <xref:System.IdentityModel.Selectors.SecurityTokenAuthenticator.CanValidateToken%28System.IdentityModel.Tokens.SecurityToken%29> 方法中返回授权策略的集合。 WCF 不提供此接口的公共实现。 下面的过程演示如何针对您自己的需求实现此目的。  
  
#### <a name="to-create-a-custom-authorization-policy"></a>创建自定义授权策略  
  
1. 定义一个实现 <xref:System.IdentityModel.Policy.IAuthorizationPolicy> 接口的新类。  
  
2. 实现 <xref:System.IdentityModel.Policy.IAuthorizationComponent.Id%2A> 只读属性。 实现此属性的一个方法是在类构造函数中生成一个全局唯一标识符 (GUID)，并在每次请求此属性的值时返回该标识符。  
  
3. 实现 <xref:System.IdentityModel.Policy.IAuthorizationPolicy.Issuer%2A> 只读属性。 此属性需要返回从令牌获取的声明集的颁发者。 此颁发者应该与令牌颁发者负责验证令牌内容的颁发机构相对应。 下面的示例使用了颁发者声明，该声明从前面的过程中创建的自定义安全令牌身份验证器传递给此类。 自定义安全令牌身份验证器使用系统提供的声明集（由 <xref:System.IdentityModel.Claims.ClaimSet.System%2A> 属性返回）来表示用户名令牌的颁发者。  
  
4. 实现 <xref:System.IdentityModel.Policy.IAuthorizationPolicy.Evaluate%2A> 方法。 此方法使用基于传入安全令牌内容的声明来填充 <xref:System.IdentityModel.Policy.EvaluationContext> 类的实例（以自变量形式传入）。 当此方法完成计算时返回 `true`。 如果该实现依赖于为计算上下文提供附加信息的其他授权策略，则在计算上下文中不存在所要求的信息时，此方法可返回 `false`。 在这种情况下，如果至少有一个授权策略修改了计算上下文，则在评估为传入消息生成的所有其他授权策略之后，WCF 将再次调用方法。  
  
     [!code-csharp[c_CustomTokenAuthenticator#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customtokenauthenticator/cs/source.cs#3)]
     [!code-vb[c_CustomTokenAuthenticator#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customtokenauthenticator/vb/source.vb#3)]  

 [演练：创建自定义客户端和服务凭据](walkthrough-creating-custom-client-and-service-credentials.md) 介绍了如何创建自定义凭据和自定义安全令牌管理器。 若要使用此处创建的自定义安全令牌身份验证器，可以修改安全令牌管理器的实现，以便从 <xref:System.IdentityModel.Selectors.SecurityTokenManager.CreateSecurityTokenAuthenticator%2A> 方法返回自定义身份验证器。 当传入适当的安全令牌要求时，该方法将返回身份验证器。  
  
#### <a name="to-integrate-a-custom-security-token-authenticator-with-a-custom-security-token-manager"></a>使自定义安全令牌身份验证器与自定义安全令牌管理器相集成  
  
1. 在自定义安全令牌管理器实现中重写 <xref:System.IdentityModel.Selectors.SecurityTokenManager.CreateSecurityTokenAuthenticator%2A> 方法。  
  
2. 向该方法添加逻辑，使其能够基于 <xref:System.IdentityModel.Selectors.SecurityTokenRequirement> 参数返回您的自定义安全令牌身份验证器。 如果令牌需求的令牌类型为用户名（由 <xref:System.IdentityModel.Tokens.SecurityTokenTypes.UserName%2A> 属性表示），并且正在为其请求安全令牌身份验证器的消息方向为输入（由 <xref:System.ServiceModel.Description.MessageDirection.Input> 字段表示），则下面的示例将返回自定义安全令牌身份验证器。  
  
     [!code-csharp[c_CustomTokenAuthenticator#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customtokenauthenticator/cs/source.cs#2)]
     [!code-vb[c_CustomTokenAuthenticator#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customtokenauthenticator/vb/source.vb#2)]  

## <a name="see-also"></a>另请参阅

- <xref:System.IdentityModel.Selectors.SecurityTokenAuthenticator>
- <xref:System.IdentityModel.Selectors.SecurityTokenRequirement>
- <xref:System.IdentityModel.Selectors.SecurityTokenManager>
- <xref:System.IdentityModel.Tokens.UserNameSecurityToken>
- [演练：创建自定义客户端和服务凭据](walkthrough-creating-custom-client-and-service-credentials.md)
- [如何：创建自定义安全令牌提供程序](how-to-create-a-custom-security-token-provider.md)
