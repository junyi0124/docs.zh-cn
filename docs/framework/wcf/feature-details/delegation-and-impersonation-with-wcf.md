---
title: WCF 的委派和模拟
description: 了解 WCF 使用哪些模拟和委托方法来限制客户端对服务域资源的访问。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- impersonation [WCF]
- delegation [WCF]
ms.assetid: 110e60f7-5b03-4b69-b667-31721b8e3152
ms.openlocfilehash: fd6c75d66251808a22445c959c18ee6026a686f1
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96291643"
---
# <a name="delegation-and-impersonation-with-wcf"></a>WCF 的委派和模拟

模拟 是一种常用技术，服务可使用该技术限制客户端对服务域资源的访问。 服务域资源可以是计算机资源，如本地文件（模拟），也可以是其他计算机上的资源，如文件共享（委托）。 有关示例应用程序，请参见 [Impersonating the Client](../samples/impersonating-the-client.md)。 有关如何使用模拟的示例，请参阅 [How to: Impersonate a Client on a Service](../how-to-impersonate-a-client-on-a-service.md)。  
  
> [!IMPORTANT]
> 请注意，在对某一服务模拟客户端时，该服务会使用客户端的凭据运行，因此可能具有高于服务器进程的权限。  
  
## <a name="overview"></a>概述  

 通常，客户端会调用某个服务，使该服务代表客户端执行某个操作。 模拟使服务可在执行操作时充当客户端。 委托使前端服务可采用某种方式将客户端的请求转发到后端服务，以使后端服务也能模拟客户端。 模拟是检查客户端是否有权执行特定操作的最常用的方法，而委托是一种使模拟功能以及客户端的标识流向后端服务的方法。 委托是一种 Windows 域功能，可在执行基于 Kerberos 的身份验证时使用。 委托不同于标识流，因为委托可传送无需拥有客户端密码即可模拟客户端的能力。与标识流相比，委托是一种权限更高的操作。  
  
 模拟和委托都要求客户端具有 Windows 标识。 如果客户端没有 Windows 标识，则唯一的选择就是使客户端的标识流向第二个服务。  
  
## <a name="impersonation-basics"></a>模拟基础知识  

 Windows Communication Foundation (WCF) 支持各种客户端凭据的模拟。 本主题说明在实现某一服务方法的过程中支持模拟调用方的服务模型。 本文还讨论了涉及模拟和 SOAP 安全的常见部署方案以及这些方案中的 WCF 选项。  
  
 本主题重点介绍使用 SOAP 安全时 WCF 中的模拟和委托。 使用传输安全时，还可以对 WCF 使用模拟和委托，如在 [传输安全中使用模拟](using-impersonation-with-transport-security.md)中所述。  
  
## <a name="two-methods"></a>两种方法  

 WCF SOAP 安全性有两种不同的方法来执行模拟。 所用的方法取决于绑定。 一种方法是从 Windows 令牌模拟，此令牌从安全支持提供程序接口 (SSPI) 或 Kerberos 身份验证获取，然后在服务上缓存。 另一种方法是从 Windows 令牌模拟，此令牌从 Kerberos 扩展获取，统称为“Service-for-User”  (S4U)。  
  
### <a name="cached-token-impersonation"></a>缓存的令牌模拟  

 您可以对以下各项执行缓存的令牌模拟：  
  
- 使用 Windows 客户端凭据的<xref:System.ServiceModel.WSHttpBinding>, <xref:System.ServiceModel.WSDualHttpBinding>和 <xref:System.ServiceModel.NetTcpBinding> 。  
  
- <xref:System.ServiceModel.BasicHttpBinding> 设置为 <xref:System.ServiceModel.BasicHttpSecurityMode> 凭据的 <xref:System.ServiceModel.BasicHttpSecurityMode.TransportWithMessageCredential> ，或任何其他标准绑定，其中客户端提供用户名凭据（服务可以将该凭据映射到有效的 Windows 帐户）。  
  
- 使用 Windows 凭据并且 <xref:System.ServiceModel.Channels.CustomBinding> 设置为 `requireCancellation` 的任何 `true`。  (属性在以下类中可用： <xref:System.ServiceModel.Security.Tokens.SecureConversationSecurityTokenParameters> 、 <xref:System.ServiceModel.Security.Tokens.SslSecurityTokenParameters> 和 <xref:System.ServiceModel.Security.Tokens.SspiSecurityTokenParameters> 。 ) 如果在绑定上使用安全对话，还必须将 `requireCancellation` 属性设置为 `true` 。  
  
