---
title: 带有扩展保护的集成 Windows 身份验证
ms.date: 03/30/2017
ms.assetid: 81731998-d5e7-49e4-ad38-c8e6d01689d0
ms.openlocfilehash: 74f421131da0e5b11fd676ff23229f5ff6ec7eca
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96241617"
---
# <a name="integrated-windows-authentication-with-extended-protection"></a>带有扩展保护的集成 Windows 身份验证

进行了可影响以下类处理集成式 Windows 身份验证的方式的改进：<xref:System.Net.HttpWebRequest>、<xref:System.Net.HttpListener>、<xref:System.Net.Mail.SmtpClient>、<xref:System.Net.Security.SslStream>、<xref:System.Net.Security.NegotiateStream> 以及 <xref:System.Net> 和相关命名空间中的相关类。 添加了对扩展保护的支持以增强安全性。  
  
 这些更改会影响使用这些类来发出 Web 请求和接收响应的应用程序，这些应用程序使用集成式 Windows 身份验证。 此更改也会影响配置为使用集成式 Windows 身份验证的 Web 服务器和客户端应用程序。  
  
 这些更改还会影响使用这些类来发出其他请求类型和接收响应的应用程序，这些应用程序使用集成式 Windows 身份验证。  
  
 支持扩展保护的更改仅适用于 Windows 7 和 Windows Server 2008 R2 上的应用程序。 扩展保护功能不可用于 Windows 的早期版本。  
  
## <a name="overview"></a>概述  

 集成式 Windows 身份验证的设计能使某些凭据质询响应变得通用，这意味着可以重新使用或转发这些响应。 质询响应至少应与目标特定的信息一起构造，最好还包含一些通道特定的信息。 然后，服务可提供扩展保护，确保凭据质询响应包含服务主体名称 (SPN) 等服务特定的信息。 凭据交换中含有此信息时，服务就能更好防范恶意使用那些可能已被不当使用的凭据质询响应。  
  
 扩展保护设计是对身份验证协议的增强，旨在减轻身份验证中继攻击。 它围绕通道和服务绑定信息的概念展开。  
  
 总体目标如下所示：  
  
1. 如果客户端更新为支持扩展保护，应用程序应为所有受支持身份验证协议提供通道绑定和服务绑定信息。 仅当有要绑定到的通道 (TLS) 时，才提供通道绑定信息。 应始终提供服务绑定信息。  
  
2. 适当配置的已更新服务器可能会在通道和服务绑定信息出现在客户端身份验证令牌中时验证这些信息，如果通道绑定不匹配则拒绝身份验证尝试。 根据部署方案，服务器可能会验证通道绑定或服务绑定或同时验证两者。  
  
3. 更新的服务器可以基于策略接受或拒绝不包含通道绑定信息的下层客户端请求。  
  
 扩展保护使用的信息由以下两个部分或其中一个部分组成：  
  
1. 通道绑定令牌（即 CBT）。  
  
2. 服务主体名称（即 SPN）形式的服务绑定信息。  
  
 服务绑定信息指示客户端想对特定服务终结点验证自己的身份。 它随以下属性从客户端传输到服务器：  
  
- SPN 值必须可用于以明文形式执行客户端身份验证的服务器。  
  
- SPN 的值是公共的。  
  
- SPN 在传输过程中必须受到加密保护，以便中间人攻击不会插入、删除或修改它的值。  
  
 CBT 是外部安全通道（如 TLS）的属性，用于通过验证了客户端的内部通道将其绑定到会话。 CBT 必须具有以下属性（也由 IETF RFC 5056 定义）：  
  
- 当存在外部通道时，CBT 的值必须是标识外部通道或服务器终结点的属性，由会话的客户端和服务器端独立获取。  
  
- 客户端发送的 CBT 的值必须是攻击者不能影响的。  
  
- 对 CBT 值的保密性不作任何保证。 但这不意味着，服务绑定值和通道绑定信息总是可由其他任何服务器进行检查（执行身份验证服务器除外），因为携带 CBT 的协议可能对其进行加密。  
  
