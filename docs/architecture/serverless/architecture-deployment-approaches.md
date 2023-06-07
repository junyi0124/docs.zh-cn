---
title: 体系结构部署方法 - 无服务器应用
description: 有关将企业体系结构部署到云的不同方式的指南，其中包括 IaaS、PaaS、容器和无服务器之间的对比。
author: JEREMYLIKNESS
ms.author: jeliknes
ms.date: 06/26/2018
ms.openlocfilehash: 7e91412600e8e4e5a0dca2a454f1cb0680c881b2
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91173336"
---
# <a name="architecture-deployment-approaches"></a>体系结构部署方法

无论用于设计商业应用程序的体系结构方法如何，这些应用程序的实现或部署都可能有所不同。 企业可以在从物理硬件到无服务器函数的所有内容上托管应用程序。

## <a name="n-tier-applications"></a>N 层应用程序

[N 层体系结构模式](/azure/architecture/guide/architecture-styles/n-tier)是一种成熟的体系结构，仅指将各种逻辑层分离为单独的物理层的应用程序。 N 层体系结构是 N 层体系结构的物理实现。 此体系结构最常见的实现包括：

- 表示层，例如 Web 应用程序。
- API 或数据访问层，如 REST API。
- 数据层，如 SQL 数据库。

![N 层体系结构](./media/n-tier-architecture.png)

N 层解决方案具有以下特征：

- 项目通常与层一致。
- 各个层的测试方法可能有所不同。
- 各层提供抽象层，例如，表示层通常不了解数据层的实现细节。
- 通常，各层仅与相邻层交互。
- 发布通常是在项目上进行管理的，因此是层、级。 简单的 API 更改可能需要重新发布整个中间层。

这种方法具有多项优势，包括：

- 隔离数据库（通常前端无法直接访问数据库后端）。
- 重用 API（例如，移动、桌面和 Web 应用客户端都可以重用相同的 API）。
- 能够相互独立地横向扩展层。
- 重构隔离：可以在不影响其他层的情况下重构一个层。

## <a name="on-premises-and-infrastructure-as-a-service-iaas"></a>本地和基础结构即服务 (IaaS)

托管应用程序的传统方法需要购买硬件并管理所有软件安装，包括操作系统。 最初，这涉及到昂贵的数据中心和物理硬件。 操作物理硬件所带来的挑战很多，包括：

- 为应对“以防万一”或高峰需求情况，需要购买额外的产品。
- 保护对硬件的物理访问。
- 硬件失败（例如磁盘失败）的责任。
- 冷却。
- 配置路由器和负载平衡器。
- 电源冗余。
- 保护软件访问。

![IaaS 方法](./media/iaas-approach.png)

通过“虚拟机”对硬件进行虚拟化可实现基础结构即服务 (IaaS)。 有效地对主机进行分区，为实例提供资源，并为其分配各自的内存、CPU 和存储空间。 该团队提供了必要的 VM，并配置到存储空间的相关网络和访问权限。

有关详细信息，请参阅[虚拟机 N 层参考体系结构](/azure/architecture/reference-architectures/virtual-machines-windows/n-tier)。

