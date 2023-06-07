---
title: 演练和技术入门概述
description: 通过 Azure 云和 Windows 容器现代化现有 .NET 应用程序 | 演练和技术入门概述
ms.date: 04/28/2018
ms.openlocfilehash: 98d33b13d2b28bfe1c35894df45e525cff0520c1
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91172139"
---
# <a name="walkthroughs-and-technical-get-started-overview"></a>演练和技术入门概述

为了限制此电子书的大小，GitHub 存储库中提供了其他技术文档和完整演练。 本章中所述的联机演练系列介绍了基于 Windows 容器和部署到 Azure 的多个环境的分步设置。

以下部分介绍每个演练的具体内容、目标和高级构想，并提供所涉及任务的关系图。 可以在 <https://github.com/dotnet-architecture/eShopModernizing/wiki> 的 eShopModernizing 应用 GitHub 存储库 wiki 中获取演练本身  。

## <a name="technical-walkthrough-list"></a>技术演练列表

以下入门演练提供示例应用（可以使用容器进行直接迁移，然后使用 Azure 中的多个部署选项进行移动）的一致且全面的技术指南。

以下每个演练都使用新的示例 eShopLegacy 和 eShopModernizing 应用，这些应用可从 <https://github.com/dotnet-architecture/eShopModernizing> 的 GitHub 中获得。

- **eShop 旧版应用（要现代化的基准应用）的教程**

- **使用 Windows 容器容器化现有 ASP.NET Web 应用（WebForms 和 MVC）**

- **使用 Windows 容器容器化现有 WCF 服务（N 层应用）**

- **将基于 Windows 容器的应用部署到 Azure VM**

- **将基于 Windows 容器的应用部署到 Azure 容器服务中的 Kubernetes**

## <a name="walkthrough-1-tour-of-eshop-legacy-apps"></a>演练 1：eShop 旧版应用的教程

### <a name="technical-walkthrough-availability"></a>技术演练可用性

eShopModernizing GitHub 存储库 wiki 中提供了完整的技术演练：

