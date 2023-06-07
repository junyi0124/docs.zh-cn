---
title: 将 eShopOnContainers 映射到 Azure 服务
description: 将 eShopOnContainers 映射到 azure 服务，如 Azure Kubernetes 服务、API 网关和 Azure 服务总线。
ms.date: 05/13/2020
ms.openlocfilehash: c4627a4b6d9d8b62737984b507e638019544ab67
ms.sourcegitcommit: 48466b8fb7332ececff5dc388f19f6b3ff503dd4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2020
ms.locfileid: "93400443"
---
# <a name="mapping-eshoponcontainers-to-azure-services"></a>将 eShopOnContainers 映射到 Azure 服务

虽然这不是必需的，但 Azure 非常适合于支持 eShopOnContainers，因为该项目构建为云本机应用程序。 应用程序是用 .NET Core 构建的，因此它可以在 Linux 或 Windows 容器上运行，具体取决于 Docker 主机。 该应用程序由多个自治微服务组成，每个都有其自己的数据。 不同的微服务展示了不同的方法，范围从简单的 CRUD 操作到更复杂的 DDD 和 CQRS 模式。 微服务通过基于消息的通信，通过 HTTP 与客户端通信。 应用程序也为客户端支持多个平台，因为它采用 HTTP 作为标准通信协议，并包括在 Android、iOS 和 Windows 平台上运行 ASP.NET Core 和 Xamarin 移动应用。

应用程序的体系结构如图2-5 所示。 左侧是客户端应用， (SPA) 风格分解为移动、传统 Web 和 Web 单页应用程序。 右侧是构成系统的服务器端组件，每个组件都可以托管在 Docker 容器和 Kubernetes 群集中。 传统 web 应用由显示为黄色的 ASP.NET Core MVC 应用程序提供支持。 此应用和移动和 web SPA 应用程序通过一个或多个 API 网关与各个微服务进行通信。 API 网关按照 "后端 for 前台" (BFF) 模式，这意味着每个网关都设计为支持给定的前端客户端。 单个微服务在 API 网关的右侧列出，同时包含业务逻辑和某种类型的持久性存储区。 不同的服务利用 SQL Server 数据库、Redis 缓存实例和 MongoDB/CosmosDB 存储。 最右侧是系统的事件总线，用于微服务之间的通信。

![eShopOnContainers 体系结构 ](./media/eshoponcontainers-architecture.png)
 **图 2-5** 。 EShopOnContainers 体系结构。

此体系结构的服务器端组件可轻松映射到 Azure 服务。

## <a name="container-orchestration-and-clustering"></a>容器业务流程和群集

可以在 Azure Kubernetes Service (AKS) 中承载和管理应用程序的容器托管服务，从 ASP.NET Core MVC 应用到单个目录和订单微服务。 应用程序可以在 Docker 和 Kubernetes 上本地运行，也可以将相同的容器部署到 AKS 中托管的过渡环境和生产环境。 可以自动执行此过程，如下一节中所示。

AKS 为容器的各个群集提供管理服务。 应用程序将为 AKS 群集中的每个微服务部署单独的容器，如以上体系结构关系图中所示。 此方法允许每个单独的服务根据其资源需求进行独立缩放。 每个微服务还可以单独部署，理想情况下，此类部署应会导致系统停机。

## <a name="api-gateway"></a>API Gateway

EShopOnContainers 应用程序具有多个前端客户端和多个不同的后端服务。 客户端应用程序与支持它们的微服务之间不存在一对一的对应关系。 在这种情况下，将客户端软件编写为以安全方式与各种后端服务进行交互时，可能会有很大的复杂性。 每个客户端都需要自行解决这一复杂性，从而导致重复，并且很多位置会在服务发生更改时进行更新或实现新的策略。

Azure API 管理 (APIM) 帮助组织以一致且可管理的方式发布 Api。 APIM 包含以下三个组件： API 网关、管理门户 (Azure 门户) 和开发人员门户。

API 网关接受 API 调用，并将其路由到相应的后端 API。 它还可提供更多的服务，例如，在无需 (修改代码的情况下动态验证 API 密钥或 JWT 令牌和 API 转换，以适应需要较低) 的客户端的客户端。

Azure 门户是定义 API 架构并将不同的 Api 打包到产品中的位置。 你还可以配置用户访问权限、查看报表以及为配额或转换配置策略。

