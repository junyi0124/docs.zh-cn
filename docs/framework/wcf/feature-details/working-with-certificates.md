---
title: 使用证书
description: 了解 x.509 数字证书功能以及如何在 WCF 中使用它们。 本文中的资源可以进一步解释这些概念。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- certificates [WCF]
ms.assetid: 6ffb8682-8f07-4a45-afbb-8d2487e9dbc3
ms.openlocfilehash: a12e723c763cdc3b9cf2105df9d0ee601f8bda1a
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90559013"
---
# <a name="working-with-certificates"></a>使用证书

对 Windows Communication Foundation (WCF) 安全性进行编程时，通常使用 X.509 数字证书对客户端和服务器进行身份验证，以及对消息进行加密和数字签名。 本主题将简要说明 X.509 数字证书的功能以及如何在 WCF 中使用它们，并提供一些主题的链接，这些主题对这些概念进行了深入说明，或揭示了如何使用 WCF 和证书来完成常见任务。

简言之，数字证书是公钥基础结构 (PKI) 的一部分，而公钥基础结构是数字证书、证书颁发机构和其他注册机构的系统，它使用公钥加密对电子事务所涉及的每一方的有效性进行确认和身份验证**。 证书颁发机构颁发证书，每个证书都具有一组包含数据的字段，例如“使用者”（向其颁发证书的实体）、有效日期（证书的有效日期）、证书颁发者（颁发证书的实体）和公钥**。 在 WCF 中，这些属性中的每一个都作为 <xref:System.IdentityModel.Claims.Claim> 进行处理，而每个声明又进一步分为两种类型：标识和权限。 有关 X.509 证书的详细信息，请参阅 [X.509 公钥证书](/windows/desktop/SecCertEnroll/about-x-509-public-key-certificates)。 有关 WCF 中的声明和授权的详细信息，请参阅[使用标识模型管理声明和授权](managing-claims-and-authorization-with-the-identity-model.md)。 有关实现 PKI 的详细信息，请参阅 [企业 PKI 和 Windows Server 2012 R2 Active Directory 证书服务](/archive/blogs/yungchou/enterprise-pki-with-windows-server-2012-r2-active-directory-certificate-services-part-1-of-2)。

证书的主要功能是向其他各方验证证书所有者的身份。 证书包含所有者的公钥，所有者保留着私钥**。 公钥可用来对发送给证书所有者的消息进行加密。 只有所有者才能访问私钥，因此，只有所有者才能解密这些消息。

证书必须由证书颁发机构（通常是第三方证书颁发机构）颁发。 在 Windows 域中，包括一个证书颁发机构，可以用来向域中的计算机颁发证书。

## <a name="viewing-certificates"></a>查看证书

若要使用证书，通常需要查看证书并检查其属性。 使用 Microsoft 管理控制台 (MMC) 管理单元工具，很容易实现这一任务。 有关详细信息，请参阅[如何：使用 MMC 管理单元查看证书](how-to-view-certificates-with-the-mmc-snap-in.md)。

## <a name="certificate-stores"></a>证书存储

证书存放在存储区中。 主要的存储区位置有两个，它们进一步分为子存储区。 如果您是计算机的管理员，就可以使用 MMC 管理单元工具查看这两个主要存储区。 非管理员只能查看当前用户存储区。

- **本地计算机存储区**。 这包含由计算机进程（如 ASP.NET）访问的证书。 此位置用于存储向客户端验证服务器身份的证书。

- **当前用户存储区**。 交互式应用程序通常将证书放在此位置，供计算机的当前用户使用。 如果要创建客户端应用程序，通常会将向服务验证用户身份的证书放在此处。

这两个存储区进一步分为子存储区。 使用 WCF 编程时，最重要的子存储区包括：

- **受信任的根证书颁发机构**。 使用此存储区中的证书可以创建证书链，通过证书链，可以追溯到此存储区中的证书颁发机构证书。

  > [!IMPORTANT]
  > 本地计算机隐式信任放置在此存储区中的任何证书，即使证书不是来自受信任的第三方证书颁发机构，也是如此。 因此，除非完全信任颁发机构并了解后果，否则不要将任何证书放入此存储区。

