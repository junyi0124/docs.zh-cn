---
title: 权限提升
ms.date: 03/30/2017
helpviewer_keywords:
- elevation of privilege [WCF]
- security [WCF], elevation of privilege
ms.assetid: 146e1c66-2a76-4ed3-98a5-fd77851a06d9
ms.openlocfilehash: 9c62e11eedaa3fa194522695a33bccf210d390df
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96254215"
---
# <a name="elevation-of-privilege"></a>权限提升

*提升* 权限是指除最初授予的权限外，授予攻击者授权权限。 例如，具有“只读”权限特权集的攻击者以某种方式将该特权集升级为包括“读取和写入”。  
  
## <a name="trusted-sts-should-sign-saml-token-claims"></a>受信任的 STS 应该为 SAML 令牌声明签名  

 安全断言标记语言 (SAML) 令牌是作为已颁发令牌默认类型的通用 XML 令牌。 SAML 令牌可以由最终 Web 服务在典型交换中信任的安全令牌服务 (STS) 构造。 SAML 令牌包含语句中的声明。 攻击者可能从有效令牌复制声明，创建新的 SAML 令牌，并以其他颁发者身份为其签名。 其意图在于确定服务器是否将验证颁发者，如果不验证，则利用这一漏洞构造 SAML 令牌，这些令牌允许超过受信任 STS 所预期特权的特权。  
  
 <xref:System.IdentityModel.Tokens.SamlAssertion> 类验证 SAML 令牌中包含的数字签名，默认 <xref:System.IdentityModel.Selectors.SamlSecurityTokenAuthenticator> 要求由 <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.CertificateValidationMode%2A> 类的 <xref:System.ServiceModel.Security.IssuedTokenServiceCredential> 设置为 <xref:System.ServiceModel.Security.X509CertificateValidationMode.ChainTrust> 时有效的 X.509 证书为 SAML 令牌签名。 `ChainTrust` 模式本身不足以确定是否信任 SAML 令牌的颁发者。 需要更为细化的信任模型的服务可以使用授权和强制策略检查由已颁发令牌身份验证生成的声明集的颁发者，或者使用 <xref:System.ServiceModel.Security.IssuedTokenServiceCredential> 上的 X.509 验证设置限制允许的签名证书集。 有关详细信息，请参阅通过标识模型和[联合和颁发的令牌](federation-and-issued-tokens.md)[管理声明和授权](managing-claims-and-authorization-with-the-identity-model.md)。  
  
## <a name="switching-identity-without-a-security-context"></a>在没有安全上下文的情况下切换标识  

 以下内容仅适用于 WinFX。  
  
 当在客户端与服务器之间建立连接时，客户端的标识不会更改，但在以下情况下除外：在打开 WCF 客户端之后，如果满足以下所有条件：  
  
