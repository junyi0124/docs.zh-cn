---
title: 安全性概述
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Communication Foundation, security
- WCF, security
ms.assetid: f478c80d-792d-4e7a-96bd-a2ff0b6f65f9
ms.openlocfilehash: c14ccb88d12622308f2474844f86b5f73c1f34b2
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96293697"
---
# <a name="windows-communication-foundation-security-overview"></a>Windows Communication Foundation 安全性概述

Windows Communication Foundation (WCF) 是一个基于 SOAP 消息的分布式编程平台，并且客户端和服务之间的消息安全对于保护数据非常重要。 WCF 提供了一种通用且可互操作的平台，用于根据现有安全基础结构和 SOAP 消息的已识别安全标准来交换安全消息。  
  
> [!NOTE]
> 有关 WCF 安全的综合指南，请参阅 [Wcf 安全指南](https://archive.codeplex.com/?p=WCFSecurity)。  
  
 如果你已使用现有技术（如 HTTPS）、Windows 集成安全性或用户名和密码对用户进行身份验证，则 WCF 使用熟悉的概念。 WCF 不仅与现有安全基础结构集成，而且还通过使用安全 SOAP 消息将分布式安全性扩展到仅 Windows 域之外。 考虑使用 WCF 作为协议（除了现有协议外）使用 SOAP 作为协议，实现现有安全机制。 例如，用于标识客户端或服务的凭据（如用户名和密码或 X.509 证书）具有可互操作的基于 XML 的 SOAP 配置文件。 使用这些配置文件时，消息利用开放式规范（如 XML 数字签名和 XML 加密）进行安全交换。 有关规范的列表，请参阅 [System-Provided 互操作性绑定支持的 Web 服务协议](web-services-protocols-supported-by-system-provided-interoperability-bindings.md)。  
  
 另一个并列项是 Windows 平台上的组件对象模型 (COM)，它可实现安全的分布式应用程序。 COM 具有全面的安全机制，因此安全上下文可以在各组件间流动；此机制可强制实现完整性、保密性和身份验证。 但是，COM 不启用跨平台、安全消息（如 WCF）。 使用 WCF，你可以在 Internet 上构建跨 Windows 域的服务和客户端。 WCF 的可互操作消息对于构建动态、业务驱动的服务非常重要，可帮助你放心地保护你的信息的安全。  
  
## <a name="windows-communication-foundation-security-benefits"></a>Windows Communication Foundation 安全性优点  

 WCF 是一个基于 SOAP 消息的分布式编程平台。 使用 WCF，您可以创建同时充当服务和服务客户端的应用程序，从而创建和处理来自不限数量的其他服务和客户端的消息。 在此类分布式应用程序中，消息可以从一个节点流向另一个节点，并在通过防火墙进入 Internet 后再通过无数个 SOAP 中间项。 这会带来各种不同的消息安全性威胁。 下面的示例说明 WCF 安全在实体之间交换消息时可帮助减轻的一些常见威胁：  
  
- 观测网络流量以获取敏感信息。 以在线银行为例，某个客户端请求将资金从一个帐户转帐至另一个帐户。 一个恶意用户截获了此消息（具有帐号和密码），随后从盗用的帐户将资金转出。  
  
- 欺诈性实体在客户端未发觉的情况下起服务的作用。 在此情况下，恶意用户（欺诈方）充当在线服务，从客户端截获消息以获取敏感信息。 然后，欺诈方使用窃取的数据将资金从盗用的帐户转出。 这种攻击也称为 " *网络钓鱼攻击*"。  
  
- 更改消息以获取与调用方所需的结果不同的结果。 例如，更改用于存款的帐号以便将资金转移到恶意帐户。  
  
- 黑客重放，恶意黑客重放同一采购订单。 例如，一家网上书店收到数百张订单并将书籍发送给并未订购这些书籍的客户。  
  
- 使某项服务无法对客户端进行身份验证。 在此情况下，服务无法确保相应人员执行该项事务。  
  
 总的来说，传输安全性可提供下列保障：  
  
- 服务终结点（响应方）身份验证。  
  
- 客户端主体（发起方）身份验证。  
  
- 消息完整性。  
  
- 消息保密性。  
  
- 重放检测。  
  
### <a name="integration-with-existing-security-infrastructures"></a>与现有安全性基础结构集成  

 通常，Web 服务部署都具有现成的安全性解决方案，例如，安全套接字层 (SSL) 或 Kerberos 协议。 某些服务则利用已部署的安全性基础结构，例如，使用 Active Directory 的 Windows 域。 当评估和采用新服务时，通常需要与这些现有技术集成。  
  
 WCF 安全与现有传输安全模型集成，并可利用现有的基础结构来实现基于 SOAP 消息安全的较新传输安全模型。  
  
### <a name="integration-with-existing-authentication-models"></a>与现有身份验证模型集成  

 任何通信安全模型的一个重要组成部分就是能够识别正在通信的实体并对其进行身份验证。 这些通信中的实体使用“数字标识”或凭据向通信对等方验证自己的身份。 随着分布式通信平台的发展，已实现多种不同的凭据身份验证和相关的安全模型。 例如，在 Internet 中，通常使用用户名和密码标识用户。 在 Intranet 中，使用 Kerberos 域控制器备份用户和服务身份验证变得越来越普遍。 在某些特定情况下，例如两个业务合作伙伴之间，证书可用于对合作伙伴的身份进行相互验证。  
  
 因此，在 Web 服务领域中，同样的服务可向内部企业客户公开，也可向外部合作伙伴或 Internet 客户公开，重要的是基础结构可提供与这些现有安全身份验证模型的集成。 WCF 安全性支持多种凭据类型 (身份验证模型) 包括：  
  
- 匿名调用方。  
  
- 用户名客户端凭据。  
  
- 证书客户端凭据。  
  
- Windows（Kerberos 协议和 NT LanMan [NTLM]）。  
  
### <a name="standards-and-interoperability"></a>标准和互操作性  

 在具有大量现有部署的环境中，很少会出现同质化现象。 分布式计算/通信平台需要与不同供应商提供的技术进行互操作。 同样地，安全性也必须能够互操作。  
  
 为了实现可互操作的安全系统，活跃在 Web 服务行业的公司编写了多种不同的标准。 具体到安全性，它们提出了几个著名的标准：WS-Security：SOAP 消息安全（由 OASIS 标准组织采用，以前称为 WS-Security）、WS-Trust、WS-SecureConversation 和 WS-SecurityPolicy。  
  
 WCF 支持多种互操作性方案。 <xref:System.ServiceModel.BasicHttpBinding> 类可支持基本安全配置文件 (BSP)，而 <xref:System.ServiceModel.WSHttpBinding> 类则支持最新的安全标准，例如 WS-Security 1.1 和 WS-SecureConversation。 通过遵守这些标准，WCF 安全可以与 Microsoft Windows 以外的操作系统和平台上承载的 Web 服务进行互操作和集成。  
  
## <a name="wcf-security-functional-areas"></a>WCF 安全性的功能划分  

 WCF 安全划分为三个功能区域：传输安全、访问控制和审核。 以下几部分简要讨论这些功能并提供指向更多信息的链接。  
  
### <a name="transfer-security"></a>传输安全性  

 传输安全性包含三项主要安全功能：完整性、保密性和身份验证。 *完整性* 是指检测消息是否已被篡改的能力。 *机密性* 是指使任何人（而非预期的收件人）无法读取消息的功能;这是通过加密实现的。 *身份验证* 可以验证声明的标识。 将这三项功能结合在一起，有助于确保消息安全地从一个点到达另一个点。  
  
#### <a name="transport-and-message-security-modes"></a>传输和消息安全模式  

 在 WCF 中，使用两种主要机制来实现传输安全性： *传输* 安全模式和 *消息* 安全模式。  
  
- *传输安全模式* 使用传输级协议（如 HTTPS）来实现传输安全性。 传输模式的优点是可以被广泛采用、可用于多个平台以及计算较为简单。 但是，它的缺点是只能保证点到点的消息安全。  
  
- 另一方面，*消息安全模式* 使用) WS-Security (和其他规范来实现传输安全性。 因为消息安全性直接应用于 SOAP 消息并与应用程序数据一起包含在 SOAP 信封内，它的优点是独立于传输协议、可扩展性更强以及可确保端到端安全性（与点到点相对）；它的缺点是比传输安全性模式慢很多倍，因为它必须处理 SOAP 消息的 XML 特性。  
  
 有关这些差异的详细信息，请参阅 [保护服务和客户端](securing-services-and-clients.md)。  
  
 第三种安全模式同时使用前两种模式，并具备这两种模式的优点。 此模式称为 `TransportWithMessageCredential`。 在此模式下，消息安全性用于对客户端进行身份验证，而传输安全性则用于对服务器进行身份验证并提供消息机密性和完整性。 得益于这一特点，`TransportWithMessageCredential` 安全模式几乎与传输安全模式一样快，并且提供与消息安全性一样的客户端身份验证可扩展性。 但是，与消息安全性模式不同，它不提供完整的端到端安全性。  
  
### <a name="access-control"></a>访问控制  

 *访问控制* 也称为授权。 *授权* 允许不同用户具有不同的权限来查看数据。 例如，因为公司的人力资源文件包含敏感的员工数据，只有管理人员才能查看员工数据。 此外，管理人员只能查看他们的直接报告数据。 在此情况下，访问控制将基于角色（“管理人员”）和管理人员的特定标识（以避免管理人员查看其他管理人员的员工记录）。  
  
 在 WCF 中，通过与公共语言运行时 (CLR) <xref:System.Security.Permissions.PrincipalPermissionAttribute> 和一组称为 " *标识模型*" 的 api 集成来提供访问控制功能。 有关访问控制和基于声明的授权的详细信息，请参阅 [扩展安全性](../extending/extending-security.md)。  
  
### <a name="auditing"></a>审核  

 *审核* 是将安全事件记录到 Windows 事件日志中。 您可以记录与安全相关的事件，例如身份验证失败（或成功）。 有关详细信息，请参阅 [审核](auditing-security-events.md)。 有关编程的详细信息，请参阅 [如何：审核安全事件](how-to-audit-wcf-security-events.md)。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Security.Permissions.PrincipalPermissionAttribute>
- [保证服务的安全](../securing-services.md)
- [常用安全方案](common-security-scenarios.md)
- [绑定与安全](bindings-and-security.md)
- [保护服务和客户端的安全](securing-services-and-clients.md)
- [身份验证](authentication-in-wcf.md)
- [授权](authorization-in-wcf.md)
- [联合令牌与颁发的令牌](federation-and-issued-tokens.md)
- [审核](auditing-security-events.md)
- [安全指导和最佳做法](security-guidance-and-best-practices.md)
- [使用配置文件配置服务](../configuring-services-using-configuration-files.md)
- [系统提供的绑定](../system-provided-bindings.md)
- [终结点创建概述](../endpoint-creation-overview.md)
- [扩展安全性](../extending/extending-security.md)
- [Windows Server App Fabric 的安全模型](/previous-versions/appfabric/ee677202(v=azure.10))
