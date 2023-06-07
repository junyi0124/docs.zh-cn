---
title: 协调安排微服务和多容器应用程序，实现高可伸缩性和高可用性
description: 必须使用处理所有容器的运行状况、工作负载和生命周期的业务流程协调程序来部署和管理实际的生产应用程序。
ms.date: 08/06/2020
ms.openlocfilehash: e522517d3a1f56d9b6150b580473c02cf7a704d7
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91160822"
---
# <a name="orchestrating-microservices-and-multi-container-applications-for-high-scalability-and-availability"></a>协调安排微服务和多容器应用程序，实现高可伸缩性和高可用性

如果应用程序在微服务基础上构建或跨多个容器拆分，则必须使用适用于生产就绪应用程序的业务流程协调程序。 如前所述，在基于微服务的方法中，每个微服务均拥有其模型和数据，如此从开发和部署的角度来看，它便是具有自主性。 但即使拥有由多个服务组成的更传统的应用程序（如 SOA），也将拥有多个容器或服务，这些容器或服务中包含需要作为分布式系统部署的单个业务应用程序。 这类系统的扩展和管理非常复杂；因此，如果想要拥有生产就绪且可缩放的多容器应用程序，则必须使用业务流程协调程序。

图 4-6 介绍如何部署到由多个微服务（容器）组成的应用程序群集。

![显示群集中的组合 Docker 应用程序的示意图。](./media/orchestrate-high-scalability-availability/composed-docker-applications-cluster.png)

**图 4-6**。 容器群集

它看上去类似于逻辑方法。 但是如何处理负载均衡、路由以及如何协调安排这些组合式应用程序呢？

Docker CLI 满足在一台主机上管理一个容器的需求，但若要管理针对更复杂的分布式应用程序在多台主机上部署的多个容器，则无法满足需求。 大多数情况下，需要一个管理平台，该平台能自动启动容器、横向扩展容器（每个映像具有多个实例）、必要时暂停或关闭容器，并且在理想情况下还能控制资源（如网络和数据存储）访问方式。

如果不仅要管理个别容器或简单的组合式应用，还要进一步管理使用微服务的较大型企业应用程序，则必须转向业务流程和群集平台。

从体系结构和开发的角度来看，如果要生成基于微服务的大型企业应用程序，请务必了解清楚下面列出的支持高级方案的平台和产品：

- **群集和业务流程协调程序。** 如果需要跨多个 Docker 主机横向扩展应用程序，如使用基于微服务的大型应用程序，至关重要的是，能够通过抽象化基础平台的复杂性，将所有主机作为单个群集进行管理。 这就是容器群集和业务流程协调程序所提供的功能。 业务流程协调程序的示例包括 Azure Service Fabric 和 Kubernetes。 Kubernetes 通过 Azure Kubernetes 服务在 Azure 中提供。

- **计划程序。** *计划*意味着管理员能够在群集中启动容器，因此计划程序也可提供用户界面来执行此操作。 群集计划程序具有多个职责：高效使用群集资源、设置用户提供的约束、有效负载均衡节点或主机间的容器，以及在提供高可用性的同时强力解决错误。

群集和计划程序的概念密切相关，因此不同供应商提供的产品通常具有这两套功能。 以下部分显示群集和计划程序最重要的平台和软件选项。 公有云（如 Azure）中普遍提供这些业务流程协调程序。

## <a name="software-platforms-for-container-clustering-orchestration-and-scheduling"></a>适用于容器群集、业务流程和计划的软件平台

