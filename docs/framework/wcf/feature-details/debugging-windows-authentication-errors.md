---
title: 调试 Windows 身份验证错误
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF, authentication
- WCF, Windows authentication
ms.assetid: 181be4bd-79b1-4a66-aee2-931887a6d7cc
ms.openlocfilehash: c8aa87bdbf9488bce8e1a62f6d1a3898f923d349
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96291656"
---
# <a name="debug-windows-authentication-errors"></a>调试 Windows 身份验证错误

使用 Windows 验证身份作为安全机制时，安全支持提供程序接口 (SSPI) 将处理安全进程。 当 SSPI 层发生安全错误时，它们将被 Windows Communication Foundation (WCF) 出现。 本主题提供一组问题以帮助诊断这些错误。  
  
 有关 Kerberos 协议的概述，请参阅 [Kerberos 说明](/previous-versions/windows/it-pro/windows-2000-server/bb742516(v=technet.10));有关 SSPI 的概述，请参阅 [sspi](/windows/win32/secauthn/sspi)。  
  
 对于 Windows 身份验证，WCF 通常使用 (SSP) 的 *Negotiate* 安全支持提供程序，该提供程序在客户端和服务之间执行 Kerberos 相互身份验证。 如果 Kerberos 协议不可用，则默认情况下，WCF 将回退到 NT LAN Manager (NTLM) 。 但是，你可以将 WCF 配置为仅使用 Kerberos 协议 (并在 Kerberos 不可用时引发异常) 。 你还可以将 WCF 配置为使用受限形式的 Kerberos 协议。  
  
## <a name="debugging-methodology"></a>调试方法  

 基本方法如下所述：  
  
1. 确定您是否正在使用 Windows 身份验证。 如果您正在使用任何其他方案，则本主题不适用。  
  
2. 如果你确定使用的是 Windows 身份验证，请确定你的 WCF 配置是使用 Kerberos direct 还是 Negotiate。  
  
3. 确定了配置是使用 Kerberos 协议还是 NTLM 后，您可以在正确的上下文中理解错误消息。  
  
### <a name="availability-of-the-kerberos-protocol-and-ntlm"></a>Kerberos 协议和 NTLM 的可用性  

 Kerberos SSP 要求域控制器作为 Kerberos 密钥发行中心 (KDC)。 只有在客户端和服务都使用域标识时，Kerberos 协议才可用。 在其他帐户组合中，将使用 NTLM，如下表摘要所示。  
  
 表的标题显示服务器可能使用的帐户类型。 左列显示客户端可能使用的帐户类型。  
  
||本地用户|Local System|域用户|域计算机|  
|-|----------------|------------------|-----------------|--------------------|  
|本地用户|NTLM|NTLM|NTLM|NTLM|  
|Local System|匿名 NTLM|匿名 NTLM|匿名 NTLM|匿名 NTLM|  
|域用户|NTLM|NTLM|Kerberos|Kerberos|  
|域计算机|NTLM|NTLM|Kerberos|Kerberos|  
  
 具体地说，四种帐户类型包括：  
  
- 本地用户：仅计算机用户配置文件。 例如 `MachineName\Administrator` 或 `MachineName\ProfileName`。  
  
- 本地系统：未加入域的计算机上的内置帐户 SYSTEM。  
  
- 域用户：Windows 域中的用户帐户。 例如：`DomainName\ProfileName`。  
  
- 域计算机：具有计算机标识并在加入 Windows 域的计算机上运行的进程。 例如：`MachineName\Network Service`。  
  
> [!NOTE]
> 在调用 <xref:System.ServiceModel.ICommunicationObject.Open%2A> 类的 <xref:System.ServiceModel.ServiceHost> 方法时，将捕获服务凭据。 每当客户端发送消息时，即会读取客户端凭据。  
  
## <a name="common-windows-authentication-problems"></a>常见的 Windows 身份验证问题  

 该节讨论一些常见的 Windows 身份验证问题和可能的纠正方法。  
  
### <a name="kerberos-protocol"></a>Kerberos 协议  
  
