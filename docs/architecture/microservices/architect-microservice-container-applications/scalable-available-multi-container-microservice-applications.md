---
title: 安排微服务和多容器应用应用程序的业务流程，以实现高可伸缩性和高可用性
description: 发现用于安排微服务和多容器应用程序以便实现高可伸缩性和可用性的选项，以及 Azure Dev Spaces 在开发 Kubernetes 应用程序生命周期时的可能性。
ms.date: 01/30/2020
ms.openlocfilehash: a61e883ab0d27300e00b177c2621c6521e85ac84
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91172497"
---
# <a name="orchestrate-microservices-and-multi-container-applications-for-high-scalability-and-availability"></a>安排微服务和多容器应用应用程序的业务流程，以实现高可伸缩性和高可用性

如果应用程序基于微服务或只是跨多个容器拆分，则必须使用适用于生产就绪应用程序的业务流程协调程序。 如前所述，在基于微服务的方法中，每个微服务均拥有其模型和数据，如此从开发和部署的角度来看，它便是具有自主性。 但即使拥有由多个服务组成的更传统的应用程序（如 SOA），也将拥有多个容器或服务，这些容器或服务中包含需要作为分布式系统部署的单个业务应用程序。 这类系统的扩展和管理非常复杂；因此，如果想要拥有生产就绪且可缩放的多容器应用程序，则必须使用业务流程协调程序。

图 4-23 介绍如何部署到由多个微服务（容器）组成的应用程序群集。

![显示群集中的组合 Docker 应用程序的示意图。](./media/scalable-available-multi-container-microservice-applications/composed-docker-applications-cluster.png)

图 4-23  。 容器群集

为每个服务实例使用一个容器。 Docker 容器是“部署单元”，一个容器代表 Docker 的一个实例。 一个主机可以处理多个容器。 它看上去类似于逻辑方法。 但是如何处理负载均衡、路由以及如何协调安排这些组合式应用程序呢？

单个 Docker 主机中的普通 Docker 引擎能够满足在一台主机上管理单映像实例的需求，但若要管理针对更复杂的分布式应用程序而部署在多个主机上的多个容器，则无法满足需求。 大多数情况下，需要一个管理平台，该平台能自动启动容器、扩展容器（每个映像具有多个实例）、必要时暂停或关闭，并且在理想情况下还能控制资源（如网络和数据存储）访问方式。

如果不仅要管理个别容器或非常简单的组合式应用，还要进一步管理使用微服务的较大型企业应用程序，则必须转向业务流程和群集平台。

从体系结构和开发的角度来看，如果要生成由基于微服务的应用程序组成的大型企业应用程序，则务必了解清楚下面列出的支持高级方案的平台和产品：

**群集和业务流程协调程序。** 如果需要跨多个 Docker 主机扩展应用程序例如生成基于微服务的大型应用程序时，能够通过抽象化基础平台的复杂性来将所有主机作为单个群集管理是至关重要的。 这就是容器群集和业务流程协调程序所提供的功能。 Kubernetes 是业务流程协调程序的一个示例，通过 Azure Kubernetes 服务在 Azure 中提供。

**计划程序。** 计划  意味着管理员能够在群集中启动容器，如此这些容器也可提供 UI。 群集计划程序具有多个职责：高效使用群集资源、设置用户提供的约束、有效负载均衡节点或主机间的容器，以及在提供高可用性的同时强力解决错误。

群集和计划程序的概念密切相关，因此不同供应商提供的产品通常具有这两套功能。 下表显示了适用于群集和计划程序的最重要的平台和软件选项。 通常在公有云（如 Azure）中提供这些业务流程协调程序。

## <a name="software-platforms-for-container-clustering-orchestration-and-scheduling"></a>适用于容器群集、业务流程和计划的软件平台

