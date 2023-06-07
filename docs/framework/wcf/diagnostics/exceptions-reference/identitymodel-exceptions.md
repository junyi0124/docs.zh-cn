---
title: IdentityModel 异常
ms.date: 03/30/2017
ms.assetid: 4ef34497-8ff5-4621-b773-7731cc721231
ms.openlocfilehash: 08d81f4eb35d0f4bda3997d6ab4dfd0ec10407e1
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96275032"
---
# <a name="identitymodel-exceptions"></a>IdentityModel 异常

本主题列出 IdentityModel 生成的所有异常。  
  
## <a name="exception-list"></a>异常列表  
  
|资源代码|当前字符串|  
|-------------------|--------------------|  
|ValueMustBeOf2Types|此参数的值必须为以下两种类型之一。|  
|SAMLSubjectNameIdentifierRequiresNameValue|为 SamlNameIdentifier 指定的“Name”不能为 Null 且长度不能为 0。|  
|TraceCodeIssuanceTokenProviderEndSecurityNegotiation|IssuanceTokenProvider 已完成安全协商。|  
|TraceCodeSecurityNewServerSessionKeyIssued|服务器已颁发新的安全会话密钥。|  
|SAMLAttributeMissingNameAttributeOnRead|缺少所读取的 SamlAttribute 的“Name”或其长度为 0。|  
|UnknownICryptoType|不支持 ICrypto 实现。|  
|TraceCodeSecurityTokenProviderClosed|已关闭安全令牌提供程序。|  
|SAMLUnableToLoadAdvice|未能加载 \<saml:advice> 元素。|  
|SAMLAuthenticationStatementMissingAuthenticationMethodOnRead|缺少 SamlAuthenticationStatement 的所读取的“AuthenticationMethod”属性或其长度为 0。|  
|UnsupportedTransformAlgorithm|不支持的转换或规范化算法。|  
|SAMLAudienceRestrictionShouldHaveOneAudience|SamlAudienceRestrictionCondition 必须包含至少一个受众 (URI)。|  
|SAMLEvidenceShouldHaveOneAssertion|SamlEvidence 必须通过 Id 或引用来引用至少一个 SamlAssertion。|  
|SAMLAudienceRestrictionInvalidAudienceValueOnRead|所读取的 SamlAudienceRestrictionCondition 缺少“Audience”元素中的值。|  
|X509ChainBuildFail|特定的 X.509 证书链生成失败。 所使用的证书具有无法验证的信任链。 请替换该证书或更改 certificateValidationMode。|  
|XDCannotFindValueInDictionaryString|在字典字符串中找不到特定的值 id。|  
|TraceCodeImportSecurityChannelBindingEntry|正在启动安全 ImportChannelBinding。|  
|PrivateKeyExchangeNotSupported|私钥不支持交换 KeySpec。|  
|TokenProviderUnableToGetToken|特定的令牌提供程序无法提供安全令牌。|  
|SAMLEntityCannotBeNullOrEmpty|特定的 SamlAssertion 实体不能为 Null 或为空。|  
|SAMLAssertionRequireOneStatement|SamlAssertion 至少需要一个语句。 请确保已向正在创建的 SamlAssertion 中添加至少一个 SamlStatement。|  
|AESInvalidInputBlockSize|输入大小必须为特定字节的倍数。|  
|AESCryptAcquireContextFailed|获取 CSP 上下文失败。|  
|SAMLAssertionRequireOneStatementOnRead|所读取的 SamlAssertion 不包含任何 SamlStatement。 SamlAssertion 必须至少包含一个 SamlStatement。|  
|TraceCodeSecuritySessionClosedFaultReceived|客户端安全会话接收到来自服务器的会话已关闭错误。|  
|TraceCodeIssuanceTokenProviderRedirectApplied|IssuanceTokenProvider 已应用重定向标头。|  
|TraceCodeSecuritySessionClosedFaultSendFailure|向客户端发送安全会话已关闭错误时发生故障。|  
|ValueMustBeZero|此参数的值必须为0。|  
|SAMLUnableToResolveSignatureKey|无法解析在 SamlAssertion 签名中找到的 SecurityKeyIdentifier。 无法验证特定颁发机构的 SamlAssertion 签名。|  
|X509IsNotInTrustedStore|特定的 X.509 证书不在被信任的人的存储区中。|  
|SAMLElementNotRecognized|不支持此特定元素。|  
|SAMLAuthorizationDecisionStatementMissingResourceAttributeOnRead|缺少所读取的 SamlAuthorizationDecisionStatement 的“Resource”属性或其长度为 0。|  
|SamlTokenMissingSignature|未签名 SamlAssertion。 可以通过设置 SigningCredentials 签名 SamlAssertions。|  
|ExpectedElementMissing|缺少所需的带有特定命名空间的元素。|  
|NoKeyIdentifierClauseFound|未在 SecurityKeyIdentifier 中找到特定类型的子句。|  
|MissingPrivateKey|私钥不在 X.509 证书中。|  
|UnexpectedEOFFromReader|XML 读取器的意外 EOF。|  
|UnsupportedKeyDerivationAlgorithm|不支持特定的密钥派生算法。|  
|TokenDoesNotSupportKeyIdentifierClauseCreation|特定的令牌不支持特定的密钥标识符子句创建。|  
|LastMessageNumberExceeded|检测到序列号协议冲突。|  
|SymmetricKeyLengthTooShort|指定的对称密钥长度太短。|  
|SAMLAuthorityBindingMissingAuthorityKindOnRead|所读取的 SamlAuthorityBinding 包含的“AuthorityKind”丢失或长度为 0。  这是不允许的。|  
|XmlTokenBufferIsEmpty|XmlTokenBuffer 为空。|  
|InvalidXmlQualifiedName|需要 Xml 限定名称，但找到的名称无效。|  
|SAMLAuthorityKindMissingName|代表 SamlAuthorityBinding 中“AuthorityKind”的 XmlQualifiedName 不能为 Null 且长度不能为 0。|  
|AESCryptEncryptFailed|加密特定的数据失败。|  
|AuthorizationContextCreated|已创建带有特定 ID 的授权上下文。|  
|SamlSerializerUnableToReadSecurityKeyIdentifier|SamlSerializer 不包含能够读取 SecurityKeyIdentifier 的 SecurityTokenSerializer。 如果使用自定义 SecurityKeyIdentifier，则必须提供自定义 SecurityTokenSerializer。|  
|TraceCodeIssuanceTokenProviderServiceTokenCacheFull|IssuanceTokenProvider 已减小了服务令牌缓存。|  
|TraceCodeSecurityTokenProviderOpened|已打开安全令牌提供程序。|  
|PublicKeyNotRSA|该公钥不是 RSA 密钥。|  
|InvalidReaderState|特定的状态对于提供的输入读取器无效。|  
|UnableToResolveReferenceUriForSignature|无法解析签名中的特定 URI 以计算摘要。|  
|EmptyBase64Attribute|在必需的 base64 属性名称和命名空间中发现空值。|  
|SAMLSubjectRequiresConfirmationMethodWhenConfirmationDataOrKeyInfoIsSpecified|当指定 Confirmation Data 或 KeyInfo 时 SAML SubjectConfirmation 要求 Confirmation 方法。|  
|SAMLAudienceRestrictionShouldHaveOneAudienceOnRead|所读取的 SamlAudienceRestrictionCondition 必须包含至少一个“Audience”值。 但是未找到任何值。|  
|TokenProviderUnableToRenewToken|特定的令牌提供程序无法续订安全令牌。|  
|AESIVLengthNotSupported|不支持特定位 IV。 仅支持 128 位 IV。|  
|SAMLAuthorityBindingMissingAuthorityKind|SamlAuthorityBinding 必须包含不为 Null 的“AuthorityKind”。|  
|TraceCodeSecuritySessionDemuxFailure|传入的消息不是现有安全会话的一部分。|  
|TokenRenewalNotSupported|特定的令牌提供程序不支持令牌续订。|  
|AtLeastOneReferenceRequired|签名中至少需要一个引用。|  
|SAMLSignatureAlreadyRead|已在 SAML 断言中读取签名。|  
|AlgorithmAndPrivateKeyMisMatch|指定的算法和私钥不匹配。|  
|EmptyTransformChainNotSupported|不支持空转换链。|  
|SspiWrapperEncryptDecryptAssert1|SSPIWrapper：： EncryptDecryptHelper&#124; "offset" 超出范围。|  
|SspiWrapperEncryptDecryptAssert2|SSPIWrapper：： EncryptDecryptHelper&#124; "size" 超出范围。 SecurityTokenManagerCannotCreateAuthenticatorForRequirement=安全令牌管理器无法为特定需求创建令牌身份验证器。|  
|UnableToCreateKeyedHashAlgorithm|无法从特定签名算法的特定值创建 KeyedHashAlgorithm。|  
|SAMLUnableToLoadAssertion|\<saml:assertion>未能加载元素。|  
|X509FindValueMismatchMulti|特定的 X509FindType 要求参数 findValue 的类型为两个值之一。 而自变量 findValue 为另外的类型。|  
|TraceCodeSecurityIdentityDeterminationSuccess|已确定 EndpointAddress 的身份。|  
|UndefinedUseOfPrefixAtElement|元素使用的特定前缀未定义命名空间。|  
|TraceCodeSecuritySessionResponderOperationFailure|服务器上安全会话操作失败。|  
|CannotFindCert|无法使用下列特定的搜索标准找到 X.509 证书：StoreName、StoreLocation、FindType、FindValue。|  
|X509InvalidUsageTime|特定的 X.509 证书使用时间无效。 使用时间不在要求的 NotBefore 时间和 NotAfter 时间之间。|  
|TraceCodeSecurityIdentityDeterminationFailure|无法确定 EndpointAddress 的身份。|  
|AsyncObjectAlreadyEnded|已对此异步结果对象调用 End 方法。|  
|ExternalDictionaryDoesNotContainAllRequiredStrings|外部字典不包含所有必需字符串的定义。 特定字符串在远程字典中不可用。|  
|TraceCodeSecuritySessionKeyRenewalFaultReceived|客户端安全会话接收到来自服务器的密钥续订错误。|  
|SAMLActionNameRequired|代表 SamlAction 的字符串不能为 Null 且长度不能为 0。|  
|SignatureVerificationFailed|签名验证失败。|  
|TraceCodeSecurityContextTokenCacheFull|SecurityContextSecurityToken 缓存已满。|  
|SAMLAssertionMissingMajorVersionAttributeOnRead|缺少所读取的 SamlAssertion 的 MajorVersion 或其长度为 0。|  
|SamlAttributeClaimRightShouldBePossessProperty|此 SamlAttribute 构造函数需要声明权利具有值 System.IdentityModel.Claims.Rights.PossessProperty。|  
|AuthorizationPolicyEvaluated|已评估带有特定 ID 的策略。|  
|SAMLUnableToLoadCondtions<!-- the misspelling here is deliberate. -->|\<saml:conditions>未能加载元素。|  
|AESKeyLengthNotSupported|不支持特定位密钥。 仅支持 128、192 和 256 位密钥。|  
|UserNameCannotBeEmpty|用户名不能为空。|  
|AlgorithmAndPublicKeyMisMatch|指定的算法和公钥不匹配。|  
|SAMLUnableToLoadCondtion<!-- the misspelling here is deliberate. -->|\<saml:conditions>未能加载元素。|  
|SamlAssertionMissingSigningCredentials|尚未在 SamlAssertion 上设置 SigningCredentials。 必须签名 SamlAssertion，请在要处理的 SamlAssertion 上设置有效的 SigningCredentials。|  
|SspiPayloadNotEncrypted|未使用 SSPI 安全上下文加密二进制数据。|  
|SAMLAuthorizationDecisionShouldHaveOneActionOnRead|所读取的 SamlAuthorizationDecisionStatement 不包含任何 SamlAction。|  
|TraceCodeSecurityBindingSecureOutgoingMessageFailure|安全协议无法保证传出消息的安全。|  
|UndefinedUseOfPrefixAtAttribute|特定属性使用的特定前缀未定义命名空间。|  
|NoInputIsSetForCanonicalization|未设置用于写入规范化输出的输入。|  
|TraceCodeSecurityPendingServerSessionAdded|挂起的安全会话已添加到服务器。|  
|AsyncCallbackException|AsyncCallback 引发异常。|  
|PrivateKeyNotRSA|该私钥不是 RSA 密钥。|  
|TraceCodeSecurityClientSessionKeyRenewed|客户端安全会话已续订会话密钥。|  
|SAMLAuthorizationDecisionStatementMissingDecisionAttributeOnRead|缺少所读取的 SamlAuthorizationDecisionStatement 的“Decision”或其长度为 0。|  
|SAMLAttributeNameAttributeRequired|为 SamlAttribute 指定的“Name”不能为 Null 且长度不能为 0。|  
|SamlSerializerRequiresExternalSerializers|SamlSerializer 需要 SecurityTokenSerializer 序列化令牌中存在的 SecurityKeyIdentifier。|  
|UnableToResolveKeyReference|令牌解析程序无法解析特定的安全密钥引用。|  
|UnsupportedKeyWrapAlgorithm|不支持特定的密钥换行算法。|  
|SAMLAssertionMissingIssuerAttributeOnRead|缺少所读取的 SamlAssertion 的“Issuer”或其长度为 0。|  
|TraceCodeIssuanceTokenProviderUsingCachedToken|IssuanceTokenProvider 已使用缓存服务令牌。|  
|AESCryptGetKeyParamFailed|获取特定的密钥参数失败。|  
|InvalidNamespaceForEmptyPrefix|空前缀的命名空间无效。|  
|AESCipherModeNotSupported|不支持特定的密码模式。 仅支持 CBC。|  
|ArgumentCannotBeEmptyString|参数必须为非空字符串。|  
|SAMLAssertionMissingMinorVersionAttributeOnRead|缺少所读取的 SamlAssertion 的 MinorVersion 或其长度为 0。|  
|SpecifiedStringNotAvailableInDictionary|指定的字符串不是当前字典中的项。|  
|KerberosApReqInvalidOrOutOfMemory|AP-REQ 无效或系统没有足够内存。|  
|FailLogonUser|指定用户的 LogonUser 失败。 请确保用户具有有效的 Windows 帐户。|  
|ValueMustBeNonNegative|此自变量的值必须为非负。|  
|X509ValidationFail|指定的 X.509 证书验证失败。|  
|TraceCodeSecuritySessionRequestorOperationSuccess|客户端已成功完成安全会话操作。|  
|SAMLActionNameRequiredOnRead|缺少 SamlAction 读取的字符串或其长度为 0。|  
|KerberosMultilegsNotSupported|指定标识为 UPN。 对运行在要求 Kerberos multi-legs 的用户帐户下的服务进行身份验证，这是不支持的。|  
|SAMLAssertionIdRequired|SamlAssertion 的“assertionId”不能为 Null 或为空。|  
|InvalidOperationForWriterState|指定的操作在指定的 XmlWriter 状态下无效。|  
|CannotValidateSecurityTokenType|指定的安全令牌身份验证器无法验证指定类型的令牌。|  
|X509FindValueMismatch|指定的 X509FindType 要求自变量 findValue 的类型为指定值。 而自变量 findValue 为另外的类型。|  
|TraceCodeSecurityClientSessionCloseSent|Close 消息由客户端安全会话发送。|  
|SuiteDoesNotAcceptAlgorithm|指定的算法套件不接受指定操作的指定算法。|  
|TraceCodeSecuritySessionRequestorOperationFailure|客户端安全会话操作失败。|  
|SAMLUnableToLoadStatement|加载 SamlStatement 失败。|  
|InnerReaderMustBeAtElement|元素必须有内部读取器。|  
|UnableToCreateTokenReference|无法创建安全令牌引用。|  
|TraceCodeSecurityBindingIncomingMessageVerified|安全协议已验证传入的消息。|  
|ObjectIsReadOnly|该对象是只读的。|  
|TraceCodeSecurityClientSessionPreviousKeyDiscarded|客户端安全会话已丢弃先前的会话密钥。|  
|SAMLTokenTimeInvalid|SamlToken 的时间无效。 当前时间在令牌的“有效”时间和“过期”时间外。|  
|TraceCodeSecurityIdentityVerificationSuccess|身份验证成功。|  
|SigningTokenHasNoKeys|指定的签名令牌没有密钥。|  
|TraceCodeSecurityIdentityVerificationFailure|身份验证失败。|  
|AESCryptImportKeyFailed|导入密钥材料失败。|  
|FailInitializeSecurityContext|InitializeSecurityContent 失败。 请确保服务主体名称正确。|  
|TraceCodeStreamSecurityUpgradeAccepted|已成功接受流安全升级。|  
|SAMLAuthorityBindingRequiresLocation|在 SamlAuthorityBinding 上指定的“Location”属性不能为 Null 且长度不能为 0。|  
|PublicKeyNotDSA|该公钥不是 DSA 密钥。|  
|ImpersonationLevelNotSupported|使用 Kerberos 的身份验证模式不支持指定的模拟级别。 请指定有效的标识或模拟级别。|  
|RequiredTargetNotSigned|需要对带有指定 ID 的元素进行签名，但是没有签名。|  
|SAMLAuthenticationStatementMissingAuthenticationInstanceOnRead|缺少 SamlAuthenticationStatement 的所读取的“AuthenticationInstant”属性或其长度为 0。|  
|SAMLEvidenceShouldHaveOneAssertionOnRead|所读取的 SamlEvidence 既不包含对 SamlAssertion 的引用，也不包含嵌入式 SamlAssertion。|  
|LengthOfArrayToConvertMustGreaterThanZero|要转换为整数的数组长度必须大于 0。|  
|InvalidAsyncResult|AsyncResult 无效。|  
|TraceCodeIssuanceTokenProviderRemovedCachedToken|IssuanceTokenProvider 已删除过期的服务令牌。|  
|IncorrectUserNameFormat|用户名格式无效。 用户名格式必须是 "username" 或 "domain \\ \username" 的形式。|  
|TraceCodeExportSecurityChannelBindingEntry|正在启动安全 ExportChannelBinding。|  
|UnsupportedInputTypeForTransform|转换不支持指定的输入类型。|  
|CannotFindDocumentRoot|无法找到文档的根。|  
|XmlBufferQuotaExceeded|缓冲处理 XML 内容所需的大小超出了缓冲区配额。|  
|TraceCodeSecuritySessionClosedResponseSendFailure|向客户端发送安全会话关闭响应时发生故障。|  
|UnableToResolveReferenceInSamlSignature|无法解析带有 AssertionID 的 SAML 签名中指定的引用。|  
|SAMLSubjectRequiresNameIdentifierOrConfirmationMethod|SamlSubject 要求指定“NameIdentifier”或“ConfirmationMethod”。 但二者都未指定。|  
|SAMLAttributeMissingNamespaceAttributeOnRead|缺少所读取的 SamlAttribute 的“Namespace”或其长度为 0。|  
|SAMLSubjectConfirmationClauseMissingConfirmationMethodOnRead|所读取的 SamlSubjectConfirmation 上找不到“ConfirmationMethod”。|  
|SecurityTokenRequirementHasInvalidTypeForProperty|令牌需求的指定属性有意外类型。 属性类型应为其他值。|  
|TraceCodeNegotiationTokenProviderAttached|已附加 NegotiationTokenProvider。|  
|TraceCodeSpnegoClientNegotiationCompleted|SpnegoTokenProvider 已完成 SSPI 协商。|  
|SAMLUnableToLoadUnknownElement|选定的 SamlSerializer 无法反序列化此元素。 请注册自定义 SamlSerializer 来反序列化自定义元素。|  
|CreateSequenceRefused|RM 目标已经拒绝创建顺序请求。|  
|TraceCodeSecuritySessionRedirectApplied|已重定向客户端安全会话。|  
|SecurityTokenRequirementDoesNotContainProperty|令牌要求不包含指定的属性。|  
|SAMLAttributeValueCannotBeNull|SamlAttribute 中找到的 attributeValues 之一为 Null 值。 请确保创建 SamlAttribute 时列表不为 Null。|  
|ValueMustBeGreaterThanZero|此参数的值必须大于 0。|  
|TraceCodeNegotiationAuthenticatorAttached|已附加 NegotiationTokenAuthenticator。|  
|ValueMustBePositive||  
|SAMLAuthorizationDecisionShouldHaveOneAction|SamlAuthorizationDecisionStatement 必须至少有一个 SamlAction。|  
|TraceCodeSecurityTokenAuthenticatorClosed|已关闭安全令牌身份验证器。|  
|TraceCodeSecurityAuditWrittenSuccess|已成功写入安全审核日志。|  
|PrivateKeyNotDSA|该私钥不是 DSA 密钥。|  
|MessageNumberRollover|已超过此顺序的最大序列号。|  
|AESPaddingModeNotSupported|不支持指定的填充模式。 仅支持 PKCS7 和 ISO10126。|  
|SAMLSubjectRequiresNameIdentifierOrConfirmationMethodOnRead|找不到所读取的 SamlSubject 所需的“NameIdentifier”和“ConfirmationMethod”元素。|  
|TraceCodeSecurityAuditWrittenFailure|写入安全审核日志时出现错误。|  
|UnsupportedCryptoAlgorithm|此上下文中不支持指定的加密算法。|  
|SigningTokenHasNoKeysSupportingTheAlgorithmSuite|签名令牌没有支持指定算法套件的密钥。|  
|SAMLNameIdentifierMissingIdentifierValueOnRead|缺少所读取的 SamlNameIdentifier 的“Identifier”字符串。|  
|SAMLSubjectStatementRequiresSubject|SAML 主题语句要求指定 SAML 主题。|  
|TraceCodeSslClientCertMissing|远程 SSL 客户端无法提供所需的证书。|  
|SAMLTokenVersionNotSupported|不支持指定的主版本和次版本。|  
|TraceCodeConfigurationIsReadOnly|配置是只读的。|  
|TraceCodeSecuritySessionRenewFaultSendFailure|向客户端发送安全会话密钥的续订错误时发生故障。|  
|TraceCodeSecurityInactiveSessionFaulted|服务器使非活动安全会话出错。|  
|SAMLUnableToLoadAttribute|加载 SamlAttribute 失败。|  
|Psha1KeyLengthInvalid|指定的 PSHA1 密钥长度无效。|  
|KeyIdentifierCannotCreateKey|SecurityKeyIdentifier 没有任何可以创建密钥的子句。|  
|X509IsInUntrustedStore|指定的 X.509 证书在不受信任的证书存储区中。|  
|UnexpectedXmlChildNode|指定类型的指定的 XML 子节点不是指定元素所预期的。|  
|TokenDoesNotMeetKeySizeRequirements|指定的令牌不满足指定算法套件的密钥大小需求。|  
|TraceCodeSecuritySessionRequestorStartOperation|已在客户端启动安全会话操作。|  
|InvalidHexString|无效的十六进制字符串格式。|  
|SamlAttributeClaimResourceShouldBeAString|此 SamlAttribute 构造函数要求声明的资源为“string”类型。|  
|SamlSigningTokenNotFound|已签名 SamlAssertion，但找不到签名 SamlAssertion 的令牌。 请确保 SecurityTokenResolver 包含签名 SamlAssertion 的令牌。|  
|TraceCodeSecuritySpnToSidMappingFailure|无法将 ServicePrincipalName 映射到 SecurityIdentifier。|  
|UnableToCreateSignatureFormatterFromAsymmetricCrypto|无法从指定的非对称加密创建指定算法的签名格式化程序。|  
|TraceCodeSecurityServerSessionClosedFaultSent|服务器安全会话已将会话关闭错误发送到客户端。|  
|UnableToFindPrefix|在指定元素中无法为指定的明显使用的前缀找到前缀。|  
|TraceCodeSecurityTokenAuthenticatorOpened|已打开安全令牌身份验证器。|  
|RequiredAttributeMissing|指定的元素需要指定的属性。|  
|LocalIdCannotBeEmpty|localId 不能为空。 请指定有效的“localId”。|  
|ValueMustBeInRange|此自变量的值必须在指定的范围内。|  
|TraceCodeIssuanceTokenProviderBeginSecurityNegotiation|IssuanceTokenProvider 已启动新的安全协商。|  
|InvalidNtMapping|指定的 X.509 证书无法映射到 Windows 帐户。 需要 UPN 主题替换名称。|  
|AESCryptSetKeyParamFailed|设置指定的密钥参数失败。|  
|TraceCodeSecuritySessionClosedResponseReceived|客户端安全会话收到来自服务器的已关闭响应。|  
|UnableToCreateSignatureDeformatterFromAsymmetricCrypto|无法从指定的非对称加密创建指定算法的签名反格式化程序。|  
|TraceCodeIdentityModelAsyncCallbackThrewException|异步回调引发异常。|  
|LengthMustBeGreaterThanZero|此自变量的长度必须大于 0。|  
|FoundMultipleCerts|使用指定的下列搜索标准找到多个 X.509 证书：StoreName、StoreLocation、FindType、FindValue。 请提供更具体的查找值。|  
|AtLeastOneTransformRequired|转换元素必须包含至少一个转换。|  
|SAMLTokenNotSerialized|SamlAssertion 无法序列化为 XML。 有关详细信息，请参见内部异常。|  
|TraceCodeSecurityBindingOutgoingMessageSecured|安全协议保证传出消息的安全。|  
|KeyIdentifierClauseDoesNotSupportKeyCreation|此 SecurityKeyIdentifierClause 不支持密钥创建。|  
|UnableToResolveTokenReference|令牌解析程序无法解析指定的令牌引用。|  
|UnsupportedEncryptionAlgorithm|不支持指定的加密算法。|  
|SamlSerializerUnableToWriteSecurityKeyIdentifier|SamlSerializer 不包含能够序列化给定 SecurityKeyIdentifier 的 SecurityTokenSerializer。 如果使用自定义 SecurityKeyIdentifier，则必须提供自定义 SecurityTokenSerializer。|  
|SAMLAttributeShouldHaveOneValue|找不到任何属性值。 SamlAttribute 属性必须至少有一个属性值。|  
|TraceCodeSecurityBindingVerifyIncomingMessageFailure|安全协议无法验证传入的消息。|  
|SamlSigningTokenMissing|传递到 SamlSecurityTokenAuthenticator 的 SamlAssertion 不包含签名令牌。|  
|NoPrivateKeyAvailable|没有可用的私钥。|  
|ValueMustBeOne|此自变量的值必须为 1。|  
|TraceCodeSecurityPendingServerSessionRemoved|服务器已激活挂起的安全会话。|  
|TraceCodeImportSecurityChannelBindingExit|已完成安全 ImportChannelBinding。|  
|X509CertStoreLocationNotValid|StoreLocation 必须为 LocalMachine 或 CurrentUser。|  
|SettingdMayBeModifiedOnlyWhenTheWriterIsInStartState|只有编写器为启动状态时，才能修改编写器设置。|  
|ArgumentInvalidCertificate|证书无效。|  
|DigestVerificationFailedForReference|指定引用的摘要式验证失败。|  
|SAMLAuthorityBindingRequiresBinding|在 SamlAuthorityBinding 上指定的“Binding”属性不能为 Null 且长度不能为 0。|  
|AESInsufficientOutputBuffer|输出缓冲区必须大于指定的字节数。|  
|SAMLAuthorityBindingMissingBindingOnRead|缺少所读取的 SamlAuthorityBinding 的“Binding”属性或其长度为 0。|  
|SAMLAuthorityBindingInvalidAuthorityKind|所读取的 SamlAuthorityBinding 具有无效的 AuthorityKind。 AuthorityKind 的格式必须为 QName。|  
|ProvidedNetworkCredentialsForKerberosHasInvalidUserName|为 Kerberos 令牌提供的 NetworkCredentials 没有有效的用户名。|  
|SSPIPackageNotSupported|不支持指定的 SSPI 包。|  
|TokenCancellationNotSupported|指定的令牌提供程序不支持令牌取消。|  
|UnboundPrefixInQName|指定的限定名称中使用了未绑定前缀。|  
|SAMLAuthorizationDecisionResourceRequired|指定给 SamlAuthorizationDecisionStatement 的“resource”不能为 Null 且长度不能为 0。|  
|TraceCodeSecurityNegotiationProcessingFailure|服务安全协商处理失败。|  
|SAMLAssertionIssuerRequired|为 SamlAssertion 指定的“Issuer”不能为 Null 或为空。|  
|UnableToCreateHashAlgorithmFromAsymmetricCrypto|无法从指定的非对称加密创建指定算法的 HashAlgorithm。|  
|SamlUnableToExtractSubjectKey|在 SamlSubject 中找到的 SecurityKeyIdentifier 无法解析为 SecurityToken。 SecurityTokenResolver 必须包含 SecurityKeyIdentifier 可以解析到的 SecurityToken。|  
|ChildNodeTypeMissing|指定的 XML 元素不具有指定类型的子元素。|  
|TraceCodeSecurityPendingServerSessionClosed|服务器已关闭挂起的安全会话。|  
|TraceCodeSecuritySessionCloseResponseSent|服务器安全会话已将关闭响应发送到客户端。|  
|TraceCodeSecurityIdentityHostNameNormalizationFailure|无法正常化终结点地址的 HostName 部分。|  
|FailAcceptSecurityContext|AcceptSecurityContext 失败。|  
|EmptyXmlElementError|指定的元素不能为空。|  
|PrefixNotDefinedForNamespace|指定命名空间的前缀未在此上下文中定义，也不能声明。|  
|SAMLAuthorizationDecisionHasMoreThanOneEvidence|所读取的 SamlAuthorizationDecisionStatement 包含一个以上的证据。 这是不允许的。|  
|SamlTokenAuthenticatorCanOnlyProcessSamlTokens|SamlSecurityTokenAuthenticator 只能处理 SamlSecurityTokens。 收到指定的 SecurityTokenType。|  
|SAMLAttributeStatementMissingAttributeOnRead|所读取的 SamlAttributeStatement 不包含任何“SamlAttribute”元素。 这是不允许的。|  
|CouldNotFindNamespaceForPrefix|无法在命名空间中查找指定的前缀。|  
|TraceCodeExportSecurityChannelBindingExit|已完成安全 ExportChannelBinding。|  
|AESCryptDecryptFailed|解密指定的数据失败。|  
|SAMLAttributeNamespaceAttributeRequired|为 SamlAttribute 指定的“Namespace”不能为 Null 且长度不能为 0。|  
|TraceCodeSpnegoServiceNegotiationCompleted|SpnegoTokenAuthenticator 已完成 SSPI 协商。|  
|TraceCodeSecurityServerSessionRenewalFaultSent|服务器安全会话已将密钥续订错误发送到客户端。|  
|AlgorithmMismatchForTransform|用于转换的算法中出现不匹配。|  
|UserNameAuthenticationFailed|使用指定的机制验证用户名/密码失败。 用户未通过身份验证。|  
|SamlInvalidSigningToken|已使用未根据协议验证的令牌对 SamlAssertion 进行签名。 如果使用的是 X.509 证书，请检查验证语义。|  
|TraceCodeSecurityServerSessionKeyUpdated|服务器已更新安全会话密钥。|  
|TraceCodeSecurityServerSessionCloseReceived|服务器安全会话接收到来自客户端的关闭消息。|  
|SAMLAuthenticationStatementMissingSubject|SamlAuthenticationStatement 缺少所需的 SamlSubjectStatement。|  
|UnexpectedEndOfFile|意外的文件结尾。|  
|UnsupportedAlgorithmForCryptoOperation|指定的操作不支持指定的算法。|  
|XmlLangAttributeMissing|缺少所需的 xml:lang 属性。|  
|TraceCodeSecurityImpersonationSuccess|服务器上的安全模拟成功。|  
|SAMLAuthorityBindingMissingLocationOnRead|缺少所读取的 SamlAuthorityBinding 的“Location”属性或其长度为 0。|  
|SAMLAttributeStatementMissingSubjectOnRead|缺少 SamlAttributeStatement 的“SamlSubject”元素。|  
|SAMLAuthorizationDecisionStatementMissingSubjectOnRead|缺少所读取的 SamlAuthorizationDecisionStatement 的“SamlSubject”元素。|  
|SAMLBadSchema|读取 SamlAssertion 时发现该指定元素与架构不符。|  
|SAMLAssertionIDIsInvalid|为 SamlAssertion 指定的“assertionId”必须以字母或“_”开始。|  
|TraceCodeSecurityActiveServerSessionRemoved|服务器已移除活动安全会话。|  
|UnableToCreateKeyedHashAlgorithmFromSymmetricCrypto|无法从指定的对称加密创建指定算法的 keyedHashAlgorithm。|  
|SAMLAuthenticationStatementMissingAuthenticationMethod|为 SamlAuthenticationStatement 指定的“AuthenticationMethod”不能为 Null 且长度不能为 0。|  
|TraceCodeSecurityImpersonationFailure|服务器上的安全模拟失败。|  
|默认|（默认值）|  
|UnsupportedNodeTypeInReader|不支持具有指定名称的指定节点类型。|