- 使用传输安全会话或消息安全会话 (建立安全上下文的过程) 关闭 (<xref:System.ServiceModel.NonDualMessageSecurityOverHttp.EstablishSecurityContext%2A> 属性设置为， `false` 以防消息安全或传输不能建立安全会话在传输安全案例中使用。 HTTPS 就是此类传输的一个示例）。  
  
- 正在使用 Windows 身份验证。  
  
- 未显式设置凭据。  
  
- 正在模拟安全上下文下调用服务。  
  
 如果满足这些条件，则用于对服务的客户端进行身份验证的标识可能会发生更改， (它可能不是模拟标识，而是在打开 WCF 客户端之后) 进程标识。 之所以出现此情况，是因为用于向服务验证客户端身份的 Windows 凭据是随每条消息传输的，而且用于身份验证的凭据是从当前线程的 Windows 标识获取的。 如果当前线程的 Windows 标识发生更改（例如，通过模拟其他调用方），则附加到消息、用于向服务验证客户端身份的凭据可能也发生更改。  
  
 如果要在将 Windows 身份验证与模拟一起使用时具有确定性行为，则需要显式设置 Windows 凭据或需要使用该服务建立安全上下文。 为此，请使用消息安全会话或传输安全会话。 例如，net.tcp 传输可以提供传输安全会话。 此外，在调用服务时，必须仅使用客户端操作的同步版本。 如果建立消息安全上下文，则与服务的连接的打开时间不应长于所配置的会话续订期限，因为在会话续订过程中标识也可能更改。  
  
### <a name="credentials-capture"></a>凭据捕获  

 以下内容适用于 .NET Framework 3.5 和后续版本。  
  
 客户端或服务所用的凭据基于当前上下文线程。 凭据是在调用客户端或服务的 `Open` 方法（对于异步调用，则为 `BeginOpen`）时获取的。 对于 <xref:System.ServiceModel.ServiceHost> 和 <xref:System.ServiceModel.ClientBase%601> 这两个类，`Open` 和 `BeginOpen` 方法继承自 <xref:System.ServiceModel.Channels.CommunicationObject.Open%2A> 类的 <xref:System.ServiceModel.Channels.CommunicationObject.BeginOpen%2A> 和 <xref:System.ServiceModel.Channels.CommunicationObject> 方法。  
  
> [!NOTE]
> 在使用 `BeginOpen` 方法时，无法保证所捕获的凭据是调用该方法的进程的凭据。  
  
## <a name="token-caches-allow-replay-using-obsolete-data"></a>令牌缓存允许使用过时数据的重放  

 WCF 使用本地安全机构 (LSA) `LogonUser` 函数通过用户名和密码对用户进行身份验证。 由于登录功能是一种代价高昂的操作，WCF 使你可以缓存代表经过身份验证的用户的令牌，以提高性能。 该缓存机制保存来自 `LogonUser` 的结果，以备将来之用。 默认情况下，此机制处于禁用状态;若要启用它，请将 <xref:System.ServiceModel.Security.UserNamePasswordServiceCredential.CacheLogonTokens%2A> 属性设置为 `true` ，或者使用 `cacheLogonTokens` 的属性 [\<userNameAuthentication>](../../configure-apps/file-schema/wcf/usernameauthentication.md) 。  
  
 可以为已缓存的令牌设置生存时间 (TTL)，方法是将 <xref:System.ServiceModel.Security.UserNamePasswordServiceCredential.CachedLogonTokenLifetime%2A> 属性设置为 <xref:System.TimeSpan> 或使用 `cachedLogonTokenLifetime` 元素的 `userNameAuthentication` 属性；默认值为 15 分钟。 请注意，令牌缓存后，提供相同的用户名和密码的任何客户端便可以使用该令牌，即使用户帐户已从 Windows 中删除或其密码已更改，也是如此。 在 TTL 过期并从缓存中删除令牌之前，WCF 允许 (可能是恶意) 用户进行身份验证。  
  
 缓解此问题：通过将 `cachedLogonTokenLifetime` 值设置为用户所需的最短时间间隔，减小攻击时限。  
  
## <a name="issued-token-authorization-expiration-reset-to-large-value"></a>已颁发令牌的授权：将到期时间重置为较大的值  

 在某些情况下，会将 <xref:System.IdentityModel.Policy.AuthorizationContext.ExpirationTime%2A> 的 <xref:System.IdentityModel.Policy.AuthorizationContext> 属性的值设置得特别大（<xref:System.DateTime.MaxValue> 字段值减去一天，或 9999 年 12 月 20 日）。  
  
 使用将已颁发的令牌作为客户端凭据类型的 <xref:System.ServiceModel.WSFederationHttpBinding> 和系统提供的任一绑定时，会出现此情况。  
  
 通过使用以下方法之一创建自定义绑定时，也会出现此情况：  
  
- <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateIssuedTokenBindingElement%2A>  
  
- <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateIssuedTokenForCertificateBindingElement%2A>  
  
- <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateIssuedTokenForSslBindingElement%2A>  
  
- <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateIssuedTokenOverTransportBindingElement%2A>  
  
 若要缓解此问题，授权策略必须检查每个授权策略的操作和过期时间。  
  
## <a name="the-service-uses-a-different-certificate-than-the-client-intended"></a>服务使用非客户端预期证书  

 在某些情况下，客户端可以使用 X.509 证书对消息进行数字签名，并使服务检索与预期不同的证书。  
  
 这可能发生在以下情况下：  
  
- 客户端使用 X.509 证书对消息进行数字签名，但是不将 X.509 证书附加到消息，而是仅使用证书的主题密钥标识符来引用它。  
  
- 服务的计算机包含具有相同公钥的两个或多个证书，但是这些证书包含不同的信息。  
  
- 服务检索一个与主题密钥标识符匹配的证书，但是该证书不是客户端打算使用的证书。 当 WCF 收到消息并验证签名时，WCF 会将非预期的 x.509 证书中的信息映射到一组不同的声明，这些声明可能会根据客户端的预期进行提升。  
  
 若要缓解此问题，请以其他方式引用 X.509 证书，如使用 <xref:System.ServiceModel.Security.Tokens.X509KeyIdentifierClauseType.IssuerSerial>。  
  
## <a name="see-also"></a>另请参阅

- [安全注意事项](security-considerations-in-wcf.md)
- [信息泄露](information-disclosure.md)
- [拒绝服务](denial-of-service.md)
- [重播攻击](replay-attacks.md)
- [篡改](tampering.md)
- [不支持的方案](unsupported-scenarios.md)