- CBT 在传输过程中必须受到加密完整性保护，以便攻击者不会插入、删除或修改它的值。  
  
 通道绑定是由客户端将 SPN 和 CBT 以防干扰方式传输给服务器而完成的。 服务器根据其策略验证通道绑定信息，并拒绝它认为自身不是预期目标的身份验证尝试。 这样，两个通道加密绑定在一起。  
  
 为了保持与现有客户端和应用程序的兼容性，可能将服务器配置为允许尚不支持扩展保护的客户端进行身份验证尝试。 这被称为“部分强化”配置（相较于“完全强化”配置）。  
  
 <xref:System.Net> 和 <xref:System.Net.Security> 命名空间中的多个组件代表调用应用程序执行集成式 Windows 身份验证。 本部分介绍为扩展 System.Net 组件在使用集成式 Windows 身份验证方面的保护对这些组件做出的更改。  
  
 当前 Windows 7 支持扩展保护。 提供了一种机制，所以应用程序可以确定操作系统是否支持扩展保护。  
  
## <a name="changes-to-support-extended-protection"></a>对支持扩展保护的更改  

 身份验证与集成式 Windows 身份验证一起使用时，其过程通常包括由目标计算机发出质询并将质询发送回客户端计算机，具体取决于使用的身份验证协议。 扩展保护将新功能添加到此身份验证进程  
  
 <xref:System.Security.Authentication.ExtendedProtection> 命名空间对采用应用程序扩展保护的身份验证提供支持。 此命名空间中的 <xref:System.Security.Authentication.ExtendedProtection.ChannelBinding> 类表示通道绑定。 此命名空间中的 <xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy> 类表示服务器用来验证传入客户端连接的扩展保护策略。 其他类成员用于扩展保护。  
  
 对于服务器应用程序，这些类包括以下内容：  
  
 含有以下元素的 <xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy>：  
  
- <xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy.OSSupportsExtendedProtection%2A> 属性，指示操作系统是否支持使用扩展保护的集成式 Windows 身份验证。  
  
- <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement> 值，该值指示应何时强制实施扩展保护策略。  
  
- <xref:System.Security.Authentication.ExtendedProtection.ProtectionScenario> 值，指示部署方案。 这会影响扩展保护的检查方式。  
  
- 可选的 <xref:System.Security.Authentication.ExtendedProtection.ServiceNameCollection>，其中包含自定义 SPN 列表，该列表用于根据客户端提供的作为身份验证预期目标的 SPN 进行匹配。  
  
- 可选的 <xref:System.Security.Authentication.ExtendedProtection.ChannelBinding>，其中包含用于验证的自定义通道绑定。 这不常见  
  
 <xref:System.Security.Authentication.ExtendedProtection.Configuration> 命名空间为采用应用程序扩展保护的身份验证提供配置支持。  
  
 进行了大量的功能更改来支持现有 <xref:System.Net> 命名空间中的扩展保护。 包括以下更改：  
  
- 新的 <xref:System.Net.TransportContext> 类添加到表示传输上下文的 <xref:System.Net> 命名空间。  
  
- 在 <xref:System.Net.HttpWebRequest> 类中新增 <xref:System.Net.HttpWebRequest.EndGetRequestStream%2A> 和 <xref:System.Net.HttpWebRequest.GetRequestStream%2A> 重载方法，允许检索 <xref:System.Net.TransportContext> 以支持客户端应用程序的扩展保护。  
  
- 对 <xref:System.Net.HttpListener> 和 <xref:System.Net.HttpListenerRequest> 类进行添加补充以支持服务器应用程序。  
  
 进行了功能更改，为现有 <xref:System.Net.Mail> 命名空间中的 SMTP 客户端应用程序提供扩展保护支持：  
  
- <xref:System.Net.Mail.SmtpClient> 类中的 <xref:System.Net.Mail.SmtpClient.TargetName%2A> 属性，在为 SMTP 客户端应用程序使用扩展保护时表示用于身份验证的 SPN。  
  
 进行了大量的功能更改来支持现有 <xref:System.Net.Security> 命名空间中的扩展保护。 包括以下更改：  
  
- 在 <xref:System.Net.Security.NegotiateStream> 类中新增 <xref:System.Net.Security.NegotiateStream.BeginAuthenticateAsClient%2A> 和 <xref:System.Net.Security.NegotiateStream.AuthenticateAsClient%2A> 重载方法，允许传递 CBT 来支持客户端应用程序的扩展保护。  
  
- 在 <xref:System.Net.Security.NegotiateStream> 类中新增 <xref:System.Net.Security.NegotiateStream.BeginAuthenticateAsServer%2A> 和 <xref:System.Net.Security.NegotiateStream.AuthenticateAsServer%2A> 重载方法，允许传递 <xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy> 来支持服务器应用程序的扩展保护。  
  