- **个人**。 此存储区用于放置与计算机用户关联的证书。 通常，此存储区用于存放在受信任的根证书颁发机构存储区中找到的证书颁发机构证书之一所颁发的证书。 此外，此处还可存放应用程序自行颁发并且信任的证书。

有关证书存储的详细信息，请参阅[证书存储](/windows/desktop/secauthn/certificate-stores)。

### <a name="selecting-a-store"></a>选择存储区

证书存储位置的选择，取决于服务或客户端运行的方式和时间。 适用以下一般规则：

- 如果 WCF 服务承载于某个 Windows 服务中，则使用“本地计算机”存储区****。 请注意，要将证书安装到本地计算机存储区，需要有管理员权限。

- 如果服务或客户端是在某个用户帐户下运行的应用程序，则使用“当前用户”存储区****。

### <a name="accessing-stores"></a>访问存储区

与计算机上的文件夹一样，存储区也受访问控制列表 (ACL) 保护。 创建由 Internet Information Services (IIS) 承载的服务时，ASP.NET 进程将在 ASP.NET 帐户下运行。 该帐户必须有权访问包含服务所用证书的存储区。 每个主要存储区都由一个默认访问列表保护，但这些列表是可以修改的。 如果创建一个单独的角色访问存储区，则必须向该角色授予访问权限。 要了解如何使用 WinHttpCertConfig.exe 工具修改访问列表，请参阅[如何：创建开发期间使用的临时证书](how-to-create-temporary-certificates-for-use-during-development.md)。

## <a name="chain-trust-and-certificate-authorities"></a>链信任和证书颁发机构

证书是在某种层次结构中创建的，其中每个证书都链接到颁发该证书的 CA。 该链接指向 CA 的证书。 然后，CA 的证书链接到颁发了原始 CA 证书的 CA。 这一过程不断重复，直至到达根 CA 的证书。 根 CA 的证书将以继承方式受到信任。

使用数字证书对实体进行身份验证时就依赖这一层次结构，它也称为信任链**。 通过双击任何证书，然后单击 " **证书路径** " 选项卡，可以使用 MMC 管理单元查看任何证书链。有关导入证书颁发机构的证书链的详细信息，请参阅 [如何：指定用于验证签名的证书颁发机构证书链](specify-the-certificate-authority-chain-verify-signatures-wcf.md)。

> [!NOTE]
> 通过将颁发机构的证书放入受信任的根颁发机构证书存储区中，可以向任何颁发机构指定受信任的根颁发机构。

### <a name="disabling-chain-trust"></a>禁用链信任

在创建新服务时，可能会使用不是由受信任的根证书颁发的证书，或者颁发证书本身可能不在受信任的根证书颁发机构存储区中。 如果仅为了开发目的，可以暂时禁用检查证书信任链的机制。 为此，需要将 `CertificateValidationMode` 属性设置为 `PeerTrust` 或 `PeerOrChainTrust`。 这两种模式都指定，证书可以是自行颁发的（对等信任），也可以是信任链的一部分。 在下列所有类上，都可以设置此属性。

|类|Property|
|-----------|--------------|
|<xref:System.ServiceModel.Security.X509ClientCertificateAuthentication>|<xref:System.ServiceModel.Security.X509ClientCertificateAuthentication.CertificateValidationMode%2A?displayProperty=nameWithType>|
|<xref:System.ServiceModel.Security.X509PeerCertificateAuthentication>|<xref:System.ServiceModel.Security.X509PeerCertificateAuthentication.CertificateValidationMode%2A?displayProperty=nameWithType>|
|<xref:System.ServiceModel.Security.X509ServiceCertificateAuthentication>|<xref:System.ServiceModel.Security.X509ServiceCertificateAuthentication.CertificateValidationMode%2A?displayProperty=nameWithType>|
|<xref:System.ServiceModel.Security.IssuedTokenServiceCredential>|<xref:System.ServiceModel.Security.IssuedTokenServiceCredential.CertificateValidationMode%2A?displayProperty=nameWithType>|

