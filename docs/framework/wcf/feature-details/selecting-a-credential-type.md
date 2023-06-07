---
title: 选择凭据类型
description: 了解凭据、凭据在 WCF 中的使用方式，以及如何为应用程序选择正确的凭据以建立声明的标识或功能。
ms.date: 03/30/2017
ms.assetid: bf707063-3f30-4304-ab53-0e63413728a8
ms.openlocfilehash: c5a47bf95ab8e22cda1beb7eeca7cb77bfc2a169
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96253968"
---
# <a name="selecting-a-credential-type"></a>选择凭据类型

*凭据* 是 WINDOWS COMMUNICATION FOUNDATION (WCF) 用于建立声明的标识或功能的数据。 例如，护照就是政府颁发的用以证明国家或地区的公民身份的凭据。 在 WCF 中，凭据可以采用多种形式，例如用户名令牌和 x.509 证书。 本主题讨论凭据、凭据在 WCF 中的使用方式以及如何为应用程序选择正确的凭据。  
  
 在许多国家和地区，驾驶执照就是凭据的一个示例。 该执照中包含表示个人身份和能力的数据。 它以持有人照片的形式包含所有权证明。 该执照由受信任的颁发机构颁发，通常是获得许可的政府部门。 该执照采用密封形式且包含一张全息图，表明它未经改动或伪造。  
  
 提交凭据涉及出示数据以及数据的所有权证明。 WCF 在传输和消息安全级别支持多种凭据类型。 例如，请考虑 WCF 中支持的两种凭据类型：用户名和 (x.509) 证书凭据。  
  
 对于用户名凭据，用户名表示已声明的标识，密码提供所有权证明。 这种情况下，受信任的颁发机构则是验证用户名和密码的系统。  
  
 使用 x.509 证书凭据，证书中的使用者名称、使用者备用名称或特定字段可用作标识声明，而其他字段（例如 `Valid From` 和 `Valid To` 字段）指定证书的有效性。  
  
## <a name="transport-credential-types"></a>传输凭据类型  

 下表列出了在传输安全模式下可以由绑定使用的客户端凭据的可能类型。 创建服务时，将 `ClientCredentialType` 属性设置为这些值之一以指定凭据类型，客户端必须提供该凭据才能与您的服务进行通信。 可以在代码或配置文件中设置这些类型。  
  