#### <a name="spnupn-problems-with-the-kerberos-protocol"></a>Kerberos 协议的 SPN/UPN 问题  

 在使用 Windows 身份验证并且 SSPI 使用或协商 Kerberos 协议时，客户端终结点使用的 URL 必须包括服务主机在服务 URL 中的完全限定域名。 此处假定在其下运行服务的帐户可以访问将该计算机添加到 Active Directory 域中时所创建的计算机（默认）服务主体名称 (SPN) 密钥，这通常可以通过在“网络服务”帐户下运行该服务来实现。 如果服务不能访问计算机 SPN 密钥，则必须在客户端的终结点标识中提供在其下运行该服务的帐户的正确 SPN 或用户主体名称 (UPN)。 有关 WCF 如何与 SPN 和 UPN 一起使用的详细信息，请参阅 [服务标识和身份验证](service-identity-and-authentication.md)。  
  
 在负载平衡方案（如网络场或网络园）中，常见的做法是为每个应用程序定义唯一帐户，为该帐户分配 SPN，并确保应用程序的所有服务都使用该帐户来运行。  
  
 为了获取服务帐户的 SPN，需要具有 Active Directory 域管理员的身份。 有关详细信息，请参阅 [适用于 Windows 的 Kerberos 技术补充](/previous-versions/msp-n-p/ff649429(v=pandp.10))。  
  
