---
title: 如何：创建安全令牌服务
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF, federation
- federation
ms.assetid: 98e82101-4cff-4bb8-a220-f7abed3556e5
ms.openlocfilehash: cfe1da7c66f5c64ac3f5346bc23e9b618db38d20
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96286456"
---
# <a name="how-to-create-a-security-token-service"></a>如何：创建安全令牌服务

安全令牌服务实现在 WS-Trust 规范中定义的协议。 此协议为颁发、续订、取消和验证安全令牌定义消息格式和消息交换模式。 给定的安全令牌服务提供这些功能中的一个或多个功能。 本主题考虑最常见的情况：实现令牌颁发。  
  
## <a name="issuing-tokens"></a>颁发令牌  

 WS-Trust 基于 `RequestSecurityToken` XML 架构定义语言 (XSD) 架构元素和用于执行令牌颁发的 `RequestSecurityTokenResponse` XSD 架构元素来定义消息格式。 此外，它还定义关联的操作统一资源标识符 (URI)。 与消息关联的操作 URI `RequestSecurityToken` 是 `http://schemas.xmlsoap.org/ws/2005/02/trust/RST/Issue` 。 与消息关联的操作 URI `RequestSecurityTokenResponse` 是   `http://schemas.xmlsoap.org/ws/2005/02/trust/RSTR/Issue` 。  
  
### <a name="request-message-structure"></a>请求消息结构  

 颁发请求消息结构通常包括以下项：  
  
- 一个请求类型 URI，其值为 `http://schemas.xmlsoap.org/ws/2005/02/trust/Issue` 。
  
- 令牌类型 URI。 对于安全断言标记语言 (SAML) 1.1 令牌，此 URI 的值为 `http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1` 。  
  
- 密钥大小值，指示与颁发的令牌关联的密钥中的位数。  
  
- 密钥类型 URI。 对于对称密钥，此 URI 的值为 `http://schemas.xmlsoap.org/ws/2005/02/trust/SymmetricKey` 。  
  
 此外，可能会显示几个其他项：  
  
- 由客户端提供的密钥材料。  
  
- 用来指示颁发的令牌将用于的目标服务的范围信息。  
  
 安全令牌服务在构造颁发响应消息时使用颁发请求消息中的信息。  
  
## <a name="response-message-structure"></a>响应消息结构  

 颁发响应消息结构通常包括以下项；  
  
- 颁发的安全令牌，例如，一个 SAML 1.1 断言。  
  
- 与安全令牌相关联的证明令牌。 对于对称密钥，这通常是密钥材料的加密形式。  
  
- 对颁发的安全令牌的引用。 通常，安全令牌服务返回两个引用：一个可以在颁发的令牌显示在随后由客户端发送的消息中时使用，另一个可以在颁发的令牌没有显示在随后的消息中时使用。  
  
 此外，可能会显示几个其他项：  
  
- 由安全令牌服务提供的密钥材料。  
  
- 计算共享密钥所需的算法。  
  
- 颁发的令牌的生存期信息。  
  
## <a name="processing-request-messages"></a>处理请求消息  

 安全令牌服务通过检查各个请求消息并确保它可以颁发满足此请求的令牌来处理颁发请求。 安全令牌服务必须先确定以下各项，然后才能构造要颁发的令牌：  
  
- 该请求实际上是一个对要颁发的令牌的请求。  
  
- 安全令牌服务支持请求的令牌类型。  
  
- 已授权请求方创建请求。  
  
- 安全令牌服务可以满足请求方对密钥材料的预期。  
  
 构造令牌的两个重要部分是确定对该令牌进行签名时使用的密钥和对共享密钥加密时使用的密钥。 需要对该令牌进行签名，以便当客户端将该令牌显示在目标服务中时，该服务可以确定由它信任的安全令牌服务来颁发该令牌。 需要以目标服务对密钥材料解密的方式来对密钥材料加密。  
  
 对一个涉及创建 <xref:System.IdentityModel.Tokens.SigningCredentials> 实例的 SAML 断言进行签名。 此类的构造函数具有以下各项：  
  