- 任何 <xref:System.ServiceModel.Channels.CustomBinding> ，其中客户端提供用户名凭据。 如果在绑定中使用安全对话，则安全对话还必须将 `requireCancellation` 属性设置为 `true`。  
  
### <a name="s4u-based-impersonation"></a>基于 S4U 的模拟  

 您可以对以下各项执行基于 S4U 的模拟：  
  
- 使用证书客户端凭据（服务可以将该凭据映射到有效的 Windows 帐户）的<xref:System.ServiceModel.WSHttpBinding>, <xref:System.ServiceModel.WSDualHttpBinding>和 <xref:System.ServiceModel.NetTcpBinding> 。  
  
- 使用 Windows 凭据并且 <xref:System.ServiceModel.Channels.CustomBinding> 属性设置为 `requireCancellation` 的任何 `false`。  
  
- 使用用户名或 Windows 客户端凭据和安全对话，并且 <xref:System.ServiceModel.Channels.CustomBinding> 属性设置为 `requireCancellation` 的任何 `false`。  
  
 服务可以模拟客户端的范围取决于服务帐户在尝试模拟时拥有的权限、使用的模拟类型和客户端可能允许的模拟范围。  
  
> [!NOTE]
> 当客户端和服务在同一计算机上运行，并且客户端在系统帐户（例如 `Local System` 或 `Network Service`）下运行时，如果安全会话是使用有状态安全上下文令牌建立的，则不能模拟客户端。 Windows 窗体或控制台应用程序通常在当前登录的帐户下运行，因此默认情况下可以模拟该帐户。 但是，当客户端为 ASP.NET 页，并且该页承载在 IIS 6.0 或 IIS 7.0 中时，默认情况下，客户端将在帐户下运行 `Network Service` 。 默认情况下，系统提供的所有支持安全会话的绑定都使用无状态安全上下文令牌 (SCT)。 但是，如果客户端是 ASP.NET 页，并且使用了具有有状态 Sct 的安全会话，则不能模拟客户端。 有关在安全会话中使用有状态 Sct 的详细信息，请参阅 [如何：为安全会话创建安全上下文令牌](how-to-create-a-security-context-token-for-a-secure-session.md)。  
  
## <a name="impersonation-in-a-service-method-declarative-model"></a>服务方法中的模拟：声明性模型  

 大多数模拟方案都涉及在调用方上下文中执行服务方法。 WCF 提供了模拟功能，使用户可以在属性中指定模拟要求，从而轻松实现此目的 <xref:System.ServiceModel.OperationBehaviorAttribute> 。 例如，在下面的代码中，WCF 基础结构在执行方法之前模拟调用方 `Hello` 。 只有当本机资源的访问控制列表 (ACL) 允许调用方访问权限时，在 `Hello` 方法内访问该本机资源的任何尝试才能成功。 若要启用模拟，请将 <xref:System.ServiceModel.OperationBehaviorAttribute.Impersonation%2A> 属性设置为 <xref:System.ServiceModel.ImpersonationOption> 枚举值之一（ <xref:System.ServiceModel.ImpersonationOption.Required?displayProperty=nameWithType> 或 <xref:System.ServiceModel.ImpersonationOption.Allowed?displayProperty=nameWithType>），如下面的示例所示。  
  
