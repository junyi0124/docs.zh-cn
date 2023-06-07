---
title: 不支持的方案
ms.date: 03/30/2017
ms.assetid: 72027d0f-146d-40c5-9d72-e94392c8bb40
ms.openlocfilehash: 2d779b49a8201b3ad53507af7710f7aef5e9321c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96289641"
---
# <a name="unsupported-scenarios"></a>不支持的方案

由于各种原因，Windows Communication Foundation (WCF) 不支持某些特定的安全方案。 例如，Windows XP Home Edition 不实现 SSPI 或 Kerberos 身份验证协议，因此 WCF 不支持在该平台上使用 Windows 身份验证运行服务。 在 Windows XP Home Edition 下运行 WCF 时，支持其他身份验证机制，例如用户名/密码和 HTTP/HTTPS 集成身份验证。

## <a name="impersonation-scenarios"></a>模拟方案

### <a name="impersonated-identity-might-not-flow-when-clients-make-asynchronous-calls"></a>当客户端进行异步调用时，模拟标识可能不会流动

 当 WCF 客户端通过模拟使用 Windows 身份验证对 WCF 服务进行异步调用时，可能会使用客户端进程的标识进行身份验证，而不是使用模拟的标识进行身份验证。

### <a name="windows-xp-and-secure-context-token-cookie-enabled"></a>已启用 Windows XP 和安全上下文令牌 cookie

WCF 不支持模拟， <xref:System.InvalidOperationException> 当存在以下条件时，将引发：

- 操作系统为 Windows XP。

- 身份验证模式产生 Windows 标识。

- <xref:System.ServiceModel.OperationBehaviorAttribute> 的 <xref:System.ServiceModel.OperationBehaviorAttribute.Impersonation%2A> 属性设置为 <xref:System.ServiceModel.ImpersonationOption.Required>。

- 创建了基于状态的安全上下文令牌 (SCT)。默认情况下禁止创建此类令牌。

 基于状态的 SCT 只能使用自定义绑定来创建。 有关详细信息，请参阅 [如何：为安全会话创建安全上下文令牌](how-to-create-a-security-context-token-for-a-secure-session.md)。 ) 在代码中，通过使用或方法 (或) 创建安全绑定元素， <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement> <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateSspiNegotiationBindingElement%28System.Boolean%29?displayProperty=nameWithType> <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateSecureConversationBindingElement%28System.ServiceModel.Channels.SecurityBindingElement%2CSystem.Boolean%29?displayProperty=nameWithType> 并将 `requireCancellation` 参数设置为 `false` ，可以启用令牌。 该参数引用 SCT 的缓存。 若将该值设置为 `false`，则启用基于状态的 SCT 功能。

 或者，在配置中，通过创建一个 <`customBinding`>，然后添加一个 <`security`> 元素，并将属性设置为 ws-secureconversation，并将 `authenticationMode` 属性设置 `requireSecurityContextCancellation` 为来 `true` 启用该标记。

> [!NOTE]
> 上述要求是特定的。 例如，<xref:System.ServiceModel.Channels.SecurityBindingElement.CreateKerberosBindingElement%2A> 创建一个产生 Windows 标识的绑定元素，但并不建立一个 SCT。 因此，你可以将其与 `Required` WINDOWS XP 上的选项一起使用。

### <a name="possible-aspnet-conflict"></a>可能的 ASP.NET 冲突

WCF 和 ASP.NET 都可以启用或禁用模拟。 当 ASP.NET 承载 WCF 应用程序时，WCF 和 ASP.NET 配置设置之间可能存在冲突。 发生冲突时，除非将属性设置为，否则会优先使用 WCF 设置， <xref:System.ServiceModel.OperationBehaviorAttribute.Impersonation%2A> <xref:System.ServiceModel.ImpersonationOption.NotAllowed> 在这种情况下，ASP.NET 模拟设置优先。