- 在 <xref:System.Net.Security.SslStream> 类中新增 <xref:System.Net.Security.SslStream.TransportContext%2A> 属性来支持客户端和服务器应用程序的扩展保护。  
  
 添加了 <xref:System.Net.Configuration.SmtpNetworkElement> 属性以支持在 <xref:System.Net.Security> 命名空间中为 SMTP 客户端配置扩展保护。  
  
## <a name="extended-protection-for-client-applications"></a>客户端应用程序的扩展保护  

 大多数客户端应用程序的扩展保护支持会自动发生。 只要 Windows 的基础版本支持扩展保护，<xref:System.Net.HttpWebRequest> 和 <xref:System.Net.Mail.SmtpClient> 类就支持扩展保护。 <xref:System.Net.HttpWebRequest> 实例发送从 <xref:System.Uri> 构造的 SPN。 默认情况下，<xref:System.Net.Mail.SmtpClient> 实例发送从 SMTP 邮件服务器的主机名构造的 SPN。  
  
 对于自定义身份验证，客户端应用程序可以使用 <xref:System.Net.HttpWebRequest> 类中的 <xref:System.Net.HttpWebRequest.EndGetRequestStream%28System.IAsyncResult%2CSystem.Net.TransportContext%40%29?displayProperty=nameWithType> 或 <xref:System.Net.HttpWebRequest.GetRequestStream%28System.Net.TransportContext%40%29?displayProperty=nameWithType> 方法，允许使用 <xref:System.Net.TransportContext.GetChannelBinding%2A> 方法来检索 <xref:System.Net.TransportContext> 和 CBT。  
  
 通过设置 <xref:System.Net.AuthenticationManager.CustomTargetNameDictionary%2A> 属性，可重写 <xref:System.Net.HttpWebRequest> 实例向给定服务发送集成式身份验证所用的 SPN。  
  
 <xref:System.Net.Mail.SmtpClient.TargetName%2A> 属性可用来设置用于 SMTP 连接的集成式 Windows 身份验证的自定义 SPN。  
  
## <a name="extended-protection-for-server-applications"></a>服务器应用程序的扩展保护  

 <xref:System.Net.HttpListener> 自动提供执行 HTTP 身份验证时验证服务绑定的机制。  
  
 最安全的方案是为 `HTTPS://` 前缀启用扩展保护。 在这种情况下，将 <xref:System.Net.HttpListener.ExtendedProtectionPolicy%2A?displayProperty=nameWithType> 设置为 <xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy>（其中 <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement> 设置为 <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.WhenSupported> 或 <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.Always>），并且将 <xref:System.Security.Authentication.ExtendedProtection.ProtectionScenario> 设置为 <xref:System.Security.Authentication.ExtendedProtection.ProtectionScenario.TransportSelected>。<xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.WhenSupported> 的值使 <xref:System.Net.HttpListener> 处于部分强化模式，而 <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.Always> 对应完全强化模式。  
  
 在此配置中，当通过外部安全通道向服务器发送请求时，将查询外部通道的通道绑定。 将此通道绑定传递到身份验证 SSPI 调用，这将验证身份验证 Blob 中的通道绑定是否匹配。 有三个可能的结果：  
  
1. 服务器的基础操作系统不支持扩展保护。 请求不会向应用程序公开，并且未授权的 (401) 响应将返回到客户端。 消息将记录到指定失败原因的 <xref:System.Net.HttpListener> 跟踪源。  
  
2. SSPI 调用失败，指示客户端指定了与从外部通道检索的期望值不匹配的通道绑定，或者指示当服务器上的扩展保护策略配置为 <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.Always> 时，客户端未能提供通道绑定。 在这两种情况下，请求不会向应用程序公开，并且未授权的 (401) 响应将返回到客户端。 消息将记录到指定失败原因的 <xref:System.Net.HttpListener> 跟踪源。  
  