此外，也可以通过配置设置此属性。 下列元素用于指定验证模式：

- [\<authentication>](../../configure-apps/file-schema/wcf/authentication-of-servicecertificate-element.md)

- [\<peerAuthentication>](../../configure-apps/file-schema/wcf/peerauthentication-element.md)

- [\<messageSenderAuthentication>](../../configure-apps/file-schema/wcf/messagesenderauthentication-element.md)

## <a name="custom-authentication"></a>自定义身份验证

使用 `CertificateValidationMode` 属性，还可以对证书的身份验证方式进行自定义。 默认情况下，级别设置为 `ChainTrust`。 若要使用 <xref:System.ServiceModel.Security.X509CertificateValidationMode.Custom> 值，还必须将 `CustomCertificateValidatorType` 属性设置为程序集和用于验证证书的类型。 若要创建自定义验证程序，必须从抽象 <xref:System.IdentityModel.Selectors.X509CertificateValidator> 类继承。

在创建自定义身份验证器时，要重写的最重要方法是 <xref:System.IdentityModel.Selectors.X509CertificateValidator.Validate%2A> 方法。 有关自定义身份验证的示例，请参阅 [X.509 证书验证程序](../samples/x-509-certificate-validator.md)示例。 有关详细信息，请参阅[自定义凭据和凭据验证](../extending/custom-credential-and-credential-validation.md)。

## <a name="using-the-powershell-new-selfsignedcertificate-cmdlet-to-build-a-certificate-chain"></a>使用 PowerShell New-selfsignedcertificate Cmdlet 构建证书链

PowerShell New-selfsignedcertificate cmdlet 创建 x.509 证书和私钥/公钥对。 可以将私钥保存在磁盘上，然后用它来颁发和签名新证书，从而模拟链状证书的层次结构。 此 cmdlet 仅用作开发服务时的辅助手段，不应用于创建用于实际部署的证书。 在开发 WCF 服务时，请使用以下步骤通过 New-selfsignedcertificate cmdlet 生成信任链。

#### <a name="to-build-a-chain-of-trust-with-the-new-selfsignedcertificate-cmdlet"></a>使用 New-selfsignedcertificate cmdlet 构建信任链

1. 使用 New-selfsignedcertificate cmdlet 创建 (自签名) 证书的临时根证书颁发机构。 将私钥保存到磁盘上。

2. 使用此新证书颁发另一个包含公钥的证书。

3. 将根颁发机构证书导入到受信任的根证书颁发机构存储区中。

4. 有关分步说明，请参阅[如何：创建开发期间使用的临时证书](how-to-create-temporary-certificates-for-use-during-development.md)。

## <a name="which-certificate-to-use"></a>要使用哪个证书？

关于证书的常见问题是要使用哪个证书以及使用它的原因。 答案取决于是对客户端编程还是对服务编程。 下面的信息提供了一个一般准则，但未对这些问题提供详尽解答。

### <a name="service-certificates"></a>服务证书

服务证书的主要任务是向客户端验证服务器的身份。 客户端对服务器进行身份验证时所进行的初始检查之一是将“使用者”字段的值与用来联系服务的统一资源标识符 (URI) 进行比较：二者的 DNS 必须匹配****。 例如，如果服务的 URI 为，则 `http://www.contoso.com/endpoint/` " **使用者** " 字段还必须包含值 `www.contoso.com` 。

请注意，该字段可以包含多个值，每个值都以一个起始值前缀来指示其值。 最常见的情况是，初始化为公用名 "CN"，例如 `CN = www.contoso.com` 。 “使用者”字段还可能为空白，这种情况下，“使用者可选名称”字段可以包含“DNS 名称”值************。

另请注意，证书“预期目的”字段的值应包含一个适当的值，例如“服务器身份验证”或“客户端身份验证”****。

### <a name="client-certificates"></a>客户端证书

