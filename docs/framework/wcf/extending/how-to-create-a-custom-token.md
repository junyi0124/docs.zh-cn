---
title: 如何：创建自定义令牌
description: 了解如何使用 SecurityToken 类在 WCF 中创建自定义安全令牌，以及如何将其与安全令牌提供程序和身份验证器集成。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- SecurityTokenParameters class
- security [WCF], creating custom tokens
- WSSecurityTokenSerializer class
- SecurityToken class
ms.assetid: 6d892973-1558-4115-a9e1-696777776125
ms.openlocfilehash: e0d80fe2433d894ef1f9e110e9090701dc305d8e
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96266748"
---
# <a name="how-to-create-a-custom-token"></a>如何：创建自定义令牌

本主题介绍如何使用 <xref:System.IdentityModel.Tokens.SecurityToken> 类创建自定义安全令牌，以及如何将其与自定义安全令牌提供程序和身份验证器进行集成。 有关完整的代码示例，请参阅 [自定义令牌](../samples/custom-token.md) 示例。  
  
 *安全令牌* 实质上是 WINDOWS COMMUNICATION FOUNDATION (WCF) 安全框架用于表示有关 SOAP 消息中发送方的声明的 XML 元素。 WCF 安全为系统提供的身份验证模式提供各种令牌。 包括由 <xref:System.IdentityModel.Tokens.X509SecurityToken> 类表示的 X.509 证书安全令牌，或由 <xref:System.IdentityModel.Tokens.UserNameSecurityToken> 类表示的用户名安全令牌。  
  
 有时，所提供的类型不支持某种身份验证模式或凭据。 这种情况下，必须创建自定义安全令牌来提供 SOAP 消息内部自定义凭据的 XML 表示形式。  
  
 下面的过程演示如何创建自定义安全令牌，以及如何将其与 WCF 安全基础结构集成。 本主题创建一个信用卡令牌，用于将客户端的信用卡相关信息传递到服务器。  
  
 有关自定义凭据和安全令牌管理器的详细信息，请参阅 [演练：创建自定义客户端和服务凭据](walkthrough-creating-custom-client-and-service-credentials.md)。  
  
 若要了解更多表示安全令牌的类，请参见 <xref:System.IdentityModel.Tokens> 命名空间。  
  
## <a name="procedures"></a>过程  

 客户端应用程序必须有一种方式来指定安全基础结构的信用卡信息。 应用程序通过自定义客户端凭据类可访问此信息。 第一步是创建一个类，用以表示自定义客户端凭据的信用卡信息。  
  
#### <a name="to-create-a-class-that-represents-credit-card-information-inside-client-credentials"></a>创建一个表示客户端凭据内信用卡信息的类  
  
1. 定义一个新类，该类在应用程序中表示信用卡信息。 下面的示例将该类命名为 `CreditCardInfo`。  
  