3. 客户端指定正确的通道绑定或能够不指定通道绑定进行连接，因为服务器上的扩展保护策略配置为 <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.WhenSupported>。请求将返回到应用程序进行处理。 不会自动执行服务器名称检查。 应用程序可能使用 <xref:System.Net.HttpListenerRequest.ServiceName%2A> 属性来选择执行其自己的服务名称验证，但在这些情况下它是多余的。  
  
 如果应用程序自己进行 SSPI 调用以基于 HTTP 请求主体中来回传递的 Blob 执行身份验证，并且想要支持通道绑定，则它需要使用 <xref:System.Net.HttpListener> 来检索外部安全通道的预期通道绑定，以便将其传递给本机 Win32 [AcceptSecurityContext](/windows/win32/api/sspi/nf-sspi-acceptsecuritycontext) 函数。 若要执行此操作，请使用 <xref:System.Net.HttpListenerRequest.TransportContext%2A> 属性并调用 <xref:System.Net.TransportContext.GetChannelBinding%2A> 方法来检索 CBT。 仅支持终结点绑定。 如果指定除 <xref:System.Security.Authentication.ExtendedProtection.ChannelBindingKind.Endpoint> 外的其他内容，将引发 <xref:System.NotSupportedException>。 如果基础操作系统支持通道绑定，<xref:System.Net.TransportContext.GetChannelBinding%2A> 方法将返回 <xref:System.Security.Authentication.ExtendedProtection.ChannelBinding><xref:System.Runtime.InteropServices.SafeHandle>，包装指向适合传递到 [AcceptSecurityContext](/windows/win32/api/sspi/nf-sspi-acceptsecuritycontext) 函数的通道绑定的指针，因为 SecBuffer 结构的 pvBuffer 成员传入了 `pInput` 参数。 <xref:System.Security.Authentication.ExtendedProtection.ChannelBinding.Size%2A> 属性包含通道绑定的以字节为单位的长度。 如果基础操作系统不支持通道绑定，该函数将返回 `null`。  
  
 另一种可能的情况是未使用代理时为 `HTTP://` 前缀启用扩展保护。 在这种情况下，将 <xref:System.Net.HttpListener.ExtendedProtectionPolicy%2A?displayProperty=nameWithType> 设置为 <xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy>（其中 <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement> 设置为 <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.WhenSupported> 或 <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.Always>），并且将 <xref:System.Security.Authentication.ExtendedProtection.ProtectionScenario> 设置为 <xref:System.Security.Authentication.ExtendedProtection.ProtectionScenario.TransportSelected>。<xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.WhenSupported> 的值使 <xref:System.Net.HttpListener> 处于部分强化模式，而 <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.Always> 对应完全强化模式。  
  
 基于已向 <xref:System.Net.HttpListener> 注册的前缀创建允许的服务名称的默认列表。 可以通过 <xref:System.Net.HttpListener.DefaultServiceNames%2A> 属性检查此默认列表。 如果此列表不全面，应用程序可以在 <xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy> 类的构造函数中指定将使用的自定义服务名称集合，而不使用默认服务名称列表。  
  
 在此配置中，当没有外部安全通道身份验证情况下向服务器发出请求，通常不进行通道绑定检查。 如果身份验证成功，将查询上下文获取客户端提供的服务名称，并根据可接受服务名称列表验证上下文。 有四个可能的结果：  
  
1. 服务器的基础操作系统不支持扩展保护。 请求不会向应用程序公开，并且未授权的 (401) 响应将返回到客户端。 消息将记录到指定失败原因的 <xref:System.Net.HttpListener> 跟踪源。  
  
2. 客户端的基础操作系统不支持扩展保护。 在 <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.WhenSupported> 配置中，身份验证尝试将成功，并且请求将返回到应用程序。 在 <xref:System.Security.Authentication.ExtendedProtection.PolicyEnforcement.Always> 配置中，身份验证尝试将失败。 请求不会向应用程序公开，并且未授权的 (401) 响应将返回到客户端。 消息将记录到指定失败原因的 <xref:System.Net.HttpListener> 跟踪源。  
  
3. 客户端的基础操作系统支持扩展保护，但是应用程序未指定服务绑定。 请求不会向应用程序公开，并且未授权的 (401) 响应将返回到客户端。 消息将记录到指定失败原因的 <xref:System.Net.HttpListener> 跟踪源。  
  
4. 客户端指定了服务绑定。 服务绑定与允许的服务绑定列表进行比较。 如果匹配，请求会返回到应用程序。 否则，请求不会向应用程序公开，并且未授权 (401) 响应将自动返回到客户端。 消息将记录到指定失败原因的 <xref:System.Net.HttpListener> 跟踪源。  
  
 如果使用可接受服务名称的允许列表这个简单方法还不够，应用程序可以通过查询 <xref:System.Net.HttpListenerRequest.ServiceName%2A> 属性提供自己的服务名称验证。 在上述情况 1 和 2 中，该属性将返回 `null`。 在第 3 种情况中，它将返回空字符串。 在第 4 种情况中，将返回由客户端指定的服务名称。  
  
 这些扩展保护功能也可由服务器应用程序用于其他请求类型的、使用了受信任代理时的身份验证。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Security.Authentication.ExtendedProtection>
- <xref:System.Security.Authentication.ExtendedProtection.Configuration>
