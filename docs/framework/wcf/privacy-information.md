---
title: Windows Communication Foundation 隐私信息
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Communication Foundation, privacy information
- WCF, privacy information
- privacy information [WCF]
ms.assetid: c9553724-f3e7-45cb-9ea5-450a22d309d9
ms.openlocfilehash: 5ecd1c39a4ad6a146c734aab8349c5caa4212662
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96249938"
---
# <a name="windows-communication-foundation-privacy-information"></a>Windows Communication Foundation 隐私信息

Microsoft 承诺保护最终用户的隐私。 使用 Windows Communication Foundation (WCF) 版本3.0 生成应用程序时，应用程序可能会影响最终用户的隐私。 例如，应用程序可能显式收集用户联系信息，或者通过 Internet 向您的网站请求或发送信息。 如果您在应用程序中嵌入了 Microsoft 技术，则该技术可能具有可能会影响隐私的自己的行为。 WCF 不会从你的应用程序向 Microsoft 发送任何信息，除非你或最终用户选择将其发送给我们。  
  
## <a name="wcf-in-brief"></a>WCF 概述  

 WCF 是使用 Microsoft .NET 框架的分布式消息框架，允许开发人员生成分布式应用程序。 在两个应用程序之间交换的消息包含标头和正文信息。  
  
 标头可能包含消息路由、安全信息、事务和其他信息，具体取决于应用程序所使用的服务。 默认情况下，消息通常要进行加密。 一个例外的情况是使用 `BasicHttpBinding`（它适用于不受保护的旧式 Web 服务）。 作为应用程序设计人员，您负责进行最终设计。 SOAP 正文中的消息包含特定于应用程序的数据;不过，这些数据（如应用程序定义的个人信息）可以通过使用 WCF 加密或机密性功能来保护。 以下几节将描述可能对隐私造成影响的功能。  
  
## <a name="messaging"></a>Messaging  

 每个 WCF 消息都有一个地址标头，该标头指定消息目标和答复的目标位置。  
  
 终结点地址的地址部分是一个标识该终结点的统一资源标识符 (URI)。 该地址可以是网络地址，也可以是逻辑地址。 该地址可能包含计算机名称（主机名、完全限定域名）和一个 IP 地址。 终结点地址还可能包含一个用于进行临时寻址的全局唯一标识符 (GUID) 或 GUID 集合，以便辨别每个地址。 每个消息都包含一个消息 ID，该消息 ID 是 GUID。 此功能遵循 WS-Addressing 引用标准。  
  
 WCF 消息传递层不会向本地计算机写入任何个人信息。 但是，如果服务开发人员创建了公开此类信息的服务（例如，通过在终结点名称中使用个人姓名，或者将个人信息包含在终结点的 Web 服务描述语言中，但不要求客户端使用 https 来访问 WSDL），则消息传递层可能会在网络级传播个人信息。 此外，如果开发人员对公开个人信息的终结点运行 " [)  ( 的元数据实用工具" 工具 ](servicemodel-metadata-utility-tool-svcutil-exe.md) ，则该工具的输出可能包含该信息，并且输出文件将写入本地硬盘。  
  
## <a name="hosting"></a>Hosting  

 WCF 中的托管功能使应用程序可以按需启动，或者允许在多个应用程序之间共享端口。 WCF 应用程序可以在 Internet Information Services (IIS) 中承载，类似于 ASP.NET。  
  
 承载功能不会在网路上公开任何特定信息，也不会在计算机上保留数据。  
  
## <a name="message-security"></a>消息安全  

 WCF 安全性为消息传递应用程序提供安全功能。 所提供的安全功能包括身份验证和授权。  
  
 身份验证通过在客户端和服务之间传递凭据来执行。 身份验证可以通过传输级安全实现，也可以通过 SOAP 消息级安全实现，如下所述：  
  
- 在 SOAP 消息安全中，身份验证通过诸如用户名/密码、X.509 证书、Kerberos 票证以及 SAML 令牌等凭据来执行，所有这些凭据都可能包含个人信息（根据颁发机构的情况而定）。  
  
