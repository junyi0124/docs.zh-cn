---
title: API 网关模式与客户端到微服务直接通信
description: 了解 API 网关模式与客户端到微服务直接通信之间的差异及二者的用途。
ms.date: 01/07/2019
ms.openlocfilehash: 88cea3b7c2fdd09bec605431308df8783c343332
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96240597"
---
# <a name="the-api-gateway-pattern-versus-the-direct-client-to-microservice-communication"></a>API 网关模式与客户端到微服务直接通信

在微服务体系结构中，每个微服务通常都会公开一组精细终结点。 这种情况可能会影响客户端到微服务通信，如本节所述。

## <a name="direct-client-to-microservice-communication"></a>客户端到微服务直接通信

使用客户端到微服务直接通信体系结构是一种可行方法。 在此方法中，客户端应用可以直接向某些微服务发出请求，如图 4-12 所示。

![显示客户端到微服务通信体系结构的示意图。](./media/direct-client-to-microservice-communication.png)

图 4-12。 使用客户端到微服务直接通信体系结构

在此方法中，每个微服务都有一个公共终结点，有时每个微服务会有不同的 TCP 端口。 特定服务的 URL 示例可能是 Azure 中的以下 URL：

`http://eshoponcontainers.westus.cloudapp.azure.com:88/`

在基于群集的生产环境中，该 URL 将映射到群集中使用的负载均衡器，该负载均衡器随后在微服务中分布请求。 在生产环境中，可以在微服务和 Internet 之间使用 [Azure 应用程序网关](/azure/application-gateway/application-gateway-introduction)等应用程序传送控制器 (ADC)。 该控制器将充当透明层，不仅执行负载均衡，还可通过提供 SSL 终端来保护服务。 这通过卸载 CPU 密集型 SSL 终端和其他到 Azure 应用程序网关的路由任务来提高主机负载。 在任何情况下，从逻辑应用程序体系结构的角度看，负载均衡器和 ADC 都是透明的。

客户端到微服务直接通信体系结构已能满足基于微服务的小型应用程序的需求，尤其是在客户端应用为服务器端 Web 应用程序（如 ASP.NET MVC 应用）的情况下。 但是，若要生成基于微服务的大型复杂应用程序（例如处理大量微服务类型），尤其是客户端应用是远程移动应用或 SPA Web 应用程序时，该方法将面临一些问题。

开发基于微服务的大型应用程序时，请考虑以下问题：

- 客户端应用如何最大限度地减少对后端发出的请求数并减少与多个微服务的聊天通信？

与多个微服务交互构建单个 UI 屏幕可增加 Internet 中的往返次数。 这会增加 UI 端的延迟时间和复杂性。 理想情况下，响应会在服务端高效聚合。 这便减少了延迟时间，因为可以并行返回多条数据，并且某些 UI 可在准备就绪后立即显示。

- 如何处理诸如授权、数据转换和动态请求调度之类的整合问题？

在每个微服务上实现安全和授权等安全性和整合问题需要进行大量的开发工作。 其中一种可行方法是在 Docker 主机或内部群集中提供这些服务，以限制从外部直接访问它们，并在集中位置（如 API 网关）实现这些整合问题。

- 客户端应用如何与使用非 Internet 友好协议的服务进行通信？

通常，客户端应用不支持服务端使用的协议（如 AMQP 或二进制协议）。 因此，必须通过 HTTP/HTTPS 等协议执行请求并随后将请求转换为其他协议。 在此情况下，中间人方法可提供帮助。

- 如何构造专为移动应用设计的外观？

多个微服务的 API 设计可能无法满足不同客户端应用程序的需求。 例如，移动应用的需求可能不同于 Web 应用的需求。 移动应用可能需要进一步优化，以便数据响应能更有效。 若要实现这一点，可以聚合多个微服务的数据并返回单组数据，有时还需从响应中删除移动应用不需要的所有数据。 当然，也可以压缩该数据。 同样，在此方案中，使用移动应用和微服务之间的外观或 API 会非常便捷。

## <a name="why-consider-api-gateways-instead-of-direct-client-to-microservice-communication"></a>为什么考虑 API 网关而不考虑客户端到微服务直接通信

在微服务体系结构中，客户端应用通常需要使用来自多个微服务的功能。 如果直接使用，则客户端需要处理对微服务终结点的多个调用。 当演进应用程序和引入新的微服务或者更新现有微服务时，会发生什么？ 如果应用程序有多个微服务，那么从客户端应用处理如此多的终结点可能非常困难。 由于客户端应用将与这些内部终结点相耦合，因此未来演进微服务可能会对客户端应用产生巨大影响。