尽管虚拟化和基础结构即服务 (IaaS) 解决了许多问题，但它仍然给基础结构团队遗留了很多问题。 该团队维护操作系统版本、应用安全修补程序并在目标计算机上安装第三方依赖项。 与在测试环境中运行相比，应用程序在生产机器上的行为通常会有所不同。 由于不同的依赖项版本和/或 OS SKU 级别而产生问题。 尽管许多组织将 N 层应用程序部署到这些目标，但许多公司可以通过将其部署到更多云的本机模型（例如[平台即服务](#platform-as-a-service-paas)）的过程受益。 具有微服务的体系结构更具挑战性，因为需要横向扩展以具有伸缩性能和复原能力。

有关详细信息，请参阅[虚拟机](/azure/virtual-machines/)。

## <a name="platform-as-a-service-paas"></a>平台即服务 (PaaS)

平台即服务 (PaaS) 提供开发人员可以直接插入的已配置解决方案。 PaaS 是托管宿主的另一个术语。 它消除了管理基本操作系统、安全补丁以及各种情况下任何第三方依赖项的需要。 平台示例包括 Web 应用、数据库和移动后端。

PaaS 解决了 IaaS 常见的挑战。 PaaS 使开发人员可以专注于代码或数据库架构，而不是其部署方式。 PaaS 优势包括：

- 付费使用模型，消除了投资闲置计算机的开销。
- 直接部署、改进的 DevOps、持续集成 (CI) 和持续交付 (CD) 管道。
- 自动升级、更新和安全修补程序。
- 按钮横向扩展和纵向扩展（弹性缩放）。

一般来说，PaaS 的主要缺点是供应商锁定。 例如，某些 PaaS 提供程序仅支持 ASP.NET、Node.js 或其他特定语言和平台。 诸如 Azure 应用服务之类的产品已经发展为可适用于多个平台并支持用于托管 Web 应用的多种语言和框架。

![平台即服务体系结构](./media/paas-architecture.png)

## <a name="software-as-a-service-saas"></a>软件即服务 (SaaS)

软件即服务或 SaaS 集中托管，并且无需本地安装或配置即可使用。 SaaS 通常托管在 PaaS 的顶层，作为用于部署软件的平台。 SaaS 提供运行并与现有软件连接的服务。 SaaS 通常特定于行业和垂直行业。 SaaS 通常已获得许可，并提供客户端/服务器模型。 大多数用于客户端的新式 SaaS 产品/服务都使用基于 Web 的应用。 公司通常将 SaaS 视为许可产品/服务的业务解决方案。 它通常不会出于应用程序可伸缩性和可维护性的考虑而实现为体系结构。 实际上，大多数 SaaS 解决方案均构建在 IaaS、PaaS 和/或无服务器后端上。

通过[示例应用程序](/azure/sql-database/saas-tenancy-welcome-wingtip-tickets-app)详细了解有关 SaaS 的信息。

## <a name="containers-and-functions-as-a-service-faas"></a>容器和功能即服务 (FaaS)

容器是一种有趣的解决方案，可实现类似 PaaS 的优势而无需 IaaS 开销。 容器本质上是一个运行时，其中包含运行唯一应用程序所需的基本知识。 主机操作系统和服务（例如存储）的内核或核心部分在主机上共享。 共享内核使容器变得轻巧（与典型虚拟机的千兆字节大小相比，有些容器大小仅为兆字节）。 在主机已在运行的情况下，可以快速启动容器，从而提升高可用性。 快速启动容器功能还提供了额外的复原能力层。 Docker 较为常用的容器实现方式之一。

容器的优点包括：

- 轻便且便携
- 自包含，因此无需安装依赖项
- 无论主机如何，都提供一致的环境（在便携式计算机上运行与在云服务器上运行完全相同）
- 可以快速预配以进行横向扩展
- 可以快速重启以便从故障中恢复

容器在容器主机上运行（容器主机又可以在裸机或虚拟机上运行）。 多个容器或同一容器的实例可以在单个主机上运行。 为了实现真正的故障转移和复原能力，必须在主机之间缩放容器。

有关 Docker 容器的详细信息，请参阅[什么是 Docker](../microservices/container-docker-introduction/docker-defined.md)。

跨主机管理容器通常需要诸如 Kubernetes 之类的业务流程工具。 配置和管理业务流程解决方案可能会增加项目的额外开销和复杂性。 幸运的是，许多云提供商通过 PaaS 解决方案提供业务流程服务，简化了容器的管理。

下图说明了 Kubernetes 安装示例。 安装地址中的节点将横向扩展并进行故障转移。 它们运行由主服务器管理的 Docker 容器实例。  kubelet 是将命令从 Kubernetes 中继到 Docker 的客户端。

![Kubernetes](./media/kubernetes-example.png)

有关业务流程的详细信息，请参阅 [Azure 上的 Kubernetes](/azure/aks/intro-kubernetes)。

函数即服务 (FaaS) 是类似于无服务器的专用容器服务。 FaaS 的特定实现称为 [OpenFaaS](https://github.com/openfaas/faas)，位于容器顶层，可以提供无服务器功能。 OpenFaaS 提供打包了运行一段代码所需的所有容器依赖项的模板。 使用模板简化了将代码部署为功能单元的过程。 OpenFaaS 面向已包含容器和业务流程协调程序的体系结构，因为它可以使用现有的基础结构。 尽管可提供无服务器功能，但它特别要求使用 Docker 和业务流程协调程序。

## <a name="serverless"></a>无服务器

无服务器体系结构明确区分了代码与其宿主环境。 你可以在  函数中实现代码，该函数由  触发器调用。 该函数退出后，可以释放其所需的所有资源。 触发操作可以是定时的手动进程、HTTP 请求或文件上传。 触发的结果是执行代码。 尽管无服务器平台各不相同，但大多数平台都提供对预定义 API 和绑定的访问权限，从而简化诸如写入数据库或排队结果之类的任务。

无服务器是非常依赖于提取抽象主机环境以专注于代码的体系结构。 可以将其视为较小服务器  。

容器解决方案为开发人员提供现有的生成脚本，从而将代码发布到无服务器就绪的映像中。 其他实现可使用现有 PaaS 解决方案提供可缩放的体系结构。

抽象意味着 DevOps 团队无需配置或管理服务器，也无需特定容器。 无服务器平台将代码托管为脚本或使用相关 SDK 生成的已打包可执行文件，并为代码缩放分配必要的资源。

下图演示了四个无服务器组件。 HTTP 请求引发 Checkout API 代码运行。 Checkout API 将代码插入数据库，并且插入操作会触发其他几个函数运行，从而可执行诸如计算任务和履行订单之类的任务。

![无服务器实现](./media/serverless-implementation.png)

无服务器的优点包括：

- **高密度。** 与容器或虚拟机相比，相同无服务器代码的许多实例可以在同一主机上运行。 跨多个主机缩放的实例可横向扩展并具有复原能力。
- **微计费。** 大多数无服务器提供程序都基于无服务器执行来计费，因而在某些情况下可以节省大量成本。
- **即时缩放。** 无服务器可以缩放以自动快速地匹配工作负载。
- **上市时间加快。** 开发人员专注于代码并直接部署到无服务器平台。 组件可以相互独立发布。

无服务器是最经常在计算上下文中讨论的内容，但也可以应用于数据。 例如，[Azure SQL](/azure/sql-database) 和 [Cosmos DB](/azure/cosmos-db) 都提供不需要配置主机或群集的云数据库。 本书重点介绍无服务器计算。

## <a name="summary"></a>总结

体系结构有多种可用选项可选，包括混合使用。 无服务器以控制和可移植性为代价，简化了应用程序功能的方法、管理和成本。 但是，许多无服务器平台的确会公开配置，帮助优化解决方案。 好的编程做法还可以产生更易于移植的代码和更少的无服务器平台锁定。 下表说明了各种架构方法。 根据规模需求、是否要管理运行时以及如何将工作负载分解为小组件来选择无服务器。 在下一章中，你将了解无服务器和其他决策点的潜在挑战。

|         |IaaS     |PaaS     |容器|无服务器|
|---------|---------|---------|---------|----------|
|缩放 |VM       |实例 |应用      |函数  |
|**摘要**|硬件|平台|OS 主机|运行时   |
|**单位** |VM       |Project  |Image    |代码      |
|**生存期**|数月|数天到数月|数分钟到数天|数毫秒到数分钟|
|**责任**|应用程序、依赖项、运行时和操作系统|应用程序和依赖项|应用程序、依赖项和运行时|函数

- **缩放**是指用于缩放应用程序的单位
- **摘要**是指实现所抽象的层
- **单元**是指所部署内容的范围
- **生存期**是指特定实例的典型运行时
- **责任**是指生成、部署和维护应用程序的开销

下一章将重点介绍无服务器体系结构、用例和设计模式。

## <a name="recommended-resources"></a>推荐的资源

- [Azure 应用程序体系结构指南](/azure/architecture/guide/)
- [Azure Cosmos DB](/azure/cosmos-db)
- [Azure SQL](/azure/sql-database)
- [N 层体系结构模式](/azure/architecture/guide/architecture-styles/n-tier)
- [Azure 上的 Kubernetes](/azure/aks/intro-kubernetes)
- [微服务](/azure/architecture/guide/architecture-styles/microservices)
- [虚拟机 N 层参考体系结构](/azure/architecture/reference-architectures/virtual-machines-windows/n-tier)
- [虚拟机](/azure/virtual-machines/)
- [什么是 Docker？](../microservices/container-docker-introduction/docker-defined.md)
- [Wingtip Tickets SaaS 应用程序](/azure/sql-database/saas-tenancy-welcome-wingtip-tickets-app)

>[!div class="step-by-step"]
>[上一页](architecture-approaches.md)
>[下一页](serverless-architecture.md)