- 使用传输安全时，身份验证通过传统的传输身份验证机制来实现，如 HTTP 身份验证方案（基本、摘要式、协商、集成 Windows 身份验证、NTLM、无身份验证和匿名）和窗体身份验证。  
  
 身份验证可以导致在相互通信的终结点之间建立安全会话。 会话由 GUID 标识，而 GUID 能够在安全会话的整个生存期保持有效。 下表显示保留的内容和保留位置。  
  
|数据|存储|  
|----------|-------------|  
|表示凭据，例如用户名、X.509 证书、Kerberos 令牌和对凭据的引用。|标准 Windows 凭据管理机制，例如 Windows 证书存储区。|  
|用户成员资格信息，例如用户名和密码。|ASP.NET 成员资格提供程序。|  
|用于向客户端证明服务身份的有关服务的标识信息。|服务的终结点地址。|  
|调用方信息。|审核日志。|  
  
## <a name="auditing"></a>审核  

 审核功能用于记录身份验证和授权事件的成功和失败。 审核记录包含以下数据：服务 URI、操作 URI 和调用方的标识。  
  
 审核功能还记录管理员修改消息日志记录的配置（启用或禁用）的时间，因为消息日志记录可能记录标头或正文中特定于应用程序的数据。 对于 Windows XP，记录记录在应用程序事件日志中。 对于 Windows Vista 和 Windows Server 2003，会在安全事件日志中记录一条记录。  
  
## <a name="transactions"></a>事务  

 事务功能为 WCF 应用程序提供事务性服务。  
  
 事务传播中使用的事物标头可能包含事务 ID 或登记 ID（这些 ID 都是 GUID）。  
  
 事务功能使用 Microsoft 分布式事务协调器 (MSDTC) 事务管理器（一个 Windows 组件）来管理事务状态。 默认情况下，事务管理器之间的通信进行加密。 事务管理器会将终结点引用、事务 ID 和登记 ID 作为其持久状态的一部分进行记录。 此状态的生存期由事务管理器的日志文件的生存期确定。 MSDTC 服务拥有并维护此日志。  
  
 事务功能实现了 WS-Coordination 和 WS-Atomic 事务标准。  
  
## <a name="reliable-sessions"></a>可靠会话  

 WCF 中的可靠会话在发生传输或中介故障时提供消息传输。 甚至在基础传输断开（例如，无线网络上的 TCP 连接断开）或丢失消息（HTTP 代理丢弃传出消息或传入消息）时，它们也会提供“一次性”消息传送。 可靠会话还可以恢复消息重新排序（这可能在多路径路由的情况下发生），从而保留消息的发送顺序。  
  
 可靠会话是使用 WS-ReliableMessaging (WS-RM) 协议来实现的。 可靠会话添加包含会话信息的 WS-RM 标头，这些会话信息用于标识与特定可靠会话关联的所有消息。 每个 WS-RM 会话都有一个 GUID 标识符。  
  
 最终用户的计算机上不会保留任何个人信息。  
  
## <a name="queued-channels"></a>排队通道  

 队列代表接收应用程序存储来自发送应用程序的消息，之后再将这些消息转发给接收应用程序。 队列有助于在某些情况下（例如，接收应用程序是瞬态应用程序）确保消息从发送应用程序传送到接收应用程序。 WCF 通过使用 Microsoft 消息队列 (MSMQ) 作为传输来提供对队列的支持。  
  
 排队通道功能不会向消息中添加标头。 相反，它会创建一个具有适当的消息队列消息属性集的消息队列消息，并调用消息队列方法来将消息放入“消息队列”队列中。 消息队列是一个可选组件，它随 Windows 一起提供。  
  
 排队通道功能不会将任何信息保留在最终用户的计算机上，因为它使用消息队列作为队列基础结构。  
  
