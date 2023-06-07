---
title: 什么是 Windows Communication Foundation
description: 了解 Windows Communication Foundation，它是用于生成面向服务的应用程序的框架。
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Communication Foundation [WCF], technology overview
- technology overview [WCF]
- WCF [WCF], technology overview
ms.assetid: 40e1009d-ef15-450b-9848-62eabe5e5738
ms.openlocfilehash: b63ce62cd39ef7961db1c0ac86c8cbd6ca871dbd
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96261886"
---
# <a name="what-is-windows-communication-foundation"></a>什么是 Windows Communication Foundation

Windows Communication Foundation (WCF) 是一个框架，用于生成面向服务的应用程序。 使用 WCF，可以将数据作为异步消息从一个服务终结点发送到另一个服务终结点。 服务终结点可以是由 IIS 承载的持续可用的服务的一部分，也可以是应用程序中承载的服务。 终结点可以是从服务终结点请求数据的服务客户端。 简单消息可以是作为 XML 发送的单个字符或单个单词，复杂消息可以是二进制数据流。 一些示例方案包括：

- 处理企业事务的安全服务。

- 将当前数据提供给其他服务（例如流量报告或其他监视服务）的服务。

- 使两个人能够实时通信或交换数据的聊天服务。

- 轮询一个或多个服务以查找数据并将其以逻辑表现形式展示出来的面板应用程序。

- 将使用 Windows Workflow Foundation 实现的工作流作为 WCF 服务公开。

- 轮询服务以查找最新数据源的 Silverlight 应用程序。

虽然在存在 WCF 之前可以创建此类应用程序，但 WCF 使终结点的开发比以往更容易。 总而言之，WCF 旨在提供一种可管理的方法，可用于创建 Web 服务和 Web 服务客户端。

## <a name="features-of-wcf"></a>WCF 的功能

WCF 包括以下功能集。 有关详细信息，请参阅 [WCF 功能详细信息](./feature-details/index.md)。

- **服务导向**

     使用 WS 标准的一个结果是 WCF 使你能够创建 *面向服务* 的应用程序。 面向服务的体系结构 (SOA) 依赖 Web 服务发送和接收数据。 这些服务具有松耦合的常规优点，而不是从一个应用程序到另一个应用程序进行硬编码。 松耦合关系意味着只要符合基本协定，则在任何平台上创建的任何客户端均可连接到所有服务。

- **互操作性**

     WCF 实现了用于 Web 服务互操作性的现代行业标准。 有关支持的标准的详细信息，请参阅 [互操作性和集成](./feature-details/interoperability-and-integration.md)。

- **多种消息模式**

     采用多种模式之一交换消息。 最常用的模式是请求/答复模式，其中一个终结点从另一个终结点请求数据， 另一个终结点进行答复。 还有其他模式，比如单向消息，其中只有一个终结点发送消息，而不期望得到答复。 更复杂的模式是双工交换模式，在该模式下，两个终结点建立连接并来回发送数据，类似于即时消息传递程序。 有关如何使用 WCF 实现不同消息交换模式的详细信息，请参阅 [约定](./feature-details/contracts.md)。

- **服务元数据**

     WCF 支持使用行业标准（如 WSDL、XML 架构和 WS 策略）中指定的格式发布服务元数据。 此元数据可用于自动生成和配置用于访问 WCF 服务的客户端。 可通过 HTTP 和 HTTPS 来发布元数据，也可使用 Web 服务元数据交换标准来发布元数据。 有关详细信息，请参阅[元数据](./feature-details/metadata.md)。

- **数据协定**

     因为 WCF 是使用 .NET Framework 生成的，所以它还包含提供要强制执行的协定的代码友好方法。 数据协定就是其中一种通用类型的协定。 实质上，当您使用 Visual C# 或 Visual Basic 对服务进行编码时，处理数据的最简单方法是使用属于数据实体的属性创建表示该数据实体的类。 WCF 包含一个用于以这种简单方式处理数据的综合系统。 在创建了表示数据的类之后，服务会自动生成使客户端能够符合所设计数据类型的元数据。 有关详细信息，请参阅 [使用数据协定](feature-details/using-data-contracts.md)。

- **安全性**

     可对消息进加密以保护隐私，而且可以要求用户对其自身进行身份验证，然后才允许接收消息。 可使用众所周知的标准（如 SSL 或 WS-SecureConversation）实现安全性。 有关详细信息，请参阅[安全性](./feature-details/security.md)。

