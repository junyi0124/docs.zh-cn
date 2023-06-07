---
title: 系统提供的绑定
description: 了解系统提供的所有 Windows Communication Foundation (WCF) 绑定。
ms.date: 06/05/2018
helpviewer_keywords:
- bindings [WCF], system-provided
ms.assetid: 2c243746-45ce-4588-995e-c17126a579a6
ms.openlocfilehash: 3c6c6b628d208aede8c547dcfa66fc189a26ae01
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "61791503"
---
# <a name="system-provided-bindings"></a>系统提供的绑定

绑定可指定在与终结点通话时所使用的通信机制，并指示如何连接到终结点。 绑定包含以下元素：

- 协议堆栈确定用于发送到终结点的消息的安全性、可靠性和上下文流设置。

- 传输确定将消息发送到终结点时使用的基础传输协议，例如 TCP 或 HTTP。

- 编码确定用于发送到终结点的消息的连网编码。 例如，文本/XML、二进制或消息传输优化机制 (MTOM)。

 本文介绍了系统提供的所有 Windows Communication Foundation (WCF) 绑定。 如果这些绑定都不符合应用程序的确切标准，则可创建自定义绑定。 有关创建自定义绑定的详细信息，请参阅[自定义绑定](./extending/custom-bindings.md)。

 通过支持 WS-Federation 协议的安全的、可互操作的绑定，联盟中的组织可以高效地对用户进行身份验证和授权。

> [!IMPORTANT]
> 始终选择包括安全性的绑定。 默认情况下，除 [\<basicHttpBinding>](../configure-apps/file-schema/wcf/basichttpbinding.md) 元素外的所有绑定都已启用安全性。 如果不选择安全绑定或禁用安全性，请务必以某种其他方式保护数据，如将其存储在受保护的数据中心中或隔离网络上。

> [!IMPORTANT]
> 除非以某种其他方式保护数据，否则决不要将双工协定与不支持安全性或已禁用安全性的绑定一起使用。

WCF 附带有以下绑定：

