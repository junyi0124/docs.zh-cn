---
title: 关于 ASP.NET Core Web 应用的 Azure 托管建议
description: 使用 ASP.NET Core 和 Azure 构建新式 Web 应用程序 | 关于 ASP.NET Web 应用的 Azure 托管建议
author: ardalis
ms.author: wiwagn
ms.date: 12/01/2020
ms.openlocfilehash: c209a7fa9ce89e12466424f750d5583a59f8b980
ms.sourcegitcommit: 45c7148f2483db2501c1aa696ab6ed2ed8cb71b2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/08/2020
ms.locfileid: "96851713"
---
# <a name="azure-hosting-recommendations-for-aspnet-core-web-apps"></a>关于 ASP.NET Core Web 应用的 Azure 托管建议

> “全球各地的业务线领导绕过 IT 部门，从云获取应用程序（又称 SaaS）并支付使用费用，就像订阅杂志一样。 不再需要服务时，他们可取消订阅，不会出现设备闲置的情况。”  
> _Gartner 分析师 Daryl Plummer\-_

无论面对应用程序的何种需求和体系结构，Microsoft Azure 均可提供支持。 托管需求可以非常简单（例如静态网站），也可以非常复杂（例如由多个服务组成的复杂的应用程序）。 对于 ASP.NET Core 整体 Web 应用程序和支持服务，推荐以下几种常见配置。 无论是完整应用程序、单个进程或数据，本文中的建议配置均按待托管资源的类型分组。

## <a name="web-applications"></a>Web 应用程序

可通过以下方式托管 Web 应用程序：

- 应用服务 Web 应用

- 容器（多个选项）

- 虚拟机 (VM)

其中，对于大多数应用场景（包括简单的基于容器的应用），推荐使用应用服务 Web 应用方法。 对于微服务体系结构，请考虑使用基于容器的方法。 如果需要更好地控制运行应用程序的计算机，请考虑使用 Azure 虚拟机。

### <a name="app-service-web-apps"></a>应用服务 Web 应用

应用服务 Web 应用提供的完全托管平台针对托管 Web 应用程序进行了优化。 它是一种平台即服务 (PaaS) 产品/服务，可使你专注于业务逻辑，而由 Azure 负责维护应用运行和缩放所需的基础结构。 应用服务 Web 应用的一些关键功能：

- DevOps 优化（持续集成和交付、多环境、A/B 测试和脚本支持）。

- 全球缩放和高可用性。

- 可连接至 SaaS 平台和本地数据。

- 安全性和符合性。

- Visual Studio 集成。

Azure 应用服务是适合大多数 Web 应用的最佳选择。 该平台集成部署与管理，站点可快速缩放以处理高流量负载，内置负载均衡和流量管理器提供高可用性。 可以使用联机迁移工具轻松将现有站点移动到 Azure 应用服务。 可以使用 Web 应用程序库中的开源应用，或使用框架和你选择的工具创建新的站点。 通过 WebJobs 功能可将后台作业处理轻松添加到应用服务 Web 应用。 如果你有使用本地数据库、在本地托管的现有 ASP.NET 应用程序，则有一条明确的迁移路径。 可以将应用服务 Web 应用与 Azure SQL 数据库一起使用（如果你愿意，也可以保护对本地数据库服务器的访问）。

![将本地 .NET 应用迁移到 Azure 应用服务的推荐策略](./media/image1-6.png)

多数情况下，将本地托管的 ASP.NET 应用迁移到应用服务 Web 应用的过程都很简单。 只需对应用本身进行极少的修改甚至无需修改，它便可以快速开始利用 Azure 应用服务 Web 应用提供的众多功能。

除了未针对云进行优化的应用，Azure 应用服务 Web 应用还是适合许多简单的整体式（非分布式）应用程序（例如许多 ASP.NET Core 应用）的出色解决方案。 此方法中的基本体系结构简单易懂，并且易于管理：

![基本的 Azure 体系结构](./media/image1-5.png)

通常单个资源组中的少量资源即足以管理此类应用。 这种[基本体系结构方法](/azure/architecture/reference-architectures/app-service-web-app/basic-web-app)非常适合通常部署为单个单元的应用，而不适合由许多独立进程构成的应用。 尽管此方法的体系结构很简单，但仍允许托管应用纵向扩展（每个节点包含更多资源）和横向扩展（更多托管节点），来满足任何需求增长。 借助自动缩放功能，可以将应用配置为根据需求和节点上的平均负载自动调整托管应用的节点数量。