| Platform | 注释 |
|:---:|:---|
| **Kubernetes** <br/> ![Kubernetes 徽标图像。](./media/orchestrate-high-scalability-availability/kubernetes-container-orchestration-system-logo.png) | [*Kubernetes*](https://kubernetes.io/) 是一款开源产品，提供各种功能，从群集基础结构和容器计划到安排功能均涵盖在内。 它能实现跨主机群集自动部署、缩放以及执行各种应用程序容器操作。 <br/> <br/> Kubernetes 提供以容器为中心的基础结构，将应用程序容器分组为逻辑单元，以便管理和发现。 <br/> <br/> Kubernetes 在 Linux 中的运用已发展成熟，但在 Windows 中相对较弱。 |
| **Azure Kubernetes 服务 (AKS)** <br/> ![Azure Kubernetes 服务徽标图像。](./media/orchestrate-high-scalability-availability/azure-kubernetes-service-logo.png) | [Azure Kubernetes 服务 (AKS)](https://azure.microsoft.com/services/kubernetes-service/) 是 Azure 中的托管 Kubernetes 容器业务流程服务，简化了 Kubernetes 群集的管理、部署和操作。 |
| **Azure Service Fabric** <br/> ![Azure Service Fabric 徽标的图像。](./media/orchestrate-high-scalability-availability/azure-service-fabric-logo.png) | [Service Fabric](/azure/service-fabric/service-fabric-overview) 是用于生成应用程序的 Microsoft 微服务平台。 它是服务的[业务流程协调程序](/azure/service-fabric/service-fabric-cluster-resource-manager-introduction)，可创建计算机群集。 Service Fabric 可将服务作为容器或纯进程进行部署。 它甚至可以在同一应用程序和群集中将进程中的服务与容器中的服务进行组合。 <br/> <br/> Service Fabric 群集可以在 Azure 中、本地或任意云中部署。 但是，Azure 中的部署使用托管方法进行了简化。 <br/> <br/> Service Fabric 提供其他可选的规定 [Service Fabric 编程模型](/azure/service-fabric/service-fabric-choose-framework)（如[有状态服务](/azure/service-fabric/service-fabric-reliable-services-introduction)和 [Reliable Actors](/azure/service-fabric/service-fabric-reliable-actors-introduction)）。 <br/> <br/> Service Fabric 在 Windows 中的运用已经成熟（已在 Windows 中发展多年），但在 Linux 中相对较弱。 <br/> <br/> 自 2017 年以来，Service Fabric 同时支持 Linux 和 Windows 容器。 |
| **Azure Service Fabric 网格** <br/> ![Azure Service Fabric 网格徽标的图像。](./media/orchestrate-high-scalability-availability/azure-service-fabric-mesh-logo.png) | [Azure Service Fabric 网格](/azure/service-fabric-mesh/service-fabric-mesh-overview)提供与 Service Fabric 相同的可靠性、任务关键性能和规模，但也提供完全托管的无服务器平台。 无需管理群集、虚拟机、存储或网络配置。 只需专注于应用程序的开发。 <br/> <br/> Service Fabric 网格支持 Windows 和 Linux 容器，从而允许使用所选择的任何编程语言和框架进行开发。

## <a name="using-container-based-orchestrators-in-azure"></a>在 Azure 中使用基于容器的业务流程协调程序

多家云供应商提供 Docker 容器支持以及 Docker 群集和业务流程支持，包括 Azure、Amazon EC2 容器服务和 Google 容器引擎。 Azure 通过 Azure Kubernetes 服务 (AKS)、Azure Service Fabric 和 Azure Service Fabric 网格提供 Docker 群集和业务流程协调程序支持。

## <a name="using-azure-kubernetes-service"></a>使用 Azure Kubernetes 服务

Kubernetes 群集汇集多个 Docker 主机，并将它们作为单个虚拟 Docker 主机公开，因此可以将多个容器部署到群集中并使用任何数量的容器实例横向扩展。 该群集将处理可伸缩性、运行状况等所有复杂的管理任务。

AKS 提供了一种方法，可在 Azure 中简化虚拟机（预配置为运行容器化应用程序）群集的创建、配置和管理。 通过使用常用开源计划和业务流程工具的优化配置，AKS 能够利用现有技能或大量不断增长的社区专业知识，在 Microsoft Azure 上部署和管理基于容器的应用程序。

Azure Kubernetes 服务优化了专门针对 Azure 的常用 Docker 群集开源和技术的配置。 可以获得一个开放的解决方案，该解决方案为容器和应用程序配置提供可移植性。 用户选择主机的大小和数量以及业务流程协调程序工具，然后 AKS 可处理其他操作。

![显示 Kubernetes 群集结构的示意图。](./media/orchestrate-high-scalability-availability/kubernetes-cluster-simplified-structure.png)

图 4-7。 Kubernetes 群集的简化结构和拓扑

图 4-7 显示 Kubernetes 群集的结构，其中主节点 (VM) 控制群集的大部分协调，可以将容器部署到从应用程序角度来看是作为单个池托管的其余节点。 这样，可以扩展到数千或甚至数万个容器。

## <a name="development-environment-for-kubernetes"></a>Kubernetes 的开发环境

此外，在 [2018 年 7 月宣布的 Docker](https://blog.docker.com/2018/07/kubernetes-is-now-available-in-docker-desktop-stable-channel/) 开发环境中，只需安装 [Docker 桌面版](https://www.docker.com/products/docker-desktop)，Kubernetes 便可在单个开发计算机（Windows 10 或 macOS）中运行。 可以在以后部署到云 (AKS) 进行进一步集成测试，如图 4-8 所示。

![显示开发计算机上的 Kubernetes 随后部署到 AKS 的示意图。](./media/orchestrate-high-scalability-availability/kubernetes-development-environment.png)

**图 4-8**。 在开发计算机和云中运行 Kubernetes

## <a name="get-started-with-azure-kubernetes-service-aks"></a>Azure Kubernetes 服务 (AKS) 入门

若要开始使用 AKS，需从 Azure 门户或使用 CLI 部署 AKS 群集。 有关向 Azure 部署 Kubernetes 群集的详细信息，请参阅[部署 Azure Kubernetes 服务 (AKS) 群集](/azure/aks/kubernetes-walkthrough-portal)。

作为 AKS 的一部分，默认安装的任何软件都不收费。 所有默认选项都通过开源软件实现。 AKS 可用于 Azure 中的多个虚拟机。 仅针对所选的计算实例以及使用的其他基础结构资源（如存储和网络）收取费用。 AKS 本身不会以增量方式收费。

有关基于 `kubectl` 和原始 `.yaml` 文件部署到 Kubernetes 的进一步实现信息，请参阅[部署到 Azure Kubernetes 服务 (AKS)](https://github.com/dotnet-architecture/eShopOnContainers/wiki/Deploy-to-Azure-Kubernetes-Service-(AKS))。

## <a name="deploy-with-helm-charts-into-kubernetes-clusters"></a>使用 Helm chart 部署到 Kubernetes 群集

将应用程序部署到 Kubernetes 群集时，可如上述章节所述，通过基于本机格式（`.yaml` 文件）的部署文件来使用原始 `kubectl.exe` CLI 工具。 但是，对于更复杂的 Kubernetes 应用程序（如部署基于微服务的复杂应用程序时），建议使用 [Helm](https://helm.sh/)。

Helm 图表有助于对即使最复杂的 Kubernetes 应用程序进行定义、版本控制、安装、共享、升级或回滚。

更进一步，也建议使用 Helm 用法，因为 Azure 中的其他 Kubernetes 环境（如 [Azure Dev Spaces](/azure/dev-spaces/azure-dev-spaces)）也基于 Helm 图表。

Helm 由 [Cloud Native Computing Foundation (CNCF)](https://www.cncf.io/) 与 Microsoft、Google、Bitnami 和 Helm 参与者社区协作维护。

有关 Helm 图表和 Kubernetes 的进一步实现信息，请参阅名为[使用 Helm 安装 eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers/wiki/Deploy-to-Azure-Kubernetes-Service-(AKS)#install-eshoponcontainers-using-helm) 的部分。

## <a name="use-azure-dev-spaces-for-you-kubernetes-application-lifecycle"></a>将 Azure Dev Spaces 用于 Kubernetes 应用程序生命周期

[Azure Dev Spaces](/azure/dev-spaces/azure-dev-spaces) 为团队提供快速的迭代 Kubernetes 开发体验。 通过尽可能少的开发计算机设置，便可以直接在 Azure Kubernetes 服务 (AKS) 中以迭代方式运行和调试容器。 可以使用熟悉的工具（如 Visual Studio、Visual Studio Code 或命令行）在 Windows、Mac 或 Linux 上进行开发。

如前所述，Azure Dev Spaces 在部署基于容器的应用程序时使用 Helm 图表。

Azure Dev Spaces 可帮助开发团队在 Kubernetes 上提高工作效率，因为它允许只需使用 Visual Studio 2017 或 Visual Studio Code，便可直接在 Azure 中的全局 Kubernetes 群集中快速迭代和调试代码。 Azure 中的这一 Kubernetes 群集是共享的托管 Kubernetes 群集，因此团队可以通过协作方式一起工作。 可以独立开发代码，然后部署到全局群集并对其他组件进行端到端测试，无需复制或模拟依赖项。

如图 4-9 所示，Azure Dev Spaces 中最独特的功能是能够创建可以运行的“空间”，这些空间会集成到群集中全局部署的其余部分：

![显示在 Azure Dev Spaces 中使用多个空间的示意图。](./media/orchestrate-high-scalability-availability/use-multiple-spaces-azure-dev.png)

**图 4-9**. 在 Azure Dev Spaces 中使用多个空间

Azure Dev Spaces 可以通过透明方式混合搭配生产微服务与开发容器实例，以便简化新版本的测试。 基本上可以在 Azure 中设置共享开发空间。 每位开发者都可以专注于自己负责的应用程序部分，并可以在已包含其方案所依赖的所有其他服务和云资源的开发空间中迭代开发“预提交”代码。 依赖项始终保持最新，开发者能够以真实反映生产的方式进行工作。

Azure Dev Spaces 提供了空间概念，使你可以独立地工作，而不必担心会干扰团队成员。 此功能基于 URL 前缀。因此如果在 URL 中为容器的请求使用开发空间前缀，Azure Dev Spaces 将运行为该空间部署的容器的特殊版本（如果存在）。 否则，会运行全局/合并版本。

有关具体示例，请参阅 [Azure Dev Spaces 上的 eShopOnContainers wiki 页面](https://github.com/dotnet-architecture/eShopOnContainers/wiki/Azure-Dev-Spaces)。

有关进一步信息，请参阅[使用 Azure Dev Spaces 进行团队开发](/azure/dev-spaces/team-development-netcore)。

## <a name="additional-resources"></a>其他资源

- **Azure Kubernetes 服务 (AKS) 入门** \
  <https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal>

- **Azure Dev Spaces** \
  <https://docs.microsoft.com/azure/dev-spaces/azure-dev-spaces>

- **Kubernetes。** 官方网站。 \
  <https://kubernetes.io/>

## <a name="using-azure-service-fabric"></a>使用 Azure Service Fabric

Azure Service Fabric 源于 Microsoft 从交付“盒子”产品到交付各种服务的转换，前者通常为整体式产品。 Service Fabric 的出现是由于大规模构建和运营大型服务，例如 Azure SQL 数据库、Azure Cosmos DB、Azure 服务总线、Cortana 后端。 越来越多的服务采用了该平台，它随着时间不断演变。 Service Fabric 不仅需要在 Azure 上运行，还需在独立的 Windows Server 部署上运行，这一点十分重要。

Service Fabric 旨在解决如何构建和运行服务，并有效利用基础结构资源等难题，让团队能够使用微服务方法来解决业务问题。

Service Fabric 提供了两个宽广区域，可帮助构建使用微服务方法的应用程序：

- 提供各种系统服务的平台，可用于部署、缩放、升级、检测、重新启动失败的服务、发现服务位置、管理状态和监视健康状况。 实际上，这些服务具备了很多前面介绍的微服务的特征。

- 有助于将应用程序构建为微服务的编程 API 或框架：[Reliable Actors 和 Reliable Services](/azure/service-fabric/service-fabric-choose-framework)。 开发人员可选择任何代码来构建微服务，但这些 API 可简化构建工作，且更深入地与平台集成。 这样开发人员就可获得运行状况信息和诊断信息，也可利用可靠的状态管理。

Service Fabric 未指定服务构建方式，开发人员可使用任何技术。 但它内置了编程 API，可简化微服务的构建。

如图 4-10 所示，开发人员可在 Service Fabric 中创建和运行微服务，既可作为简单进程也可作为 Docker 容器运行。 此外还可在同一 Service Fabric 群集中混合基于容器的微服务与基于进程的微服务。

![显示 Azure Service Fabric 群集比较情况的示意图。](./media/orchestrate-high-scalability-availability/azure-service-fabric-cluster-types.png)

**图 4-10**。 在 Azure Service Fabric 中将微服务部署为进程或容器

在第一个图像中，你将看到作为进程的微服务，其中每个节点运行每个微服务的一个进程。 在第二个图中，你将看到作为容器的微服务，其中每个节点运行包含多个容器的 Docker（每个微服务对应一个容器）。 基于 Linux 和 Windows 主机的 Service Fabric 群集可分别运行 Docker Linux 容器和 Windows 容器。

有关 Azure Service Fabric 中容器支持的最新信息，请参阅 [Service Fabric 和容器](/azure/service-fabric/service-fabric-containers-overview)。

Service Fabric 充分展示了什么是一个好的平台，开发人员可在其中定义不同于物理实现的逻辑体系结构（业务微服务或边界上下文）。 例如，如果在 [Azure Service Fabric](/azure/service-fabric/service-fabric-overview) 中实现[有状态可靠服务](/azure/service-fabric/service-fabric-reliable-services-introduction)（将在[无状态微服务与有状态微服务](#stateless-versus-stateful-microservices)部分介绍），则会获得一个由多个物理服务的业务微服务概念。

如图 4-10 所示，从逻辑/业务微服务角度考虑，实现 Service Fabric 有状态可靠服务时，通常需要实现两个层级的服务。 第一层是后端有状态可靠服务，用于处理多个分区（每个分区均为一项有状态服务）。 第二层是前端服务或网关服务，负责跨多个分区或有状态服务实例进行路由，聚合数据。 该网关服务还处理访问后端服务的客户端通信，通信带有重试循环。 如果实现自定义服务，则其称为网关服务；或者，还可使用现成的 Service Fabric [反向代理](/azure/service-fabric/service-fabric-reverseproxy)。

![显示容器中的多个有状态服务的示意图。](./media/orchestrate-high-scalability-availability/service-fabric-stateful-business-microservice.png)

**图 4-11**。 具有多个有状态服务实例和一个自定义网关前端的业务微服务

在任何情况下，使用 Service Fabric 有状态可靠服务时，都伴有由多个物理服务组成的逻辑或业务微服务（边界上下文）。 这两项服务，以及网关服务和分区服务都可作为 ASP.NET Web API 服务实现，如图 4-11 所示。 Service Fabric 提供了在容器中支持多个有状态可靠服务的方案。

在 Service Fabric 中，可对成组服务进行分组，并将其部署为 [Service Fabric 应用程序](/azure/service-fabric/service-fabric-application-model)，即业务流程协调程序或群集的打包和部署单元。 因此，Service Fabric 应用程序同样可映射到该自治业务和逻辑微服务边界或边界上下文，这样开发人员就可以自主部署这些服务。

### <a name="service-fabric-and-containers"></a>Service Fabric 和容器

对于 Service Fabric 中的容器，开发人员也可在 Service Fabric 群集中的容器映像内部署服务。 如图 4-12 所示，大多数情况下，每个服务仅有一个容器。

![显示每个服务的一个容器送入数据库的示意图。](./media/orchestrate-high-scalability-availability/azure-service-fabric-business-microservice.png)

图 4-12。 Service Fabric 中具有多个服务（容器）的业务微服务

Service Fabric 应用程序可以运行访问外部数据库的多个容器，并且整个集将作为业务微服务的逻辑边界。 但是，Service Fabric 中也可能有所谓的“sidecar”容器（两个容器必须一同部署为逻辑服务）。 重要的是，业务微服务是多个内聚元素的逻辑边界。 在许多情况下，它可能是具有单个数据模型的单个服务，但在其他一些情况下，也可能具有多个物理服务。

请注意，可在同一 Service Fabric 应用程序中混合进程中的服务和容器中的服务，如图 4-13 所示。

![显示同一应用中进程和容器中的服务的示意图。](./media/orchestrate-high-scalability-availability/business-microservice-mapped-to-service-fabric-application.png)

图 4-13。 映射到含容器和有状态服务的 Service Fabric 应用程序的业务微服务

有关 Azure Service Fabric 中容器支持的详细信息，请参阅 [Service Fabric 和容器](/azure/service-fabric/service-fabric-containers-overview)。

## <a name="stateless-versus-stateful-microservices"></a>无状态微服务与有状态微服务

如前所述，每个微服务（逻辑边界上下文）必须有其域模型（数据和逻辑）。 对于无状态微服务，数据库来自外部，采用 SQL Server 等关系选项，或 Azure Cosmos DB 或 MongoDB 等 NoSQL 选项。

但在 Service Fabric 中，服务本身可以是有状态的，也就是说数据驻留在微服务中。 此数据不仅能存在于同一服务器中，还可以存在于微服务进程中、内存中、硬盘驱动器中，并能复制到其他节点。 图 4-14 显示了不同的方法。

![显示无状态服务和有状态服务的比较情况的示意图。](./media/orchestrate-high-scalability-availability/stateless-vs-stateful-microservices.png)

图 4-14。 无状态微服务与有状态微服务

在无状态服务中，状态（持久性、数据库）保留在微服务外部。 在有状态服务中，状态保留在微服务内部。 无状态方法是完全有效的，相比有状态微服务，它更易实现，因为这种方法类似于用户熟悉的传统模式。 但无状态微服务会在进程和数据源之间产生延迟。 若要试图增加缓存和队列来提高性能，该方法会涉及更多要移动的内容。 结果就是，最终会得到具有许多层级的复杂体系结构。

相反，[有状态微服务](/azure/service-fabric/service-fabric-reliable-services-introduction#when-to-use-reliable-services-apis)采用的方案更高级，因为域逻辑和数据之间没有延迟。 有状态服务有利于处理繁重数据、游戏后端、数据库即服务和其他低延迟方案，因为它能启用本地状态，提高访问速度。

无状态服务和有状态服务相辅相成。 例如，如图 4-14 右图所示，有状态服务可拆分为多个分区。 若要访问这些分区，可能需要无状态服务充当网关服务，该服务了解如何基于分区键处理每个分区。

有状态服务确实存在缺点。 它们强加了一个高度复杂的级别，以便横向扩展。完成跨有状态微服务和数据分区复制数据等任务时，必须解决通常由外部数据库系统实现的功能。 但这正是含[有状态可靠服务](/azure/service-fabric/service-fabric-reliable-services-introduction#when-to-use-reliable-services-apis)的 [Azure Service Fabric](/azure/service-fabric/service-fabric-reliable-services-platform-architecture) 等业务流程协调程序可发挥重要作用的一方面，它可使用 [Reliable Services API](/azure/service-fabric/service-fabric-work-with-reliable-collections) 和 [Reliable Actors](/azure/service-fabric/service-fabric-reliable-actors-introduction) 简化有状态微服务的开发过程和生命周期。

其他微服务框架来自 Microsoft Research 的 Microsoft [Orleans](https://github.com/dotnet/orleans) 和 [Akka.NET](https://getakka.net/)，这些框架允许有状态服务、支持 Actor 模式并改进了业务逻辑和数据之间的容错与延迟。 这两个框架目前都在改善其对 Docker 的支持。

请记住，Docker 容器本身是无状态的。 若要实现有状态服务，需到前面提到的某个级别更高的规范框架。

## <a name="using-azure-service-fabric-mesh"></a>使用 Azure Service Fabric 网格

Azure Service Fabric 网格是一种完全托管的服务，它使开发人员可以构建和部署关键的应用程序，而无需管理任何基础结构。 使用 Service Fabric 网格可构建并运行按需缩放的安全的分布式微服务应用程序。

如图 4-15 所示，Service Fabric 网格上托管的应用程序进行运行和缩放时，无需考虑为它提供支持的基础结构。

![显示从本地存储库到 Service Fabric 网格的部署的示意图。](media/orchestrate-high-scalability-availability/deploy-microservice-containers-apps-service-fabric-mesh.png)

**图 4-15**。 将微服务/容器应用程序部署到 Service Fabric 网格

事实上，Service Fabric 网格由包含数千台计算机的群集组成。 所有群集操作都向开发人员隐藏。 只需上传容器并指定所需资源、可用性要求和资源限制。 Service Fabric 网格会自动分配应用程序部署所请求的基础结构，还会处理基础结构故障，从而确保应用程序高度可用。 只需关心应用程序的运行状况和响应能力，而不是基础结构。

有关进一步信息，请参阅 [Service Fabric 网格文档](/azure/service-fabric-mesh/)。

## <a name="choosing-orchestrators-in-azure"></a>在 Azure 中选择业务流程协调程序

下表提供有关根据工作负荷和 OS 焦点使用哪个业务流程协调程序的指南。

![比较 Kubernetes 和 Service Fabric 的表的图像。](media/orchestrate-high-scalability-availability/orchestrator-selection-azure-guidance.png)

**图 4-16**。 Azure 指南中的业务流程协调程序选择

>[!div class="step-by-step"]
>[上一页](soa-applications.md)
>[下一页](deploy-azure-kubernetes-service.md)
