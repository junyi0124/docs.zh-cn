---
title: 微服务体系结构
description: 适用于容器化 .NET 应用程序的 .NET 微服务体系结构 | 微服务体系结构的 30,000 英尺视图。
ms.date: 09/20/2018
ms.openlocfilehash: d1c58d218be9e5f8c0ae8ae732f9bdd06674a2c2
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "71834386"
---
# <a name="microservices-architecture"></a>微服务体系结构

顾名思义，微服务体系结构是一种将服务器应用程序生成为一组小型服务的方法。 这意味着微服务体系结构主要面向后端，虽然该方法也会用于前端。 每个服务都在自己的进程中运行，并使用 HTTP/HTTPS、WebSocket 或 [AMQP](https://en.wikipedia.org/wiki/Advanced_Message_Queuing_Protocol) 等协议与其他进程进行通信。 每个微服务在特定的上下文边界内实现特定的端到端域或业务功能，每个微服务都必须自主开发，并且可以独立部署。 最后，每个微服务都应拥有其自己的相关域数据模型和域逻辑（主权和分散式数据管理），并且可以基于不同的数据存储技术（SQL、NoSQL）和不同的编程语言。

微服务应该有多大？ 在开发微服务时，大小不应成为重点。 相反，重点应该是创建松散耦合的服务以便自主地为每个服务进行开发、部署和缩放。 当然，在标识和设计微服务时，只要与其他微服务不存在过多的直接依赖项，就应尝试让它们尽可能地小。 比微服务的大小更重要的是，它必须具有内部内聚，并且独立于其他服务。

为何选择微服务体系结构？ 简而言之，它提供了长期的灵活性。 通过允许基于许多独立的可部署服务（每个服务都具有粒度和自主生命周期）创建应用程序，微服务在复杂、大型和高度可缩放的系统中支持更好的可维护性。

微服务的另外一大优势是，可以独立横向扩展。 可以横向扩展特定的微服务，而无需将单个整体应用程序作为一个单位扩展。 这样便可单独缩放需要更多处理能力或网络带宽以支撑需求的功能区域，而不用横向扩展应用程序中不需要缩放的其他区域。 这意味着节省成本，因为所需硬件更少。

![两种部署方法之间的差异关系图。](./media/microservices-architecture/monolith-deployment-vs-microservice-approach.png)

**图 4-6**。 整体部署与微服务方法

如图 4-6 所示，在传统的整体方法中，应用程序会通过在多个服务器/VM 中克隆整个应用程序进行缩放。 在微服务方法中，功能会分离到较小的服务中，因此每个服务都可以独立缩放。 微服务方法允许对每个微服务进行灵活更改和快速迭代，因为可以更改复杂、大型和可缩放的应用程序特定的小区域。

构建基于微服务的细粒度应用程序使持续集成和持续交付实践可行。 它还加快了应用程序中新功能的交付。 应用程序的细粒度组合还支持以隔离方式运行和测试微服务，并在维护它们之间的清除协定的同时进行自主升级。 只要不更改接口或协定，就可以更改任何微服务的内部实现或添加新功能，而不会中断其他微服务。

以下是使用基于微服务的系统成功投入生产的重要方面：

- 对服务和基础结构的监视和运行状况检查。

- 服务（即云和业务流程协调程序）的可缩放基础结构。

- 多个级别的安全设计和实现：身份验证、授权、机密管理、安全通信等。

- 快速应用程序交付，通常不同的团队重点负责不同的微服务。

- DevOps 和 CI/CD 实践和基础结构。

其中，本指南只涵盖或介绍了前三项内容。 最后两项与应用程序生命周期相关，在附加的[《使用 Microsoft 平台和工具的容器化 Docker 应用程序的生命周期》](https://aka.ms/dockerlifecycleebook)电子书中进行了介绍。

## <a name="additional-resources"></a>其他资源

- **Mark Russinovich。微服务：云推动的应用程序革命** \
  <https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/>

- **Martin Fowler。Microservices** \（微服务）\
  <https://www.martinfowler.com/articles/microservices.html>

- **Martin Fowler。Microservice Prerequisites** \（微服务先决条件）\
  <https://martinfowler.com/bliki/MicroservicePrerequisites.html>

- **Jimmy Nilsson。Chunk Cloud Computing** \（区块云计算）\
  <https://www.infoq.com/articles/CCC-Jimmy-Nilsson>

- **Cesar de la Torre。Containerized Docker Application Lifecycle with Microsoft Platform and Tools**（使用 Microsoft 平台和工具的容器化 Docker 应用程序生命周期）（可下载的电子书）\
  <https://aka.ms/dockerlifecycleebook>

>[!div class="step-by-step"]
>[上一页](service-oriented-architecture.md)
>[下一页](data-sovereignty-per-microservice.md)
