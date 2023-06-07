---
title: 容器化整体式应用程序
description: 容器化整体式应用程序虽然无法从微服务体系结构中获得所有好处，但却具有可立即提供的重要部署优势。
ms.date: 01/30/2020
ms.openlocfilehash: b26a9b79ec00ba06404a12d62343ea31d67151cb
ms.sourcegitcommit: 6d4ee46871deb9ea1e45bb5f3784474e240bbc26
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/11/2020
ms.locfileid: "90022878"
---
# <a name="containerizing-monolithic-applications"></a>容器化整体式应用程序

你可能想构建单个整体部署的 Web 应用程序或服务，并将其部署为容器。 应用程序本身在内部可能并非整体式，只是结构化为多个库、组件甚至层（应用程序层、域层、数据访问层等等）。 但在外部，应用程序则是单个容器 - 单个进程、单个 Web 应用程序或单个服务。

若要管理此模型，可部署单个容器来表示应用程序。 若要增加容量，可进行横向扩展，即，只需添加更多副本并将负载均衡器置于前面。 为了简单起见，在单个容器或 VM 中管理单个部署。

![显示整体式容器化应用程序组件的关系图。](./media/containerize-monolithic-applications/monolithic-containerized-application.png)

**图 4-1**。 容器化整体式应用程序的体系结构示例

如图 4-1 中所示，可以在每个容器添加多个组件、库或内部层。 整体式容器化应用程序将其大部分功能集中在一个具有内部层或库的容器中，并通过在多个服务器/VM 上克隆容器来进行横向扩展。 然而，这种整体式模式可能违背了容器原则（“一个容器在一个进程中做一件事”），但在某些情况下没有关系。

如果应用程序在增长，需要它进行缩放，这种方法的缺点就会变得显而易见。 如果整个应用程序都可缩放，这就不是问题了。 但是，在大多数情况下，应用程序中只有少数几个部分需要进行缩放，而其他组件很少用到。

例如，在典型的电子商务应用程序中，很可能需要缩放产品信息子系统，因为更多的顾客仅浏览产品而非购买产品。 使用购物车的顾客比使用付款管道的多。 较少的顾客会评论或查看购买记录。 而且你可能只需要少量的员工管理货物和营销活动。 如果缩放整体式设计，则会多次部署并按相同等级缩放这些不同任务的所有代码。

缩放应用程序的方法有多种，如：水平复制、拆分应用程序的不同区域，以及分割类似的经营理念或数据。 但除了缩放所有组件的问题外，更改单个组件还需要完全重新测试整个应用程序，以及完全重新部署所有实例。

然而，整体式方法非常常见，因为应用程序的开发最初比微服务方法更容易。 因此，许多组织都使用此体系结构方法进行开发。 一些组织获得了不错的成效，而其他组织却快要达到极限。 许多组织使用这种模型设计应用程序，因为几年前的工具和基础结构难以构建面向服务的体系结构 (SOA)，而且在应用程序增长之前他们也没有发现这种需要。

从基础结构的角度来看，每台服务器可以在同一台主机上运行多个应用程序，在资源使用率中具有可接受的效率比率，如图 4-2 所示。

![显示一个主机在容器中运行许多应用的关系图。](./media/containerize-monolithic-applications/host-multiple-apps-containers.png)

**图 4-2**。 整体式方法：主机运行多个应用，每个应用作为一个容器运行

Microsoft Azure 中的整体式应用程序可以使用每个实例的专用 VM 进行部署。 此外，使用 [Azure 虚拟机规模集](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/)可轻松缩放 VM。 [Azure 应用服务](https://azure.microsoft.com/services/app-service/)还可运行整体式应用程序和轻松缩放实例，无需管理 VM。 自 2016 年起，Azure 应用服务也可以运行 Docker 容器的单个实例，从而简化部署。

像 QA 环境或有限的生产环境一样，可以部署多个 Docker 主机 VM 并使用 Azure 平衡器平衡这些 VM，如图 4-3 所示。 这样，你可以使用粗粒度方法管理缩放，因为整个应用程序都在单个容器中。

![显示几个主机运行整体式应用容器的关系图。](./media/containerize-monolithic-applications/docker-infrastructure-monolithic-application.png)

**图 4-3**。 多个主机纵向扩展单容器应用程序的示例

使用传统的部署技术可以管理各种主机的部署。 通过 `docker run` 或 `docker-compose` 等命令可以手动管理 Docker 主机，也可以通过持续交付 (CD) 管道等来自动化管理。

## <a name="deploying-a-monolithic-application-as-a-container"></a>将整体式应用程序部署为容器

使用容器管理整体式应用程序部署具有一些益处。 缩放容器实例比再部署另外的 VM 要快得多，也容易地多。 即使是使用虚拟机规模集，启动 VM 也需要时间。 部署为传统应用程序实例而非容器时，管理应用程序的配置就属于 VM 的一部分，这并不是理想的方式。

将更新部署为 Docker 映像会快得多，并且网络效率更高。 Docker 映像通常会在几秒内启动，加快了推出速度。 拆除 Docker 映像实例与发出 `docker stop` 命令一样简单，通常在一秒钟以内便可完成。

因为容器设计为不可更改，所以不必担心损坏的 VM。 与此相反，VM 的更新脚本可能会忘记某些特定配置的帐户或磁盘上的剩余文件。

虽然整体式应用程序可以从 Docker 中获益，但我们仅涉及到这些益处。 管理容器的其他益处来自于使用容器业务流程协调程序进行部署，此协调程序负责管理每个容器实例的各种实例和生命周期。 将整体式应用程序分解为可以单独缩放、开发和部署的子系统是进入微服务领域的切入点。

## <a name="publishing-a-single-container-based-application-to-azure-app-service"></a>将基于单个容器的应用程序发布到 Azure 应用服务

无论是想验证部署到 Azure 的容器，还是想应用程序只作为单容器应用程序，Azure 应用服务都能够用一种适合的方式可缩放的基于单个容器的服务。 Azure 应用服务使用简单。 此服务与 Git 完美集成，可方便获取代码、在 Visual Studio 中构建此服务并将其直接部署到 Azure。

![显示容器注册表的“创建应用服务”对话框的屏幕截图。](./media/containerize-monolithic-applications/publish-azure-app-service-container.png)

**图 4-4**。 将单容器应用程序从 Visual Studio 2019 发布到 Azure 应用服务

如果没有 Docker，需要 Azure 应用服务中不支持的其他功能、框架或依赖项时，则必须等到 Azure 团队更新应用服务中的这些依赖项。 或者必须切换到 Azure 云服务或 VM 等其他服务，你可以在其中获得更进一步的控制权，且可以为应用程序安装所需的组件或框架。

借助 Visual Studio 2017 及更高版本中的容器支持，你能够在应用程序环境中包括任何想要的内容，如图 4-4 所示。 由于容器中正在运行此支持，因此如果向应用程序添加依赖项，则可以将依赖项包含在 Dockerfile 或 Docker 映像中。

仍然如图 4-4 所示，发布流通过容器注册表发布映像。 这可以是 Azure 容器注册表（一个与 Azure 中的部署密切相关并由 Azure Active Directory 组和帐户保护的注册表），也可以是任何其他 Docker 注册表（如 Docker 中心或本地注册表）。

>[!div class="step-by-step"]
>[上一页](index.md)
>[下一页](docker-application-state-data.md)