- 密钥的 <xref:System.IdentityModel.Tokens.SecurityKey>，用于对 SAML 断言进行签名。  
  
- 用于标识要使用的签名算法的字符串。  
  
- 用于标识要使用的摘要算法的字符串。  
  
- 或者，用于标识要用来对断言进行签名的密钥的 <xref:System.IdentityModel.Tokens.SecurityKeyIdentifier>。  
  
 [!code-csharp[c_CreateSTS#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#1)]
 [!code-vb[c_CreateSTS#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#1)]  
  
 对共享密钥加密涉及两个操作：首先获得该密钥材料，然后使用目标服务用来对该共享密钥解密的密钥对该共享密钥进行加密。 通常使用该目标服务的公钥。  
  
 [!code-csharp[c_CreateSTS#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#2)]
 [!code-vb[c_CreateSTS#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#2)]  
  
 此外，需要加密密钥的 <xref:System.IdentityModel.Tokens.SecurityKeyIdentifier>。  
  
 [!code-csharp[c_CreateSTS#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#3)]
 [!code-vb[c_CreateSTS#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#3)]  
  
 然后此 <xref:System.IdentityModel.Tokens.SecurityKeyIdentifier> 用于创建一个作为 `SamlSubject` 一部分的 `SamlToken`。  
  
 [!code-csharp[c_CreateSTS#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#4)]
 [!code-vb[c_CreateSTS#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#4)]  
  
 有关详细信息，请参阅 [联合示例](../samples/federation-sample.md)。  
  
## <a name="creating-response-messages"></a>创建响应消息  

 一旦安全令牌服务处理颁发请求并构造要随校验密钥一起颁发的令牌，就需要构造响应消息，其中至少包括请求的令牌、证明令牌和颁发的令牌引用。 此颁发的令牌通常是从 <xref:System.IdentityModel.Tokens.SamlSecurityToken> 中创建的 <xref:System.IdentityModel.Tokens.SamlAssertion>，如以下示例所示。  
  
 [!code-csharp[c_CreateSTS#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#5)]
 [!code-vb[c_CreateSTS#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#5)]  
  
 如果安全令牌服务提供共享密钥材料，则通过创建一个 <xref:System.ServiceModel.Security.Tokens.BinarySecretSecurityToken> 来构造证明令牌。  
  
 [!code-csharp[c_CreateSTS#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#6)]
 [!code-vb[c_CreateSTS#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#6)]  
  
 有关当客户端和 security token service 都为共享密钥提供密钥材料时如何构造证明令牌的详细信息，请参阅 [联合示例](../samples/federation-sample.md)。  
  
 通过创建 <xref:System.IdentityModel.Tokens.SecurityKeyIdentifierClause> 类的实例来构造颁发的令牌的引用。  
  
 [!code-csharp[c_CreateSTS#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_creatests/cs/source.cs#7)]
 [!code-vb[c_CreateSTS#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_creatests/vb/source.vb#7)]  
  
 然后这些不同的值序列化到返回客户端的响应消息中。  
  
## <a name="example"></a>示例  

 有关 security token service 的完整代码，请参阅 [联合示例](../samples/federation-sample.md)。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.IdentityModel.Tokens.SigningCredentials>
- <xref:System.IdentityModel.Tokens.SecurityKey>
- <xref:System.IdentityModel.Tokens.SecurityKeyIdentifier>
- <xref:System.IdentityModel.Tokens.SamlSecurityToken>
- <xref:System.IdentityModel.Tokens.SamlAssertion>
- <xref:System.ServiceModel.Security.Tokens.BinarySecretSecurityToken>
- <xref:System.IdentityModel.Tokens.SecurityKeyIdentifierClause>
- [联合示例](../samples/federation-sample.md)
