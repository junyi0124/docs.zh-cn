---
title: 演练：创建自定义客户端和服务凭据
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 2b5ba5c3-0c6c-48e9-9e46-54acaec443ba
ms.openlocfilehash: ddd9f03e26f7a8345f1070715e4877c533c361fb
ms.sourcegitcommit: 59e36e65ac81cdd094a5a84617625b2a0ff3506e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "80345264"
---
# <a name="walkthrough-creating-custom-client-and-service-credentials"></a>演练：创建自定义客户端和服务凭据

本主题演示如何实现自定义客户端和服务凭据以及如何在应用程序代码中使用自定义凭据。

## <a name="credentials-extensibility-classes"></a>凭据扩展性类

<xref:System.ServiceModel.Description.ClientCredentials>和<xref:System.ServiceModel.Description.ServiceCredentials>类是 Windows 通信基础 （WCF） 安全扩展性的主要入口点。 这些凭据类提供 API，应用程序代码可以使用这些 API 来设置凭据信息和将凭据类型转换为安全令牌。 （*安全令牌*是用于在 SOAP 消息中传输凭据信息的窗体。这些凭据类的职责可以分为两个方面：

- 为应用程序提供 API 以设置凭据信息。

- 用作 <xref:System.IdentityModel.Selectors.SecurityTokenManager> 实现的工厂。

WCF 中提供的默认实现支持系统提供的凭据类型，并创建能够处理这些凭据类型的安全令牌管理器。

## <a name="reasons-to-customize"></a>自定义原因

自定义客户端或服务凭据类有多种原因。 最重要的是要求更改处理系统提供的凭据类型的默认 WCF 安全行为，尤其是出于以下原因：

- 无法使用其他扩展点进行的更改。

- 添加新的凭据类型。

- 添加新的自定义安全令牌类型。

本主题说明如何实现自定义客户端和服务凭据以及如何在应用程序代码中使用它们。

## <a name="first-in-a-series"></a>系列主题中的第一个主题

创建自定义凭据类只是第一步，因为自定义凭据的原因是更改有关凭据预配、安全令牌序列化或身份验证的 WCF 行为。 本节中的其他主题说明如何创建自定义序列化程序和身份验证器。 在这一方面，创建自定义凭据类是系列主题中的第一个主题。 后续操作（创建自定义序列化程序和身份验证器）只有在创建自定义凭据后才能进行。 基于本主题的其他主题包括：

- [如何：创建自定义安全令牌提供程序](how-to-create-a-custom-security-token-provider.md)

- [如何：创建自定义安全令牌身份验证器](how-to-create-a-custom-security-token-authenticator.md)

- [如何：创建自定义令牌](how-to-create-a-custom-token.md)。

## <a name="procedures"></a>过程

#### <a name="to-implement-custom-client-credentials"></a>实现自定义客户端凭据

1. 定义一个从 <xref:System.ServiceModel.Description.ClientCredentials> 类派生的新类。

2. 可选。 为新凭据类型添加新方法或新属性。 如果未添加新凭据类型，请跳过此步骤。 下面的示例添加 `CreditCardNumber` 属性。

3. 重写 <xref:System.ServiceModel.Security.SecurityCredentialsManager.CreateSecurityTokenManager%2A> 方法。 使用自定义客户端凭据时，WCF 安全基础结构会自动调用此方法。 此方法负责创建和返回 <xref:System.IdentityModel.Selectors.SecurityTokenManager> 类实现的实例。

    > [!IMPORTANT]
    > 需要特别注意的是，将重写 <xref:System.ServiceModel.Security.SecurityCredentialsManager.CreateSecurityTokenManager%2A> 方法以创建自定义安全令牌管理器。 派生自 <xref:System.ServiceModel.ClientCredentialsSecurityTokenManager> 的安全令牌管理器必须返回派生自 <xref:System.IdentityModel.Selectors.SecurityTokenProvider> 的自定义安全令牌提供程序，才能创建实际的安全令牌。 如果不遵循此模式创建安全令牌，则当缓存 <xref:System.ServiceModel.ChannelFactory> 对象（这是 WCF 客户端代理的默认行为）时，您的应用程序可能无法正常工作，从而可能会导致面临特权提升攻击。 自定义凭据对象将作为 <xref:System.ServiceModel.ChannelFactory> 的一部分进行缓存。 然而，在每次调用时都会创建自定义 <xref:System.IdentityModel.Selectors.SecurityTokenManager>，只要在 <xref:System.IdentityModel.Selectors.SecurityTokenManager> 中放置了令牌创建逻辑，它就可以缓解安全威胁。