### <a name="app-service-web-apps-for-containers"></a>用于容器的应用服务 Web 应用

除了支持直接托管 Web 应用，[用于容器的应用服务 Web 应用](https://azure.microsoft.com/services/app-service/containers/)还可用于在 Windows 和 Linux 上运行容器化的应用程序。 借助此服务，可以轻松部署和运行可随业务缩放的容器化的应用程序。 此类应用具有上面列出的应用服务 Web 应用的所有功能。 此外，用于容器的 Web 应用还支持使用 Docker Hub、Azure 容器注册表和 GitHub 来简化 CI/CD。 你可以使用 Azure DevOps 定义将更改发布到注册表的生成和部署管道。 然后可以在过渡环境中测试这些更改，并使用部署槽位自动将其部署到生产环境，无需停机即完成升级。 还可以同样轻松地回滚到以前的版本。

在某些应用场景中，用于容器的 Web 应用是最理想的选择。 如果现有应用可以容器化，则无论是在 Windows 容器还是 Linux 容器中，都可以使用此工具集轻松托管这些应用。 只需发布容器，然后将用于容器的 Web 应用配置为从所选注册表中拉取该映像的最新版本。 这是从经典应用托管模型迁移到云优化模型的“直接迁移”方法。

![将容器化的本地 .NET 应用程序迁移到用于容器的 Azure Web 应用](./media/image1-8.png)

如果开发团队能够转为使用基于容器的开发过程，这种方法也很有效。 使用容器开发应用的“内部循环”包括使用容器生成应用。 对代码和容器配置所做的更改将推送到源代码管理，自动生成功能负责将新容器映像发布到 Docker Hub 或 Azure 容器注册表等注册表。 然后，这些映像将用作其他开发以及生产部署的基础，如下图所示：

![端到端的 Docker DevOps 生命周期工作流](./media/image1-7.png)

使用容器进行开发具有许多优点，特别是在生产环境中使用容器时。 在运行应用的每个环境（从本地开发计算机到生成和测试系统再到生产）中，用于托管应用的容器配置是相同的。 此方法大大降低了由于计算机配置或软件版本的差异而导致缺陷的可能性。 开发人员还可以使用他们最有效的工具，包括操作系统，因为容器可以在任何 OS 上运行。 在某些情况下，涉及多个容器的分布式应用程序在单个开发计算机上运行时可能非常耗费资源。 在这种情况下，可能适合升级为使用 Kubernetes 和 Azure Dev Spaces，下一节将对此进行介绍。

随着较大型应用程序的各个部分被分解为其各自较小的、独立的 *微服务*，可以使用其他设计模式来改善应用的行为。 可以使用 *API 网关* 来简化访问并将客户端与其后端分离，而不是直接使用各个服务。 通过为不同的前端提供单独的服务后端，还可以让服务随其使用者而演化。 可以通过单独的 *sidecar* 容器访问公共服务，该容器可能包含使用 *ambassador* 模式的公共客户端连接库。

![注有几种常见设计模式的微服务示例体系结构。](./media/image1-10.png)

[详细了解在生成基于微服务的系统时要考虑的设计模式。](/azure/architecture/microservices/design/patterns)

### <a name="azure-kubernetes-service"></a>Azure Kubernetes 服务

Azure Kubernetes 服务 (AKS) 管理托管的 Kubernetes 环境，即使没有容器业务流程专业知识，也可快速轻松地部署和管理容器化应用程序。 此外，它还通过预配、升级和按需缩放资源，消除了持续操作和维护的负担，而无需应用程序脱机。

通过将大量责任转移至 Azure，AKS 降低了管理 Kubernetes 群集的复杂性和运营开销。 作为一项托管的 Kubernetes 服务，Azure 将为你处理运行状况监视和维护等关键任务。 并且，你仅需支付群集中的代理节点，而无需支付主节点。 作为一项托管的 Kubernetes 服务，AKS 提供以下功能：

- 自动化的 Kubernetes 版本升级和修补。
- 轻松的群集缩放。
- 自我修复型托管控制平面（主节点）。
- 节省成本：仅支付运行的代理池节点。

Azure 可处理 AKS 群集中的节点管理，因此我们不再需要手动执行许多任务，例如群集升级。 由于 Azure 会处理这些关键维护任务，因此，AKS 不提供对群集的直接访问（例如使用 SSH）。

使用 AKS 的团队也可以使用 Azure Dev Spaces。 Azure Dev Spaces 允许团队直接使用他们在 AKS 中运行的整个微服务体系结构或应用程序，帮助团队专注于微服务应用程序的开发和快速迭代。 Azure Dev Spaces 还提供一种独立更新微服务体系结构各部分的方法，而不会影响 AKS 群集的其余部分或其他开发人员。

![Azure Dev Spaces 工作流示例](./media/image1-9.gif)

Azure Dev Spaces：

- 最大限度地减少本地计算机设置时间和资源要求
- 允许团队更快地循环访问
- 减少团队所需的集成环境数量
- 进行开发/测试时，无需在分布式系统中模拟某些服务

[详细了解 Azure Dev Spaces](/azure/dev-spaces/about)

### <a name="azure-virtual-machines"></a>Azure 虚拟机

如果现有应用程序需要经过大量修改才可在应用服务中运行，为简化迁移到云的操作，可选择虚拟机。 然而，与 Azure 应用服务相比，正确配置、保护和维护 VM 需要更多时间和 IT 专业知识。 如要使用 Azure 虚拟机，请务必考虑到 VM 环境的修补、更新和管理所需的持续性维护工作。 Azure 虚拟机属于基础结构即服务 (IaaS)，而应用服务属于 PaaS。 还应考虑将应用作为 Windows 容器部署到用于容器的 Web 应用是否可能成为方案的可行选项。

## <a name="logical-processes"></a>逻辑进程

对于可从应用程序的剩余部分分离的单独逻辑进程，可以“无服务器”方式将其独立部署到 Azure Functions。 通过 Azure Functions 可编写解决特定问题所需的代码，无需担心运行它的应用程序或基础结构。 可选择各种编程语言，包括 C\#、F\#、Node.js、Python 和 PHP，以便你选择一种最高效语言来处理手头任务。 与多数基于云的解决方案一样，仅需为使用的时间量支付费用，而且 Azure Functions 按需扩展非常可靠。

## <a name="data"></a>data

Azure 提供多种数据存储选择，以便应用程序可使用恰当的数据提供程序处理相关数据。

对于事务和关系数据，Azure SQL 数据库是最佳选择。 对于以读取为主的高性能数据，受 Azure SQL 数据库支持的 Redis 缓存是一个不错的解决方案。

非结构化 JSON 数据可用多种方式进行存储，包括 SQL 数据库列、Blob、Azure 存储中的表，以及 Azure Cosmos DB。 其中，Azure Cosmos DB 可提供最佳的查询功能，建议用于必须支持查询的大量基于 JSON 的文档。

暂时命令或者用于安排应用程序行为的基于事件的数据可使用 Azure 服务总线或 Azure 存储队列。 Azure 服务总线具有更强的灵活性，建议将该服务用于处理应用程序内和应用程序间的非普通消息传递。

## <a name="architecture-recommendations"></a>体系结构建议

应用程序要求应决定其体系结构。 有多种不同的 Azure 服务可供使用。 选择正确的服务是一项重要决定。 Microsoft 提供一系列参考体系结构，帮助用户确定针对常见方案优化的典型体系结构。 可找到一个与应用程序要求高度契合或至少提供一个出发点的参考体系结构。

图 11-1 显示了一个示例参考体系结构。 该图介绍了一个建议用于为市场营销而优化的 Sitecore 内容管理系统网站的体系结构方式。

![图 11-1](./media/image11-2.png)

**Figure 11-1**。 Sitecore 市场营销网站参考体系结构。

**参考：Azure 托管建议**

- Azure解决方案体系结构\
  <https://azure.microsoft.com/solutions/architecture/>

- Azure 基本 Web 应用程序体系结构\
  <https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/basic-web-app>

- 微服务设计模式\
  <https://docs.microsoft.com/azure/architecture/microservices/design/patterns>

- Azure 开发人员指南\
  <https://azure.microsoft.com/campaigns/developer-guide/>

- Web 应用概述\
  <https://docs.microsoft.com/azure/app-service/app-service-web-overview>

- 用于容器的 Web 应用\
  <https://azure.microsoft.com/services/app-service/containers/>

- Azure Kubernetes 服务 (AKS) 简介\
  <https://docs.microsoft.com/azure/aks/intro-kubernetes>

>[!div class="step-by-step"]
>[上一页](development-process-for-azure.md)