因此，对于基于微服务的应用程序来说，有一个间接的中间级别或中间层（网关）将非常方便。 如果没有 API 网关，则客户端应用必须将请求直接发送给微服务，这会引发问题，如以下问题：

- **耦合**：如果没有 API 网关模式，客户端应用将与内部微服务相耦合。 客户端应用需要知道如何在微服务中分解应用程序的多个区域。 在演进和重构内部微服务时，这些操作会影响维护，因为它们会导致客户端应用的重大更改，原因在于直接引用来自客户端应用的内部微服务。 客户端应用需要频繁更新，这使得解决方案更难发展。

- 过多的往返：在客户端应用中，单个页面/屏幕可能需要多次调用多个服务。 这可能导致客户端和服务器之间的多次网络往返，从而显著增加了延迟时间。 在中级水平处理的聚合可以提高客户端应用的性能和用户体验。

- 安全性问题：如果没有网关，所有微服务必定会暴露在“外部世界”中，如此，相较于隐藏客户端应用不直接使用的内部微服务，这种情况下攻击面更大。 攻击面越小，应用程序越安全。

- 跨领域问题：每个公开发布的微服务都必须处理授权和 SSL 等问题。 在许多情况下，这些问题可以在一个单独的层次上处理，以便简化内部微服务。

## <a name="what-is-the-api-gateway-pattern"></a>什么是 API 网关模式？