|     |   |
|:---:|---|
| **Kubernetes** <br> ![Kubernetes 徽标图像。](./media/scalable-available-multi-container-microservice-applications/kubernetes-container-orchestration-system-logo.png) | [*Kubernetes*](https://kubernetes.io/) 是一款开源产品，提供各种功能，从群集基础结构和容器计划到安排功能均涵盖在内。 它能实现跨主机群集自动部署、缩放以及执行各种应用程序容器操作。 <br><br> Kubernetes  提供以容器为中心的基础结构，将应用程序容器分组为逻辑单元，以便管理和发现。 <br><br> Kubernetes  在 Linux 中的运用已发展成熟，但在 Windows 中相对较弱。 |
| **Azure Kubernetes 服务 (AKS)** <br> ![Azure Kubernetes 服务徽标图像。](./media/scalable-available-multi-container-microservice-applications/azure-kubernetes-service-logo.png) | [AKS](https://azure.microsoft.com/services/kubernetes-service/) 是 Azure 中的托管 Kubernetes 容器业务流程服务，简化了 Kubernetes 群集的管理、部署和操作。 |

## <a name="using-container-based-orchestrators-in-microsoft-azure"></a>在 Microsoft Azure 中使用基于容器的业务流程协调程序

多家云供应商提供 Docker 容器支持以及 Docker 群集和业务流程支持，包括 Microsoft Azure、Amazon EC2 容器服务和 Google 容器引擎。 Microsoft Azure 通过 Azure Kubernetes 服务 (AKS) 提供 Docker 群集和业务流程协调程序支持。

## <a name="using-azure-kubernetes-service"></a>使用 Azure Kubernetes 服务

Kubernetes 群集汇集多个 Docker 主机，并将它们作为单个虚拟 Docker 主机公开，因此可以将多个容器部署到群集中并使用任何数量的容器实例横向扩展。 该群集将处理可伸缩性、运行状况等所有复杂的管理任务。

AKS 提供了一种方法，可在 Azure 中简化虚拟机（预配置为运行容器化应用程序）群集的创建、配置和管理。 通过使用常用开源计划和业务流程工具的优化配置，AKS 能够利用现有技能或大量不断增长的社区专业知识，在 Microsoft Azure 上部署和管理基于容器的应用程序。

Azure Kubernetes 服务优化了专门针对 Azure 的常用 Docker 群集开源和技术的配置。 可以获得一个开放的解决方案，该解决方案为容器和应用程序配置提供可移植性。 用户选择主机的大小和数量以及业务流程协调程序工具，然后 AKS 可处理其他操作。

![显示 Kubernetes 群集结构的示意图。](./media/scalable-available-multi-container-microservice-applications/kubernetes-cluster-simplified-structure.png)

图 4-24  。 Kubernetes 群集的简化结构和拓扑

在图 4-24 中可以看到 Kubernetes 群集的结构，其中主节点（虚拟机）控制群集的大部分协调，可以将容器部署到其余节点，这些节点从应用程序角度来看是作为单个池进行管理，允许扩展到数千甚至是数万个容器。

## <a name="development-environment-for-kubernetes"></a>Kubernetes 的开发环境

在开发环境中，[Docker 于 2018 年 7 月宣布](https://blog.docker.com/2018/07/kubernetes-is-now-available-in-docker-desktop-stable-channel/)只需安装 [Docker 桌面](https://docs.docker.com/install/)，Kubernetes 便还可以在单个开发计算机（Windows 10 或 macOS）中运行。 可以在以后部署到云 (AKS) 进行进一步集成测试，如图 4-25 所示。

![显示开发计算机上的 Kubernetes 随后部署到 AKS 的示意图](./media/scalable-available-multi-container-microservice-applications/kubernetes-development-environment.png)

图 4-25  。 在开发计算机和云中运行 Kubernetes

## <a name="getting-started-with-azure-kubernetes-service-aks"></a>Azure Kubernetes 服务 (AKS) 入门

若要开始使用 AKS，需从 Azure 门户或使用 CLI 部署 AKS 群集。 有关在 Azure 中部署 Kubernetes 群集的详细信息，请参阅[部署 Azure Kubernetes 服务 (AKS) 群集](/azure/aks/kubernetes-walkthrough-portal)。

作为 AKS 的一部分，默认安装的任何软件都不收费。 所有默认选项都通过开源软件实现。 AKS 可用于 Azure 中的多个虚拟机。 仅针对所选的计算实例以及使用的其他基础结构资源（如存储和网络）收取费用。 AKS 本身不会以增量方式收费。

Kubernetes 的默认生产部署选项是使用 Helm 图表，这将在下一部分中进行介绍。

## <a name="deploy-with-helm-charts-into-kubernetes-clusters"></a>使用 Helm chart 部署到 Kubernetes 群集

将应用程序部署到 Kubernetes 群集时，可以如上一节中已经提到的那样，通过基于本机格式（.yaml 文件）的部署文件来使用原始 kubectl.exe CLI 工具。 但是，对于更复杂的 Kubernetes 应用程序（如部署基于微服务的复杂应用程序时），建议使用 [Helm](https://helm.sh/)。

Helm 图表可帮助对即使最复杂的 Kubernetes 应用程序进行定义、版本控制、安装、共享、升级或回滚。

更进一步，也建议使用 Helm 用法，因为 Azure 中的其他 Kubernetes 环境（如 [Azure Dev Spaces](/azure/dev-spaces/azure-dev-spaces)）也基于 Helm 图表。

Helm 由 [Cloud Native Computing Foundation (CNCF)](https://www.cncf.io/) 与 Microsoft、Google、Bitnami 和 Helm 参与者社区协作维护。

有关 Helm 图表和 Kubernetes 的详细实现信息，请参阅[使用 Helm 图表将 eShopOnContainers 部署到 AKS](https://github.com/dotnet-architecture/eShopOnContainers/wiki/Deploy-to-Azure-Kubernetes-Service-(AKS)) 的帖子。

## <a name="use-azure-dev-spaces-for-your-kubernetes-application-lifecycle"></a>将 Azure Dev Spaces 用于 Kubernetes 应用程序生命周期

[Azure Dev Spaces](/azure/dev-spaces/azure-dev-spaces) 为团队提供快速的迭代 Kubernetes 开发体验。 通过尽可能少的开发计算机设置，便可以直接在 Azure Kubernetes 服务 (AKS) 中以迭代方式运行和调试容器。 在 Windows、Mac 或 Linux 上使用 Visual Studio、Visual Studio Code 中或命令行等熟悉的工具进行开发。

如前所述，Azure Dev Spaces 在部署基于容器的应用程序时使用 Helm 图表。

Azure Dev Spaces 可帮助开发团队在 Kubernetes 上提高工作效率，因为它允许只需使用 Visual Studio 2019 或 Visual Studio Code，便可直接在 Azure 中的全局 Kubernetes 群集中快速迭代和调试代码。 Azure 中的这一 Kubernetes 群集是共享的托管 Kubernetes 群集，因此团队可以通过协作方式一起工作。 可以独立开发代码，然后部署到全局群集并对其他组件进行端到端测试，无需复制或模拟依赖项。

如图 4-26 所示，Azure Dev Spaces 中最独特的功能是能够创建可以运行的“空间”，它们会集成到群集中全局部署的其余部分。

![显示在 Azure Dev Spaces 中使用多个空间的示意图。](./media/scalable-available-multi-container-microservice-applications/use-multiple-spaces-azure-dev.png)

**图 4-26**。 在 Azure Dev Spaces 中使用多个空间

基本上可以在 Azure 中设置共享开发空间。 每位开发者都可以专注于自己负责的应用程序部分，并可以在已包含其方案所依赖的所有其他服务和云资源的开发空间中迭代开发预提交代码。 依赖项始终保持最新，开发者能够以真实反映生产的方式进行工作。

Azure Dev Spaces 提供了空间这一概念，允许你在相对隔离的环境中工作，而无需担心干扰你的团队工作。 每个开发空间都是层次结构的一部分，允许你使用你自己进行中的微服务从“顶部”主开发空间替代一个（或多个）微服务。

此功能基于 URL 前缀，因此，在使用 URL 中的任何开发空间前缀时，如果在开发空间中存在该前缀，则将从目标微服务提供请求，否则，它将被转发到在层次结构中找到的目标微服务的第一个实例，并最终达到顶部的主开发空间。

若要获取具体示例的实际视图，请参阅 [Azure Dev Spaces 上的 eShopOnContainers wiki 页面](https://github.com/dotnet-architecture/eShopOnContainers/wiki/Azure-Dev-Spaces)。

有关进一步信息，请查看[使用 Azure Dev Spaces 进行团队开发](/azure/dev-spaces/team-development-netcore)上的文章。

## <a name="additional-resources"></a>其他资源

- **Azure Kubernetes 服务 (AKS) 入门** \
  <https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal>

- **Azure Dev Spaces** \
  <https://docs.microsoft.com/azure/dev-spaces/azure-dev-spaces>

- **Kubernetes** 官方网站。 \
  <https://kubernetes.io/>

>[!div class="step-by-step"]
>[上一页](resilient-high-availability-microservices.md)
>[下一页](../docker-application-development-process/index.md)
