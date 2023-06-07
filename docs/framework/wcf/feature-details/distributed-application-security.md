---
title: 分布式应用程序安全
ms.date: 03/30/2017
helpviewer_keywords:
- distributed application security [WCF]
- security [WCF], transfer
ms.assetid: 53928a10-e474-46d0-ab90-5f98f8d7b668
ms.openlocfilehash: 80878857145c7a4e09106b6e1c4cb9ad68b7680a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96254241"
---
# <a name="distributed-application-security"></a>分布式应用程序安全

Windows Communication Foundation (WCF) 安全性分为三个主要功能区域：传输安全、访问控制和审核。 传输安全提供完整性、保密性和身份验证。 传输安全由传送安全、消息安全或 `TransportWithMessageCredential` 实现。  
  
 有关 WCF 消息安全的概述，请参阅 [安全性概述](security-overview.md)。 有关 WCF 安全的其他两个部分的详细信息，请参阅 [授权](authorization-in-wcf.md) 和 [审核](auditing-security-events.md)。  
  
## <a name="transfer-security-scenarios"></a>传输安全方案  

 采用 WCF 传输安全性的常见方案包括：  
  
- 使用 Windows 确保传输安全。 WCF 客户端和服务部署在 Windows 域中 (或 Windows 林中) 。 消息包含个人数据，因此要求客户端和服务相互进行身份验证，要求实现消息完整性和消息保密性。 此外，还需要有已发生特定事务的证明，例如，消息的接收方应记录签名信息。  
  
- 使用 `UserName` 和 HTTPS 确保传输安全。 需要开发 WCF 客户端和服务才能在 Internet 上工作。 客户端凭据根据数据库（其中的内容为用户名/密码对）进行身份验证。 服务是用受信任的安全套接字层 (SSL) 证书部署在一个 HTTPS 地址的。 由于消息是通过 Internet 传输的，因此，客户端和服务需要相互进行身份验证，并且必须在传输过程中保持消息的保密性和完整性。  
  
- 使用证书确保传输安全。 需要开发 WCF 客户端和服务才能在公共 internet 上工作。 客户端和服务都具有可用于确保消息安全的证书。 客户端和服务通过 Internet 进行相互通信，执行要求消息完整性、保密性和相互身份验证的重要事务。  
  
## <a name="integrity-confidentiality-and-authentication"></a>完整性、保密性和身份验证  

 三项功能 — 完整性、保密性和身份验证 — 合称为传输安全。 传输安全提供的这些功能，有助于减轻对分布式应用程序的威胁。 下表简要介绍构成传输安全的三项功能。  
  
|函数|描述|  
|--------------|-----------------|  
|完整性|*完整性* 是指数据是完整且准确的，尤其是在从一个点到另一个点并可能被多个参与者读取后。 完整性必须得到维护，以免数据被篡改，这通常是由消息的数字签名实现的。|  
|机密性|*机密性* 是指确保消息尚未被预期读者之外的任何人读取。 例如，信用卡号在通过 Internet 发送时必须保密。 保密性通常是用公钥/私钥方案进行数据加密实现的。|  
|身份验证|*身份验证* 是验证声明的标识。 例如，当使用银行帐户时，必须只允许帐户的实际所有者取款。 有多种方式可以实现身份验证。 一个常用方法是使用用户/密码系统。 第二个方法是使用第三方提供的 X.509 证书。|  
  
## <a name="security-modes"></a>安全模式  

 WCF 具有几种传输安全性模式，下表对此进行了说明。  
  
|“模式”|说明|  
|----------|-----------------|  
|无|传输层和消息层都不提供安全措施。 默认情况下，任何预定义绑定都不会使用此模式（元素除外），也不会使用 [\<basicHttpBinding>](../../configure-apps/file-schema/wcf/basichttpbinding.md) <xref:System.ServiceModel.BasicHttpBinding> 类。|  
|Transport|使用安全传送（如 HTTPS）实现完整性、保密性和相互身份验证。|  
|消息|使用 SOAP 消息安全实现完整性、保密性和相互身份验证。 SOAP 消息是按照 WS-Security 标准获得保护的。|  
|混合模式|使用传送安全实现完整性、保密性和服务器身份验证。 使用消息安全（WS-Security 和其他标准）实现客户端身份验证。<br /><br /> （此模式的枚举值是 `TransportWithMessageCredential`。）|  
|推送、请求和匿名|在传送级别和消息级别都执行保护和身份验证。 此模式仅在元素中可用 [\<netMsmqBinding>](../../configure-apps/file-schema/wcf/netmsmqbinding.md) 。|  
  
