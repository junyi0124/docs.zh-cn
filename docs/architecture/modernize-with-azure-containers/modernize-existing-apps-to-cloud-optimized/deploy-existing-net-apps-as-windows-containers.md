---
title: 将现有 .NET 应用部署为 Windows 容器
description: 通过 Azure 云和 Windows 容器现代化现有 .NET 应用程序 | 将现有 .NET 应用部署为 Windows 容器
ms.date: 04/29/2018
ms.openlocfilehash: 15e99e2ec0edd072a3d47d5c212ebbbf6705ecef
ms.sourcegitcommit: 465547886a1224a5435c3ac349c805e39ce77706
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/21/2020
ms.locfileid: "81738429"
---
# <a name="deploy-existing-net-apps-as-windows-containers"></a>将现有 .NET 应用部署为 Windows 容器

基于 Windows 容器的部署适用于云优化的应用程序和云原生应用程序。

但在本指南中，尤其在以下部分中，主要介绍将 Windows 容器用于云优化  的应用程序，其中无需重新架构应用程序。

## <a name="what-are-containers-linux-or-windows"></a>什么是容器？ （Linux 或 Windows）

容器是将应用程序打包到其自己的独立包中的一种方法。 在其容器中，应用程序不受容器外部的应用程序或进程的影响。 应用程序作为一个进程成功运行所依赖的一切都在容器内部。 无论容器可能移动到哪里，就直接依赖项而言，都将始终满足应用程序的要求，因为它与需要运行的所有内容（库依赖项、运行时等）捆绑在一起。

容器的主要特性是，它使环境在不同的部署中是相同的，因为容器本身附带了所需的所有依赖项。 你可以在计算机上调试应用程序，然后将其部署到保证具有相同环境的另一台计算机上。

容器是容器映像的实例。 借助容器映像，可打包应用或服务（例如快照）并采用可靠且可重现的方式对其进行部署。 可以说 Docker 不只是一种技术，还是一种原理和过程。

随着容器在日常变得越来越普遍，它们正成为行业范围内的“部署单元”。

## <a name="benefits-of-containers-docker-engine-on-linux-or-windows"></a>容器的优点（Linux 或 Windows 上的 Docker 引擎）

使用容器（也可以定义为轻型构建块）构建应用程序，显著提高了敏捷性，有助于跨任何基础结构构建、传送和运行任何应用程序。

借助容器，只需稍微更改或无需更改代码，就可以将任何应用从开发带到生产，这是因为跨 Microsoft 开发人员工具、操作系统和云进行了 Docker 集成。

当部署到普通虚拟机时，你可能已经有了将 ASP.NET 应用部署到虚拟机的方法。 但是，使用 Puppet 等部署工具或类似工具，你的方法可能会涉及多个手动步骤或复杂的自动化过程。 你可能需要执行诸如修改配置项、在服务器之间复制应用程序内容以及基于 .msi 设置运行交互式安装程序之类的任务，然后进行测试。 部署中的所有这些步骤都会增加部署时间和风险。 每当目标环境中不存在依赖项时，都将失败。

在 Windows 容器中，打包应用程序的过程是完全自动化的。 Windows 容器基于 Docker 平台，该平台为容器部署提供自动更新和回滚功能。 使用 Docker 引擎所获得的主要改进就是利用所有依赖项创建映像，这些映像就像应用程序的快照。 映像是 Docker 映像（在本例中为 Windows 容器映像）。 映像在容器中运行 ASP.NET 应用，无需返回到源代码。 容器快照成为部署单元。

许多组织正在容器化现有的整体式应用程序，原因如下：

- **通过改进的部署来释放敏捷性**。 容器在开发和运营之间提供一致的部署协定。 在使用容器时，你不会听到开发人员说：“为什么它能在我的计算机上使用却不能用在生产中？” 他们可以说：“它作为容器运行，因此将在生产中运行。” 打包的应用程序及其所有依赖项可在任何受支持的基于容器的环境中执行。 它将以预期在所有部署目标（开发、QA、暂存、生产）中运行的方式运行。 容器消除了从一个阶段进入下一阶段时产生的大部分摩擦，从而大大改善了部署，让你可以更快地传送。

- **成本降低**。 容器可通过合并和删除现有硬件，或通过以更高的单位硬件密度运行应用程序来降低成本。

- **可移植性**。 容器是模块化的，并且是可移植的。 Docker 容器在任何服务器操作系统（Linux 和 Windows）上、在任何主要公有云（Microsoft Azure、Amazon AWS、Google、IBM）以及本地和私有或混合云环境中均受支持。

- **控制性**。 容器提供在容器级别受控的灵活安全的环境。 可以通过在容器上设置执行约束策略来保护、隔离甚至限制容器。 如讲述 Windows 容器的部分所述，Windows Server 2016 和 Hyper-V 容器提供其他企业支持选项。

使用容器开发和维护应用程序时，敏捷性、可移植性和控制性的重大改进最终会导致成本显著降低。