|绑定|配置元素|描述|
|-------------|---------------------------|-----------------|
|<xref:System.ServiceModel.BasicHttpBinding>|[\<basicHttpBinding>](../configure-apps/file-schema/wcf/basichttpbinding.md)|一个绑定，适用于与符合 WS-Basic Profile 的 Web 服务（例如基于 ASP.NET Web 服务 (ASMX) 的服务）进行的通信。 此绑定使用 HTTP 作为传输协议，并使用文本/XML 作为默认的消息编码。|
|<xref:System.ServiceModel.WSHttpBinding>|[\<wsHttpBinding>](../configure-apps/file-schema/wcf/wshttpbinding.md)|一个安全且可互操作的绑定，适合于非双工服务约定。|
|<xref:System.ServiceModel.WSDualHttpBinding>|[\<wsDualHttpBinding>](../configure-apps/file-schema/wcf/wsdualhttpbinding.md)|一个安全且可互操作的绑定，适用于双工服务协定或通过 SOAP 媒介进行的通信。|
|<xref:System.ServiceModel.WSFederationHttpBinding>|[\<wsFederationHttpBinding>](../configure-apps/file-schema/wcf/wsfederationhttpbinding.md)|一个支持 WS 联合身份验证协议的安全的、可互操作的绑定，使联盟中的组织可以高效地对用户进行身份验证和授权。|
|<xref:System.ServiceModel.NetHttpBinding>|[\<netHttpBinding>](../configure-apps/file-schema/wcf/nethttpbinding.md)|为使用 HTTP 或 WebSocket 服务而设计且默认情况下使用二进制编码的绑定。|
|<xref:System.ServiceModel.NetHttpsBinding>|[\<netHttpsBinding>](../configure-apps/file-schema/wcf/nethttpsbinding.md)|为使用 HTTP 或 WebSocket 服务而设计且默认情况下使用二进制编码的安全绑定。|
|<xref:System.ServiceModel.NetTcpBinding>|[\<netTcpBinding>](../configure-apps/file-schema/wcf/nettcpbinding.md)|一个安全且经过优化的绑定，适用于 WCF 应用程序之间跨计算机的通信。|
|<xref:System.ServiceModel.NetNamedPipeBinding>|[\<netNamedPipeBinding>](../configure-apps/file-schema/wcf/netnamedpipebinding.md)|一个安全、可靠且经过优化的绑定，适用于 WCF 应用程序之间计算机上的通信。|
|<xref:System.ServiceModel.NetMsmqBinding>|[\<netMsmqBinding>](../configure-apps/file-schema/wcf/netmsmqbinding.md)|一个排队绑定，适用于 WCF 应用程序之间的计算机间的通信。|
|<xref:System.ServiceModel.NetPeerTcpBinding>|[\<netPeerTcpBinding>](../configure-apps/file-schema/wcf/netpeertcpbinding.md)|一个支持多计算机安全通信的绑定。|
|<xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding>|[\<msmqIntegrationBinding>](../configure-apps/file-schema/wcf/msmqintegrationbinding.md)|一个适合于 WCF 应用程序和现有消息队列应用程序之间的跨计算机通信的绑定。|
|<xref:System.ServiceModel.BasicHttpContextBinding>|[\<basicHttpContextBinding>](../configure-apps/file-schema/wcf/basichttpcontextbinding.md)|一个绑定，适用于与符合 WS-Basic Profile 且允许使用 HTTP Cookie 交换上下文的 Web 服务进行的通信。|
|<xref:System.ServiceModel.NetTcpContextBinding>|[\<netTcpContextBinding>](../configure-apps/file-schema/wcf/nettcpcontextbinding.md)|一个安全且经过优化的绑定，适用于允许使用 SOAP 标头交换上下文的 WCF 应用程序之间跨计算机的通信。|
|<xref:System.ServiceModel.WebHttpBinding>|[\<webHttpBinding>](../configure-apps/file-schema/wcf/webhttpbinding.md)|一个绑定，可用于为通过 HTTP 请求（而不是 SOAP 消息）公开的 WCF Web 服务配置终结点。|
|<xref:System.ServiceModel.WSHttpContextBinding>|[\<wsHttpContextBinding>](../configure-apps/file-schema/wcf/wshttpcontextbinding.md)|一个安全且可互操作的绑定，适用于允许使用 SOAP 标头交换上下文的非双工服务协定。|
|<xref:System.ServiceModel.UdpBinding>|[\<udpBinding>](../configure-apps/file-schema/wcf/udpbinding.md)|同时向大量客户端发送突然爆发出的简单消息时使用的绑定。|

 下表显示系统提供的每个绑定的功能。 在表列中可找到绑定；在行中列出了功能，在第二个表中描述了这些功能。 下表提供所用绑定缩写的概要。 若要选择绑定，应确定哪列能满足所需的所有行中的功能。

|绑定|互操作性|安全性（默认）|会话<br />(默认)|事务|双工|编码（默认）|流式处理<br />(默认)|
|-------------|----------------------|--------------------------|-----------------------------|------------------|------------|--------------------------|-------------------------------|
|<xref:System.ServiceModel.BasicHttpBinding>|Basic Profile 1.1（基本配置文件 1.1）|（无）、传输、消息、混合|（无）|（无）|n/a|文本、(MTOM)|是<br />（缓冲式）|
|<xref:System.ServiceModel.WSHttpBinding>|WS|传输、（消息）、混合|（无）、可靠会话、安全会话|（无）、是|n/a|（文本）、MTOM|否|
|<xref:System.ServiceModel.WSDualHttpBinding>|WS|（消息）、无|（可靠会话）、安全会话|（无）、是|是|（文本）、MTOM|否|
|<xref:System.ServiceModel.WSFederationHttpBinding>|WS-Federation|（消息）、混合、无|（无）、可靠会话、安全会话|（无）、是|否|（文本）、MTOM|否|
|<xref:System.ServiceModel.NetHttpBinding>|.NET|（无）、传输、消息、TransportWithMessageCredential、TransportCredentialOnly|请参见下面的注释|None|请参见下面的注释|（二进制）、文本、MTOM|是（缓冲式）|
|<xref:System.ServiceModel.NetHttpsBinding>|.NET|（传输）、TransportWithMessageCredential|请参见下面的注释|None|请参见下面的注释|（二进制）、文本、MTOM|是<br />（缓冲式）|
|<xref:System.ServiceModel.NetTcpBinding>|.NET|（传输）、消息、无、混合|（传输）、可靠会话、安全会话|（无）、是|是|二进制|是<br />（缓冲式）|
|<xref:System.ServiceModel.NetNamedPipeBinding>|.NET|（传输）、无|无、（传输）|（无）、是|是|二进制|是<br />（缓冲式）|
|<xref:System.ServiceModel.NetMsmqBinding>|.NET|消息、（传输）、无|（无）、传输|无、（是）|否|二进制|否|
|<xref:System.ServiceModel.NetPeerTcpBinding>|对等|（传输）|（无）|（无）|是||否|
|<xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding>|MSMQ|（传输）|（无）|无、（是）|n/a|n/a|否|
|<xref:System.ServiceModel.BasicHttpContextBinding>|Basic Profile 1.1（基本配置文件 1.1）|（无）、传输、消息、混合|（无）|（无）|n/a|文本、(MTOM)|是<br />（缓冲式）|
|<xref:System.ServiceModel.NetTcpContextBinding>|.NET|（传输）、消息、无、混合|（传输）、可靠会话、安全会话|（无）、是|是|二进制|是<br />（缓冲式）|
|<xref:System.ServiceModel.WSHttpContextBinding>|WS|传输、（消息）、混合|（无）、可靠会话、安全会话|（无）、是|n/a|文本、(MTOM)|否|
|<xref:System.ServiceModel.UdpBinding> <br /><br /> **注意：** 可以通过实现此绑定所实现的标准 SOAP-over-UDP（UDP 上的 SOAP）规范来获得互操作性。|.NET|（无）|（无）|（无）|n/a|（文本）|否|

