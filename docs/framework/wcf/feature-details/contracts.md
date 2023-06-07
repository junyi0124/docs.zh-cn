---
title: 协定
ms.date: 03/30/2017
helpviewer_keywords:
- WCF [WCF], contracts
- contracts [WCF]
- Windows Communication Foundation [WCF], contracts
ms.assetid: c8364183-4ac1-480b-804a-c5e6c59a5d7d
ms.openlocfilehash: b51bbd1a8a9bfc8963cee429dab41fdf9b4f594c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96286754"
---
# <a name="contracts"></a>协定

本部分演示如何定义和实现 Windows Communication Foundation (WCF) 约定。 服务协定指定终结点与外界通信的内容。 更具体地说，它是有关一组特定消息的声明，这些消息被组织成基本消息交换模式 (MEP)，如请求/答复、单向和双工。 如果说服务协定是一组在逻辑上相关的消息交换，那么服务操作就是单个消息交换。 例如，`Hello` 操作显然必须接受一条消息（以便调用方能够发出问候），并可能返回也可能不返回一条消息（具体取决于操作的礼节性）。  
  
 有关协定和其他核心 WCF 概念的详细信息，请参阅 [基本 Windows Communication Foundation 概念](../fundamental-concepts.md)。 本主题重点介绍对服务协定的理解。 有关如何生成使用服务协定连接到服务的客户端的详细信息，请参阅 [WCF 客户端概述](../wcf-client-overview.md)。 有关客户端通道、客户端体系结构和其他客户端问题的详细信息，请参阅 [客户](clients.md)端。  
  
## <a name="overview"></a>概述  

 本主题提供了设计和实现 WCF 服务的高级概念。 各个子主题提供了有关具体设计和实现细节的更多详细信息。 在设计和实现您的 WCF 应用程序之前，建议您执行以下操作：  
  
- 了解什么是服务协定、服务协定的工作原理以及如何创建服务协定。  
  
- 了解运行时配置或承载环境可能不支持的协定状态最低要求。  
  
## <a name="service-contracts"></a>服务协定  

 服务协定是一个声明，它提供了有关以下方面的信息：  
  
- 服务中操作的分组方式。  
  
- 针对交换的消息所进行的各种操作的签名。  
  
- 这些消息的数据类型。  
  
- 操作的位置。  
  
- 用于支持与服务成功通信的特定协议和序列化格式。  
  
 例如，采购订单协定可能具有一个 `CreateOrder` 操作，该操作接受订单信息类型输入并返回成功或失败信息，包括一个订单标识符。 它还可能具有一个 `GetOrderStatus` 操作，该操作接受一个订单标识符并返回订单状态信息。 此类服务协定需要指定：  
  
- 采购订单协定由 `CreateOrder` 和 `GetOrderStatus` 操作组成。  
  
- 这些操作指定了输入消息和输出消息。  
  
- 这些消息可以携带的数据。  
  
- 有关成功处理消息所必需的通信基础结构的分类声明。 例如，这些详细信息包括建立成功通信是否需要安全以及需要哪些形式的安全。  
  
 为了将此类信息传递给其他平台上的应用程序 (包括非 Microsoft 平台) ，XML 服务协定以标准 XML 格式公开表示，例如 [Web Services 描述语言 (WSDL) ](https://www.w3.org/TR/2001/NOTE-wsdl-20010315) 和 [XML 架构 (XSD) ](https://www.w3.org/XML/Schema)等。 许多平台的开发人员都可以使用此公共协定信息创建可与该服务通信的应用程序，既因为开发人员理解规范的语言，又因为这些语言通过描述服务支持的公共形式、格式和协议，支持互操作。 有关 WCF 如何处理此类信息的详细信息，请参阅 [元数据](metadata.md)。  
  
 然而，协定可以用多种方式表示，并且尽管 WSDL 和 XSD 语言非常适合以可理解的方式描述服务，但这些语言很难直接使用 — 无论如何，这些语言只能描述服务，而不能描述服务协定实现。 因此，WCF 应用程序使用托管属性、接口和类来定义和实现服务的结构。  
  
 如果客户端或其他服务实施者需要，可以将在托管类型中定义的生成协定 (称为元数据（WSDL 和 XSD）转换为 *导出*) ，尤其是在其他平台上。 结果可以得到一个简单的编程模型，可以使用公共元数据向任何客户端应用程序描述该模型。 基础 SOAP 消息的详细信息（例如，传输和安全相关信息）可以留给 WCF，这会自动执行到服务协定类型系统和 XML 类型系统之间的必要转换。  
  
 有关设计协定的详细信息，请参阅 [设计服务协定](../designing-service-contracts.md)。 有关实现协定的详细信息，请参阅 [实现服务协定](../implementing-service-contracts.md)。  
  
 此外，WCF 还提供了完全在消息级开发服务协定的功能。 有关在消息级别开发服务协定的详细信息，请参阅 [使用消息协定](using-message-contracts.md)。 有关在非 SOAP XML 中开发服务的详细信息，请参阅 [与 POX 应用程序的互操作性](interoperability-with-pox-applications.md)。  
  
### <a name="understanding-the-hierarchy-of-requirements"></a>了解需求的层次结构  

 服务协定对操作进行分组；指定消息交换模式 (MEP)、消息类型和这些消息携带的数据类型；并指示某个实现为了支持协定而必须具有的运行时行为的类别（例如，它可能要求对消息进行加密和签名）。 但是，服务协定本身并未明确指定如何满足这些需求，而只是指定这些需求必须得到满足。 加密的类型或消息的签名方式取决于满足这些要求的服务的实现和配置。  
  
 请注意协定为添加行为而对服务协定实现和运行时配置提出特定要求的方式。 为了公开某一服务以供使用而必须满足的这组要求建立于前一组要求之上。 如果协定对实现提出要求，那么实现可能会对配置和绑定提出更多的要求，以便使服务能够运行。 最后，主机应用程序还必须支持服务配置和绑定所添加的任何需求。  
  
 在设计、实现、配置和承载 Windows Communication Foundation (WCF) 服务应用程序时，这一加法要求过程非常重要。 例如，协定可能会指定需要支持某一会话。 如果是这样，您必须配置绑定以支持该协定性需求，否则服务实现将无法正常工作。 或者，如果您的服务要求集成 Windows 身份验证并在 Internet Information Services (IIS) 中承载，则服务所在的 Web 应用程序必须启用集成 Windows 身份验证并禁用匿名支持。 有关不同服务主机应用程序类型的功能和影响的详细信息，请参阅 [托管](hosting.md)。  
  
## <a name="see-also"></a>另请参阅

- [终结点：地址、绑定和协定](endpoints-addresses-bindings-and-contracts.md)
- [设计服务协定](../designing-service-contracts.md)
- [实现服务协定](../implementing-service-contracts.md)