|设置|描述|  
|-------------|-----------------|  
|无|指定客户端不需要提供任何凭据。 这相当于匿名客户端。|  
|基本|为客户端指定基本身份验证。 有关其他信息，请参阅 RFC2617-[HTTP authentication：基本和摘要式身份验证](ftp://ftp.rfc-editor.org/in-notes/rfc2617.txt)。|  
|摘要|为客户端指定摘要式身份验证。 有关其他信息，请参阅 RFC2617-[HTTP authentication：基本和摘要式身份验证](ftp://ftp.rfc-editor.org/in-notes/rfc2617.txt)。|  
|Ntlm|指定 NT LAN Manager (NTLM) 身份验证。 在由于某种原因无法使用 Kerberos 身份验证时使用。 你还可以通过将属性设置为来禁用其用作回退 <xref:System.ServiceModel.Security.WindowsClientCredential.AllowNtlm%2A> `false` ，这会使 WCF 在使用 NTLM 时尽力引发异常。 请注意，将此属性设置为 `false` 可能不阻止通过网络发送 NTLM 凭据。|  
|Windows|指定 Windows 身份验证。 若要在 Windows 域上仅指定 Kerberos 协议，则将 <xref:System.ServiceModel.Security.WindowsClientCredential.AllowNtlm%2A> 属性设置为 `false`（默认值为 `true`）。|  
|证书|使用 X.509 证书执行客户端身份验证。|  
|密码|用户必须提供用户名和密码。 使用 Windows 身份验证或其他自定义解决方案验证用户名/密码对。|  
  
### <a name="message-client-credential-types"></a>消息客户端凭据类型  

 下表列出了在创建使用消息安全的应用程序时可以使用的可能的凭据类型。 可以在代码或配置文件中使用这些值。  
  
|设置|描述|  
|-------------|-----------------|  
|无|指定客户端不需要提供凭据。 这相当于匿名客户端。|  
|Windows|允许在使用 Windows 凭据建立的安全上下文中交换 SOAP 消息。|  
|用户名|允许服务可以要求使用用户名凭据对客户端进行身份验证。 请注意，WCF 不允许对用户名进行任何加密操作，例如生成签名或加密数据。 WCF 可确保在使用用户名凭据时确保传输的安全性。|  
|证书|允许服务可以要求使用 X.509 证书对客户端进行身份验证。|  
|已颁发的令牌|根据安全策略配置的自定义令牌类型。 默认令牌类型为安全断言标记语言 (SAML)。 令牌由安全令牌服务颁发。 有关详细信息，请参阅 [联合和颁发的令牌](federation-and-issued-tokens.md)。|  
  
### <a name="negotiation-model-of-service-credentials"></a>服务凭据的协商模型  

 *协商* 是通过交换凭据在客户端和服务之间建立信任关系的过程。 该过程在客户端和服务之间以迭代方式进行，以便仅公开协商过程下一步所需的信息。 实际上，最终结果是将服务凭据传递给要在后续操作中使用的客户端。  
  
 但有一个例外，默认情况下，在使用消息级安全性时，WCF 中系统提供的绑定会自动协商服务凭据。  (异常是 <xref:System.ServiceModel.BasicHttpBinding> ，默认情况下不启用安全性。 ) 若要禁用此行为，请参见 <xref:System.ServiceModel.MessageSecurityOverHttp.NegotiateServiceCredential%2A> 和 <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.NegotiateServiceCredential%2A> 属性。  
  
> [!NOTE]
> 当 SSL 安全与 .NET Framework 3.5 及更高版本一起使用时，WCF 客户端将使用其证书存储区中的中间证书和 SSL 协商期间收到的中间证书，对服务的证书执行证书链验证。 .NET Framework 3.0 仅使用本地证书存储区中安装的中间证书。  
  
#### <a name="out-of-band-negotiation"></a>带外协商  

 如果禁用自动协商，则在将任何消息发送到服务之前必须向客户端提供服务凭据。 这也称为 *带* 外设置。 例如，如果指定的凭据类型为证书，且禁用了自动协商，则客户端必须联系服务所有者以在运行客户端应用程序的计算机上接收和安装证书。 例如，当要严格控制哪些客户端可以访问企业对企业方案中的服务时，可以执行上述操作。 此带外协商可以通过电子邮件完成，x.509 证书存储在 Windows 证书存储中，使用 Microsoft 管理控制台 (MMC) 证书 "管理单元等工具。  
  
> [!NOTE]
> <xref:System.ServiceModel.ClientBase%601.ClientCredentials%2A> 属性用于提供使用证书的服务，该证书通过带外协商实现。 这是使用 <xref:System.ServiceModel.BasicHttpBinding> 类时所必需的，因为绑定不允许自动协商。 该属性还可用于不相关的双工方案中。 在此方案中，无需客户端先向服务器发送请求，服务器就将消息发送到客户端。 由于服务器没有来自客户端的请求，因此它必须使用客户端的证书加密发送到客户端的消息。  
  
## <a name="setting-credential-values"></a>设置凭据值  

 选择安全模式后，就必须指定实际凭据。 例如，如果将凭据类型设置为“证书”，那么必须将特定的凭据（例如特定的 X.509 凭据）与服务或客户端相关联。  
  
 根据是对服务进行编程还是对客户端进行编程，设置凭据的方法会略有不同。  
  
### <a name="setting-service-credentials"></a>设置服务凭据  

 如果使用的是传输模式，且使用 HTTP 作为传输，则必须使用 Internet Information Services (IIS)，或使用证书配置端口。 有关详细信息，请参阅 [传输安全性概述](transport-security-overview.md) 和 [HTTP 传输安全](http-transport-security.md)。  
  
 若要在代码中使用凭据配置服务，则创建 <xref:System.ServiceModel.ServiceHost> 类的一个实例，并使用 <xref:System.ServiceModel.Description.ServiceCredentials> 类指定适当的凭据，该类可通过 <xref:System.ServiceModel.ServiceHostBase.Credentials%2A> 属性访问。  
  
#### <a name="setting-a-certificate"></a>设置证书  

 若要使用 X.509 证书（用于对客户端的服务进行身份验证）配置服务，则使用 <xref:System.ServiceModel.Security.X509CertificateInitiatorServiceCredential.SetCertificate%2A> 类的 <xref:System.ServiceModel.Security.X509CertificateRecipientServiceCredential> 方法。  
  
 若要使用客户端证书配置服务，则使用 <xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential.SetCertificate%2A> 类的 <xref:System.ServiceModel.Security.X509CertificateInitiatorServiceCredential> 方法。  
  
#### <a name="setting-windows-credentials"></a>设置 Windows 凭据  

 如果客户端指定了有效的用户名和密码，则使用该凭据对客户端进行身份验证。 否则，使用当前已登录用户的凭据。  
  
### <a name="setting-client-credentials"></a>设置客户端凭据  

 在 WCF 中，客户端应用程序使用 WCF 客户端连接到服务。 每个客户端都派生自 <xref:System.ServiceModel.ClientBase%601> 类，且客户端上的 <xref:System.ServiceModel.ClientBase%601.ClientCredentials%2A> 属性允许客户端凭据的各种值的规范。  
  
#### <a name="setting-a-certificate"></a>设置证书  

 若要使用 X.509 证书（用于对服务的客户端进行身份验证）配置服务，则使用 <xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential.SetCertificate%2A> 类的 <xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential> 方法。  
  
## <a name="how-client-credentials-are-used-to-authenticate-a-client-to-the-service"></a>如何使用客户端凭据对服务的客户端进行身份验证  

 使用 <xref:System.ServiceModel.ClientBase%601.ClientCredentials%2A> 属性或 <xref:System.ServiceModel.ChannelFactory.Credentials%2A> 属性提供与服务进行通信所需的客户端凭据信息。 安全通道使用此信息对服务的客户端进行身份验证。 身份验证可通过以下两种模式之一来实现：  
  
- 在发送第一条消息之前使用一次客户端凭据，使用 WCF 客户端实例建立安全上下文。 然后，所有的应用程序消息都通过安全上下文受到保护。  
  
- 客户端凭据用于对发送到服务的每个应用程序消息进行身份验证。 在这种情况下，未在客户端和服务之间建立上下文。  
  
### <a name="established-identities-cannot-be-changed"></a>已建立的标识无法更改  

 使用第一种方法时，已建立的上下文将与客户端标识永久关联。 也就是说，一旦建立了安全上下文，就无法更改与客户端相关联的标识。  
  
> [!IMPORTANT]
> 无法切换标识时，要格外注意一种情况（即启用建立安全上下文时的默认行为）。 如果创建与第二个服务进行通信的服务，则无法更改用于向第二个服务打开 WCF 客户端的标识。 如果允许多个客户端使用第一个服务，且该服务访问第二个服务时将模拟客户端，则这将成为问题所在。 如果该服务重新使用所有调用方的相同客户端，则对第二个服务的所有调用都是以第一个调用方的标识进行的，该标识用于打开第二个服务的客户端。 换言之，该服务将第一个客户端的标识用于它的所有客户端以与第二个服务进行通信。 这可以导致特权提升。 如果这不是服务所需的行为，就必须跟踪每个调用方并为每个单独的调用方创建第二个服务的新客户端，并确保该服务仅使用正确调用方的正确客户端与第二个服务进行通信。  
  
 有关凭据和安全会话的详细信息，请参阅安全 [会话的安全注意事项](security-considerations-for-secure-sessions.md)。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.ClientBase%601?displayProperty=nameWithType>
- <xref:System.ServiceModel.ClientBase%601.ClientCredentials%2A?displayProperty=nameWithType>
- <xref:System.ServiceModel.Description.ClientCredentials.ClientCertificate%2A?displayProperty=nameWithType>
- <xref:System.ServiceModel.BasicHttpMessageSecurity.ClientCredentialType%2A?displayProperty=nameWithType>
- <xref:System.ServiceModel.HttpTransportSecurity.ClientCredentialType%2A?displayProperty=nameWithType>
- <xref:System.ServiceModel.MessageSecurityOverHttp.ClientCredentialType%2A?displayProperty=nameWithType>
- <xref:System.ServiceModel.MessageSecurityOverMsmq.ClientCredentialType%2A?displayProperty=nameWithType>
- <xref:System.ServiceModel.MessageSecurityOverTcp.ClientCredentialType%2A?displayProperty=nameWithType>
- <xref:System.ServiceModel.TcpTransportSecurity.ClientCredentialType%2A?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential.SetCertificate%2A?displayProperty=nameWithType>
- <xref:System.ServiceModel.Security.X509CertificateInitiatorServiceCredential.SetCertificate%2A?displayProperty=nameWithType>
- [安全性概念](security-concepts.md)
- [保护服务和客户端的安全](securing-services-and-clients.md)
- [WCF 安全编程](programming-wcf-security.md)
- [HTTP 传输安全](http-transport-security.md)