[eShopModernizing wiki 演练](https://github.com/dotnet-architecture/eShopModernizing/wiki)

### <a name="overview"></a>概述

在本演练中，可以浏览三个示例旧版应用程序的初始实现。 前两个示例 Web 应用具有整体式体系结构，并且是使用经典 ASP.NET 创建的。 第一个应用程序基于 ASP.NET 4.x MVC；第二个应用程序基于 ASP.NET 4.x Web Forms。
第三个应用是由客户端 WinForms 应用和服务器端 [Windows Communication Foundation (WCF)](../../framework/wcf/whats-wcf.md) 服务组成的 3 层应用。

[eShopModernizing GitHub 存储库](https://github.com/dotnet-architecture/eShopModernizing)中提供了所有这些应用程序。

### <a name="goals"></a>目标

本演练的主要目的只是熟悉这些应用及其代码和配置。 可以配置应用，使其生成并使用模拟数据（而无需使用 SQL 数据库）进行测试。 此可选配置以分离的方式基于依赖项注入。

### <a name="scenario-1-aspnet-web-apps"></a>应用场景 1：ASP.NET Web 应用

下图显示了原始旧版 ASP.NET Web 应用程序的简单方案。

![原始旧版 ASP.NET Web 应用程序的简单体系结构方案](./media/image5-1.png)

从业务域角度来看，这两个应用提供相同的目录管理功能。 eShop 企业团队的成员将使用该应用查看和编辑产品目录。

下图显示了初始应用屏幕截图。

![ASP.NET MVC 和 ASP.NET Web Forms 应用程序（现有/旧技术）](./media/image5-2.png)

ASP.NET 4.x 或早期版本中的依赖项（对于 MVC 或 Web Forms）意味着这些应用程序不会在 .NET Core 上运行，除非使用 ASP.NET Core MVC 来完全重写代码。

### <a name="scenario-2-wcf-service-and-winforms-client-app-3-tier-app"></a>应用场景 2：WCF 服务和 WinForms 客户端应用（3 层应用）

下图显示了原始 3 层旧版应用程序的简单方案。

![具有 WCF 服务和 WinForms 客户端应用的原始旧版 3 层应用的简单体系结构方案](./media/image5-1.5.png)

### <a name="benefits"></a>优点

此演练的优点非常简单：只是熟悉代码和初始应用。

### <a name="next-steps"></a>后续步骤

在 GitHub wiki 上更深入了解此内容：

- [有关基准 ASP.NET MVC 和 Web Forms“旧版”应用的教程](https://github.com/dotnet-architecture/eShopModernizing/wiki/01.-Tour-on-the-ASP.NET-MVC-and-WebForms-apps-implementation-code)
- [有关基准 WCF 服务和 WinForms（3 层）“旧版”应用的教程](https://github.com/dotnet-architecture/eShopModernizing/wiki/21.-Tour-on-the-WCF-service-and-WinForms-apps)

## <a name="walkthrough-2-containerize-your-existing-net-applications-with-windows-containers"></a>演练 2：使用 Windows 容器容器化现有 .NET 应用程序

### <a name="overview"></a>概述

使用 Windows 容器改进现有 .NET 应用程序（例如，基于 MVC、Web Forms 或 WCF 的应用程序）到生产、开发和测试环境的部署。

### <a name="goals"></a>目标

本演练的目的是演示用于容器化现有 .NET Framework 应用程序的多个选项。 你可以：

- 使用 [Visual Studio 2017 Tools for Docker](/aspnet/core/host-and-deploy/docker/visual-studio-tools-for-docker)（Visual Studio 2017 或更高版本）容器化应用程序。

- 通过手动添加 [Dockerfile](https://docs.docker.com/engine/reference/builder/)，然后使用 [Docker CLI](https://docs.docker.com/engine/reference/commandline/cli/) 来容器化应用程序。

- 使用 [Img2Docker](https://github.com/docker/communitytools-image2docker-win) 工具（Docker 的开源工具）容器化应用程序。

本演练重点介绍 Visual Studio 2017 Tools for Docker 方法，但另外两种方法与使用 Dockerfile 非常相似。

### <a name="scenario-1-containerized-aspnet-web-apps"></a>应用场景 1：容器化 ASP.NET Web 应用

下图显示了容器化 eShop 旧版 Web 应用应用程序的方案。

![开发环境中的容器化 ASP.NET 应用程序的简化体系结构关系图](./media/image5-3.png)

### <a name="scenario-2-containerized-wcf-service"></a>应用场景 2：容器化 WCF 服务

下图显示了具有容器化 WCF 服务的 3 层应用的方案。

![开发环境中的容器化 WCF 服务的简化体系结构关系图](./media/image5-3.5.png)

### <a name="benefits"></a>优点

在容器中运行整体式应用程序有优点。 首先，创建应用程序的映像。 自此，每个部署会在相同的环境中运行。 每个容器会使用相同的操作系统版本，安装相同版本的依赖项，使用同一个 .NET Framework 版本，并通过同一个流程生成。 基本上，使用 Docker 映像控制应用程序的依赖项。 部署容器时，依赖项随应用程序移动。

另一个优点是，开发人员可以在 Windows 容器提供的一致环境中运行应用程序。 仅某些版本中出现的问题会立即被发现，而不会出现在过渡或生产环境中。 应用程序在容器中运行后，开发团队的成员使用的开发环境之间的差异将不那么重要。

容器化应用程序还具有较为平缓的的横向扩展曲线。 使用容器化应用，VM 或物理计算机可以拥有更多应用程序和服务实例（与每台计算机的常规应用程序部署相比）。 这将转换为更高的密度和更少的所需资源，尤其是在使用业务流程协调程序（如 Kubernetes）时。

在理想情况下，容器化不需要对应用程序代码 (C\#) 进行任何更改。 在大多数情况下，只需要 Docker 部署元数据文件（Dockerfile 和 Docker Compose 文件）。

### <a name="next-steps"></a>后续步骤

在 GitHub wiki 上更深入了解此内容：

- [如何使用 Windows 容器和 Docker 容器化 .NET Framework Web 应用](https://github.com/dotnet-architecture/eShopModernizing/wiki/02.-How-to-containerize-the-.NET-Framework-web-apps-with-Windows-Containers-and-Docker)
- [向 WCF 服务添加 Docker 支持](https://github.com/dotnet-architecture/eShopModernizing/wiki/22.-Adding-Docker-Support)

## <a name="walkthrough-3-deploy-your-windows-containers-based-app-to-azure-vms"></a>演练 3：将基于 Windows 容器的应用部署到 Azure VM

### <a name="technical-walkthrough-availability"></a>技术演练可用性

eShopModernizing GitHub 存储库 wiki 中提供了完整的技术演练：<https://github.com/dotnet-architecture/eShopModernizing/wiki/06.-Deploying-your-Windows-Containers-based-app-into-Azure-VMs-(Including-CI-CD)>

### <a name="overview"></a>概述

通过在 Azure 中部署到 Windows Server 2016 虚拟机 (VM) 上的 Docker 主机，可以快速设置开发/测试/过渡环境。 还可以为你提供一个公共场所，供测试人员或业务用户验证应用。 VM 还可以是有效的基础结构即服务 (IaaS) 生产环境。

### <a name="goals"></a>目标

本演练的目的是介绍将 Windows 容器部署到基于 Windows Server 2016 或更高版本的 Azure VM 的多个替代方案。

### <a name="scenarios"></a>方案

本演练介绍了多个方案。

#### <a name="scenario-a-deploy-to-an-azure-vm-from-a-dev-pc-through-docker-engine-connection"></a>方案 A：通过 Docker 引擎连接从开发电脑部署到 Azure VM

![通过 Docker 引擎连接从开发电脑部署到 Azure VM](./media/image5-4.png)

**图 5-4**。 通过 Docker 引擎连接从开发电脑部署到 Azure VM

#### <a name="scenario-b-deploy-to-an-azure-vm-through-a-docker-registry"></a>情况 B：通过 Docker 注册表部署到 Azure VM

![通过 Docker 注册表部署到 Azure VM](./media/image5-5.png)

**图 5-5**。 通过 Docker 注册表部署到 Azure VM

#### <a name="scenario-c-deploy-to-an-azure-vm-from-cicd-pipelines-in-azure-devops-services"></a>方案 C：从 Azure DevOps Services 中的 CI/CD 管道部署到 Azure VM

![从 Azure DevOps Services 中的 CI/CD 管道部署到 Azure VM](./media/image5-6.png)

**如 5-6**。 从 Azure DevOps Services 中的 CI/CD 管道部署到 Azure VM

### <a name="azure-vms-for-windows-containers"></a>适用于 Windows 容器的 Azure VM

适用于 Windows 容器的 Azure VM 是基于安装了 Docker 引擎的 Windows Server 2016、Windows 10 或更高版本的 VM。 在大多数情况下，在 Azure VM 中使用 Windows Server 2016。

Azure 当前提供名为“包含容器的 Windows Server 2016”的 VM  。 通过 Windows Server Core 或 Windows Nano Server，可以使用此 VM 尝试新的 Windows Server 容器功能。 安装容器操作系统映像后，VM 即可用于 Docker。

### <a name="benefits"></a>优点

尽管可以将 Windows 容器部署到本地 Windows Server 2016 VM，但在部署到 Azure 时，可以使用随时可用的 Windows Server 容器 VM 来更轻松地入门。 还可以获取可供测试人员访问的常用联机位置，并通过 Azure 虚拟机规模集进行自动缩放。

### <a name="next-steps"></a>后续步骤

在 GitHub wiki 上更深入了解此内容：

<https://github.com/dotnet-architecture/eShopModernizing/wiki/06.-Deploying-your-Windows-Containers-based-app-into-Azure-VMs-(Including-CI-CD)>

## <a name="walkthrough-4-deploy-your-windows-containers-based-apps-to-azure-container-instances-aci"></a>演练 4：将基于 Windows 容器的应用部署到 Azure 容器实例 (ACI)

### <a name="technical-walkthrough-availability"></a>技术演练可用性

eShopModernizing GitHub 存储库 wiki 中提供了完整的技术演练：

[将应用部署到 ACI（Azure 容器实例）](https://github.com/dotnet-architecture/eShopModernizing/wiki/05.-Deploying-the-Apps-to-ACI-(Azure-Container-Instances))

### <a name="overview"></a>概述

[Azure 容器实例 (ACI)](/azure/container-instances/) 是创建容器开发/测试/过渡环境（可在其中部署容器的单个实例）的最快方式。

### <a name="goals"></a>目标

本演练演示了将 Windows 容器部署到 Azure 容器实例 (ACI) 以及将 eShopModernizing 应用部署到 ACI 的主要方案。

### <a name="scenarios"></a>方案

将 eShopModernizing 应用部署到 ACI 中可能会发生变化，例如仅部署一个应用或部署所有应用（MVC 应用、WebForms 应用或 WCF 服务）。 在以下方案中，你可以看到 ASP.NET MVC 应用和 SQL Server 容器，它们都作为容器部署到 ACI（Azure 容器实例）中。

![从开发环境部署到 ACI](./media/image5-3.5.6.png)

### <a name="benefits"></a>优点

使用 Azure 容器实例，可以在 Azure 中轻松创建和管理 Docker 容器，无需预配虚拟机或采用更高级别的服务。 使用 ACI，可以在 Azure 中直接部署 Windows 容器，并在几秒钟内使用完全限定的域名 (FQDN) 向 Internet 公开该容器（前提是 Docker 注册表（例如 Docker Hub 或 Azure 容器注册表）中已准备好 Windows 容器映像）。

### <a name="considerations"></a>注意事项

将包含完全 .NET Framework/ASP.NET 或 SQL Server 的 Windows 容器部署到 Azure 容器实例 (ACI) 的速度不如部署到常规 Docker 主机（例如，包含 Windows 容器的 Windows Server 2016）的速度快，因为每次都必须下载 Docker 映像（从 Docker 注册表中拉取），并且 SQL 容器映像 (15.1 GB) 和 ASP.NET 容器映像 (13.9 GB) 的大小非常大，但它比维护你自己的 Docker 主机（包含 Azure 中的 Windows 容器 VM 的永久在线 Windows Server 2016）更便宜，更不用说整个业务流程协调程序（例如 Azure Kubernetes 服务 (AKS)），而这也是生产部署的一个不错选择。

主要结论是，使用 Azure 容器实例是适用于开发/测试方案和 CI/CD 管道的最终选择。

### <a name="next-steps"></a>后续步骤

在 GitHub wiki 上更深入了解此内容：

[https://github.com/dotnet-architecture/eShopModernizing/wiki/05.-Deploying-the-Apps-to-ACI-(Azure-Container-Instances)](https://github.com/dotnet-architecture/eShopModernizing/wiki/05.-Deploying-the-Apps-to-ACI-(Azure-Container-Instances))

## <a name="walkthrough-5-deploy-your-windows-containers-based-apps-to-kubernetes-in-azure-container-service"></a>演练 5：将基于 Windows 容器的应用部署到 Azure 容器服务中的 Kubernetes

### <a name="technical-walkthrough-availability"></a>技术演练可用性

eShopModernizing GitHub 存储库 wiki 中提供了完整的技术演练：

<https://github.com/dotnet-architecture/eShopModernizing/wiki/04.-How-to-deploy-your-Windows-Containers-based-apps-into-Kubernetes-in-Azure-Container-Service-(Including-CI-CD)>

### <a name="overview"></a>概述

基于 Windows 容器的应用程序很快需要使用平台，使其与 IaaS VM 离得更远。 这是为了轻松实现高可伸缩性和更好的自动可伸缩性，并显著改进自动化部署和版本控制。 可以使用 [Azure 容器服务](https://azure.microsoft.com/services/container-service/)中提供的业务流程协调程序 [Kubernetes](https://kubernetes.io/) 来实现这些目标。

### <a name="goals"></a>目标

本演练的目的是了解如何将基于 Windows 容器的应用程序部署到 Azure 容器服务中的 Kubernetes（也称为 K8s  ）。 从头开始部署到 Kubernetes 的过程分为两个步骤：

1. 将 Kubernetes 群集部署到 Azure 容器服务。

2. 将应用程序和相关资源部署到 Kubernetes 群集。

### <a name="scenarios"></a>方案

#### <a name="scenario-a-deploy-directly-to-a-kubernetes-cluster-from-a-dev-environment"></a>方案 A：从开发环境直接部署到 Kubernetes 群集

![从开发环境直接部署到 Kubernetes 群集](./media/image5-7.png)

**图 5-7**。 从开发环境直接部署到 Kubernetes 群集

#### <a name="scenario-b-deploy-to-a-kubernetes-cluster-from-cicd-pipelines-in-azure-devops-services"></a>情况 B：从 Azure DevOps Services 中的 CI/CD 管道部署到 Kubernetes 群集

![从 Azure DevOps Services 中的 CI/CD 管道部署到 Kubernetes 群集](./media/image5-8.png)

**图 5-8**。 从 Azure DevOps Services 中的 CI/CD 管道部署到 Kubernetes 群集

### <a name="benefits"></a>优点

部署到 Kubernetes 中的群集有许多优点。 最大的优点是，可以获得一个生产就绪环境，在该环境中，你可以根据要使用的容器实例数（现有节点中的内部可伸缩性）以及群集中的节点数或 VM 数（群集的全局可伸缩性）来扩展应用程序。

Azure 容器服务优化了专门针对 Azure 的常用开源工具和技术。 可以获得一个开放的解决方案，该解决方案为容器和应用程序配置提供可移植性。 用户选择主机的大小和数量以及业务流程协调程序工具，然后容器服务可处理其他操作。

使用 Kubernetes，开发人员可以在考虑物理计算机和虚拟机时取得进展，规划支持以下功能的以容器为中心的基础结构等等：

- 基于多个容器的应用程序

- 复制容器实例和水平自动缩放

- 命名和发现（例如，内部 DNS）

- 均衡负载

- 滚动更新

- 分布机密

- 应用程序运行状况检查

### <a name="next-steps"></a>后续步骤

在 GitHub wiki 上更深入了解此内容：<https://github.com/dotnet-architecture/eShopModernizing/wiki/04.-How-to-deploy-your-Windows-Containers-based-apps-into-Kubernetes-in-Azure-Container-Service-(Including-CI-CD)>

## <a name="walkthrough-6-deploy-your-windows-containers-based-apps-to-azure-app-service-for-containers"></a>演练 6：将基于 Windows 容器的应用部署到用于容器的 Azure 应用服务

### <a name="technical-walkthrough-availability"></a>技术演练可用性

eShopModernizing GitHub 存储库 wiki 中提供了完整的技术演练：

<https://github.com/dotnet-architecture/eShopModernizing/wiki/Deploy-Windows-Container-to-Azure-App-Service>

### <a name="overview"></a>概述

可以轻松地将使用 Windows 容器的简单容器化应用程序部署到用于容器的 Azure 应用服务。 对于大多数基于 Windows 容器的应用程序，建议采用此方法。

### <a name="goals"></a>目标

本演练的目的是了解如何将基于 Windows 容器的应用程序部署到注册表（Docker Hub 或 Azure 容器注册表）中用于容器的 Azure 应用服务。

### <a name="scenario"></a>方案

![将基于 Windows 容器的应用部署到用于容器的 Azure 应用服务](./media/image5-11.png)

### <a name="benefits"></a>优点

部署到用于容器的 Azure 应用服务提供的容器优点与 Azure 应用服务的 PaaS 优点配对。 应用服务可以轻松地在垂直和水平方向上缩放，并且可以配置为自动缩放以满足不断变化的需求。 可以在零停机的情况下执行更新，也可以轻松地配置注册表中的持续部署。

### <a name="next-steps"></a>后续步骤

在 GitHub wiki 上更深入了解此内容：<https://github.com/dotnet-architecture/eShopModernizing/wiki/Deploy-Windows-Container-to-Azure-App-Service>

> [!div class="step-by-step"]
> [上一页](modernize-existing-apps-to-cloud-optimized/migrate-to-hybrid-cloud-scenarios.md)
> [下一页](conclusions.md) <!-- Next Chapter -->