- **多种传输和编码方式**

     可通过多种内置传输协议和编码中的任意一种发送消息。 最常见的协议和编码是使用超文本传输协议（ (HTTP) 发送文本编码的 SOAP 消息，以便在万维网上使用。 另外，WCF 还允许通过 TCP、命名管道或 MSMQ 发送消息。 这些消息可以编码为文本，也可以使用优化的二进制格式。  使用 MTOM 标准可有效地发送二进制数据。 如果所提供的传输或编码都不符合您的需要，您可以创建自己的自定义传输或编码。 有关 WCF 支持的传输和编码的详细信息，请参阅 [传输](./feature-details/transports.md)。

- **可靠的排队消息**

     WCF 支持使用通过 WS-Reliable 消息传递和使用 MSMQ 实现的可靠会话来实现可靠的消息交换。 有关 WCF 中可靠和排队消息支持的详细信息，请参阅 [队列和可靠会话](./feature-details/queues-and-reliable-sessions.md)。

- **持久性消息**

     持久性消息决不会由于通信中断而丢失。 持久性消息模式的消息会始终保存到数据库中。 如果发生中断，数据库将允许您在恢复连接后恢复消息交换。 还可以使用 Windows Workflow Foundation (WF) 创建持久消息。 有关详细信息，请参阅 [工作流服务](./feature-details/workflow-services.md)。

- **事务**

     WCF 还支持使用以下三种事务模型之一的事务： AtomicTransactions、命名空间中的 Api <xref:System.Transactions> 和 Microsoft 分布式事务处理协调器。 有关 WCF 中事务支持的详细信息，请参阅 [事务](./feature-details/transactions-in-wcf.md)。

- **AJAX 和 REST 支持**

     REST 是不断发展的 Web 2.0 技术的一个示例。 可以将 WCF 配置为处理未包装在 SOAP 信封中的 "纯文本" XML 数据。 WCF 还可以扩展以支持特定的 XML 格式，如 ATOM (常见的 RSS 标准) ，甚至非 XML 格式，如 JavaScript 对象表示法 (JSON) 。

- **扩展性**

     WCF 体系结构具有大量扩展点。 如果需要额外功能，它还提供许多入口点，允许您自定义服务的行为。 有关可用扩展点的详细信息，请参阅 [扩展 WCF](./extending/index.md)。

## <a name="wcf-integration-with-other-microsoft-technologies"></a>WCF 与其他 Microsoft 技术的集成

WCF 是一个灵活的平台。 由于这种极大的灵活性，还在其他一些 Microsoft 产品中使用 WCF。 通过了解 WCF 的基础知识，你还可以直接使用这些产品中的任何一个。

与 WCF 配对的第一种技术是 (WF) Windows Workflow Foundation。 工作流通过将工作流中的步骤封装为 "活动" 来简化应用程序开发。 在 Windows Workflow Foundation 的第一版中，开发人员必须为工作流创建主机。 Windows Workflow Foundation 的下一个版本与 WCF 集成。 这允许在 WCF 服务中轻松承载任何工作流。 可以通过在 Visual Studio 2012 或更高版本中自动选择 WF/WCF 项目类型来实现此目的。

Microsoft BizTalk Server R2 也使用 WCF 作为通信技术。 BizTalk 设计用于接收一种标准化格式的数据，然后将其转换为另一种格式。 必须将消息传递到其中央消息框，在这里可以使用严格的映射，也可以通过使用其中一种 BizTalk 功能（如其工作流引擎）来转换消息。 BizTalk 现在可以使用 WCF 业务线 (LOB) 适配器将消息传递到消息框。

Microsoft Silverlight 是一个用于创建可互操作的、丰富 Web 应用程序的平台，允许开发人员创建媒体密集的网站（如流视频）。 从版本2开始，Silverlight 合并了 WCF 作为通信技术，以将 Silverlight 应用程序连接到 WCF 终结点。

Windows Server AppFabric 应用程序服务器的托管功能专门用于部署和管理使用 WCF 进行通信的应用程序。 托管功能包括丰富的工具和配置选项，专门为启用了 WCF 的应用程序而设计。

## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel>
- [Windows Communication Foundation 基础概念](fundamental-concepts.md)
- [Windows Communication Foundation 体系结构](architecture.md)
- [指南与最佳做法](guidelines-and-best-practices.md)
- [入门教程](getting-started-tutorial.md)
- [文档指南](guide-to-the-documentation.md)
- [基本 WCF 编程](basic-wcf-programming.md)
- [Windows Communication Foundation 示例](/previous-versions/dotnet/netframework-3.5/ms751514(v=vs.90))