客户端证书通常不是由第三方证书颁发机构颁发的。 相反，在当前用户位置的个人存储区中，通常包含由根证书颁发机构存放在此的证书，其预期目的为“客户端身份验证”。 客户端可以在需要相互身份验证时使用此类证书。

## <a name="online-revocation-and-offline-revocation"></a>联机吊销和脱机吊销

### <a name="certificate-validity"></a>证书有效性

每个证书仅在给定时间段内有效，称为“有效期”**。 有效期由 X.509 证书的“有效起始日期”和“有效结束日期”字段定义********。 在身份验证过程中，将对证书进行检查以确定证书是否仍在有效期内。

### <a name="certificate-revocation-list"></a>证书吊销列表

在有效期内，证书颁发机构随时可以吊销证书。 吊销证书有多种原因，例如危及证书私钥的安全。

如果证书吊销，则从被吊销证书继承的任何链都将无效，在身份验证过程中也不受信任。 为了查看吊销了哪些证书，每个证书颁发者都发布一个加盖了时间戳和日期戳的证书吊销列表 (CRL)**。 可以使用联机吊销或脱机吊销来查看此列表，方法是将以下类的 `RevocationMode` 或 `DefaultRevocationMode` 属性设置为 <xref:System.Security.Cryptography.X509Certificates.X509RevocationMode> 枚举值之一：<xref:System.ServiceModel.Security.X509ClientCertificateAuthentication>、<xref:System.ServiceModel.Security.X509PeerCertificateAuthentication>、<xref:System.ServiceModel.Security.X509ServiceCertificateAuthentication> 和 <xref:System.ServiceModel.Security.IssuedTokenServiceCredential> 类。 所有属性的默认值都是 `Online`。