### <a name="assembly-loads-may-fail-under-impersonation"></a>程序集加载可能在模拟后失败

如果所模拟的上下文没有加载程序集的访问权限，并且这是公共语言运行库 (CLR) 第一次试图加载该 AppDomain 的程序集，则 <xref:System.AppDomain> 会缓存失败。 随后进行的加载该程序集（或多个程序集）的尝试仍将失败，即使撤消了模拟，并且恢复之后的上下文具有加载该程序集的访问权限。 这是因为 CLR 在用户上下文更改后不会重新尝试负载。 必须重新启动应用程序域才能从失败中恢复。

> [!NOTE]
> <xref:System.ServiceModel.Security.WindowsClientCredential.AllowedImpersonationLevel%2A> 类的 <xref:System.ServiceModel.Security.WindowsClientCredential> 属性的默认值为 <xref:System.Security.Principal.TokenImpersonationLevel.Identification>。 大多数情况下，标识级模拟上下文不具有加载任何其他程序集的权限。 这是默认值，因此这是一个需要注意的非常常见的条件。 当模拟进程不具有 `SeImpersonate` 特权时，也会发生标识级模拟。 有关详细信息，请参阅 [委派和模拟](delegation-and-impersonation-with-wcf.md)。

### <a name="delegation-requires-credential-negotiation"></a>委托需要凭据协商

若要将 Kerberos 身份验证协议与委托联合使用，必须实现带有凭据协商的 Kerberos 协议（有时称作多段或多步 Kerberos）。 如果不用凭据协商来实现 Kerberos 协议（有时称作单稳或单路 Kerberos），则会引发异常。 有关如何实现凭据协商的详细信息，请参阅 [调试 Windows 身份验证错误](debugging-windows-authentication-errors.md)。

## <a name="cryptography"></a>密码

### <a name="sha-256-supported-only-for-symmetric-key-usages"></a>SHA-256 仅支持对称密钥用法

WCF 支持多种加密和签名摘要创建算法，可以在系统提供的绑定中使用算法套件来指定。 为了提高安全性，WCF 支持安全哈希算法 (SHA) 2 算法，特别是 SHA-256，用于创建签名摘要哈希。 此版本仅在使用对称密钥（例如，Kerberos 密钥）并且没有使用 X.509 证书对消息进行签名的情况下才支持 SHA-256。 WCF 不支持 (在 x.509 证书中使用的 RSA 签名) 使用 SHA-256 哈希，因为目前缺乏对 WinFX 中 RSA-SHA256 的支持。

### <a name="fips-compliant-sha-256-hashes-not-supported"></a>不支持 FIPS 兼容的 SHA-256 哈希

WCF 不支持符合 SHA-256 FIPS 的哈希，因此在要求使用符合 FIPS 标准的算法的系统上，WCF 不支持使用 SHA-256 的算法套件。

### <a name="fips-compliant-algorithms-may-fail-if-registry-is-edited"></a>如果编辑了注册表，符合 FIPS 标准的算法可能会失败

使用本地安全设置 Microsoft 管理控制台 (MMC) 管理单元，可以启用和禁用与美国联邦信息处理标准 (FIPS) 兼容的算法。 您还可以在注册表中访问该设置。 但请注意，WCF 不支持使用注册表重置设置。 如果该值设置为除 1 或 0 之外的任何值，CLR 和操作系统之间就可能出现不一致的结果。

### <a name="fips-compliant-aes-encryption-limitation"></a>符合 FIPS 标准的 AES 加密限制

符合 FIPS 标准的 AES 加密不适用于标识级别模拟下的双工回拨。

### <a name="cngksp-certificates"></a>CNG/KSP 证书

*加密 API：下一代 (CNG)* 是 CryptoAPI 的长期替换。 此 API 在 Windows Vista、Windows Server 2008 和更高版本的 windows 版本上的非托管代码中提供。

 .NET Framework 4.6.1 和更早版本不支持这些证书，因为它们使用旧 CryptoAPI 来处理 CNG/KSP 证书。 将这些证书用于 .NET Framework 4.6.1 和早期版本将导致异常。

 以下是判断证书是否使用 KSP 的两种可行方法：

