---
title: 配置系统提供的绑定
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Communication Foundation [WCF], system-provided bindings
- WCF [WCF], system-provided bindings
- bindings [WCF], system-provided
ms.assetid: 443f8d65-f1f2-4311-83b3-4d8fdf7ccf16
ms.openlocfilehash: ab608ba954eafd9035335f00c8755629c5703c22
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96284155"
---
# <a name="configuring-system-provided-bindings"></a>配置系统提供的绑定

绑定可指定在与终结点通话时所使用的通信机制，并指示如何连接到终结点。 绑定由元素组成，这些元素定义了如何对 Windows Communication Foundation (WCF) 通道进行分层以提供所需的通信功能。 绑定包含三种类型的元素：  
  
- 协议通道绑定元素，用于确定要用于发送到终结点的消息的安全性、可靠性、上下文流设置或用户定义的协议。  
  
- 传输通道绑定元素，用于确定在向终结点发送消息时要使用的基础传输协议，例如 TCP 或 HTTP。  
  
- 消息编码绑定元素，用于确定要对发送到终结点的消息使用的网络编码，例如，文本/XML、二进制或消息传输优化机制 (MTOM)。  
  
 本主题介绍了系统提供的所有 Windows Communication Foundation (WCF) 绑定。 如果这些绑定没有一个完全符合应用程序的需求，则可以使用 <xref:System.ServiceModel.Channels.CustomBinding> 类创建一个绑定。 有关创建自定义绑定的详细信息，请参阅[自定义绑定](../extending/custom-bindings.md)。  
  
> [!IMPORTANT]
> 选择启用了安全性的绑定。 默认情况下，除 <xref:System.ServiceModel.BasicHttpBinding> 绑定之外的所有绑定都启用了安全性。 如果不选择安全绑定或禁用了安全性，请确保以某种其他方式保护网络交换，例如在安全的数据中心或独立的网络上进行网络交换。  
  
> [!IMPORTANT]
> 不要将双工协定用于不支持安全性或已禁用安全性的绑定，除非通过其他一些方式来确保网络交换的安全。  
  
## <a name="system-provided-bindings"></a>系统提供的绑定  

 以下绑定随 WCF 一起提供。  
  
|绑定|配置元素|描述|  
|-------------|---------------------------|-----------------|  
|<xref:System.ServiceModel.BasicHttpBinding>|[\<basicHttpBinding>](../../configure-apps/file-schema/wcf/basichttpbinding.md)|一个绑定，适用于与符合 WS-Basic Profile 的 Web 服务（例如基于 ASP.NET Web 服务 (ASMX) 的服务）进行的通信。 此绑定使用 HTTP 作为传输协议，并使用文本/XML 作为默认的消息编码。|  
|<xref:System.ServiceModel.WSHttpBinding>|[\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md)|一个安全且可互操作的绑定，适合于非双工服务约定。|  
|<xref:System.ServiceModel.WS2007HttpBinding>|[\<ws2007HttpBinding>](../../configure-apps/file-schema/wcf/ws2007httpbinding.md)|一个安全且可互操作的绑定，可为 <xref:System.ServiceModel.WSHttpBinding.Security%2A>, <xref:System.ServiceModel.ReliableSession> 的正确版本和 <xref:System.ServiceModel.WSHttpBindingBase.TransactionFlow%2A> 绑定元素提供支持。|  
|<xref:System.ServiceModel.WSDualHttpBinding>|[\<wsDualHttpBinding>](../../configure-apps/file-schema/wcf/wsdualhttpbinding.md)|一个安全且可互操作的绑定，适用于双工服务协定或通过 SOAP 媒介进行的通信。|  
|<xref:System.ServiceModel.WSFederationHttpBinding>|[\<wsFederationHttpBinding>](../../configure-apps/file-schema/wcf/wsfederationhttpbinding.md)|一个安全且可互操作的绑定，支持 WS 联合协议并使联合中的组织可以高效地对用户进行身份验证和授权。|  
|<xref:System.ServiceModel.WS2007FederationHttpBinding>|[\<ws2007FederationHttpBinding>](../../configure-apps/file-schema/wcf/ws2007federationhttpbinding.md)|一个安全且可互操作的绑定，它派生自 <xref:System.ServiceModel.WS2007HttpBinding>并支持联合安全性。|  
|<xref:System.ServiceModel.NetTcpBinding>|[\<netTcpBinding>](../../configure-apps/file-schema/wcf/nettcpbinding.md)|一个安全且经过优化的绑定，适用于 WCF 应用程序之间跨计算机的通信。|  
|<xref:System.ServiceModel.NetNamedPipeBinding>|[\<netNamedPipeBinding>](../../configure-apps/file-schema/wcf/netnamedpipebinding.md)|一个安全、可靠且经过优化的绑定，适用于 WCF 应用程序之间计算机上的通信。|  
|<xref:System.ServiceModel.NetMsmqBinding>|[\<netMsmqBinding>](../../configure-apps/file-schema/wcf/netmsmqbinding.md)|一个排队绑定，适用于 WCF 应用程序之间的计算机间的通信。|  
|<xref:System.ServiceModel.NetPeerTcpBinding>|[\<netPeerTcpBinding>](../../configure-apps/file-schema/wcf/netpeertcpbinding.md)|一个支持多计算机安全通信的绑定。|  
|<xref:System.ServiceModel.WebHttpBinding>|[\<webHttpBinding>](../../configure-apps/file-schema/wcf/webhttpbinding.md)|一个绑定，可用于为通过 HTTP 请求（而不是 SOAP 消息）公开的 WCF Web 服务配置终结点。|  
|<xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding>|[\<msmqIntegrationBinding>](../../configure-apps/file-schema/wcf/msmqintegrationbinding.md)|适用于 WCF 应用程序与现有消息队列之间的跨计算机通信的绑定 (也称为 MSMQ) 应用程序。|  
  
