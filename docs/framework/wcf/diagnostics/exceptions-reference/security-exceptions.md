---
title: 安全异常
ms.date: 03/30/2017
ms.assetid: 76d5e5cd-e4f4-404f-9a5a-ec3522494ad8
ms.openlocfilehash: e96c317862867b9e461eb2d13dce6ede5b30cf13
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75348235"
---
# <a name="security-exceptions"></a>安全异常

本主题列出所有安全异常。

## <a name="exception-list"></a>异常列表

|资源代码|资源字符串|
|-------------------|---------------------|
|AnonymousLogonsAreNotAllowed|服务不允许匿名登录。|
|AtLeastOneContractOperationRequestRequiresProtectionLevelNotSupportedByBinding|必须保护请求消息。 这是指定协定的操作所要求的。 保护必须由指定的绑定提供。|
|AtLeastOneContractOperationResponseRequiresProtectionLevelNotSupportedByBinding|必须保护响应消息。 这是指定协定的操作所要求的。 保护必须由指定的绑定提供。|
|AtMostOnePrimarySignatureInReceiveSecurityHeader|安全标头中只允许有一个主签名。|
|BadContextTokenFaultReason|安全上下文令牌已过期或无效。 未处理该消息。|
|BadEncryptionState|对于此操作，EncryptedData 或 EncryptedKey 处于无效状态。|
|BasicHttpMessageSecurityRequiresCertificate|BasicHttp 绑定要求 BasicHttpBinding.Security.Message.ClientCredentialType 等效于安全消息的 BasicHttpMessageCredentialType.Certificate 凭据类型。 为 UserName 凭据选择 Transport 或 TransportWithMessageCredential 安全性。|
|BasicTokenCannotBeWrittenWithoutEncryption|不进行加密无法写入基本令牌。|
|BindingDoesNotSupportProtectionForRst|指定协定的指定绑定是使用 SecureConversation 配置的，但是身份验证模式无法提供协商所必需的基于请求/答复的完整性和保密性。|
|BindingDoesNotSupportWindowsIdenityForImpersonation|指定协定操作需要 Windows 标识进行自动模拟。 指定协定的指定绑定未提供代表调用方的 Windows 标识。|
|CachedNegotiationStateQuotaReached|服务无法缓存协商状态，因为已达到指定容量。 请重试请求。|
|CacheQuotaReached|无法添加该项目。 最大缓存大小已指定。|
|CannotDetermineSPNBasedOnAddress|客户端无法以 SspiNegotiation/Kerberos 为目的根据指定目标地址中的标识确定服务主体名称。 目标地址标识必须是 UPN 标识（如 acmedomain\\\alice）或 SPN 标识（如 host/bobs-sfpreviewcluster）。|
|CannotFindCert|无法使用下列指定的搜索标准找到 X.509 证书：StoreName、StoreLocation、FindType、FindValue。|
|CannotFindCertForTarget|无法使用下列指定的搜索标准为指定目标找到 X.509 证书：StoreName、StoreLocation、FindType、FindValue。|
|CannotFindCorrelationStateForApplyingSecurity|无法找到将安全性应用到应答器上的答复所需的相关性状态。|
|CannotFindNegotiationState|无法找到指定上下文的协商状态。|
|CannotFindSecuritySession|无法找到具有指定 ID 的安全会话。|
|CannotImportProtectionLevelForContract|用于导入进程的策略无法为指定协定导入绑定。 该绑定的保护需求与已经为该协定导入的绑定不兼容。 必须重新配置该绑定。|
|CannotImportSupportingTokensForOperationWithoutRequestAction|安全策略导入失败。 该安全策略的操作作用域中包含支持令牌需求。 协定描述没有为此操作相关联的请求消息指定操作。|
|CannotIssueRstTokenType|无法颁发指定类型的令牌。|
|CannotObtainIssuedTokenKeySize|无法确定已颁发令牌的密钥大小。|
|CannotPerformImpersonationOnUsernameToken|不可能使用客户端令牌进行模拟。 指定协定的指定绑定将用户名安全令牌用于注册了成员资格提供程序的客户端身份验证。 对该客户端使用不同类型的安全令牌。|
|CannotPerformS4UImpersonationOnPlatform|指定协定的指定绑定仅在 Windows Server 2003 和更高版本的 Windows 上支持模拟。 在启用取消操作的情况下使用 SspiNegotiated 身份验证和具有安全对话的绑定。|
|CannotReadKeyIdentifier|无法从指定命名空间的指定元素中读取 KeyIdentifier。|
|CannotReadToken|对于具有指定 ValueType 的 BinarySecretSecurityToken，无法从指定命名空间的指定元素中读取令牌。 如果希望此元素有效，则确保已将安全性配置为使用具有指定的名称、命名空间和值类型的令牌。|
|CertificateUnsupportedForHttpTransportCredentialOnly|TransportCredentialOnly 安全模式中不支持基于证书的客户端身份验证。 请选择 Transport 安全模式。|
|ClaimTypeCannotBeEmpty|claimType 不能为空字符串。|
|ClientCertificateNotProvided|尚未提供该客户端的证书。 可以在 ClientCredentials 或 ServiceCredentials 上设置该证书。|
|ClientCredentialTypeMustBeSpecifiedForMixedMode|ClientCredentialType.None 对于 TransportWithMessageCredential 安全模式无效。 请指定凭据类型或使用其他安全模式。|
|ConfigurationSchemaInsuffientForSecurityBindingElementInstance|配置架构不足以描述以下安全绑定元素的非标准配置：|
|DerivedKeyTokenGenerationAndLengthTooHigh|派生密钥的指定生成和长度产生的密钥派生偏移量大于所允许的最大偏移量。|
|DnsIdentityCheckFailedForIncomingMessage|传入消息标识检查失败。 远程终结点的所需域名系统 (DNS) 标识已指定。 远程终结点已提供指定的域名系统 (DNS) 请求。 如果此远程终结点合法，您可以通过在创建通道代理时将域名系统标识指定为 EndpointAddress 的“标识”属性来解决此问题。|
|DnsIdentityCheckFailedForOutgoingMessage|传出消息的标识检查失败。远程终结点应有指定的域名系统标识。 远程终结点已提供域名系统 (DNS) 请求。 如果此远程终结点合法，您可以通过在创建通道代理时明确地将 DNS 标识指定为 EndpointAddress 的“标识”属性来解决此问题。|
|DuplicateIdInMessageToBeVerified|指定的 ID 在提供以进行验证的消息中出现两次。|
|EmptyBase64Attribute|在必需的 base-64 属性名称和命名空间中发现空值。|
|ExportOfBindingWithAsymmetricAndTransportSecurityNotSupported|安全策略导出失败。 该绑定同时包含 AsymmetricSecurityBindingElement 和安全传输绑定元素。 不支持此类绑定的策略导出。|
|ExportOfBindingWithSymmetricAndTransportSecurityNotSupported|安全策略导出失败。 该绑定同时包含 SymmetricSecurityBindingElement 和安全传输绑定元素。 不支持此类绑定的策略导出。|
|ExportOfBindingWithTransportSecurityBindingElementAndNoTransportSecurityNotSupported|安全策略导出失败。 绑定包含 TransportSecurityBindingElement，但是没有实现 ITransportTokenAssertionProvider 的传输绑定元素。 不支持此类绑定的策略导出。 请确保绑定中的传输绑定元素实现 ITransportTokenAssertionProvider 接口。|
|FoundMultipleCerts|使用指定的下列搜索标准找到多个 X.509 证书：StoreName、StoreLocation、FindType、FindValue。 请提供更具体的查找值。|
|FoundMultipleCertsForTarget|使用下列指定的搜索标准为指定目标找到多个 X.509 证书：StoreName、StoreLocation、FindType、FindValue。 请提供更具体的查找值。|
|HeaderDecryptionNotSupportedInWsSecurityJan2004|SecurityVersion.WSSecurityJan2004 不支持标头解密。 请使用 SecurityVersion.WsSecurityXXX2005 及更高版本或使用传输安全来加密完整的消息。|
|IdentityCheckFailedForIncomingMessage|传入消息标识检查失败。 目标终结点的所需标识已指定。|
|IdentityCheckFailedForOutgoingMessage|传出消息的标识检查失败。 目标终结点的所需标识已指定。|
|IncorrectSpnOrUpnSpecified|安全支持提供程序接口 (SSPI) 身份验证失败。 服务器可能未在具有指定标识的帐户下运行。 如果服务器正在服务帐户（如网络服务）下运行，请将该帐户的 ServicePrincipalName 指定为该服务器的 EndpointAddress 中的标识。 如果服务器正在用户帐户下运行，请将该帐户的 UserPrincipalName 指定为该服务器的 EndpointAddress 中的标识。|
|InvalidAttributeInSignedHeader|指定的签名标头包含指定的属性。 所需的属性已指定。|
|InvalidCloseResponseAction|收到带有指定无效操作的安全会话关闭响应。|
|InvalidQName|QName 无效。|
|InvalidRenewResponseAction|收到带有指定无效操作的安全会话续订响应。|
|InvalidSspiNegotiation|安全支持提供程序接口协商失败。|
|IssuerBindingNotPresentInTokenRequirement|安全令牌管理器需求在描述安全对话的令牌需求中指定引导安全绑定元素。 令牌要求按如下方式指定。|
|KeyLengthMustBeMultipleOfEight|为对称密钥指定的密钥长度不是 8 的倍数。|
|LsaAuthorityNotContacted|内部 SSL 错误（有关详细信息，请参阅 Win32 状态代码）。 请检查服务器证书以确定它是否能够交换密钥。|
|MaximumPolicyRedirectionsExceeded|已达到递归策略提取限制。 请检查以确定联合身份验证服务链中是否存在循环。|
|MessagePartSpecificationMustBeImmutable|在设置前，消息部分规范必须保持不变。|
|MissingCustomCertificateValidator|X509CertificateValidationMode.Custom 要求 CustomCertificateValidator。 请指定 CustomCertificateValidator 属性。|
|MissingCustomUserNamePasswordValidator|UserNamePasswordValidationMode.Custom 要求 CustomUserNamePasswordValidator。 请指定 CustomUserNamePasswordValidator 属性。|
|MissingMembershipProvider|UserNamePasswordValidationMode.MembershipProvider 要求 MembershipProvider。 请指定 MembershipProvider 属性。|
|NoBinaryNegoToSend|未向另一方发送任何二进制协商。|
|NoEncryptionPartsSpecified|没有为具有指定操作的消息指定加密消息部分。|
|NoKeyInfoInEncryptedItemToFindDecryptingToken|在加密项中找不到用于查找解密令牌的 KeyInfo 值。|
|NonceLengthTooShort|指定的 Nonce 太短。 要求的最小 Nonce 长度是 4 个字节。|
|NoOutgoingEndpointAddressAvailableForDoingIdentityCheck|不存在可用于检查要发送消息的标识的传出 EndpointAddress。|
|NoOutgoingEndpointAddressAvailableForDoingIdentityCheckOnReply|不存在可用于检查接收回复的标识的传出 EndpointAddress。|
|NoPartsOfMessageMatchedPartsToSign|没有创建签名，因为不存在符合提供的消息部分规范的消息部分。|
|NoPrincipalSpecifiedInAuthorizationContext|没有在授权上下文中指定自定义主体。|
|NoSignatureAvailableInSecurityHeaderToDoReplayDetection|安全标头中不存在可用于为重播检测提供 Nonce 的签名。|
|NoSignaturePartsSpecified|没有为具有指定操作的消息指定签名消息部分。|
|NoSigningTokenAvailableToDoIncomingIdentityCheck|不存在可用于执行传入标识检查的签名令牌。|
|NoTimestampAvailableInSecurityHeaderToDoReplayDetection|安全标头中不存在可用于执行重播检测的时间戳。|
|NoTransportTokenAssertionProvided|安全策略专家失败。 提供的指定类型的传输令牌断言没有创建包括 sp:TransportBinding 安全策略断言的传输令牌断言。|
|OnlyOneOfEncryptedKeyOrSymmetricBindingCanBeSelected|可以使用对称令牌提供程序和对称令牌验证器或不对称令牌提供程序配置对称安全协议。 不能同时使用此二者来配置它。|
|OperationCannotBeDoneOnReceiverSideSecurityHeaders|无法在接收方安全标头上完成此操作。|
|OperationDoesNotAllowImpersonation|指定服务操作（隶属于具有指定名称和命名空间的协定）不允许模拟。|
|PolicyRequiresConfidentialityWithoutIntegrity|指定操作的消息安全策略需要没有完整性的机密性。 不支持没有完整性的机密性。|
|PrimarySignatureIsRequiredToBeEncrypted|主签名必须加密。|
|PropertySettingErrorOnProtocolFactory|指定安全协议工厂上必需的属性尚未设置或者具有无效的值。|
|ProtocolFactoryCouldNotCreateProtocol|协议工厂无法创建协议。|
|PublicKeyNotRSA|该公钥不是 RSA 密钥。|
|RequiredMessagePartNotEncrypted|所指定的必需的消息部分未加密。|
|RequiredMessagePartNotEncryptedNs|所指定的必需的消息部分未加密。|
|RequiredMessagePartNotSigned|所指定的必需的消息部分未签名。|
|RequiredMessagePartNotSignedNs|所指定的必需的消息部分未签名。|
|RequiredSecurityHeaderElementNotSigned|具有指定 ID 的指定安全标头元素必须签名。|
|RequiredSecurityTokenNotEncrypted|具有指定附件模式的指定安全令牌必须加密。|
|RequiredSecurityTokenNotSigned|具有指定附件模式的指定安全令牌必须签名。|
|RequiredSignatureMissing|签名必须在安全标头中。|
|RequireNonCookieMode|配置具有指定命名空间的指定绑定，以颁发 cookie 安全上下文令牌。 COM+ 集成服务不支持 cookie 安全上下文令牌。|
|RevertingPrivilegeFailed|恢复操作失败，并引发指定的异常。|
|RSTRAuthenticatorIncorrect|RequestSecurityTokenResponse CombinedHash 不正确。|
|SecureConversationCancelNotAllowedFaultReason|绑定不允许安全对话取消。|
|SecureConversationDriverVersionDoesNotSupportSession|配置的 SecureConversation 版本不支持会话。 使用 WSSecureConversationFeb2005 或更高版本。|
|SecureConversationRequiredByReliableSession|无法创建没有安全对话的可靠会话。 启动安全对话。|
|SecurityAuditFailToLoadDll|指定动态链接库 (dll) 加载失败。|
|SecurityAuditNotSupportedOnChannelFactory|通道工厂上不支持 SecurityAuditBehavior。|
|SecurityAuditPlatformNotSupported|当前平台不支持将审核消息写入安全日志。 必须将审核消息写入应用程序日志。|
|SecurityBindingElementCannotBeExpressedInConfig|为终结点导入安全策略。 安全策略包含 Windows Communication Foundation 配置上未描述的需求。 查找生成的配置文件中所需的 SecurityBindingElement 参数的有关注释。 使用代码创建正确的绑定元素。 配置文件中的绑定配置不安全。|
|SecurityBindingSupportsOneWayOnly|指定协定的指定绑定的 SecurityBinding 仅支持 OneWay 操作。|
|SecurityContextDoesNotAllowImpersonation|无法启动模拟，因为具有指定操作的请求消息中 UltimateReceiver 角色的 SecurityContext 未映射到 Windows 标识。|
|SecurityListenerClosing|侦听器未接受新的安全对话，因为其正在关闭。|
|SecurityListenerClosingFaultReason|服务器当前未接受新的安全对话，因为其正在关闭。 请稍后重试。|
|SecurityProtocolFactoryShouldBeSetBeforeThisOperation|执行此操作以前必须先设置安全协议工厂。|
|SecuritySessionAbortedFaultReason|安全会话已终止。 这可能是因为会话已长时间未接收到任何消息。|
|SecuritySessionKeyIsStale|必须先续订会话密钥方可保护应用程序消息安全。|
|SecuritySessionLimitReached|无法创建安全会话。 请稍后重试。|
|SecuritySessionNotPending|没有具有指定 ID 的安全会话处于挂起状态。|
|SecurityTokenParametersHasIncompatibleInclusionMode|使用安全令牌参数配置指定绑定，该参数具有指定的不兼容安全令牌包含模式。 请指定其他安全令牌包含模式。|
|SecurityVersionDoesNotSupportEncryptedKeyBinding|使用不兼容的安全版本配置指定协定的指定绑定，该版本不支持对 EncryptedKeys 的未附加引用。 使用指定的版本或更高的版本作为绑定的安全版本。|
|SecurityVersionDoesNotSupportSignatureConfirmation|指定的 SecurityVersion 不支持签名确认。 使用更高版本的 SecurityVersion。|
|SecurityVersionDoesNotSupportThumbprintX509KeyIdentifierClause|使用安全版本配置指定协定的指定绑定，该安全版本不支持使用证书的指纹值来外部引用 X.509 令牌。 使用指定的版本或更高的版本作为绑定的安全版本。|
|SenderSideSupportingTokensMustSpecifySecurityTokenParameters|必须使用支持令牌为每条消息指定安全令牌参数。|
|ServerCertificateNotProvided|接收方未提供其证书。 TLS 协议需要此证书。 双方都必须访问其证书。|
|SignatureConfirmationNotSupported|配置的 SecurityVersion 不支持签名确认。 请使用 WSSecurityXXX2005 或更高版本。|
|SignatureConfirmationRequiresRequestReply|协议工厂必须支持请求/回复安全，以便提供签名确认。|
|SignatureNotExpected|此消息不需要签名。|
|SigningTokenHasNoKeys|指定的签名令牌没有密钥。 安全令牌用于需要它执行加密操作的上下文中，但令牌不包含加密密钥。 标记类型不支持加密操作或特定标记实例不包含加密密钥。 请检查配置，以确保未在需要加密操作的上下文（例如，认可支持令牌）中指定禁用加密的令牌类型（如 UserNameSecurityToken）。|
|SpnegoImpersonationLevelCannotBeSetToNone|安全支持提供程序接口不支持模拟等级“None”。 请指定 Identification、Impersonation 或 Delegation 等级。|
|SslClientCertMustHavePrivateKey|指定的证书必须有私钥。 该进程必须具有访问私钥的权限。|
|SslServerCertMustDoKeyExchange|指定的证书必须具有能够进行密钥交换的私钥。 该进程必须具有访问私钥的权限。|
|StandardsManagerCannotWriteObject|令牌序列化程序无法序列化指定对象。  如果这是自定义类型，则必须提供自定义序列化程序。|
|TimeStampHasCreationAheadOfExpiry|安全时间戳无效，因为其创建时间大于或等于其过期时间。|
|TimeStampHasCreationTimeInFuture|安全时间戳无效，因为其创建时间是将来时间。 当前时间已指定，允许的时钟偏差已指定。|
|TimeStampHasExpiryTimeInPast|安全时间戳已过时，因为其过期时间是过去的时间。 当前时间已指定，允许的时钟偏差已指定。|
|TimeStampWasCreatedTooLongAgo|安全时间戳已过时，因为其创建时间是很久之前的时间。 当前时间、最大时间戳生存期和允许的时钟偏差已指定。|
|TokenProviderCannotGetTokensForTarget|令牌提供程序无法获得指定目标的令牌。|
|TooManyIssuedSecurityTokenParameters|联合安全链的一环包含多个 IssuedSecurityTokenParameters。 InfoCard 系统仅支持每环一个 IssuedSecurityTokenParameters。|
|TransportDoesNotProtectMessage|指定协定的指定绑定是使用要求传输级完整性和保密性的身份验证模式进行配置的。 但是，传输不能提供完整性和保密性。|
|TrustApr2004DoesNotSupportCertainIssuedTokens|WSTrustApr2004 不支持颁发 X.509 证书或 EncryptedKey。 请使用 WsTrustFeb2005 或更高版本。|
|TrustDriverVersionDoesNotSupportSession|配置的 Trust 版本不支持会话。 请使用 WSTrustFeb2005 或更高版本。|
|UnableToCreateICryptoFromTokenForSignatureVerification|无法从指定令牌创建 ICrypto 接口以进行签名验证。|
|UnableToCreateSymmetricAlgorithmFromToken|无法从令牌创建指定的对称算法。|
|UnableToDeriveKeyFromKeyInfoClause|指定的 KeyInfo 子句解析为指定的令牌，该令牌不包含可用于派生的对称密钥。|
|UnableToFindTokenAuthenticator|找不到指定令牌类型的令牌身份验证器。 根据当前安全设置，无法接受该类型的令牌。|
|UnableToLoadCertificateIdentity|无法加载配置中指定的 X.509 证书标识。|
|UnexpectedEmptyElementExpectingClaim|来自指定命名空间的指定元素为空，且未指定有效的标识请求。|
|UnknownEncodingInBinarySecurityToken|读取二进制安全令牌时，遇到无法识别的编码。|
|UnsecuredMessageFaultReceived|从另一方收到未进行安全处理或安全处理不正确的错误。 有关错误代码和详细信息，请参见内部 FaultException。|
|UnsupportedPasswordType|不支持指定用户名令牌所使用的密码类型。|
|UnsupportedSecureConversationBootstrapProtectionRequirements|无法导入安全策略。 不支持安全对话引导绑定的保护需求。 安全对话引导的保护要求要求必须对请求和响应均进行签名和加密。|
|UnsupportedSecurityPolicyAssertion|在指定的安全策略导入过程中检测到不支持的安全策略断言。|