## <a name="credentials-and-transfer-security"></a>凭据和传输安全  

 *凭据* 是用于建立声明的标识或功能的数据。 提交凭据涉及出示数据以及数据的所有权证明。 WCF 在传输和消息安全级别支持多种凭据类型。 可以为 WCF 绑定指定凭据类型。  
  
 在许多国家和地区，驾驶执照就是凭据的一个示例。 该执照中包含表示个人身份和能力的数据。 它以持有人照片的形式包含所有权证明。 该执照由受信任的颁发机构颁发，通常是获得许可的政府部门。 该执照采用密封形式且包含一张全息图，表明它未经改动或伪造。  
  
 作为示例，请考虑 WCF 中支持的两种凭据类型：用户名和 (x.509) 证书凭据。  
  
 对于用户名凭据，用户名表示已声明标识，密码表示所有权证明。 这种情况下，受信任的颁发机构则是验证用户名和密码的系统。  
  
 在证书凭据中，主题名称、主题备用名称或证书中的特定字段可用于表示已声明标识和/或功能。 凭据中的数据所有权证明的建立，是用关联私钥生成签名实现的。  
  
 有关如何对传输安全性进行编程和指定凭据的详细信息，请参阅 [绑定和安全](bindings-and-security.md) 和 [安全行为](security-behaviors-in-wcf.md)。  
  
### <a name="transport-client-credential-types"></a>传送客户端凭据类型  

 下表列出了在创建使用传输安全的应用程序时可能使用的值。 在代码或绑定设置中，可以使用这些值。  
  
|设置|描述|  
|-------------|-----------------|  
|无|指定客户端不需要提供任何凭据。 这相当于匿名客户端。|  
|基本|指定基本身份验证。 有关详细信息，请参阅 RFC2617 "[HTTP Authentication：基本和摘要式身份验证](http://schemas.xmlsoap.org/ws/2004/10/discovery/ws-discovery.pdf)"。|  
|摘要|指定摘要式身份验证。 有关详细信息，请参阅 RFC2617 "[HTTP Authentication：基本和摘要式身份验证](http://schemas.xmlsoap.org/ws/2004/10/discovery/ws-discovery.pdf)"。|  
|Ntlm|指定在 Windows 域中使用 SSPI 协商进行 Windows 身份验证。<br /><br /> 要使用 SSPI 协商，就需要使用 Kerberos 协议或 NT LanMan (NTLM)。|  
|Windows|指定在 Windows 域中使用 SSPI 进行 Windows 身份验证。 SSPI 选择 Kerberos 协议或 NTLM 作为身份验证服务。<br /><br /> SSPI 首先尝试 Kerberos 协议；如果失败，则使用 NTLM。|  
|证书|使用证书（通常是 X.509）执行客户端身份验证。|  
  
### <a name="message-client-credential-types"></a>消息客户端凭据类型  

 下表列出了在创建使用消息安全的应用程序时可能使用的值。 在代码或绑定设置中，可以使用这些值。  
  
|设置|描述|  
|-------------|-----------------|  
|无|允许服务与匿名客户端交互。|  
|Windows|允许在 Windows 凭据的已通过身份验证的上下文中执行 SOAP 消息交换。 使用 SSPI 协商机制选择 Kerberos 协议或 NTLM 作为身份验证服务。|  
|用户名|允许服务可以要求使用用户名凭据对客户端进行身份验证。 请注意，WCF 不允许使用用户名进行任何加密操作，例如生成签名或加密数据。 因此，在使用用户名凭据时，WCF 会强制保护传输。|  
|证书|允许服务要求使用证书对客户端进行身份验证。|  
|CardSpace|允许服务要求使用 CardSpace 对客户端进行身份验证。|  
  
### <a name="programming-credentials"></a>凭据编程  

 对于每个客户端凭据类型，WCF 编程模型允许您使用服务行为和通道行为来指定凭据值和凭据验证程序。  
  
 WCF 安全性具有两种类型的凭据：服务凭据行为和通道凭据行为。 WCF 中的凭据行为指定实际数据，即用于满足通过绑定表示的安全要求的凭据。 在 WCF 中，客户端类是在操作调用和消息之间进行转换的运行时组件。 所有客户端都是从 <xref:System.ServiceModel.ClientBase%601> 类继承的。 通过基类的 <xref:System.ServiceModel.ClientBase%601.ClientCredentials%2A> 属性，可以指定不同的客户端凭据值。  
  
 在 WCF 中，服务行为是应用于实现服务协定的类的特性 (接口) 以编程方式控制服务。 使用 <xref:System.ServiceModel.Description.ServiceCredentials> 类，可以为服务凭据指定证书，也可以为不同的客户端凭据类型指定客户端验证设置。  
  
### <a name="negotiation-model-for-message-security"></a>消息安全的协商模型  

 在消息安全模式中，通过执行传输安全，可以在客户端带外配置服务凭据。 例如，如果使用的是存储在 Windows 证书存储区中的证书，则必须使用一个工具，如 Microsoft 管理控制台 (MMC) 单元。  
  
 在消息安全模式中，通过执行传输安全，还可以与客户端交换服务凭据，作为初始协商的一部分。 若要启用协商，请将 <xref:System.ServiceModel.MessageSecurityOverHttp.NegotiateServiceCredential%2A> 属性设置为 `true`。  
  
## <a name="see-also"></a>另请参阅

- [终结点创建概述](../endpoint-creation-overview.md)
- [系统提供的绑定](../system-provided-bindings.md)
- [安全性概述](security-overview.md)
- [Windows Server App Fabric 的安全模型](/previous-versions/appfabric/ee677202(v=azure.10))
