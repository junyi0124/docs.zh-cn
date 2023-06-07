---
title: 整体式应用程序
description: 了解容器化整体式应用程序的核心概念。
ms.date: 08/06/2020
ms.openlocfilehash: c9a5baf209a47f62f421a236c0b04fe5dae37e3a
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91163539"
---
# <a name="monolithic-applications"></a>整体式应用程序

在此情况下，生成单个整体式 Web 应用或服务，并将其部署为容器。 在应用程序中，结构可能并非整体式；它可能包含多个库、组件甚至层（应用程序层、域层、数据访问层等）。 但在外部，它是单个容器，如单个进程、单个 Web 应用或单个服务。

若要管理此模型，可部署单个容器来表示应用程序。 若要缩放此模型，只需添加更多副本，并将负载均衡器置于前面即可。 为了简单起见，在单个容器或虚拟机 (VM) 中管理单个部署。

遵循“一个容器在一个进程中做一件事”的原则，整体模式会产生冲突。 如图 4-1 中所示，可以在每个容器内添加多个组件/库或内部层。

![显示通过克隆应用进行横向扩展的单一应用的示意图。](./media/monolithic-applications/monolithic-application-architecture-example.png)

**图 4-1**。 整体式应用程序体系结构示例

整体式应用在单个进程或容器中具有其全部或大部分功能，并且它在内部层或库中进行组件化。 这种方法的缺点是应用程序增长时，需要将它进行缩放。 如果整个应用程序都已缩放，这就不是问题了。 但在大多数情况下，应用程序中只有一小部分是瓶颈，需要进行缩放，而其他组件使用较少。

在典型的电子商务示例中，可能需要缩放产品信息组件。 众多客户浏览产品，但并不购买它们。 使用购物车的顾客比使用付款管道的多。 较少的顾客会评论或查看购买记录。 而且你可能只需要少量的员工（在一个区域内）管理货物和营销活动。 通过缩放整体式设计，可多次部署所有代码。

除了“缩放所有组件”问题外，更改单个组件还需要完全重新测试整个应用程序，以及完全重新部署所有实例。

整体式方法很常见，并且许多组织均使用此体系结构方法进行开发。 许多人都在享受非常好的结果，而其他人则遇到限制。 许多组织使用这种模型设计应用程序，因为工具和基础结构难以构建 SOA，而且在应用程序增长之前他们也没有发现这种需要。

从基础结构的角度来看，每台服务器可以在同一台主机上运行多个应用程序，在资源使用率中具有可接受的效率比率，如图 4-2 所示。

![显示一个在单独容器中具有多个应用的主机的示意图。](./media/monolithic-applications/host-with-multiple-apps-containers.png)

**图 4-2**。 运行多个应用/容器的主机

最后，从可用性的角度来看，整体式应用程序必须作为一个整体进行部署；这意味着如果必须停止并启动，则在部署期间所有功能和所有用户都将受到影响  。 在某些情况下，使用 Azure 和容器可以最大限度地减少这些情况并降低应用程序停机的可能性，如图 4-3 所示。

可以通过为每个实例使用专用 VM 在 Azure 中部署整体式应用程序。 使用 [Azure VM 规模集](/azure/virtual-machine-scale-sets/) 可轻松地缩放 VM。

此外，还可使用 [Azure 应用服务](https://azure.microsoft.com/services/app-service/)运行整体式应用程序并轻松缩放实例，无需管理 VM。 Azure 应用服务还可运行 Docker 容器的单个实例，从而简化部署。

可以将多个 VM 部署为 Docker 主机，并为每个 VM 运行任意数量的容器。 然后，通过使用 Azure 负载均衡器（如图 4-3 所示），可以管理缩放。

![显示单一应用横向扩展到不同主机的示意图。](./media/monolithic-applications/multiple-hosts-from-single-docker-container.png)

**图 4-3**。 多个主机横向扩展单个 Docker 应用程序

可以通过传统部署技术管理主机本身的部署。

例如，可以使用 `docker run` 和 `docker-compose up` 等命令从命令行管理 Docker 容器，还可以在持续交付 (CD) 管道中自动化该容器，并从 Azure DevOps Services 部署到 Docker 主机。

## <a name="monolithic-application-deployed-as-a-container"></a>部署为容器的整体式应用程序

使用容器管理整体式部署具有一些益处。 缩放容器实例比部署额外的 VM 要快得多，也容易得多。

将更新部署为 Docker 映像会快得多，并且网络效率更高。 Docker 容器通常会在几秒内启动，加快了推出速度。 拆除 Docker 容器与调用 `docker stop` 命令一样简单，通常在一秒钟以内便可完成。

由于从设计上来说，容器的本质是不可变的，因此你无需担心 VM 损坏，因为更新脚本忘记考虑磁盘上剩下的某些特定配置或文件。

虽然整体式应用可以从 Docker 中受益，但我们仅涉及这些益处的提示。 管理容器的更大益处来自于使用容器业务流程协调程序进行部署，此协调程序负责管理每个容器实例的各种实例和生命周期。 将整体式应用程序分解为可以单独缩放、开发和部署的子系统是进入微服务领域的切入点。

若要了解如何使用容器“直接迁移”整体式应用程序以及如何实现应用程序现代化，可阅读此额外的 Microsoft 指南[使用 Azure 云和 Windows 容器更新现有 .NET 应用程序](../../modernize-with-azure-containers/index.md)，也可从 <https://aka.ms/LiftAndShiftWithContainersEbook> 以 PDF 格式下载该指南。

## <a name="publish-a-single-docker-container-app-to-azure-app-service"></a>将单个 Docker 容器应用发布到 Azure 应用服务

无论是因为想快速验证部署到 Azure 的容器，还是因为应用只是单容器应用，Azure 应用服务都能提供一种合适的方式来提供可缩放的单容器服务。

使用 Azure 应用服务是直观的，并且可以快速启动和运行，因为它提供了很好的 Git 集成来获取代码，在 Microsoft Visual Studio 中生成代码，并直接将其部署到 Azure。 但是，传统上（没有 Docker），如果需要应用服务中不支持的其他功能、框架或依赖项，则需要等待，直到 Azure 团队在应用服务中更新这些依赖项，或者需要切换到你可以进一步控制并为应用程序安装所需组件或框架的其他服务（如 Service Fabric、云服务甚至普通 VM）。

现在，如图 4-4 所示，在使用 Visual Studio 2017 时，Azure 应用服务中的容器支持使你能够在应用环境中包含任何所需内容。 如果在应用中添加了依赖项，由于在容器中运行它，因此可以在 Dockerfile 或 Docker 映像中包含这些依赖项。

![显示容器注册表的“创建应用服务”对话框的屏幕截图。](./media/monolithic-applications/publish-azure-app-service-container.png)

**图 4-4**。 从 Visual Studio 应用/容器将容器发布到 Azure 应用服务

图 4-4 还显示了发布流通过容器注册表推送映像，该容器注册表可以是 Azure 容器注册表（一个与 Azure 中的部署密切相关并由 Azure Active Directory 组和帐户保护的注册表），也可以是任何其他 Docker 注册表（如 Docker Hub 或本地注册表）。

>[!div class="step-by-step"]
>[上一页](common-container-design-principles.md)
>[下一页](state-and-data-in-docker-applications.md)