#### <a name="kerberos-protocol-direct-requires-the-service-to-run-under-a-domain-machine-account"></a>Kerberos 协议定向要求在域计算机帐户下运行服务  

 当 `ClientCredentialType` 属性设置为 `Windows` 且 <xref:System.ServiceModel.MessageSecurityOverHttp.NegotiateServiceCredential%2A> 属性设置为 `false` 时，会发生这种情况，如下面的代码所示。  
  
 [!code-csharp[C_DebuggingWindowsAuth#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_debuggingwindowsauth/cs/source.cs#1)]
 [!code-vb[C_DebuggingWindowsAuth#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_debuggingwindowsauth/vb/source.vb#1)]  
  
 若要解决这个问题，请在加入域的计算机上使用域计算机帐户（比如“网络服务”）来运行该服务。  
  
### <a name="delegation-requires-credential-negotiation"></a>委托要求凭据协商  

 若要与委托一起使用 Kerberos 身份验证协议，必须用凭据协商实现 Kerberos 协议（有时称作“多路”或“多步”Kerberos）。 如果不用凭据协商实现 Kerberos 协议（有时称作“单稳”或“单路”Kerberos），则会引发异常。  
  
 若要用凭据协商实现 Kerberos，请执行下列步骤：  
  
1. 通过将 <xref:System.ServiceModel.Security.WindowsClientCredential.AllowedImpersonationLevel%2A> 设置为 <xref:System.Security.Principal.TokenImpersonationLevel.Delegation> 来实现委托。  
  
2. 要求 SSPI 协商：  
  
    1. 如果要使用标准绑定，请将 `NegotiateServiceCredential` 属性设置为 `true`。  
  
    2. 如果要使用自定义绑定，请将 `AuthenticationMode` 元素的 `Security` 属性设置为 `SspiNegotiated`。  
  
3. 通过禁用 NTLM 来要求 SSPI 协商使用 Kerberos：  
  
    1. 为此，可在代码中使用下面的语句：`ChannelFactory.Credentials.Windows.AllowNtlm = false`  
  
    2. 或者在配置文件中将 `allowNtlm` 属性设置为 `false`。 此属性包含在中 [\<windows>](../../configure-apps/file-schema/wcf/windows-of-clientcredentials-element.md) 。  
  
### <a name="ntlm-protocol"></a>NTLM 协议  
  
#### <a name="negotiate-ssp-falls-back-to-ntlm-but-ntlm-is-disabled"></a>协商 SSP 回退到 NTLM，但 NTLM 已被禁用  

 <xref:System.ServiceModel.Security.WindowsClientCredential.AllowNtlm%2A>属性设置为 `false` ，这将导致 WINDOWS COMMUNICATION FOUNDATION (WCF) ，以便在使用 NTLM 时尽力引发异常。 将此属性设置为 `false` 可能不会阻止通过网络发送 NTLM 凭据。  
  
 下面演示了如何禁用回退到 NTLM。  
  
 [!code-csharp[C_DebuggingWindowsAuth#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_debuggingwindowsauth/cs/source.cs#4)]
 [!code-vb[C_DebuggingWindowsAuth#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_debuggingwindowsauth/vb/source.vb#4)]  
  
#### <a name="ntlm-logon-fails"></a>NTLM 登录失败  

 客户端凭据在服务上无效。 请检查是否正确设置了用户名和密码，用户名和密码是否对应于运行服务的计算机已知的帐户。 NTLM 使用指定的凭据登录到服务计算机。 虽然这些凭据在运行客户端的计算机上可能有效，但如果这些凭据在服务计算机上无效，登录也会失败。  
  
#### <a name="anonymous-ntlm-logon-occurs-but-anonymous-logons-are-disabled-by-default"></a>发生匿名 NTLM 登录，但默认情况下禁用匿名登录  

 创建客户端时，<xref:System.ServiceModel.Security.WindowsClientCredential.AllowedImpersonationLevel%2A> 属性设置为 <xref:System.Security.Principal.TokenImpersonationLevel.Anonymous>（如下面的示例所示），但默认情况下，服务器不允许匿名登录。 发生这种情况是因为 <xref:System.ServiceModel.Security.WindowsServiceCredential.AllowAnonymousLogons%2A> 类的 <xref:System.ServiceModel.Security.WindowsServiceCredential> 属性的默认值为 `false`。  
  
 下面的客户端代码尝试启用匿名登录（请注意，默认属性为 `Identification`）。  
  
 [!code-csharp[C_DebuggingWindowsAuth#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_debuggingwindowsauth/cs/source.cs#5)]
 [!code-vb[C_DebuggingWindowsAuth#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_debuggingwindowsauth/vb/source.vb#5)]  
  
 下面的服务代码将默认值更改为允许服务器匿名登录。  
  
 [!code-csharp[C_DebuggingWindowsAuth#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_debuggingwindowsauth/cs/source.cs#6)]
 [!code-vb[C_DebuggingWindowsAuth#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_debuggingwindowsauth/vb/source.vb#6)]  
  
 有关模拟的详细信息，请参阅 [委派和模拟](delegation-and-impersonation-with-wcf.md)。  
  
 或者，客户端正在使用内置帐户 SYSTEM 作为 Windows 服务运行。  
  
### <a name="other-problems"></a>其他问题  
  
#### <a name="client-credentials-are-not-set-correctly"></a>客户端凭据设置不正确  

 Windows 身份验证使用由 <xref:System.ServiceModel.Security.WindowsClientCredential> 类的 <xref:System.ServiceModel.ClientBase%601.ClientCredentials%2A> 属性返回的 <xref:System.ServiceModel.ClientBase%601> 实例，而不是 <xref:System.ServiceModel.Security.UserNamePasswordClientCredential>。 下面演示一个不正确的示例。  
  
 [!code-csharp[C_DebuggingWindowsAuth#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_debuggingwindowsauth/cs/source.cs#2)]
 [!code-vb[C_DebuggingWindowsAuth#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_debuggingwindowsauth/vb/source.vb#2)]  
  
 下面演示的是正确的示例。  
  
 [!code-csharp[C_DebuggingWindowsAuth#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_debuggingwindowsauth/cs/source.cs#3)]
 [!code-vb[C_DebuggingWindowsAuth#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_debuggingwindowsauth/vb/source.vb#3)]  
  
#### <a name="sspi-is-not-available"></a>SSPI 不可用  

 以下操作系统不支持使用 Windows 身份验证作为服务器： Windows XP Home Edition、Windows XP Media Center Edition 和 Windows Vista Home 版本。  
  
#### <a name="developing-and-deploying-with-different-identities"></a>使用不同的标识开发和部署  

 如果在一台计算机上开发应用程序，并在另一台计算机上部署它，然后在每台计算机上使用不同的帐户类型进行身份验证，则可能遇到不同的行为。 例如，假定使用 `SSPI Negotiated` 身份验证模式，在 Windows XP Pro 计算机上开发应用程序。 如果使用本地用户帐户进行身份验证，则会使用 NTLM 协议。 在开发应用程序后，将服务部署到使用域帐户运行它的 Windows Server 2003 计算机。 此时，客户端将无法对服务进行身份验证，因为它将使用 Kerberos 和域控制器。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Security.WindowsClientCredential>
- <xref:System.ServiceModel.Security.WindowsServiceCredential>
- <xref:System.ServiceModel.Security.WindowsClientCredential>
- <xref:System.ServiceModel.ClientBase%601>
- [委托和模拟](delegation-and-impersonation-with-wcf.md)
- [不支持的方案](unsupported-scenarios.md)
