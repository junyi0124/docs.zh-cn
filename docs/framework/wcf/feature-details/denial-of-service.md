---
title: 拒绝服务
ms.date: 03/30/2017
helpviewer_keywords:
- denial of service [WCF]
ms.assetid: dfb150f3-d598-4697-a5e6-6779e4f9b600
ms.openlocfilehash: 6bf0dd8af32f50164138092684c4b82f12134718
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96291591"
---
# <a name="denial-of-service"></a>拒绝服务

当系统处于过载状态而无法处理消息或者处理速度极慢时，会出现拒绝服务的情况。  
  
## <a name="excess-memory-consumption"></a>过量占用内存  

 如果读取的 XML 文档具有大量唯一的本地名称、命名空间或前缀，则可能会出现问题。 如果使用的是从 <xref:System.Xml.XmlReader> 派生的类，并针对每一项调用了 <xref:System.Xml.XmlReader.LocalName%2A>、<xref:System.Xml.XmlReader.Prefix%2A> 或 <xref:System.Xml.XmlReader.NamespaceURI%2A> 属性，则返回的字符串将添加到 <xref:System.Xml.NameTable> 中。 <xref:System.Xml.NameTable> 包含的集合的大小绝不会减小，它会创建字符串句柄的虚拟“内存泄漏”。  
  
 缓解措施包括：  
  
- 从 <xref:System.Xml.NameTable> 类派生并实施最大大小配额。 （当 <xref:System.Xml.NameTable> 已满时，将无法阻止使用或切换 <xref:System.Xml.NameTable>。）  
  
- 避免使用所提到的这些属性，而是尽可能将 <xref:System.Xml.XmlReader.MoveToAttribute%2A> 方法与 <xref:System.Xml.XmlReader.IsStartElement%2A> 方法结合使用；这些方法不返回字符串，从而可以避免过度填充 <xref:System.Xml.NameTable> 集合的问题。  
  
## <a name="malicious-client-sends-excessive-license-requests-to-service"></a>恶意客户端向服务发送过多的许可证请求  

 如果恶意客户端通过过多的许可证请求来攻击某个服务，则可能会致使服务器占用过量的内存。  
  
 缓解：使用 <xref:System.ServiceModel.Channels.LocalServiceSecuritySettings> 类的下列属性：  
  
- <xref:System.ServiceModel.Channels.LocalServiceSecuritySettings.MaxCachedCookies%2A>：控制在 `SecurityContextToken` 或 `SPNego` 协商之后服务器所缓存的有时限的 `SSL` 的最大数目。  
  
- <xref:System.ServiceModel.Channels.LocalServiceSecuritySettings.IssuedCookieLifetime%2A>：控制在 `SecurityContextTokens` 或 `SPNego` 协商之后服务器所颁发的 `SSL` 的生存期。 在此时间期限内，服务器将缓存 `SecurityContextToken`。  
  
- <xref:System.ServiceModel.Channels.LocalServiceSecuritySettings.MaxPendingSessions%2A>：控制在服务器上建立但尚未针为其处理任何应用程序消息的安全对话的最大数目。 此配额可防止客户端在服务上建立安全对话从而致使服务维护每个客户端的状态，但是从不使用这些状态。  
  
- <xref:System.ServiceModel.Channels.LocalServiceSecuritySettings.InactivityTimeout%2A>：控制服务使安全对话保持活动状态（而不接收来自对话客户端的应用程序消息）的最长时间。 此配额可防止客户端在服务上建立安全对话从而致使服务维护每个客户端的状态，但是从不使用这些状态。  
  
## <a name="wsdualhttpbinding-or-dual-custom-bindings-require-client-authentication"></a>WSDualHttpBinding 或双向自定义绑定要求客户端身份验证  

 默认情况下，<xref:System.ServiceModel.WSDualHttpBinding> 会启用安全性。 但是，如果通过将 <xref:System.ServiceModel.MessageSecurityOverHttp.ClientCredentialType%2A> 属性设置为 <xref:System.ServiceModel.MessageCredentialType.None> 而禁用客户端身份验证，则恶意用户可能会致使针对第三个服务进行拒绝服务攻击。 发生此问题的原因在于，恶意客户端可能会指示服务向第三个服务发送消息流。  
  
 为了缓解此问题，请不要将该属性设置为 `None`。 另请注意，在创建具有双向消息模式的自定义绑定时也可能出现此问题。  
  
## <a name="auditing-event-log-can-be-filled"></a>可以填充审核事件日志  

 如果恶意用户了解到审核功能处于启用状态，则该攻击者可能会发送导致写入审核项的无效消息。 如果以这种方式填充审核日志，则审核系统会出现故障。  
  
 为了缓解此问题，请将 <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.SuppressAuditFailure%2A> 属性设置为 `true`，然后使用事件查看器的属性来控制审核行为。 有关使用事件查看器查看和管理事件日志的详细信息，请参阅 [事件查看器](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc766042(v=ws.11))。 有关详细信息，请参阅 [审核](auditing-security-events.md)。  
  