4. 重写 <xref:System.ServiceModel.Description.ClientCredentials.CloneCore%2A> 方法。

    [!code-csharp[c_CustomCredentials#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customcredentials/cs/source.cs#1)]
    [!code-vb[c_CustomCredentials#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customcredentials/vb/client/client.vb#1)]

#### <a name="to-implement-a-custom-client-security-token-manager"></a>实现自定义客户端安全令牌管理器

1. 定义一个从 <xref:System.ServiceModel.ClientCredentialsSecurityTokenManager> 派生的新类。

2. 可选。 如果必须创建一个自定义 <xref:System.IdentityModel.Selectors.SecurityTokenManager.CreateSecurityTokenProvider%28System.IdentityModel.Selectors.SecurityTokenRequirement%29> 实现，则重写 <xref:System.IdentityModel.Selectors.SecurityTokenProvider> 方法。 有关自定义安全令牌提供程序的详细信息，请参阅[如何：创建自定义安全令牌提供程序](how-to-create-a-custom-security-token-provider.md)。

3. 可选。 如果必须创建一个自定义 <xref:System.IdentityModel.Selectors.SecurityTokenManager.CreateSecurityTokenAuthenticator%28System.IdentityModel.Selectors.SecurityTokenRequirement%2CSystem.IdentityModel.Selectors.SecurityTokenResolver%40%29> 实现，则重写 <xref:System.IdentityModel.Selectors.SecurityTokenAuthenticator> 方法。 有关自定义安全令牌身份验证器的详细信息，请参阅[如何：创建自定义安全令牌身份验证器](how-to-create-a-custom-security-token-authenticator.md)。

4. 可选。 如果必须创建一个自定义 <xref:System.IdentityModel.Selectors.SecurityTokenManager.CreateSecurityTokenSerializer%2A>，则重写 <xref:System.IdentityModel.Selectors.SecurityTokenSerializer> 方法。 有关自定义安全令牌和自定义安全令牌序列化器的详细信息，请参阅[如何：创建自定义令牌](how-to-create-a-custom-token.md)。

    [!code-csharp[c_CustomCredentials#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customcredentials/cs/source.cs#2)]
    [!code-vb[c_CustomCredentials#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customcredentials/vb/client/client.vb#2)]

#### <a name="to-use-a-custom-client-credentials-from-application-code"></a>在应用程序代码中使用自定义客户端凭据

1. 创建生成的表示服务接口的客户端的实例，或创建指向要与之通信的服务的 <xref:System.ServiceModel.ChannelFactory> 的实例。

2. 从 <xref:System.ServiceModel.Description.ServiceEndpoint.Behaviors%2A> 集合中删除系统提供的客户端凭据行为，此集合可以通过 <xref:System.ServiceModel.ChannelFactory.Endpoint%2A> 属性访问。

3. 创建自定义客户端凭据类的一个新实例并将其添加到 <xref:System.ServiceModel.Description.ServiceEndpoint.Behaviors%2A> 集合中，此集合可以通过 <xref:System.ServiceModel.ChannelFactory.Endpoint%2A> 属性访问。

    [!code-csharp[c_CustomCredentials#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customcredentials/cs/source.cs#3)]
    [!code-vb[c_CustomCredentials#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customcredentials/vb/client/client.vb#3)]

前面的过程演示如何从应用程序代码中使用客户端凭据。 也可以使用应用程序配置文件配置 WCF 凭据。 使用应用程序配置通常比硬编码更可取，因为它允许更改应用程序参数而无需更改源代码、重新编译和重新部署。

下一个过程说明如何为配置自定义凭据提供支持。

#### <a name="creating-a-configuration-handler-for-custom-client-credentials"></a>创建自定义客户端凭据的配置处理程序

1. 定义一个从 <xref:System.ServiceModel.Configuration.ClientCredentialsElement> 派生的新类。

2. 可选。 为所有其他要通过应用程序配置公开的配置参数添加属性。 下面的示例添加一个名为 `CreditCardNumber` 的属性。

3. 重写 <xref:System.ServiceModel.Configuration.BehaviorExtensionElement.BehaviorType%2A> 属性以返回使用配置元素创建的自定义客户端凭据类的类型。

4. 重写 <xref:System.ServiceModel.Configuration.BehaviorExtensionElement.CreateBehavior%2A> 方法。 此方法负责根据从配置文件加载的设置创建和返回自定义凭据类的一个实例。 从此方法中调用基 <xref:System.ServiceModel.Configuration.ClientCredentialsElement.ApplyConfiguration%28System.ServiceModel.Description.ClientCredentials%29> 方法以检索加载到客户端凭据实例中的系统提供的凭据设置。

5. 可选。 如果您在步骤 2 中添加了其他属性，则需要重写 <xref:System.Configuration.ConfigurationElement.Properties%2A> 属性，以便注册其他配置设置，使配置框架可以识别它们。 组合使用您的属性与基类属性可以通过此自定义客户端凭据配置元素来配置系统提供的设置。

    [!code-csharp[c_CustomCredentials#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customcredentials/cs/source.cs#7)]
    [!code-vb[c_CustomCredentials#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customcredentials/vb/service/service.vb#7)]

获得配置处理程序类后，即可将其集成到 WCF 配置框架中。 这使自定义客户端凭据能够用于客户端终结点行为元素中，如下一个过程所示。

#### <a name="to-register-and-use-a-custom-client-credentials-configuration-handler-in-the-application-configuration"></a>在应用程序配置中注册和使用自定义客户端凭据配置处理程序

1. 向配置文件添加`extensions`<>元素和<>`behaviorExtensions`元素。

2. 向<>`add``behaviorExtensions`元素添加<>元素，`name`并将属性设置为适当的值。

3. 将 `type` 特性设置为完全限定类型名称。 此外还包括程序集名称和其他程序集属性。

    ```xml
    <system.serviceModel>
      <extensions>
        <behaviorExtensions>
          <add name="myClientCredentials" type="Microsoft.ServiceModel.Samples.MyClientCredentialsConfigHandler, CustomCredentials, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
        </behaviorExtensions>
      </extensions>
    </system.serviceModel>
    ```

4. 注册配置处理程序后，可以在相同的配置文件中使用自定义凭据元素，而不是系统提供的<>`clientCredentials`元素。 可以同时使用系统提供的属性和任何添加到配置处理程序实现的新属性。 下面的示例使用 `creditCardNumber` 属性设置自定义属性的值。

    ```xml
    <behaviors>
      <endpointBehaviors>
        <behavior name="myClientCredentialsBehavior">
          <myClientCredentials creditCardNumber="123-123-123"/>
        </behavior>
      </endpointBehaviors>
    </behaviors>
    ```

#### <a name="to-implement-custom-service-credentials"></a>实现自定义服务凭据

1. 定义一个从 <xref:System.ServiceModel.Description.ServiceCredentials> 派生的新类。

2. 可选。 添加新属性以为将要添加的新凭据值提供 API。 如果未添加新凭据值，请跳过此步骤。 下面的示例添加 `AdditionalCertificate` 属性。

3. 重写 <xref:System.ServiceModel.Security.SecurityCredentialsManager.CreateSecurityTokenManager%2A> 方法。 使用自定义客户端凭据时，WCF 基础结构会自动调用此方法。 此方法负责创建和返回 <xref:System.IdentityModel.Selectors.SecurityTokenManager> 类的实现的实例（在下一个过程中说明）。

4. 可选。 重写 <xref:System.ServiceModel.Description.ServiceCredentials.CloneCore%2A> 方法。 只有在将新属性或内部字段添加到自定义客户端凭据实现时，才需要此操作。

    [!code-csharp[c_CustomCredentials#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customcredentials/cs/source.cs#4)]
    [!code-vb[c_CustomCredentials#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customcredentials/vb/service/service.vb#4)]

#### <a name="to-implement-a-custom-service-security-token-manager"></a>实现自定义服务安全令牌管理器

1. 定义一个从 <xref:System.ServiceModel.Security.ServiceCredentialsSecurityTokenManager> 类派生的新类。

2. 可选。 如果必须创建一个自定义 <xref:System.IdentityModel.Selectors.SecurityTokenManager.CreateSecurityTokenProvider%2A> 实现，则重写 <xref:System.IdentityModel.Selectors.SecurityTokenProvider> 方法。 有关自定义安全令牌提供程序的详细信息，请参阅[如何：创建自定义安全令牌提供程序](how-to-create-a-custom-security-token-provider.md)。

3. 可选。 如果必须创建一个自定义 <xref:System.IdentityModel.Selectors.SecurityTokenManager.CreateSecurityTokenAuthenticator%2A> 实现，则重写 <xref:System.IdentityModel.Selectors.SecurityTokenAuthenticator> 方法。 有关自定义安全令牌身份验证器的详细信息，请参阅[如何：创建自定义安全令牌身份验证器](how-to-create-a-custom-security-token-authenticator.md)主题。

4. 可选。 如果必须创建一个自定义 <xref:System.IdentityModel.Selectors.SecurityTokenManager.CreateSecurityTokenSerializer%28System.IdentityModel.Selectors.SecurityTokenVersion%29>，则重写 <xref:System.IdentityModel.Selectors.SecurityTokenSerializer> 方法。 有关自定义安全令牌和自定义安全令牌序列化器的详细信息，请参阅[如何：创建自定义令牌](how-to-create-a-custom-token.md)。

    [!code-csharp[c_CustomCredentials#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customcredentials/cs/source.cs#5)]
    [!code-vb[c_CustomCredentials#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customcredentials/vb/service/service.vb#5)]

#### <a name="to-use-custom-service-credentials-from-application-code"></a>在应用程序代码中使用自定义服务凭据

1. 创建 <xref:System.ServiceModel.ServiceHost> 的实例：

2. 从 <xref:System.ServiceModel.Description.ServiceDescription.Behaviors%2A> 集合中删除系统提供的服务凭据行为。

3. 创建自定义服务凭据类的一个新实例并将其添加到 <xref:System.ServiceModel.Description.ServiceDescription.Behaviors%2A> 集合中。

    [!code-csharp[c_CustomCredentials#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customcredentials/cs/source.cs#6)]
    [!code-vb[c_CustomCredentials#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customcredentials/vb/service/service.vb#6)]

使用过程"和"`To create a configuration handler for custom client credentials`中前面描述的步骤添加对配置的支持。`To register and use a custom client credentials configuration handler in the application configuration`唯一的区别是使用类<xref:System.ServiceModel.Configuration.ServiceCredentialsElement>而不是类<xref:System.ServiceModel.Configuration.ClientCredentialsElement>作为配置处理程序的基础类。 这样，使用系统提供的 `<serviceCredentials>` 元素的任何地方都可以使用自定义服务凭据元素。

## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Description.ClientCredentials>
- <xref:System.ServiceModel.Description.ServiceCredentials>
- <xref:System.ServiceModel.Security.SecurityCredentialsManager>
- <xref:System.IdentityModel.Selectors.SecurityTokenManager>
- <xref:System.ServiceModel.Configuration.ClientCredentialsElement>
- <xref:System.ServiceModel.Configuration.ServiceCredentialsElement>
- [如何：创建自定义安全令牌提供程序](how-to-create-a-custom-security-token-provider.md)
- [如何：创建自定义安全令牌身份验证器](how-to-create-a-custom-security-token-authenticator.md)
- [如何：创建自定义令牌](how-to-create-a-custom-token.md)