你还可以使用 `revocationMode`) 的 (的属性 [\<authentication>](../../configure-apps/file-schema/wcf/authentication-of-clientcertificate-element.md) [\<serviceBehaviors>](../../configure-apps/file-schema/wcf/servicebehaviors.md) 和) 的 [\<authentication>](../../configure-apps/file-schema/wcf/authentication-of-clientcertificate-element.md) ([\<endpointBehaviors>](../../configure-apps/file-schema/wcf/endpointbehaviors.md) 设置配置模式。

## <a name="the-setcertificate-method"></a>SetCertificate 方法

在 WCF 中，必须经常指定一个或一组证书，供服务或客户端用来对消息进行身份验证、加密或数字签名。 使用表示 X.509 证书的各个类的 `SetCertificate` 方法，可以以编程方式实现这一操作。 下面的类使用 `SetCertificate` 方法指定一个证书。

|类|方法|
|-----------|------------|
|<xref:System.ServiceModel.Security.PeerCredential>|<xref:System.ServiceModel.Security.PeerCredential.SetCertificate%2A>|
|<xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential>|<xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential.SetCertificate%2A>|
|<xref:System.ServiceModel.Security.X509CertificateRecipientServiceCredential>|<xref:System.ServiceModel.Security.X509CertificateRecipientServiceCredential.SetCertificate%2A>|
|<xref:System.ServiceModel.Security.X509CertificateInitiatorServiceCredential>|
|<xref:System.ServiceModel.Security.X509CertificateInitiatorServiceCredential.SetCertificate%2A>|

`SetCertificate` 方法通过指定一个存储区位置和存储区、一个指定证书字段的“查找”类型（`x509FindType` 参数）和一个要在字段中查找的值来实现其功能。 例如，下面的代码创建一个 <xref:System.ServiceModel.ServiceHost> 实例，并使用 `SetCertificate` 方法设置用于向客户端验证服务身份的服务证书。

[!code-csharp[c_WorkingWithCertificates#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_workingwithcertificates/cs/source.cs#1)]
[!code-vb[c_WorkingWithCertificates#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_workingwithcertificates/vb/source.vb#1)]

### <a name="multiple-certificates-with-the-same-value"></a>具有相同值的多个证书

一个存储区可以包含多个具有相同主题名称的证书。 这意味着如果将 `x509FindType` 指定为 <xref:System.Security.Cryptography.X509Certificates.X509FindType.FindBySubjectName> 或 <xref:System.Security.Cryptography.X509Certificates.X509FindType.FindBySubjectDistinguishedName>，并且多个证书具有相同的值，则会引发异常，因为无法区分需要的是哪个证书。 通过将 `x509FindType` 设置为 <xref:System.Security.Cryptography.X509Certificates.X509FindType.FindByThumbprint>，可以缓解这一问题。 指纹字段包含一个唯一值，可以用来查找存储区中的特定证书。 但是，这种方法也有缺点，如果证书已吊销或续订，则 `SetCertificate` 方法会失败，因为指纹也消失了。 或者，如果证书不再有效，则身份验证将失败。 缓解这一问题的方法是将 `x590FindType` 参数设置为 <xref:System.Security.Cryptography.X509Certificates.X509FindType.FindByIssuerName> 并指定颁发机构的名称。 如果不需要特定颁发机构，也可以设置一个其他的 <xref:System.Security.Cryptography.X509Certificates.X509FindType> 枚举值，例如 <xref:System.Security.Cryptography.X509Certificates.X509FindType.FindByTimeValid>。

## <a name="certificates-in-configuration"></a>配置中的证书

此外，通过配置也可以设置证书。 如果创建的是服务，则会在下指定凭据，包括证书 [\<serviceBehaviors>](../../configure-apps/file-schema/wcf/servicebehaviors.md) 。 在对客户端进行编程时，将在下指定证书 [\<endpointBehaviors>](../../configure-apps/file-schema/wcf/endpointbehaviors.md) 。

## <a name="mapping-a-certificate-to-a-user-account"></a>将证书映射到用户帐户

IIS 和 Active Directory 的一个功能是将证书映射到 Windows 用户帐户。 有关此功能的详细信息，请参阅 [Map certificates to user accounts](/previous-versions/windows/it-pro/windows-server-2003/cc736706(v=ws.10))（将证书映射到用户帐户）。

有关使用 Active Directory 映射的详细信息，请参阅 [Mapping Client Certificates with Directory Service Mapping](/previous-versions/windows/it-pro/windows-server-2003/cc758484(v=ws.10))（使用目录服务映射来映射客户端证书）。

如果启用了这一功能，则可以将 <xref:System.ServiceModel.Security.X509ClientCertificateAuthentication.MapClientCertificateToWindowsAccount%2A> 类的 <xref:System.ServiceModel.Security.X509ClientCertificateAuthentication> 属性设置为 `true`。 在 "配置" 中，可以将 `mapClientCertificateToWindowsAccount` 元素的属性设置 [\<authentication>](../../configure-apps/file-schema/wcf/authentication-of-servicecertificate-element.md) 为 `true` ，如下面的代码所示。

```xml
<serviceBehaviors>
 <behavior name="MappingBehavior">
  <serviceCredentials>
   <clientCertificate>
    <authentication certificateValidationMode="None" mapClientCertificateToWindowsAccount="true" />
   </clientCertificate>
  </serviceCredentials>
 </behavior>
</serviceBehaviors>
```

将 X.509 证书映射到表示 Windows 用户帐户的令牌，这一过程视为权限提升，这是因为一旦执行了映射，就可使用 Windows 令牌获得对受保护资源的访问权限。 因此，域策略要求 X.509 证书在映射之前符合其策略。 SChannel 安全数据包强制执行此要求**。

使用 .NET Framework 3.5 或更高版本时，在将证书映射到 Windows 帐户之前，WCF 确保证书符合域策略。

在 WCF 第一版中，可在无需考虑域策略的情况下，执行映射。 因此，如果启用了映射，而 X.509 证书不满足域策略，则在第一版下运行正常的早期应用程序，可能会失败。

## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Channels>
- <xref:System.ServiceModel.Security>
- <xref:System.ServiceModel>
- <xref:System.Security.Cryptography.X509Certificates.X509FindType>
- [保护服务和客户端的安全](securing-services-and-clients.md)
