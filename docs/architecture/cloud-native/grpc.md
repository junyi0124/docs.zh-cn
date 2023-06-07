---
title: gRPC
description: 了解 gRPC，它在云本机应用程序中的作用，以及它与 HTTP RESTful 通信有何不同。
author: robvet
no-loc:
- Blazor
- Blazor WebAssembly
ms.date: 05/13/2020
ms.openlocfilehash: 9ed6906bd388a1ddef7f97bbaac001b4274853f9
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91158079"
---
# <a name="grpc"></a>gRPC

到目前为止，我们致力于 [基于 REST 的](/azure/architecture/best-practices/api-design) 通信。 我们已经看到，REST 是一种灵活的体系结构样式，它定义了对实体资源的基于 CRUD 的操作。 客户端使用请求/响应通信模型跨 HTTP 与资源进行交互。 尽管 REST 是广泛实现的，但一种较新的通信技术 gRPC 已在云本机社区中获得巨大的动力。

## <a name="what-is-grpc"></a>什么是 gRPC？

gRPC 是一种新式的高性能框架， (RPC) 协议演变旧的 [远程过程调用 ](https://en.wikipedia.org/wiki/Remote_procedure_call) 。 在应用程序级别，gRPC 简化了客户端和后端服务之间的消息传送。 GRPC 是源自 Google 的开源和  [云本机计算基础的一部分 () CNCF ](https://www.cncf.io/) 云本机产品/服务的生态系统。 CNCF 考虑 gRPC [孵化项目](https://github.com/cncf/toc/blob/master/process/graduation_criteria.adoc)。 孵化表示最终用户在生产应用程序中使用技术，而项目的参与者数量是正常的。

典型的 gRPC 客户端应用程序将公开一个用于实现业务操作的本地进程内函数。 在这种情况下，该本地函数调用远程计算机上的另一个函数。 本地调用似乎是对远程服务的透明进程外调用。 RPC 管道对计算机之间的点到点网络通信、序列化和执行进行了抽象。

在云本机应用程序中，开发人员通常跨编程语言、框架和技术。 此 *互操作性* 使消息协定和跨平台通信所需的管道复杂化。  gRPC 提供了一个 "统一的水平层" 来抽象这些问题。 开发人员代码在其本地平台上侧重于业务功能，而 gRPC 则负责处理通信。

gRPC 提供跨最常见开发堆栈（包括 Java、JavaScript、c #、中转、Swift 和 NodeJS）的全面支持。

## <a name="grpc-benefits"></a>gRPC 权益

gRPC 对其传输协议使用 HTTP/2。 与 HTTP 1.1 兼容，HTTP/2 功能很多高级功能：

- 用于数据传输的二进制组帧协议-与基于文本的 HTTP 1.1 不同。
- 多路复用支持通过同一连接发送多个并行请求-HTTP 1.1 限制处理一次请求/响应消息。
- 同时发送客户端请求和服务器响应的双向全双工通信。
- 内置流式处理可对大型数据集进行异步流式处理的请求和响应。
- 减少网络使用量的标头压缩。

gRPC 是轻型且高性能的。 与 JSON 序列化相比，与 JSON 序列化相比，它的最高速率为60-80%。 在 Microsoft [Windows Communication Foundation (WCF) ](../../framework/wcf/whats-wcf.md) 行话，gRPC 性能超出了高度优化的 [wcf-nettcp 绑定](/dotnet/api/system.servicemodel.nettcpbinding?view=netframework-4.8)的速度和效率。 不同于 Microsoft stack 的 Wcf-nettcp，gRPC 是跨平台的。

## <a name="protocol-buffers"></a>协议缓冲区

gRPC 拥有称为 [协议缓冲区](https://developers.google.com/protocol-buffers/docs/overview)的开源技术。 它们提供了一种高效且平台中立的序列化格式，用于序列化服务相互发送的结构化消息。 使用跨平台接口定义语言 (IDL) ，开发人员定义每个微服务的服务协定。 作为基于文本的文件实现的协定将 `.proto` 描述每个服务的方法、输入和输出。 同一约定文件可用于在不同开发平台上构建的 gRPC 客户端和服务。

使用 proto 文件，Protobuf 编译器 `protoc` 生成目标平台的客户端和服务代码。 此代码包括以下组件：

- 由客户端和服务共享的强类型对象，表示消息的服务操作和数据元素。
- 一个强类型的基类，其中包含远程 gRPC 服务可以继承和扩展的必需的网络管道。
- 包含调用远程 gRPC 服务所需的管道的客户端存根。

在运行时，每条消息都序列化为标准 Protobuf 表示形式，并在客户端和远程服务之间交换。 与 JSON 或 XML 不同，Protobuf 消息会序列化为已编译的二进制字节。

Microsoft 体系结构站点提供了 [gRPC FOR WCF 开发人员](../grpc-for-wcf-developers/index.md)的书籍，提供了 GRPC 和协议缓冲区的深入介绍。

## <a name="grpc-support-in-net"></a>.NET 中的 gRPC 支持

gRPC 集成到 .NET Core 3.0 SDK 和更高版本中。 以下工具支持它：

- 安装了 web 开发工作负荷的 Visual Studio 2019、版本16.3 或更高版本。
- Visual Studio Code
- dotnet CLI

SDK 包括用于终结点路由、内置 IoC 和日志记录的工具。 开源 Kestrel web 服务器支持 HTTP/2 连接。 图4-20 显示了一个 Visual Studio 2019 模板，该模板基架了 gRPC 服务的主干项目。 请注意 .NET Core 如何完全支持 Windows、Linux 和 macOS。

![Visual Studio 2019 中的 gRPC 支持](./media/visual-studio-2019-grpc-template.png)

图 4-20****。 Visual Studio 2019 中的 gRPC 支持
  
图4-21 显示了 Visual Studio 2019 中包含的内置基架生成的主干 gRPC 服务。  

![Visual Studio 2019 中的 gRPC 项目](./media/grpc-project.png  )

图 4-21  。 Visual Studio 2019 中的 gRPC 项目

在上图中，记下 proto 说明文件和服务代码。 正如您很快将看到的那样，Visual Studio 会在 Startup 类和基础项目文件中同时生成附加配置。

## <a name="grpc-usage"></a>gRPC 用法

为以下方案优选 gRPC：

- 同步后端微服务到微服务的通信，需要立即响应才能继续处理。
- 需要支持混合编程平台的 Polyglot 环境。
- 低延迟和高吞吐量通信，其中的性能至关重要。
- 点到点实时通信-gRPC 可以实时推送消息而无需轮询，并对双向流式处理提供了极佳支持。
- 网络约束环境–二进制 gRPC 消息始终小于等效的基于文本的 JSON 消息。

在撰写本文时，gRPC 主要用于后端服务。 新式浏览器无法提供支持前端 gRPC 客户端所需的 HTTP/2 控制级别。 这就是，有了支持 [gRPC 的 .net](https://devblogs.microsoft.com/aspnet/grpc-web-for-net-now-available/) ，它支持通过 JavaScript 或技术生成的基于浏览器的应用的 gRPC 通信 Blazor WebAssembly 。 [gRPC](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) 使 ASP.NET Core gRPC 应用支持浏览器应用中的 gRPC 功能：

- 强类型的代码生成客户端
- Compact Protobuf 消息
- 服务器流式处理

## <a name="grpc-implementation"></a>gRPC 实现

Microsoft 的 [容器上](https://github.com/dotnet-architecture/eShopOnContainers)的微服务参考体系结构 eShop，演示了如何在 .net Core 应用程序中实现 gRPC 服务。 图4-22 显示后端体系结构。

![容器上的 eShop 的后端体系结构](./media/eshop-with-aggregators.png)

**图 4-22**。 容器上的 eShop 的后端体系结构

在上图中，请注意 eShop 如何通过公开多个 API 网关来通过 [前端模式](/azure/architecture/patterns/backends-for-frontends) (BFF) 。 本章前面介绍了 BFF 模式。 请注意，聚合微服务 (以灰色) ，位于 Web 购物 API 网关和后端购物微服务之间。 聚合器接收来自客户端的单个请求，将其调度到不同的微服务，聚合结果并将其发送回发出请求的客户端。 此类操作通常需要同步通信，才能生成立即响应。 在 eShop 中，使用 gRPC 执行从聚合器的后端调用，如图4-23 所示。

![容器的 eShop 中的 gRPC](./media/grpc-implementation.png)

图 4-23  。 容器的 eShop 中的 gRPC

gRPC 通信需要客户端和服务器组件。 在上图中，请注意购物聚合器如何实现 gRPC 客户端。 客户端将 (以红色) 对后端微服务进行同步调用，其中每个调用都实现 gRPC 服务器。 客户端和服务器都利用 .NET Core SDK 中内置的 gRPC 管道。 客户端 *存根* 提供用于调用远程 gRPC 调用的管道。 服务器端组件提供自定义服务类可以继承和使用的 gRPC 管道。

同时公开 RESTful API 和 gRPC 通信的微服务需要使用多个终结点来管理流量。 你将打开一个终结点，该终结点侦听 RESTful 调用的 HTTP 流量，另一个用于 gRPC 调用。 必须为 gRPC 通信所需的 HTTP/2 协议配置 gRPC 终结点。

尽管我们正在努力将微服务与异步通信模式分离，但某些操作需要直接调用。 gRPC 应该是微服务之间直接同步通信的主要选择。 基于 HTTP/2 和协议缓冲区的高性能通信协议使其成为最佳选择。

## <a name="looking-ahead"></a>正在寻找

仔细看，gRPC 将继续为云本机系统提供吸引力。 性能优势和轻松开发非常引人注目。 但是，REST 可能会长时间发生。 它 transact-sql 公开的 Api 和向后兼容性的原因。

>[!div class="step-by-step"]
>[上一页](service-to-service-communication.md)
>[下一页](service-mesh-communication-infrastructure.md)