- 对 `p/invoke` 执行 `CertGetCertificateContextProperty`，并对返回的 `dwProvType` 检查 `CertGetCertificateContextProperty`。

- 使用命令  `certutil` 行中的命令来查询证书。 有关详细信息，请参阅 [用于排查证书问题的 Certutil 任务](/previous-versions/orphan-topics/ws.10/cc772619(v=ws.10))。

## <a name="message-security-fails-if-using-aspnet-impersonation-and-aspnet-compatibility-is-required"></a>如果需要使用 ASP.NET 模拟和 ASP.NET 兼容性，消息安全将失败

WCF 不支持以下设置组合，因为它们可能会阻止客户端身份验证发生：

- ASP.NET 模拟已启用。 这是通过将 `impersonate` <> 元素的属性设置为来在 Web.config 文件中完成的 `identity` `true` 。

- 通过将 `aspNetCompatibilityEnabled` 的属性设置 [\<serviceHostingEnvironment>](../../configure-apps/file-schema/wcf/servicehostingenvironment.md) 为，可以启用 ASP.NET 兼容模式 `true` 。

- 使用了消息模式安全。

解决方法是关闭 ASP.NET 兼容性模式。 或者，如果 ASP.NET 兼容模式是必需的，则禁用 ASP.NET 模拟功能并改为使用 WCF 提供的模拟。 有关详细信息，请参阅 [委派和模拟](delegation-and-impersonation-with-wcf.md)。

## <a name="ipv6-literal-address-failure"></a>IPv6 文本地址失败

当客户端和服务位于同一台计算机上，并且为服务使用了 IPv6 文本地址时，安全请求将失败。

 如果服务和客户端位于不同的计算机上，IPv6 文本地址有效。

## <a name="wsdl-retrieval-failures-with-federated-trust"></a>联合信任的 WSDL 检索失败

对于联合信任链中的每个节点，WCF 只需要一个 WSDL 文档。 指定终结点时，务必不要设置循环。 可能导致循环的一种情形是，将联合信任链的 WSDL 下载用于同一 WSDL 文档中的两个或多个链接。 可能产生此问题的一个常见方案是联合服务，其中安全令牌服务器和服务包含在同一 ServiceHost 内。

 举例来说，若某个服务具有以下三个终结点地址，便可能出现此情况：

- `http://localhost/CalculatorService/service` (服务) 

- `http://localhost/CalculatorService/issue_ticket` (STS) 

- `http://localhost/CalculatorService/mex` (元数据终结点) 

 这将引发异常。

 若将 `issue_ticket` 终结点放在其他地方，此方案便可正常工作。

## <a name="wsdl-import-attributes-can-be-lost"></a>WSDL 导入属性可能会丢失

执行 WSDL 导入时，WCF 丢失了对 `<wst:Claims>` 模板中 `RST` 元素的属性的跟踪。 如果在 `<Claims>` 或 `WSFederationHttpBinding.Security.Message.TokenRequestParameters` 中直接指定 `IssuedSecurityTokenRequestParameters.AdditionalRequestParameters`，而不是直接使用声明类型集合，则在 WSDL 导入期间会发生此情况。  由于导入丢失了特性，绑定没有通过 WSDL 正确地往返，因此它在客户端上是不正确的。

 解决方法是，导入完毕后直接在客户端上修改绑定。

## <a name="see-also"></a>另请参阅

- [安全注意事项](security-considerations-in-wcf.md)
- [信息泄露](information-disclosure.md)
- [权限提升](elevation-of-privilege.md)
- [拒绝服务](denial-of-service.md)
- [篡改](tampering.md)
- [重播攻击](replay-attacks.md)