2. 向类添加相应的属性，以便应用程序可以设置自定义令牌所需的必要信息。 在此示例中，该类具有三个属性：`CardNumber`、`CardIssuer` 和 `ExpirationDate`。  
  
     [!code-csharp[c_CustomToken#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customtoken/cs/source.cs#4)]
     [!code-vb[c_CustomToken#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customtoken/vb/source.vb#4)]  
  
 接下来，必须创建一个表示自定义安全令牌的类。 安全令牌提供程序、身份验证器和序列化程序类使用此类将有关安全令牌的信息传递到 WCF 安全基础结构。  
  
#### <a name="to-create-a-custom-security-token-class"></a>创建自定义安全令牌类  
  
1. 定义一个从 <xref:System.IdentityModel.Tokens.SecurityToken> 类派生的新类。 此示例创建一个名为 `CreditCardToken` 的类。  
  
2. 重写 <xref:System.IdentityModel.Tokens.SecurityToken.Id%2A> 属性。 该属性用于获取安全令牌的本地标识符，本地标识符用于从 SOAP 消息内其他元素指向安全令牌 XML 表示形式。 在本示例中，令牌标识符可以作为构造函数参数传递给该属性，也可以在每次创建安全令牌实例时随机生成一个新的令牌标识符。  
  
3. 实现 <xref:System.IdentityModel.Tokens.SecurityToken.SecurityKeys%2A> 属性。 该属性返回一个安全密钥集合，这些密钥是安全令牌实例表示的。 WCF 可以使用此类密钥对 SOAP 消息的部分进行签名或加密。 在本示例中，信用卡安全令牌不能包含任何安全密钥；因此，该实现始终返回一个空集合。  
  
4. 重写 <xref:System.IdentityModel.Tokens.SecurityToken.ValidFrom%2A> 和 <xref:System.IdentityModel.Tokens.SecurityToken.ValidTo%2A> 属性。 WCF 使用这些属性来确定安全令牌实例的有效性。 在本示例中，信用卡安全令牌只有到期日期，因此，`ValidFrom` 属性返回一个 <xref:System.DateTime>，它表示实例的创建日期和时间。  
  
     [!code-csharp[c_CustomToken#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customtoken/cs/source.cs#1)]
     [!code-vb[c_CustomToken#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customtoken/vb/source.vb#1)]  
  
 在创建新的安全令牌类型时，需要实现 <xref:System.ServiceModel.Security.Tokens.SecurityTokenParameters> 类。 该实现在安全绑定元素配置中用于表示新的令牌类型。 安全令牌参数类用作模板，用于在处理消息时与实际安全令牌实例进行匹配。 该模板提供附加属性，应用程序可以使用这些附加属性来指定标准，安全令牌必须符合该标准才能使用或进行身份验证。 下面的示例未添加任何其他属性，因此在 WCF 基础结构搜索要使用或验证的安全令牌实例时只匹配安全令牌类型。  
  
#### <a name="to-create-a-custom-security-token-parameters-class"></a>创建自定义安全令牌参数类  
  
1. 定义一个从 <xref:System.ServiceModel.Security.Tokens.SecurityTokenParameters> 类派生的新类。  
  
2. 实现 <xref:System.ServiceModel.Security.Tokens.SecurityTokenParameters.CloneCore%2A> 方法。 复制类中定义的所有内部字段（如果有）。 此示例未定义任何附加字段。  
  
3. 实现 <xref:System.ServiceModel.Security.Tokens.SecurityTokenParameters.SupportsClientAuthentication%2A> 只读属性。 如果该类表示的安全令牌类型可以用来向服务验证客户端身份，则该属性返回 `true`。 在本示例中，信用卡安全令牌可以用来向服务验证客户端身份。  
  
4. 实现 <xref:System.ServiceModel.Security.Tokens.SecurityTokenParameters.SupportsServerAuthentication%2A> 只读属性。 如果该类表示的安全令牌类型可以用来向客户端验证服务身份，则该属性返回 `true`。 在本示例中，信用卡安全令牌不能用来向客户端验证服务身份。  
  
5. 实现 <xref:System.ServiceModel.Security.Tokens.SecurityTokenParameters.SupportsClientWindowsIdentity%2A> 只读属性。 如果该类表示的安全令牌类型可以映射到 Windows 帐户，则该属性返回 `true`。 这种情况下，身份验证结果由一个 <xref:System.Security.Principal.WindowsIdentity> 类实例表示。 在本示例中，令牌无法映射到 Windows 帐户。  
  
6. 实现 <xref:System.ServiceModel.Security.Tokens.SecurityTokenParameters.CreateKeyIdentifierClause%28System.IdentityModel.Tokens.SecurityToken%2CSystem.ServiceModel.Security.Tokens.SecurityTokenReferenceStyle%29> 方法。 此方法由 WCF 安全框架调用，要求引用此安全令牌参数类所表示的安全标记实例。 实际安全令牌实例和 <xref:System.ServiceModel.Security.Tokens.SecurityTokenReferenceStyle>（指定所请求的引用类型）都作为自变量传递到此方法。 在本示例中，信用卡安全令牌仅支持内部引用。 <xref:System.IdentityModel.Tokens.SecurityToken> 类具有创建内部引用的功能，因此，该实现不需要附加代码。  
  
7. 实现 <xref:System.ServiceModel.Security.Tokens.SecurityTokenParameters.InitializeSecurityTokenRequirement%28System.IdentityModel.Selectors.SecurityTokenRequirement%29> 方法。 此方法由 WCF 调用，以将安全令牌参数类实例转换为类的实例 <xref:System.IdentityModel.Selectors.SecurityTokenRequirement> 。 安全令牌提供程序使用转换结果来创建相应的安全令牌实例。  
  
     [!code-csharp[c_CustomToken#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customtoken/cs/source.cs#2)]
     [!code-vb[c_CustomToken#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customtoken/vb/source.vb#2)]  
  
 安全令牌被传递到 SOAP 消息内部，这要求在安全令牌的内存中表示形式与网络表示形式之间有一种转换机制。 WCF 使用安全令牌序列化程序来完成此任务。 每个自定义令牌都必须有一个自定义安全令牌序列化程序，用于对 SOAP 消息中的自定义安全令牌进行序列化和反序列化。  
  
> [!NOTE]
> 默认情况下启用派生密钥。 如果创建自定义安全令牌并将其用作主要令牌，WCF 将从中派生一个密钥。 如果这样做，则它将调用自定义安全令牌序列化程序以便为自定义安全令牌写入 <xref:System.IdentityModel.Tokens.SecurityKeyIdentifierClause>，同时将 `DerivedKeyToken` 序列化到网络。 在接收端，如果从网络反序列化令牌，则 `DerivedKeyToken` 序列化程序期望 `SecurityTokenReference` 元素作为其下的顶级子元素。 如果自定义安全令牌序列化程序在序列化其子句类型时未添加 `SecurityTokenReference` 元素，则会引发异常。  
  
#### <a name="to-create-a-custom-security-token-serializer"></a>创建自定义安全令牌序列化程序  
  
1. 定义一个从 <xref:System.ServiceModel.Security.WSSecurityTokenSerializer> 类派生的新类。  
  
2. 重写 <xref:System.ServiceModel.Security.WSSecurityTokenSerializer.CanReadTokenCore%28System.Xml.XmlReader%29> 方法，该方法使用一个 <xref:System.Xml.XmlReader> 来读取 XML 流。 如果序列化程序实现可以根据给定的当前元素对安全令牌进行反序列化，则该方法返回 `true`。 在本示例中，该方法检查 XML 读取器的当前 XML 元素是否具有正确的元素名称和命名空间。 如果不具有，则调用该方法的基类实现来处理该 XML 元素。  
  
3. 重写 <xref:System.ServiceModel.Security.WSSecurityTokenSerializer.ReadTokenCore%28System.Xml.XmlReader%2CSystem.IdentityModel.Selectors.SecurityTokenResolver%29> 方法。 此方法读取安全令牌的 XML 内容，并为其构造相应的内存表示形式。 如果它不能识别传入的 XML 读取器当前读取的 XML 元素，则会调用基类实现来处理系统提供的令牌类型。  
  
4. 重写 <xref:System.ServiceModel.Security.WSSecurityTokenSerializer.CanWriteTokenCore%28System.IdentityModel.Tokens.SecurityToken%29> 方法。 如果可以将令牌的内存表示形式（作为参数传入）转换为 XML 表示形式，则此方法返回 `true`。 如果无法转换，则会调用基类实现。  
  
5. 重写 <xref:System.ServiceModel.Security.WSSecurityTokenSerializer.WriteTokenCore%28System.Xml.XmlWriter%2CSystem.IdentityModel.Tokens.SecurityToken%29> 方法。 此方法将安全令牌的内存表示形式转换为 XML 表示形式。 如果此方法无法转换，则会调用基类实现。  
  
     [!code-csharp[c_CustomToken#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customtoken/cs/source.cs#3)]
     [!code-vb[c_CustomToken#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customtoken/vb/source.vb#3)]  
  
 完成前面四个过程之后，将自定义安全令牌与安全令牌提供程序、身份验证器、管理器以及客户端和服务凭据进行集成。  
  
#### <a name="to-integrate-the-custom-security-token-with-a-security-token-provider"></a>将自定义安全令牌与安全令牌提供程序进行集成  
  
1. 安全令牌提供程序可以创建、修改（如果需要）和返回令牌的实例。 若要创建自定义安全令牌的自定义提供程序，请创建一个从 <xref:System.IdentityModel.Selectors.SecurityTokenProvider> 类继承的类。 下面的示例重写 <xref:System.IdentityModel.Selectors.SecurityTokenProvider.GetTokenCore%2A> 方法以返回 `CreditCardToken` 的实例。 有关自定义安全令牌提供程序的详细信息，请参阅 [如何：创建自定义安全令牌提供程序](how-to-create-a-custom-security-token-provider.md)。  
  
     [!code-csharp[c_CustomToken#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customtoken/cs/source.cs#6)]
     [!code-vb[c_CustomToken#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customtoken/vb/source.vb#6)]  
  
#### <a name="to-integrate-the-custom-security-token-with-a-security-token-authenticator"></a>将自定义安全令牌与安全令牌身份验证器进行集成  
  
1. 当安全令牌从消息中提取出来，安全令牌身份验证器即对其内容进行验证。 若要为自定义安全令牌创建自定义身份验证器，请创建一个从 <xref:System.IdentityModel.Selectors.SecurityTokenAuthenticator> 类继承的类。 下面的示例将会重写 <xref:System.IdentityModel.Selectors.SecurityTokenAuthenticator.ValidateTokenCore%2A> 方法。 有关自定义安全令牌验证器的详细信息，请参阅 [如何：创建自定义安全令牌身份验证](how-to-create-a-custom-security-token-authenticator.md)器。  
  
     [!code-csharp[c_CustomToken#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customtoken/cs/source.cs#7)]
     [!code-vb[c_CustomToken#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customtoken/vb/source.vb#7)]  
  
     [!code-csharp[c_CustomToken#12](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customtoken/cs/source.cs#12)]
     [!code-vb[c_CustomToken#12](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customtoken/vb/source.vb#12)]  
  
#### <a name="to-integrate-the-custom-security-token-with-a-security-token-manager"></a>将自定义安全令牌与安全令牌管理器进行集成  
  
1. 安全令牌管理器可以创建相应的令牌提供程序、安全身份验证器和令牌序列化程序实例。 若要创建自定义令牌管理器，请创建一个从 <xref:System.ServiceModel.ClientCredentialsSecurityTokenManager> 类继承的类。 该类的主要方法使用 <xref:System.IdentityModel.Selectors.SecurityTokenRequirement> 来创建相应的提供程序以及客户端或服务凭据。 有关自定义安全令牌管理器的详细信息，请参阅 [演练：创建自定义客户端和服务凭据](walkthrough-creating-custom-client-and-service-credentials.md)。  
  
     [!code-csharp[c_CustomToken#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customtoken/cs/source.cs#8)]
     [!code-vb[c_CustomToken#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customtoken/vb/source.vb#8)]  
  
     [!code-csharp[c_CustomToken#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customtoken/cs/source.cs#9)]
     [!code-vb[c_CustomToken#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customtoken/vb/source.vb#9)]  
  
#### <a name="to-integrate-the-custom-security-token-with-custom-client-and-service-credentials"></a>将自定义安全令牌与自定义客户端和服务凭据进行集成  
  
1. 必须添加自定义客户端和服务凭据，才能向应用程序提供一个 API，使其能够指定先前创建的自定义安全令牌基础结构所使用的自定义令牌信息，从而提供自定义安全令牌内容并对内容进行身份验证。 下面的示例演示如何执行此操作。 有关自定义客户端和服务凭据的详细信息，请参阅 [演练：创建自定义客户端和服务凭据](walkthrough-creating-custom-client-and-service-credentials.md)。  
  
     [!code-csharp[c_CustomToken#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customtoken/cs/source.cs#10)]
     [!code-vb[c_CustomToken#10](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customtoken/vb/source.vb#10)]  
  
     [!code-csharp[c_customToken#11](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customtoken/cs/source.cs#11)]
     [!code-vb[c_customToken#11](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customtoken/vb/source.vb#11)]  
  
 先前创建的自定义安全令牌参数类用于告知 WCF 安全框架，当与服务通信时，必须使用自定义安全令牌。 下面的过程演示如何执行此操作。  
  
#### <a name="to-integrate-the-custom-security-token-with-the-binding"></a>将自定义安全令牌与绑定进行集成  
  
1. 在 <xref:System.ServiceModel.Channels.SecurityBindingElement> 类上公开的一个令牌参数集合中，必须指定自定义安全令牌参数类。 下面的示例使用 `SignedEncrypted` 所返回的集合。 代码将信用卡自定义令牌添加到从客户端发送到服务的每个消息中，并对令牌内容自动进行了签名和加密。  
  
     [!code-csharp[c_CustomToken#13](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customtoken/cs/source.cs#13)]
     [!code-vb[c_CustomToken#13](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customtoken/vb/source.vb#13)]  
  
 本主题显示实现和使用自定义令牌所需的各种不同的代码块。 若要查看所有这些代码片段如何结合在一起的完整示例，请参阅 [自定义令牌](../samples/custom-token.md)。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.IdentityModel.Tokens.SecurityToken>
- <xref:System.ServiceModel.Security.Tokens.SecurityTokenParameters>
- <xref:System.ServiceModel.Security.WSSecurityTokenSerializer>
- <xref:System.IdentityModel.Selectors.SecurityTokenProvider>
- <xref:System.IdentityModel.Selectors.SecurityTokenAuthenticator>
- <xref:System.IdentityModel.Policy.IAuthorizationPolicy>
- <xref:System.IdentityModel.Selectors.SecurityTokenRequirement>
- <xref:System.IdentityModel.Selectors.SecurityTokenManager>
- <xref:System.ServiceModel.Description.ClientCredentials>
- <xref:System.ServiceModel.Description.ServiceCredentials>
- <xref:System.ServiceModel.Channels.SecurityBindingElement>
- [演练：创建自定义客户端和服务凭据](walkthrough-creating-custom-client-and-service-credentials.md)
- [如何：创建自定义安全令牌身份验证器](how-to-create-a-custom-security-token-authenticator.md)
- [如何：创建自定义安全令牌提供程序](how-to-create-a-custom-security-token-provider.md)