使用多个客户端应用来设计和生成基于微服务的大型复杂应用程序时，[API 网关](https://microservices.io/patterns/apigateway.html) 是非常不错的方法。 这一服务可为某些微服务组提供单一入口点。 这类似于面向对象设计的[外观模式](https://en.wikipedia.org/wiki/Facade_pattern)，但在此情况下，它是分布式系统的一部分。 因为构建时考虑了客户端应用的需求，所以 API 网关模式有时也称为“用于前端的后端”([BFF](https://samnewman.io/patterns/architectural/bff/))。

因此，API 网关位于客户端应用和微服务之间。 它充当反向代理，将请求从客户端路由到服务。 它还可以提供其他跨领域功能，例如身份验证、SSL 终止和缓存。

图 4-13 演示自定义 API 网关如何通过几个微服务适应简化的基于微服务的体系结构。

![显示作为自定义服务实现的 API 网关的示意图。](./media/direct-client-to-microservice-communication-versus-the-API-Gateway-pattern/custom-service-api-gateway.png)

图 4-13。 使用作为自定义服务实现的 API 网关

应用程序将连接到单个终结点，即 API 网关，该终结点配置为将请求转发到各个微服务。 在此示例中，API 网关将作为自定义 ASP.NET Core WebHost 服务实现，并作为容器运行。

将使用面向多个不同客户端应用的单个自定义 API 网关服务，在该图中突出显示这一点非常重要。 这一事实可能存在很大风险，因为 API 网关服务将根据客户端应用的多种不同要求而不断增长和发展。 最终，它将因这些不同的需求而膨胀，实际上，它会类似于整体式应用程序或整体式服务。 正因如此，强烈建议将 API 网关拆分成多个服务或多个更小的 API 网关（例如每种客户端应用外形规格一个网关）。

在实现 API 网关模式时需小心谨慎。 通常，不建议使用单个 API 网关聚合应用程序的所有内部微服务。 如果使用这种方式，它将作为整体式聚合器或业务流程协调程序，且会耦合所有微服务，违背微服务的自主性。

因此，应根据业务边界和客户端应用分隔 API 网关，并且不将其用作所有内部微服务的单个聚合器。

当将 API 网关层拆分为多个 API 网关时，如果应用程序有多个客户端应用，那么在识别多个 API 网关类型时，这可能是一个主枢轴，这样你就可以为每个客户端应用提供所需的不同外观。 本例是一个名为“用于前端的后端”([BFF](https://samnewman.io/patterns/architectural/bff/)) 模式，其中每个 API 网关可以专为每个客户端应用类型提供不同的 API，甚至可以通过实现特定的适配器代码（该代码在下方调用多个内部微服务），根据客户端外形规格提供不同 API，如下图所示：

![显示多个自定义 API 网关的示意图。](./media/direct-client-to-microservice-communication-versus-the-API-Gateway-pattern/multiple-custom-api-gateways.png)

图 4-13.1。 使用多个自定义 API 网关

图 4-13.1 显示按客户端类型分开的 API 网关；一个用于移动客户端，一个用于 Web 客户端。 传统 Web 应用连接到使用 Web API 网关的 MVC 微服务。 示例显示一个使用多个细化 API 网关的简化体系结构。 在本例中，为每个 API 网关识别的边界纯粹基于“用于前端的后端”([BFF](https://samnewman.io/patterns/architectural/bff/)) 模式，因此仅基于每个客户端应用所需的 API。 但是在更大的应用程序中，还应该更进一步，并创建基于业务边界的附加 API 网关作为第二个设计枢轴。

## <a name="main-features-in-the-api-gateway-pattern"></a>API 网关模式中的主要功能

API 网关可以提供多个功能。 然而，根据产品，它可能提供更丰富或更简单的功能，任何 API 网关最重要和最基本的功能都采用以下设计模式：

反向代理或网关路由。 API 网关提供一个反向代理将请求（第 7 层路由，通常是 HTTP 请求）重定向或路由到内部微服务的终结点。 网关为客户端应用提供单个终结点或 URL，然后将请求映射到一组内部微服务。 此路由功能有助于将客户端应用从微服务中分离出来；而且在升级整体式 API 服务时，将 API 网关置于整体式 API 服务和客户端应用之间，操作会变得非常方便，然后可以添加新的 API 作为新的微服务，同时仍然可以使用旧的整体式 API 服务，直到将来它拆分成多个微服务为止。 由于 API 网关，客户端应用不会注意到所使用的 API 是否已实现为内部微服务或整体式 API，更重要的是，当对整体式 API 进行演进并将其重构为微服务时，得益于 API 网关路由，任何 URI 更改均不会对客户端应用造成影响。

有关详细信息，请参阅[网关路由模式](/azure/architecture/patterns/gateway-routing)。

请求聚合。 作为网关模式的一部分，你可以将多个针对多个内部微服务的客户端请求（通常是 HTTP 请求）聚合到单个客户端请求中。 如果客户端页面/屏幕需要来自多个微服务的信息，此模式特别方便。 通过这种方法，客户端应用向 API 网关发送一个单一请求，该网关向内部微服务发送多个请求，然后聚合结果，并将所有内容发送回客户端应用。 这种设计模式的主要优势和目标是减少客户端应用和后端 API 之间的隔阂，这对于微服务所在的数据中心中的远程应用至关重要，如移动应用或来自客户端远程浏览器 JavaScript 的 SPA 应用的请求。 对于在服务器环境中执行请求的常规 Web 应用（如 ASP.NET Core MVC Web 应用），这种模式并不重要，因为延迟时间比远程客户端应用要小得多。

根据你所使用的 API 网关产品，或许能够执行此聚合。 但是，在许多情况下，在 API 网关范围内创建聚合微服务更加灵活，因此你可以在代码（即 C# 代码）中定义聚合：

有关详细信息，请参阅[网关聚合模式](/azure/architecture/patterns/gateway-aggregation)。

跨领域问题或网关卸载。 根据每个 API 网关产品提供的功能，你可以将功能从单个微服务转移到网关，从而通过将跨领域问题整合到一个层级中来简化每个微服务的实现。 这对于在每个内部微服务中难以正确实现的特殊功能而言特别方便，比如以下功能：

- 身份验证和授权
- 服务发现集成
- 响应缓存
- 重试策略、断路器和 QoS
- 速率限制和遏制
- 负载均衡
- 日志记录、跟踪、相关性
- 标头、查询字符串和声明转换
- IP 允许列表

有关详细信息，请参阅[网关卸载模式](/azure/architecture/patterns/gateway-offloading)。

## <a name="using-products-with-api-gateway-features"></a>使用带 API 网关功能的产品

根据每个实现，API 网关产品可能会提供更多跨领域问题。 我们将在此处探讨：

- [Azure API 管理](https://azure.microsoft.com/services/api-management/)
- [Ocelot](https://github.com/ThreeMammals/Ocelot)

### <a name="azure-api-management"></a>Azure API 管理

[Azure API 管理](https://azure.microsoft.com/services/api-management/)（如图 4-14 所示）不仅能够满足 API 网关需求，还提供从 API 收集见解等功能。 如果正在使用 API 管理解决方案，则 API 网关仅是整个 API 管理解决方案中的一个组成部分。

![显示如何使用 Azure API 管理作为 API 网关的示意图。](./media/direct-client-to-microservice-communication-versus-the-API-Gateway-pattern/api-gateway-azure-api-management.png)

图 4-14。 为 API 网关使用 Azure API 管理

Azure API 管理同时满足了 API 网关和管理需求（如日志记录、安全和计量等）。在这种情况下，使用如 Azure API 管理之类的产品时，拥有单个 API 网关不会存在较大风险，因为这类 API 网关“更精细”，这意味着不会实现可能发展成整体式组件的自定义 C# 代码。

API 网关产品通常表现为用于入口通信的反向代理，也可以从内部微服务筛选 API，并授权此单层中的已发布 API。

API 管理系统中提供的见解有助于理解 API 的使用方式与性能表现。 通过查看近实时分析报告并标识可能影响业务的趋势，从而实现这一点。 此外，还可获取有关请求和响应活动的日志记录，以进一步进行联机和脱机分析。

借助 Azure API 管理，可以使用密钥、令牌和 IP 筛选器来保护 API。 通过这些功能，可以执行灵活且细化的配额和速率限制，使用策略修改 API 的形状和行为，并通过响应缓存提高性能。

在本指南和参考示例应用程序 (eShopOnContainers) 中，我们仅讨论较简单的自定义容器化体系结构，以便将重点放在未使用 PaaS 产品（如 Azure API 管理）的普通容器上。 但是对于部署到 Microsoft Azure 的基于微服务的大型应用程序，我们建议在生产中评估以 Azure API 为基础管理 API 网关。

### <a name="ocelot"></a>Ocelot

[Ocelot](https://github.com/ThreeMammals/Ocelot) 是一个轻型 API 网关，推荐用于更简单的方法。 Ocelot 是一个基于 .NET Core 的开放源代码 API 网关，专为在其系统中需要统一入口点的微服务体系结构打造。 它具有轻型、快速且可缩放的特点，并在许多其他功能中提供路由和身份验证。

在 [eShopOnContainers 引用应用程序](https://github.com/dotnet-architecture/eShopOnContainers)中选用 Ocelot 的主要原因是因为 Ocelot 是一个 .NET Core 轻型 API 网关，你可以将其部署到要在其中部署微服务/容器的同一应用程序部署环境，如 Docker 主机、Kubernetes 等。并且由于它基于 .NET Core，因此将允许你跨平台在 Linux 或 Windows 上进行部署。

前面的图表显示在容器中运行自定义 API 网关，正是你在容器和基于微服务的应用程序中运行 Ocelot 的方式。

此外，市场上还有很多其他产品提供 API 网关功能，比如 Apigee、Kong、MuleSoft、WSO2，以及提供服务网格入口控制器功能的 Linkerd 和 Istio 等其他产品。

在介绍完初始体系结构和模式部分之后，下一节将介绍如何使用 [Ocelot](https://github.com/ThreeMammals/Ocelot) 来实现 API 网关。

## <a name="drawbacks-of-the-api-gateway-pattern"></a>API 网关模式的缺点

- 实现 API 网关时，会将该层与内部微服务进行耦合，这是它的最大缺点。 此类耦合可能会给应用程序带来严重问题。 Azure 服务总线团队的架构师 Clemens Vaster 在 2016 年 GOTO 的“[消息传递和微服务](https://www.youtube.com/watch?v=rXi5CLjIQ9k)”会议中将此潜在难题描述为“新的 ESB”。

- 使用微服务 API 网关创建其他可能的单一故障点。

- 由于其他网络调用，API 网关可能会导致响应时间加长。 但是，相较于经常直接调用内部微服务的客户端接口，这种额外调用的影响更小。

- 如果未正确扩展，API 网关可能成为瓶颈。

- 如果 API 网关包含自定义逻辑和数据聚合，则它需要额外的开发成本并且需要在日后进行维护。 开发人员必须更新 API 网关，公开每个微服务的终结点。 此外，内部微服务中的实现更改可能引起 API 网关级别的代码更改。 但是，如果 API 网关仅应用安全性、日志记录和版本控制（与使用 Azure API 管理时一样），则可能不会产生额外开发成本。

- 如果是由一个团队开发的 API 网关，则可能出现开发瓶颈。 这也是最好使用多个精细 API 网关满足不同客户端需求的另一原因。 还可以将 API 网关内部分隔到多个区域或多个层，这些区域或层由使用内部微服务的不同团队所有。

## <a name="additional-resources"></a>其他资源

- **Chris Richardson.模式：API 网关/用于前端的后端** \
  <https://microservices.io/patterns/apigateway.html>

- **API 网关模式** \
  <https://docs.microsoft.com/azure/architecture/microservices/gateway>

- **聚合和组合模式** \
  <https://microservices.io/patterns/data/api-composition.html>

- **Azure API 管理** \
  <https://azure.microsoft.com/services/api-management/>

- **Udi Dahan.面向服务的组合** \
  <https://udidahan.com/2014/07/30/service-oriented-composition-with-video/>

- **Clemens Vasters。2016 年 GOTO 消息传递和微服务（视频）**  \
  <https://www.youtube.com/watch?v=rXi5CLjIQ9k>

- Nutshell 中的 API 网关（ASP.NET Core API 网关教程系列） \
  <https://www.pogsdotnet.com/2018/08/api-gateway-in-nutshell.html>

>[!div class="step-by-step"]
>[上一页](identify-microservice-domain-model-boundaries.md)
>[下一页](communication-in-microservice-architecture.md)