开发人员门户用作开发人员的主要资源。 它为开发人员提供 API 文档、交互式测试控制台，并报告自己的使用情况。 开发人员还使用门户来创建和管理自己的帐户，包括订阅和 API 密钥支持。

使用 APIM，应用程序可以公开几个不同的服务组，每个组都为特定的前端客户端提供后端。 对于复杂的方案，建议使用 APIM。 为了简化需求，可以使用轻型 API 网关 Ocelot。 EShopOnContainers 应用使用 Ocelot 是因为其简易性，因为它可以与应用程序本身部署到同一个应用程序环境中。 [了解有关 eShopOnContainers、APIM 和 Ocelot 的详细信息。](../microservices/architect-microservice-container-applications/direct-client-to-microservice-communication-versus-the-api-gateway-pattern.md#azure-api-management)

如果你的应用程序正在使用 AKS，另一个选项是将 Azure 网关入口控制器部署为 AKS 群集中的 pod。 这允许你的群集与 Azure 应用程序网关集成，使网关能够将流量负载均衡到 AKS pod。 [了解有关 AKS 的 Azure 网关入口控制器的详细信息](https://github.com/Azure/application-gateway-kubernetes-ingress)。

## <a name="data"></a>数据

EShopOnContainers 使用的各种后端服务具有不同的存储要求。 多个微服务使用 SQL Server 数据库。 购物篮微服务利用 Redis 缓存来实现持久。 位置微服务需要一个 MongoDB API 用于其数据。 Azure 支持每个数据格式。

对于 SQL Server 数据库支持，Azure 提供的产品适用于单个数据库，最高可缩放的 SQL 数据库弹性池。 可以配置单个微服务，使其能够快速、轻松地与各自的单独 SQL Server 数据库通信。 这些数据库可根据需要进行缩放，以根据其需要支持每个单独的微服务。

EShopOnContainers 应用程序在请求之间存储用户的当前购物篮。 这由将数据存储在 Redis 缓存中的购物篮微服务管理。 在开发过程中，可以将此缓存部署在容器中，而在生产环境中，可以利用 Azure Cache for Redis。 适用于 Redis 的 Azure 缓存是一种完全托管的服务，提供高性能和可靠性，无需自行部署和管理 Redis 实例或容器。

位置微服务将 MongoDB NoSQL 数据库用于持久性。 在开发过程中，可以将数据库部署在其自己的容器中，而在生产环境中，服务可以利用 [Azure Cosmos DB 的 API For MongoDB](/azure/cosmos-db/mongodb-introduction)。 Azure Cosmos DB 的优点之一是能够利用多种不同的通信协议，其中包括 SQL API 和常见 NoSQL Api，包括 MongoDB、Cassandra、Gremlin 和 Azure 表存储。 Azure Cosmos DB 提供了一种完全托管的全球分布式数据库作为一种服务，可进行缩放以满足使用它的服务的需要。

[第5章](distributed-data.md)更详细地介绍了云中的分布式数据。

## <a name="event-bus"></a>事件总线

应用程序使用事件在不同服务之间传递更改。 此功能可通过多种实现实现，在本地 eShopOnContainers 应用程序使用 [RabbitMQ](https://www.rabbitmq.com/)。 在 Azure 中托管时，应用程序将使用 [Azure 服务总线](/azure/service-bus/) 进行消息传送。 Azure 服务总线是一种完全托管的集成消息代理，它允许应用程序和服务以分离、可靠、异步的方式相互通信。 Azure 服务总线支持单独的队列和单独的 *主题* ，以支持发布服务器方案。 EShopOnContainers 应用程序使用 Azure 服务总线的主题来支持将消息从一个微服务分发到任何其他需要响应给定消息的微服务。

## <a name="resiliency"></a>复原

在部署到生产环境后，eShopOnContainers 应用程序将能够利用几项 Azure 服务来改善其复原能力。 应用程序发布运行状况检查，这些检查可与 Application Insights 集成，以根据应用程序的可用性提供报告和警报。 Azure 资源还提供可用于识别和更正 bug 和性能问题的诊断日志。 资源日志提供有关应用程序使用不同 Azure 资源的时间和方式的详细信息。 你将在 [第6章](resiliency.md)了解有关云本机复原功能的详细信息。

>[!div class="step-by-step"]
>[上一页](introduce-eshoponcontainers-reference-app.md)
>[下一页](deploy-eshoponcontainers-azure.md)