## <a name="invalid-implementations-of-iauthorizationpolicy-can-cause-service-to-become-unresponsive"></a>无效的 IAuthorizationPolicy 实现会导致服务无法响应  

 <xref:System.IdentityModel.Policy.IAuthorizationPolicy.Evaluate%2A>对接口的错误实现调用方法 <xref:System.IdentityModel.Policy.IAuthorizationPolicy> 可能会导致服务无法响应。  
  
 缓解：仅使用受信任的代码。 即，仅使用在编写后经过测试的代码或者来自受信任提供者的代码。 未经深思熟虑，请勿允许在代码中插入对 <xref:System.IdentityModel.Policy.IAuthorizationPolicy> 的不受信任的扩展。 这适用于服务实现中所使用的全部扩展。 WCF 不会对使用扩展点插入的应用程序代码和外接程序代码进行任何区分。  
  
## <a name="kerberos-maximum-token-size-may-need-resizing"></a>可能需要调整最大 Kerberos 令牌大小  

 如果客户端属于许多组（大约 900 个，尽管实际数字因组的数目而异），则可能会在消息头的块超过 64 KB 时出现问题。 在这种情况下，可以增加 Kerberos 令牌的最大大小。 你可能还需要增加最大 WCF 消息大小以容纳更大的 Kerberos 令牌。  
  
## <a name="autoenrollment-results-in-multiple-certificates-with-same-subject-name-for-machine"></a>自动注册功能会为计算机生成多个具有相同主题名称的证书  

 自动 *注册* 是 Windows Server 2003 的一种功能，用于自动注册用户和计算机的证书。 如果计算机处于启用了该功能的域中，那么，每当有新计算机加入网络中时，都会自动创建一个既定目的为客户端身份验证的 X.509 证书，并将其插入本地计算机的“个人”证书存储区。 但是，自动注册功能对它在缓存中创建的所有证书使用同一主题名称。  
  
 影响是 WCF 服务可能无法在具有自动注册功能的域上打开。 出现此问题的原因在于，有多个证书具有计算机的完全限定域名系统 (DNS) 名称，从而使得默认的服务 X.509 凭据搜索条件可能会不明确。 一个证书源于自动注册功能；而另一个可能是自行颁发的证书。  
  
 若要缓解这种情况，请通过在上使用更精确的搜索条件来引用要使用的确切证书 [\<serviceCredentials>](../../configure-apps/file-schema/wcf/servicecredentials.md) 。 例如，使用 <xref:System.Security.Cryptography.X509Certificates.X509FindType.FindByThumbprint> 选项并按照证书的唯一指纹（哈希）来指定证书。  
  
 有关自动注册功能的详细信息，请参阅 [Windows Server 2003 中的证书自动注册](/previous-versions/windows/it-pro/windows-server-2003/cc778954(v=ws.10))。  
  
## <a name="last-of-multiple-alternative-subject-names-used-for-authorization"></a>用于授权的多个备选主题名称中的最后一个  

 在极少数情况下，如果 X.509 证书包含多个备选主题名称，并且您使用备选主题名称进行授权，则授权可能会失败。  
  
## <a name="protect-configuration-files-with-acls"></a>使用 ACL 保护配置文件  

 可以在代码和配置文件中为 CardSpace 颁发的令牌指定必需的和可选的声明。 这会导致在发送到安全令牌服务的 `RequestSecurityToken` 消息中发出相应的元素。 攻击者可能会通过修改代码或配置来移除必需的或可选的声明，从而可能会让安全令牌服务颁发不允许访问目标服务的令牌。  
  
 缓解措施：要求具有访问计算机的权限才能修改配置文件。 使用文件访问控制列表 (ACL) 来保护配置文件。 WCF 要求代码位于应用程序目录或全局程序集缓存中，然后才允许从配置加载此类代码。 使用目录 ACL 可以保护目录。  
  
## <a name="maximum-number-of-secure-sessions-for-a-service-is-reached"></a>达到了服务安全会话的最大数目  

 当客户端由某个服务成功进行身份验证，而且与此服务建立了安全会话时，此服务会记住该会话，直到该会话被客户端取消或者过期。 对于建立的每个会话都将进行计数，直到达到与该服务的同时活动会话的最大数目限制。 达到该限制时，尝试与该服务创建新会话的客户端将被拒绝，直到一个或多个活动会话过期或者被客户端取消。 一个客户端可以与某个服务建立多个会话，对于每个会话都将计数，直到达到相应的限制。  
  
> [!NOTE]
> 在使用有状态会话时，上述内容并不适用。 有关有状态会话的详细信息，请参阅 [如何：为安全会话创建安全上下文令牌](how-to-create-a-security-context-token-for-a-secure-session.md)。  
  
 若要缓解此问题，请通过设置 <xref:System.ServiceModel.Channels.SecurityBindingElement> 类的 <xref:System.ServiceModel.Channels.SecurityBindingElement> 属性来设置活动会话的最大数目限制以及会话的最长生存期限制。  
  
## <a name="see-also"></a>另请参阅

- [安全注意事项](security-considerations-in-wcf.md)
- [信息泄露](information-disclosure.md)
- [权限提升](elevation-of-privilege.md)
- [拒绝服务](denial-of-service.md)
- [重播攻击](replay-attacks.md)
- [篡改](tampering.md)
- [不支持的方案](unsupported-scenarios.md)