## <a name="com-integration"></a>COM+ 集成  

 此功能包装现有的 COM 和 COM + 功能，以创建与 WCF 服务兼容的服务。 此功能不使用特定的标头，并且不会在最终用户的计算机上保留数据。  
  
## <a name="com-service-moniker"></a>COM 服务标记  

 这为标准 WCF 客户端提供非托管包装。 此功能在网络上没有特定的标头，也不会在计算机上持久保留数据。  
  
## <a name="peer-channel"></a>对等通道  

 对等通道允许使用 WCF 开发多方应用程序。 多方消息传递发生在网格环境中。 网格由节点可以加入的名称来标识。 对等通道中的每个节点都在用户指定的端口创建一个 TCP 侦听器，并与网格中的其他节点建立连接以确保连接的弹性。 为了与网格中的其他节点进行连接，节点还会与网格中的其他节点交换一些数据，包括侦听器地址和计算机的 IP 地址。 在网格中四处发送的消息可能包含与发送方相关的安全信息，以防止发生消息欺骗和篡改。  
  
 最终用户的计算机上不存储任何个人信息。  
  
## <a name="it-professional-experience"></a>IT 专业经验  
  
### <a name="tracing"></a>跟踪  

 WCF 基础结构的诊断功能记录传递到传输和服务模型层的消息，以及与这些消息关联的活动和事件。 默认情况下，此功能处于禁用状态。 使用应用程序的配置文件启用此功能，并且可以在运行时使用 WCF WMI 提供程序修改跟踪行为。 在启用此功能后，跟踪基础结构会向已配置的侦听器发出包含消息、活动和处理事件的诊断跟踪。 输出的格式和位置由管理员的侦听器配置选择确定，但通常是 XML 格式化文件。 管理员负责设置跟踪文件上的访问控制列表 (ACL)。 具体而言，在由 Windows 激活系统 (WAS) 进行承载时，管理员应确保不是从公共虚拟根目录提供这些文件（如果不需要）。  
  
 有两种类型的跟踪：消息日志记录和服务模型诊断跟踪，下一节将对其进行介绍。 每种类型都通过其自身的跟踪源进行配置，它们分别是 <xref:System.ServiceModel.Configuration.DiagnosticSection.MessageLogging%2A> 和 <xref:System.ServiceModel>。 这两种日志记录跟踪源都捕获应用程序本地的数据。  
  
### <a name="message-logging"></a>消息日志记录  

 消息日志记录跟踪源（<xref:System.ServiceModel.Configuration.DiagnosticSection.MessageLogging%2A>）使管理员可以记录流经系统的消息。 通过配置，用户可以决定是记录完整的消息还是仅记录消息头、是否在传输和/或服务模型层记录以及是否包括格式不正确的消息。 另外，用户可以配置筛选来限制要记录的消息范围。  
  
 默认情况下，消息日志记录被禁用。 本地计算机管理员可以阻止应用程序级管理员启用消息日志记录。  
  
#### <a name="encrypted-and-decrypted-message-logging"></a>加密和解密消息日志记录  

 消息按下列术语所描述的方式进行记录、加密或解密。  
  
 传输日志记录  
 记录在传输级接收和发送的消息。 这些消息包含所有标头，并且在网络上发送之前以及在接收时可能进行加密。  
  
 如果消息在网络上发送之前以及在接收时进行加密，那么还会记录加密后的消息。 一个例外的情况是使用安全协议 (https)：尽管消息在网络上进行了加密，但是在发送之前以及接收之后会记录解密的消息。  
  
 服务日志记录  
 在通道标头处理已经发生后，刚好在进入用户代码之前和之后记录在服务模型层接收或发送的消息。  
  
 即使消息在网络上进行了保护和加密，在此层记录的消息也是解密消息。  
  
 格式不正确的消息日志记录  
 记录 WCF 基础结构无法理解或处理的消息。  
  
 消息按原样（即，加密或不加密）进行记录。  
  
 如果以解密或未加密形式记录消息，则默认情况下，WCF 将在记录消息之前从消息中删除安全密钥和潜在的个人信息。 下面几节将描述要删除的消息以及删除时间。 计算机管理员和应用程序部署人员都必须执行一定的配置操作来更改默认行为，以便记录密钥和潜在的个人信息。  
  