## <a name="what-is-docker"></a>什么是 Docker？

[Docker](https://www.docker.com/) 是一种[开源项目](https://github.com/docker/docker)，用于将应用程序自动部署为可在云或本地运行的便携式独立容器。 Docker 也是一家促进和发展这项技术的[公司](https://www.docker.com/)。 该公司与云、Linux 和 Windows 供应商（包括 Microsoft）协作。

![显示 Docker 如何在混合云中部署容器的关系图。](./media/deploy-existing-net-apps-as-windows-containers/docker-deploys-containers-all-layers.png)

**图 4-6.** Docker 在混合云的所有层部署容器

对于熟悉虚拟机的人而言，众多容器可能看起来非常相似。 容器在操作系统上运行、具有文件系统，并且可以通过网络访问，就像它是物理或虚拟计算机系统一样。 但是容器背后的技术和概念与虚拟机有很大不同。 从开发人员的角度来看，容器必须被对待得更像是一个进程。 事实上，容器具有一个进程的单一入口点。

Docker 容器（为简单起见，下面统称为“容器”  ）可以在 Linux 和 Windows 上本机运行。 运行常规容器时，Windows 容器只能在 Windows 主机（主机服务器或虚拟机）上运行，而 Linux 容器只能在 Linux 主机上运行。 但是，在最新版本的 Windows Server 和 Hyper-V 容器中，Linux 容器还可以使用目前仅在 Windows Server 容器中可用的 Hyper-V 隔离技术，在 Windows Server 上本机运行。

在不久的将来，同时具有 Linux 和 Windows 容器的混合环境可能会成为现实，甚至变得很常见。

## <a name="benefits-of-windows-containers-for-your-existing-net-applications"></a>将 Windows 容器用于现有 .NET 应用程序的好处

使用 Windows 容器的好处基本上与从容器中获得的好处大致相同。 使用 Windows 容器可大大提高敏捷、可移植性和控制性。

现有的 .NET 应用程序是指使用 .NET Framework 创建的那些应用程序。 例如，它们可能是传统的 ASP.NET Web 应用程序，它们不使用相对来说较新的 .NET Core，并在 Linux、Windows 和 MacOS 上跨平台运行。

.NET Framework 中的主要依赖项为 Windows。 它还具有辅助依赖项，例如 IIS 和传统 ASP.NET 中的 System.Web。

必须在 Windows 上运行 .NET Framework 应用程序一段时间。 若要容器化现有 .NET Framework 应用程序，并且不能或不需要投资 .NET Core 的迁移（“如果它能正常工作，则不迁移”），则关于容器的唯一选择便是使用 Windows 容器。

因此，Windows 容器的一个主要优点是，为你提供一种通过容器化来现代化在 Windows 上运行的现有 .NET Framework 应用程序的方式。 最后，Windows 容器可以为你带来你所寻求的使用容器的好处 - 敏捷性、可移植性和更好的控制性。

## <a name="choose-an-os-to-target-with-net-based-containers"></a>选择一个操作系统以基于 .NET 的容器为目标

考虑到 Docker 支持的操作系统的多样性，以及 .NET Framework 与 .NET Core 之间的差异，应根据所使用的框架以特定的操作系统和特定版本为目标。

对于 Windows，可使用 Windows Server Core 或 Windows Nano Server。 这些 Windows 版本提供 .NET Framework 或 .NET Core 应用程序可能需要的不同特性（例如 IIS 与像 Kestrel 这样的自托管 Web 服务器）。

对于 Linux，正式的 .NET Docker 映像（如 Debian）中提供并支持多个发行版本。

图 4-7 显示了可以作为目标的操作系统版本，具体取决于应用的 .NET Framework 版本。

![显示根据 .NET Framework 版本判断以哪个操作系统为目标的关系图。](./media/deploy-existing-net-apps-as-windows-containers/dotnet-framework-operating-systems.png)

**图 4-7.** 根据 .NET Framework 版本判断作为目标的操作系统

在基于 .NET Framework 应用程序的现有或旧版应用程序的迁移方案中，主要依赖项位于 Windows 和 IIS 上。 唯一的选择是使用基于 Windows Server Core 和 .NET Framework 的 Docker 映像。

将映像名称添加到 Dockerfile 文件后，可以使用标记来选择操作系统和版本，如以下基于 .NET Framework 的 Windows 容器映像的示例所示：

> | **标记** | **系统和版本** |
> |---|---|
> | **microsoft/dotnet-framework:4.x-windowsservercore** | Windows Server Core 上的 .NET Framework 4.x |
> | **microsoft/aspnet:4.x-windowsservercore** | Windows Server Core 上的 .NET Framework 4.x 以及其他 ASP.NET 自定义项 |

对于 .NET Core（适用于 Linux 和 Windows 的跨平台），标记应如下所示：

> | **标记** | **系统和版本**
> |---|---|
> | **microsoft/dotnet:2.0.0-runtime** | Linux 上的 .NET Core 2.0 仅运行时 |
> | **microsoft/dotnet:2.0.0-runtime-nanoserver** | Windows Nano Server 上的 .NET Core 2.0 仅运行时 |

### <a name="multi-arch-images"></a>多体系结构映像

从 2017 年年中开始，还可以在 Docker 中使用一项称为[多体系结构](https://github.com/moby/moby/issues/15866)映像的新功能。 .NET Core Docker 映像可使用多体系结构标记。 你的 Dockerfile 文件不再需要定义你作为目标的操作系统。 多体系结构功能允许在多个机器配置中使用单个标记。 例如，对于多体系结构，你可以使用通用标记：microsoft/dotnet:2.0.0-runtime  。 如果从 Linux 容器环境拉取该标记，则会获得基于 Debian 的映像。 如果从 Windows 容器环境拉取该标记，则会获得基于 Nano Server 的映像。

对于 .NET Framework 映像，因为传统 .NET Framework 仅支持 Windows，所以不能使用多体系结构功能。

## <a name="windows-container-types"></a>Windows 容器类型

与 Linux 容器一样，Windows Server 容器使用 Docker 引擎进行管理。 与 Linux 容器不同，Windows 容器包括两个不同的容器类型或运行时间 - Windows Server 容器和 Hyper-V 隔离。

Windows Server 容器  ：通过进程和命名空间隔离技术提供应用程序隔离。 Windows Server 容器与容器主机和主机上运行的所有容器共享内核。 这些容器不提供恶意安全边界，且不用于隔离不受信任的代码。 由于共享内核空间，这些容器需要相同的内核版本和配置。

**Hyper-V 隔离**：通过在高度优化的虚拟机中运行各容器来扩展 Windows Server 容器提供的隔离。 在此配置中，容器主机的内核不与同一主机上的其他容器共享。 这些容器针对恶意多租户托管而设计，具有与虚拟机相同的安全保证。 因为这些容器不与主机或主机上的其他容器共享内核，所以它们可以运行具有不同版本和配置（具有受支持的版本）的内核。 例如，Windows 10 上的所有 Windows 容器都使用 Hyper-V 隔离来利用 Windows Server 内核版本和配置。

无论有无 Hyper-V 隔离，在 Windows 上运行容器都是运行时决策。 你可能最初选择使用 Hyper-V 隔离创建容器，然后在运行时选择将其作为 Windows Server 容器运行。

### <a name="additional-resources"></a>其他资源

- **Windows 容器文档**

    <https://docs.microsoft.com/virtualization/windowscontainers/>

- **Windows 容器基础知识**

    <https://docs.microsoft.com/virtualization/windowscontainers/about/>

- **信息图：Microsoft 和容器**

    <https://info.microsoft.com/rs/157-GQE-382/images/Container%20infographic%201.4.17.pdf>

## <a name="the-container-ecosystem-in-azure"></a>Azure 中的容器生态系统

在前面的部分，已经介绍了 Docker 容器的优点，以及适用于 .NET 应用程序的特定容器映像的详细信息。 所有这些一般信息都是为了开发或容器化应用程序所需的基础信息。
但是，在考虑生产部署环境甚至 QA 和开发/测试环境时，Microsoft Azure 提供了各种开放且广泛的选择，即云中的完整容器生态系统（如下图所示）。 根据具体的应用程序需求，应选择一个或另一个 Azure 产品。

![Azure 中的容器生态系统的关系图。](./media/deploy-existing-net-apps-as-windows-containers/azure-container-ecosystem.png)

**图 4-7.5.** Azure 中的容器生态系统

在 Azure 中的容器生态系统中，以下支持容器的产品被视为基础结构：

- **Azure 容器实例 (ACI)**
- **Azure 虚拟机**（在容器的支持下）
- **Azure 虚拟机规模集**（在容器的支持下）

从这三个产品来看，ACI 提供莫大的好处，即不需要维护基础操作系统、也不需要升级/修补等，但 ACI 仍被定位于基础结构级别，如本书后面部分所述。

不过 Azure 中支持容器的产品在 PaaS（平台即服务）级别的定位更重要：

- **Azure 应用服务**
- **Azure Kubernetes 服务（AKS 和 ACS）**
- **Azure Batch**

接下来，Azure 容器注册表是 Azure 中托管的高度可缩放容器注册表，可在注册和部署自定义容器映像时在以前的所有产品中使用。

此外，还可以在容器中使用 Azure 中的其他托管服务，如 Azure SQL 数据库、Azure Redis 缓存、Azure Cosmos DB 等。 此外还有 Azure 市场中的第三方解决方案/平台，如 Cloud Foundry 和 OpenShift，还可以在其中使用 Azure 内的容器。

在接下来的部分，可以浏览 Microsoft 关于何时使用每个 Azure 产品和解决方案的建议，尤其是在以 Windows 容器为目标时。

>[!div class="step-by-step"]
>[上一页](what-about-cloud-native-applications.md)
>[下一页](when-not-to-deploy-to-windows-containers.md)