> [!IMPORTANT]
> <xref:System.ServiceModel.NetHttpBinding> 是为使用 HTTP 或 WebSocket 服务设计的绑定，默认情况下使用二进制编码。 <xref:System.ServiceModel.NetHttpBinding> 将检测它是否与请求-答复协定或双工协定结合使用，并更改其行为以进行匹配 - 它将针对请求-答复协定使用 HTTP，并针对双工协定使用 WebSocket。 可以使用重写此行为<xref:System.ServiceModel.Channels.WebSocketTransportUsage>绑定设置：双工时 - 这是默认值，其行为如前所述。 从不 - 阻止使用 WebSocket。 尝试使用具有此设置的双工协定将导致异常。 始终 - 甚至对于请求-答复协定，也会强制使用 WebSocket。 NetHttpBinding 支持 HTTP 模式和 WebSocket 模式下的可靠会话。 在 WebSocket 模式下，会话由传输来提供。

 下表对上表列出的功能进行说明。

|功能|描述|
|-------------|-----------------|
|互操作性类型|指定绑定用来确保互操作的协议或技术。|
|安全性|指定如何保护通道：<br />-None:SOAP 消息的不安全和客户端未经身份验证。<br />-传输：在传输层上满足安全要求。<br />-消息：在消息层上满足安全要求。<br />混合：声明包含在 message;传输层满足完整性和保密性需求。|
|会话|指定此绑定是否支持会话协定。|
|事务|指定是否启用事务。|
|双工|指定是否支持双工协定。 请注意，此功能要求支持绑定中的会话。|
|编码|指定消息的网络格式。 允许的值包括：<br />- 文本：例如 UTF-8。<br />- 二进制<br />消息传输优化机制 (MTOM):一种方法的高效编码的 SOAP 信封上下文中的二进制 XML 元素。|
|流式处理|指定传入和传出消息是否支持流。 使用绑定上的 `TransferMode` 属性可设置值。 允许的值包括：<br />- <xref:System.ServiceModel.TransferMode.Buffered>：对请求和响应消息进行缓冲处理。<br />- <xref:System.ServiceModel.TransferMode.Streamed>：对请求和响应消息进行流式处理。<br />- <xref:System.ServiceModel.TransferMode.StreamedRequest>：对请求消息进行流式处理，对响应消息进行缓冲处理。<br />- <xref:System.ServiceModel.TransferMode.StreamedResponse>：对请求消息进行缓冲处理，对响应消息进行流式处理。|

## <a name="see-also"></a>请参阅

- [终结点创建概述](endpoint-creation-overview.md)
- [使用绑定配置服务和客户端](using-bindings-to-configure-services-and-clients.md)
- [基本 WCF 编程](basic-wcf-programming.md)