#### <a name="information-removed-from-message-headers-when-logging-decryptedunencrypted-messages"></a>在记录解密/未加密消息时从消息头中删除的信息  

 在以解密/未加密形式记录消息时，默认情况下，将在记录消息之前从消息头和消息正文中删除安全密钥和潜在的个人信息。 下面的列表显示 WCF 如何考虑密钥和潜在的个人信息。  
  
 被删除的密钥：  
  
 \- 对于 xmlns： wst = " http://schemas.xmlsoap.org/ws/2004/04/trust " 和 xmlns： wst = " http://schemas.xmlsoap.org/ws/2005/02/trust "  
  
 wst:BinarySecret  
  
 wst:Entropy  
  
 \- 对于 xmlns： wsse = " http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.1.xsd " 和 xmlns： wsse = " http://docs.oasis-open.org/wss/2005/xx/oasis-2005xx-wss-wssecurity-secext-1.1.xsd "  
  
 wsse:Password  
  
 wsse:Nonce  
  
 被删除的潜在个人信息：  
  
 \- 对于 xmlns： wsse = " http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.1.xsd " 和 xmlns： wsse = " http://docs.oasis-open.org/wss/2005/xx/oasis-2005xx-wss-wssecurity-secext-1.1.xsd "  
  
 wsse:Username  
  
 wsse:BinarySecurityToken  
  
 \- 对于 xmlns： saml = "urn： oasis： names： tc： SAML：1.0： assertion"，以下)  (中的项将被删除：  
  
 \<断言  
  
 MajorVersion="1"  
  
 MinorVersion="1"  
  
 AssertionId="[ID]"  
  
 Issuer="[string]"  
  
 IssueInstant="[dateTime]"  
  
 >  
  
 \<Conditions NotBefore="[dateTime]" NotOnOrAfter="[dateTime]">  
  
 \<AudienceRestrictionCondition>  
  
 \<Audience>oma-uri\</Audience>+  
  
 \</AudienceRestrictionCondition>*  
  
 \<DoNotCacheCondition />*  
  
 <\!--抽象基类型  
  
 \<Condition />*  
  
 -->  
  
 \</Conditions>?  
  
 \<Advice>  
  
 \<AssertionIDReference>识别\</AssertionIDReference>*  
  
 \<Assertion>断言\</Assertion>*  
  
 [any]*  
  
 \</Advice>?  
  
 <\!--抽象基类型  
  
 \<Statement />*  
  
 \<SubjectStatement>  
  
 \<Subject>  
  
 `<NameIdentifier`  
  
 `NameQualifier="[string]"?`  
  
 `Format="[uri]"?`  
  
 `>`  
  
 `[string]`  
  
 `</NameIdentifier>?`  
  
 \<SubjectConfirmation>  
  
 \<ConfirmationMethod>AnyUri\</ConfirmationMethod>+  
  
 \<SubjectConfirmationData>[任何] \</SubjectConfirmationData> ？  
  
 \<ds:KeyInfo>...\</ds:KeyInfo>?  
  
 \</SubjectConfirmation>?  
  
 \</Subject>  
  
 \</SubjectStatement>*  
  
 -->  
  
 \<AuthenticationStatement  
  
 AuthenticationMethod="[uri]"  
  
 AuthenticationInstant="[dateTime]"  
  
 >  
  
 [Subject]  
  
 `<SubjectLocality`  
  
 `IPAddress="[string]"?`  
  
 `DNSAddress="[string]"?`  
  
 `/>?`  
  
 <AuthorityBinding  
  
 AuthorityKind="[QName]"  
  
 Location="[uri]"  
  
 Binding="[uri]"  
  
 />*  
  
 \</AuthenticationStatement>*  
  
 \<AttributeStatement>  
  
 [Subject]  
  
 \<Attribute  
  
 AttributeName="[string]"  
  
 AttributeNamespace="[uri]"  
  
 >  
  
 `<AttributeValue>[any]</AttributeValue>+`  
  
 \</Attribute>+  
  
 \</AttributeStatement>*  
  
 \<AuthorizationDecisionStatement  
  
 Resource="[uri]"  
  
 决策 = "[允许&#124;拒绝&#124;不确定]"  
  
 >  
  
 [Subject]  
  
 \<Action Namespace="[uri]">类似\</Action>+  
  
 \<Evidence>  
  
 \<AssertionIDReference>识别\</AssertionIDReference>+  
  
 \<Assertion>断言\</Assertion>+  
  
 \</Evidence>?  
  
 \</AuthorizationDecisionStatement>*  
  
 \</Assertion>  
  
#### <a name="information-removed-from-message-bodies-when-logging-decryptedunencrypted-messages"></a>在记录解密/未加密消息时从消息正文中删除的信息  

 如前所述，WCF 从消息头中删除已记录的解密/未加密消息的密钥和已知的潜在个人信息。 另外，WCF 将从以下列表中的正文元素和操作的消息正文中删除密钥和已知的潜在个人信息，其中描述了密钥交换中涉及的安全消息。  
  
 对于下列命名空间：  
  
 xmlns： wst = " http://schemas.xmlsoap.org/ws/2004/04/trust " 和 xmlns： wst = " http://schemas.xmlsoap.org/ws/2005/02/trust " (例如，如果没有可用的操作)   
  
 删除那些涉及密钥交换的正文元素的信息：  
  
 wst:RequestSecurityToken  
  
 wst:RequestSecurityTokenResponse  
  
 wst:RequestSecurityTokenResponseCollection  
  
 还要删除下列每个操作的信息：  
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RST/Issue`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RSTR/Issue`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RST/Renew`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RSTR/Renew`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RST/Cancel`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RSTR/Cancel`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RST/Validate`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RSTR/Validate`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RST/SCT`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RSTR/SCT`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RST/SCT/Amend`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RSTR/SCT/Amend`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RST/SCT/Renew`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RSTR/SCT/Renew`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RST/SCT/Cancel`
  
- `http://schemas.xmlsoap.org/ws/2005/02/trust/RSTR/SCT/Cancel`
  
- `http://schemas.xmlsoap.org/ws/2004/04/security/trust/RST/SCT`
  
- `http://schemas.xmlsoap.org/ws/2004/04/security/trust/RSTR/SCT`
  
- `http://schemas.xmlsoap.org/ws/2004/04/security/trust/RST/SCT-Amend`
  
- `http://schemas.xmlsoap.org/ws/2004/04/security/trust/RSTR/SCT-Amend`
  
#### <a name="no-information-is-removed-from-application-specific-headers-and-body-data"></a>不会从特定于应用程序的标头和正文数据中删除任何信息  

 WCF 不跟踪特定于应用程序的标头中的个人信息 (例如，查询字符串) 或正文数据 (例如，信用卡号码) 。  
  
 启用消息日志记录后，特定于应用程序的标头中的个人信息和正文信息在日志中可见。 同样，应用程序部署人员负责设置配置和日志文件上的 ACL。 如果不希望此信息可见，还可以关闭日志记录，或者在记录日志文件后从日志文件中筛选出此信息。  
  
### <a name="service-model-tracing"></a>服务模型跟踪  

 使用服务模型跟踪源（<xref:System.ServiceModel>）可以对与消息处理相关的活动和事件进行跟踪。 此功能使用 <xref:System.Diagnostics> 中的 .NET Framework 诊断功能。 就像 <xref:System.ServiceModel.Configuration.DiagnosticSection.MessageLogging%2A> 属性一样，用户可以使用 .NET Framework 应用程序配置文件对其位置和 ACL 进行配置。 与消息日志记录一样，文件位置总是在管理员启用跟踪时进行配置，这样，管理员便可以控制 ACL。  
  
 当消息在范围内时，跟踪包含消息头。 上一节中描述的有关隐藏消息头中的潜在个人信息的相同规则同样适用：默认情况下，将先前标识的个人信息从跟踪中包含的标头中删除。 计算机管理员和应用程序部署人员都必须修改配置，以便记录潜在的个人信息。 但是，跟踪中会记录特定于应用程序的标头中包含的个人信息。 应用程序部署人员负责设置配置和跟踪文件上的 ACL。 它们还可以禁用跟踪来隐藏此信息，或在记录后从跟踪文件中筛选出此信息。  
  
 作为服务模型跟踪的一部分，当消息流经基础结构的不同部分时，由唯一的 ID（称为活动 ID，通常是 GUID）将不同的活动链接到一起。  
  
#### <a name="custom-trace-listeners"></a>自定义跟踪侦听器  

 对于消息日志记录和跟踪记录，可以配置一个自定义跟踪侦听器，该侦听器可以在网络上发送跟踪和消息（例如，向远程数据库发送）。 应用程序部署人员负责配置自定义侦听器或授权用户执行这一操作。 它们还负责在远程位置公开的任何个人信息，并将 Acl 正确地应用到此位置。  
  
### <a name="other-features-for-it-professionals"></a>IT 专业人员的其他作用  

 WCF 有一个 WMI 提供程序，该提供程序通过随 Windows) 提供的 WMI (公开 WCF 基础结构配置信息。 默认情况下，WMI 接口可供管理员使用。  
  
 WCF 配置使用 .NET Framework 配置机制。 配置文件存储在计算机上。 应用程序开发人员和管理员为应用程序的每个需求创建配置文件和 ACL。 配置文件可以包含终结点地址和指向证书存储区中的证书的链接。 证书可用于提供应用程序数据，以配置应用程序所使用的功能的各种属性。  
  
 WCF 还通过调用方法使用 .NET Framework 进程转储功能 <xref:System.Environment.FailFast%2A> 。  
  
### <a name="it-pro-tools"></a>IT 专业工具  

 WCF 还提供了以下 Windows SDK 附带的 IT 专业工具。  
  
#### <a name="svctraceviewerexe"></a>SvcTraceViewer.exe  

 查看器将显示 WCF 跟踪文件。 该查看器显示跟踪中包含的所有信息。  
  
#### <a name="svcconfigeditorexe"></a>SvcConfigEditor.exe  

 编辑器允许用户创建和编辑 WCF 配置文件。 该编辑器显示配置文件中包含的所有信息。 使用文本编辑器可以完成同样的任务。  
  
#### <a name="servicemodel_reg"></a>ServiceModel_Reg  

 此工具使用户可以管理计算机上的 ServiceModel 安装。 该工具在运行时在控制台窗口中显示状态消息，在此过程中，可能会显示有关 WCF 安装的配置的信息。  
  
#### <a name="wsatconfigexe-and-wsatuidll"></a>WSATConfig.exe 和 WSATUI.dll  

 这些工具使 IT 专业人员能够在 WCF 中配置互操作 WS-AtomicTransaction 网络支持。 这两种工具显示并使用户可以更改存储在注册表中的最常用 WS-AtomicTransaction 设置的值。  
  
## <a name="cross-cutting-features"></a>横切功能  

 下列功能是横切功能。 即，它们可以用前面的任何功能构成。  
  
### <a name="service-framework"></a>服务框架  

 标头可以包含一个实例 ID，该 ID 是一个将消息与 CLR 类的一个实例相关联的 GUID。  
  
 Web 服务描述语言 (WSDL) 包含端口的定义。 每个端口都具有一个终结点地址和一个表示应用程序所使用的服务的绑定。 可以使用配置禁用公开 WSDL。 计算机上不会保留任何信息。  
  
## <a name="see-also"></a>另请参阅

- [Windows Communication Foundation](index.md)
- [安全性](./feature-details/security.md)