> [!NOTE]
> 当服务具有比远程客户端更高的凭据时，在 <xref:System.ServiceModel.OperationBehaviorAttribute.Impersonation%2A> 属性设置为 <xref:System.ServiceModel.ImpersonationOption.Allowed>的情况下将使用服务的凭据。 也就是说，如果低权限的用户提供其凭据，则较高权限的服务会使用服务的凭据执行该方法，并可以使用低权限的用户不能使用的资源。  
  
 [!code-csharp[c_ImpersonationAndDelegation#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_impersonationanddelegation/cs/source.cs#1)]
 [!code-vb[c_ImpersonationAndDelegation#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_impersonationanddelegation/vb/source.vb#1)]  
  
 仅当调用方使用可以映射到 Windows 用户帐户的凭据进行身份验证时，WCF 基础结构才能模拟调用方。 如果将服务配置为使用无法映射到 Windows 用户帐户的凭据进行身份验证，则不会执行服务方法。  
  
> [!NOTE]
> 在 Windows XP 上，如果创建了有状态 SCT，则模拟会失败，导致 <xref:System.InvalidOperationException> 。 有关详细信息，请参阅 [不支持的方案](unsupported-scenarios.md)。  
  
## <a name="impersonation-in-a-service-method-imperative-model"></a>服务方法中的模拟：命令性模型  

 有时，调用方不需要模拟整个服务方法，只需模拟它的一部分就行。 在这种情况下，请获取服务方法中调用方的 Windows 标识并以强制方式执行模拟。 为此，请使用 <xref:System.ServiceModel.ServiceSecurityContext.WindowsIdentity%2A> 的 <xref:System.ServiceModel.ServiceSecurityContext> 属性返回 <xref:System.Security.Principal.WindowsIdentity> 类的实例并在使用该实例前调用 <xref:System.Security.Principal.WindowsIdentity.Impersonate%2A> 方法。  
  
> [!NOTE]
> 请确保使用 Visual Basic `Using` 语句或 c # `using` 语句以自动还原模拟操作。 如果不使用语句，或者使用 Visual Basic 或 c # 以外的编程语言，请确保还原模拟级别。 否则可导致拒绝服务和特权升级攻击。  
  
 [!code-csharp[c_ImpersonationAndDelegation#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_impersonationanddelegation/cs/source.cs#2)]
 [!code-vb[c_ImpersonationAndDelegation#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_impersonationanddelegation/vb/source.vb#2)]  
  
## <a name="impersonation-for-all-service-methods"></a>所有服务方法的模拟  

 在某些情况下，您必须在调用方上下文中执行服务的所有方法。 如果不想逐个方法地显式启用此功能，可以使用 <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior>。 如下面的代码所示，将 <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior.ImpersonateCallerForAllOperations%2A> 属性设置为 `true`。 即会从 <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior> 类的行为集合中检索 <xref:System.ServiceModel.ServiceHost> 。 另外请注意，应用于每个方法的 `Impersonation` 的 <xref:System.ServiceModel.OperationBehaviorAttribute> 属性也必须设置为 <xref:System.ServiceModel.ImpersonationOption.Allowed> 或 <xref:System.ServiceModel.ImpersonationOption.Required>。  
  
 [!code-csharp[c_ImpersonationAndDelegation#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_impersonationanddelegation/cs/source.cs#3)]
 [!code-vb[c_ImpersonationAndDelegation#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_impersonationanddelegation/vb/source.vb#3)]  
  
 下表描述了和的所有可能组合的 WCF `ImpersonationOption` 行为 `ImpersonateCallerForAllServiceOperations` 。  
  
|`ImpersonationOption`|`ImpersonateCallerForAllServiceOperations`|行为|  
|---------------------------|------------------------------------------------|--------------|  
|必选|不适用|WCF 模拟调用方|  
|然后用户才能访问|false|WCF 不模拟调用方|  
|然后用户才能访问|true|WCF 模拟调用方|  
|NotAllowed|false|WCF 不模拟调用方|  
|NotAllowed|true|不允许。 （引发 <xref:System.InvalidOperationException> 。）|  
  
## <a name="impersonation-level-obtained-from-windows-credentials-and-cached-token-impersonation"></a>从 Windows 凭据和缓存的令牌模拟获取的模拟级别  

 在某些情况下，当使用 Windows 凭据时，客户端能够部分控制服务执行的模拟级别。 情况之一是客户端指定 Anonymous 模拟级别。 另一种情况是执行缓存的令牌模拟。 这是通过设置 <xref:System.ServiceModel.Security.WindowsClientCredential.AllowedImpersonationLevel%2A> 类的 <xref:System.ServiceModel.Security.WindowsClientCredential> 属性来完成的，此类可作为通用 <xref:System.ServiceModel.ChannelFactory%601> 类的属性进行访问。  
  
> [!NOTE]
> 指定 Anonymous 模拟级别可导致客户端匿名登录到服务。 因此，无论是否执行模拟，服务都必须允许匿名登录。  
  
 客户端可以将模拟级别指定为 <xref:System.Security.Principal.TokenImpersonationLevel.Anonymous>、 <xref:System.Security.Principal.TokenImpersonationLevel.Identification>、 <xref:System.Security.Principal.TokenImpersonationLevel.Impersonation>或 <xref:System.Security.Principal.TokenImpersonationLevel.Delegation>。 只在指定的级别产生令牌，如下面的代码所示。  
  
 [!code-csharp[c_ImpersonationAndDelegation#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_impersonationanddelegation/cs/source.cs#4)]
 [!code-vb[c_ImpersonationAndDelegation#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_impersonationanddelegation/vb/source.vb#4)]  
  
 下表指定从缓存的令牌模拟时服务获取的模拟级别。  
  
|`AllowedImpersonationLevel` 值|服务具有 `SeImpersonatePrivilege`|服务和客户端能够委托|缓存的令牌 `ImpersonationLevel`|  
|---------------------------------------|------------------------------------------|--------------------------------------------------|---------------------------------------|  
|匿名|是|不适用|模拟|  
|匿名|否|不适用|标识|  
|标识|不适用|不适用|标识|  
|模拟|是|不适用|模拟|  
|模拟|否|不适用|标识|  
|委托|是|是|委托|  
|委托|是|否|模拟|  
|委托|否|不适用|标识|  
  
## <a name="impersonation-level-obtained-from-user-name-credentials-and-cached-token-impersonation"></a>从用户名凭据和缓存的令牌模拟获取的模拟级别  

 通过向服务传递其用户名和密码，客户端可以使 WCF 以该用户身份登录，这等效于将 `AllowedImpersonationLevel` 属性设置为 <xref:System.Security.Principal.TokenImpersonationLevel.Delegation> 。  (`AllowedImpersonationLevel` 和类上提供了 <xref:System.ServiceModel.Security.WindowsClientCredential> <xref:System.ServiceModel.Security.HttpDigestClientCredential> 。 ) 下表提供了在服务接收用户名凭据时获得的模拟级别。  
  
|`AllowedImpersonationLevel`|服务具有 `SeImpersonatePrivilege`|服务和客户端能够委托|缓存的令牌 `ImpersonationLevel`|  
|---------------------------------|------------------------------------------|--------------------------------------------------|---------------------------------------|  
|不适用|是|是|委托|  
|不适用|是|否|模拟|  
|不适用|否|不适用|标识|  
  
## <a name="impersonation-level-obtained-from-s4u-based-impersonation"></a>从基于 S4U 的模拟获取的模拟级别  
  
|服务具有 `SeTcbPrivilege`|服务具有 `SeImpersonatePrivilege`|服务和客户端能够委托|缓存的令牌 `ImpersonationLevel`|  
|----------------------------------|------------------------------------------|--------------------------------------------------|---------------------------------------|  
|是|是|不适用|模拟|  
|是|否|不适用|标识|  
|否|不适用|不适用|标识|  
  
## <a name="mapping-a-client-certificate-to-a-windows-account"></a>将客户端证书映射到 Windows 帐户  

 客户端有可能使用证书向服务验证自己的身份，并让服务通过 Active Directory 将客户端映射到现有帐户。 下面的 XML 演示如何将服务配置为使用映射证书。  
  
```xml  
<behaviors>  
  <serviceBehaviors>  
    <behavior name="MapToWindowsAccount">  
      <serviceCredentials>  
        <clientCertificate>  
          <authentication mapClientCertificateToWindowsAccount="true" />  
        </clientCertificate>  
      </serviceCredentials>  
    </behavior>  
  </serviceBehaviors>  
</behaviors>  
```  
  
 下面的代码演示如何配置服务。  
  
```csharp
// Create a binding that sets a certificate as the client credential type.  
WSHttpBinding b = new WSHttpBinding();  
b.Security.Message.ClientCredentialType = MessageCredentialType.Certificate;  
  
// Create a service host that maps the certificate to a Windows account.  
Uri httpUri = new Uri("http://localhost/Calculator");  
ServiceHost sh = new ServiceHost(typeof(HelloService), httpUri);  
sh.Credentials.ClientCertificate.Authentication.MapClientCertificateToWindowsAccount = true;  
```  
  
## <a name="delegation"></a>委托  

 若要委托给后端服务，服务必须使用客户端的 Windows 标识对后端服务执行 Kerberos 多段（不带 NTLM 回退的 SSPI）身份验证或 Kerberos 直接身份验证。 若要委托给后端服务，可以创建 <xref:System.ServiceModel.ChannelFactory%601> 和通道，然后在模拟客户端时通过该通道进行通信。 采用这种形式的委托时，后端服务与前端服务之间可以保持的距离取决于前端服务实现的模拟级别。 如果模拟级别为 <xref:System.Security.Principal.TokenImpersonationLevel.Impersonation>，则前端服务和后端服务必须在同一台计算机上运行。 如果模拟级别为 <xref:System.Security.Principal.TokenImpersonationLevel.Delegation>，则前端服务和后端服务既可以在不同的计算机上运行，也可以在同一台计算机上运行。 启用委托级别的模拟需要将 Windows 域策略配置为允许委托。 有关配置 Active Directory 以提供委托支持的详细信息，请参阅 [Enabling Delegated Authentication](/previous-versions/windows/it-pro/windows-server-2003/cc780217(v=ws.10))（启用委托的身份验证）。  
  
> [!NOTE]
> 如果客户端使用与后端服务上的 Windows 帐户相对应的用户名和密码向前端服务进行身份验证，则前端服务可通过重用客户端的用户名和密码，向后端服务进行身份验证。 这是一种功能特别强大的标识流形式，因为将用户名和密码传递到后端服务会使后端服务可以执行模拟，但它不会形成委托，因为未使用 Kerberos。 Active Directory 对委托的控制不适用于用户名和密码的身份验证。  
  
### <a name="delegation-ability-as-a-function-of-impersonation-level"></a>作为模拟级别功能的委托功能  
  
|模拟级别|服务可以执行跨进程的委托|服务可以执行跨计算机的委托|  
|-------------------------|---------------------------------------------------|---------------------------------------------------|  
|<xref:System.Security.Principal.TokenImpersonationLevel.Identification>|否|否|  
|<xref:System.Security.Principal.TokenImpersonationLevel.Impersonation>|是|否|  
|<xref:System.Security.Principal.TokenImpersonationLevel.Delegation>|是|是|  
  
 下面的代码示例演示如何使用委托。  
  
 [!code-csharp[c_delegation#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_delegation/cs/source.cs#1)]
 [!code-vb[c_delegation#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_delegation/vb/source.vb#1)]  
  
### <a name="how-to-configure-an-application-to-use-constrained-delegation"></a>如何将应用程序配置为使用受约束的委托  

 在使用受约束的委托之前，必须将发送方、接收方和域控制器配置为使用受约束的委托。 下面的过程列出启用受约束的委托的步骤。 有关委托和受约束委托之间的区别的详细信息，请参阅 [Windows Server 2003 Kerberos Extensions](/previous-versions/windows/it-pro/windows-server-2003/cc738207(v=ws.10)) （Windows Server 2003 Kerberos 扩展）中讨论受约束委托的部分。  
  
1. 在域控制器上，为用于运行客户端应用程序的帐户清除 **“敏感帐户，不能被委派”** 复选框。  
  
2. 在域控制器上，为用于运行客户端应用程序的帐户选中 **“帐户可以委派其他帐户”** 复选框。  
  
3. 在域控制器上，通过单击 **“信任计算机作为委派”** 选项，对中间层计算机进行配置，以便信任该计算机进行委托。  
  
4. 在域控制器上，通过单击 **“仅信任此计算机来委派指定的服务”** 选项，将中间层计算机配置为使用受约束的委托。  
  
 有关配置约束委派的详细说明，请参阅 [Kerberos 协议转换和约束委派](/previous-versions/windows/it-pro/windows-server-2003/cc739587(v=ws.10))。
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.OperationBehaviorAttribute>
- <xref:System.ServiceModel.OperationBehaviorAttribute.Impersonation%2A>
- <xref:System.ServiceModel.ImpersonationOption>
- <xref:System.ServiceModel.ServiceSecurityContext.WindowsIdentity%2A>
- <xref:System.ServiceModel.ServiceSecurityContext>
- <xref:System.Security.Principal.WindowsIdentity>
- <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior>
- <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior.ImpersonateCallerForAllOperations%2A>
- <xref:System.ServiceModel.ServiceHost>
- <xref:System.ServiceModel.Security.WindowsClientCredential.AllowedImpersonationLevel%2A>
- <xref:System.ServiceModel.Security.WindowsClientCredential>
- <xref:System.ServiceModel.ChannelFactory%601>
- <xref:System.Security.Principal.TokenImpersonationLevel.Identification>
- [将模拟用于传输安全](using-impersonation-with-transport-security.md)
- [模拟客户端](../samples/impersonating-the-client.md)
- [如何：在服务上模拟客户端](../how-to-impersonate-a-client-on-a-service.md)
- [ServiceModel 元数据实用工具 (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md)