## <a name="binding-features"></a>绑定功能  

 下表显示了每个系统提供的绑定所提供的一些主要功能。 第一列中列出了这些绑定，并在表中描述有关功能的信息。 下表提供所用绑定缩写的概要。 若要选择绑定，应确定哪列能满足所需的所有行中的功能。  
  
|绑定|互操作性|安全模式（默认）|会话<br /><br /> （默认值）|事务|双工|  
|-------------|----------------------|----------------------------------|-----------------------------|------------------|------------|  
|<xref:System.ServiceModel.BasicHttpBinding>|Basic Profile 1.1（基本配置文件 1.1）|（无）、传输、消息、混合|无、（无）|（无）|不适用|  
|<xref:System.ServiceModel.WSHttpBinding>|WS|无、传输、（消息）、混合|（无）、传输、可靠会话|（无）、是|不适用|  
|<xref:System.ServiceModel.WS2007HttpBinding>|WS-Security、WS-Trust、WS-SecureConversation、WS-SecurityPolicy|无、传输、（消息）、混合|（无）、传输、可靠会话|（无）、是|不适用|  
|<xref:System.ServiceModel.WSDualHttpBinding>|WS|无、（消息）|（可靠会话）|（无）、是|是|  
|<xref:System.ServiceModel.WSFederationHttpBinding>|WS-Federation|无、（消息）、混合|（无）、可靠会话|（无）、是|否|  
|<xref:System.ServiceModel.WS2007FederationHttpBinding>|WS-Federation|无、（消息）、混合|（无）、可靠会话|（无）、是|否|  
|<xref:System.ServiceModel.NetTcpBinding>|.NET|无、（传输）、消息、<br /><br /> Mixed|可靠对话、（传输）|（无）、是|是|  
|<xref:System.ServiceModel.NetNamedPipeBinding>|.NET|无、<br /><br /> （传输）|无、（传输）|（无）、是|是|  
|<xref:System.ServiceModel.NetMsmqBinding>|.NET|无、消息、（传输）、两者|（无）|（无）、是|否|  
|<xref:System.ServiceModel.NetPeerTcpBinding>|对等|无、消息、（传输）、混合|（无）|（无）|是|  
|<xref:System.ServiceModel.WebHttpBinding>|.Net|无、传输、TransportCredentialOnly|（无）|（无）|不适用|  
|<xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding>|MSMQ|无、（传输）|（无）|（无）、是|不适用|  
  
 下表解释上一个表中的功能。  
  
|功能|说明|  
|-------------|-----------------|  
|互操作性类型|指定绑定用来确保互操作的协议或技术。|  
|安全性|指定如何保护通道：<br /><br /> -None：不保护 SOAP 消息，并且不会对客户端进行身份验证。<br />-Transport：在传输层满足安全要求。<br />-Message：在消息层满足安全要求。<br />-Mixed：此安全模式称为 `TransportWithMessageCredentials` 。 此模式在消息级别上处理凭据，并由传输层来满足完整性和保密性需求。<br />-两者：同时使用消息级别和传输级别安全。 此功能仅可用于 <xref:System.ServiceModel.NetMsmqBinding>。|  
|会话|指定此绑定是否支持会话协定。|  
|事务|指定是否启用事务。|  
|双工|指定是否支持双工协定。 注意，此功能要求在绑定中支持会话。|  
|流式处理|指定是否支持消息流处理。|  
  
## <a name="see-also"></a>另请参阅

- [终结点创建概述](../endpoint-creation-overview.md)
- [使用绑定配置服务和客户端](../using-bindings-to-configure-services-and-clients.md)
- [基本 WCF 编程](../basic-wcf-programming.md)
