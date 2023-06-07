---
title: 什么是 Docker？
description: 深入了解 Docker，这里的一个简单类比可能会对你有所帮助。
ms.date: 08/06/2020
ms.openlocfilehash: b23a8719170deca706ee44d10337c340d72605fb
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91165918"
---
# <a name="what-is-docker"></a>什么是 Docker？

[Docker](https://www.docker.com/) 是一种[开源项目](https://github.com/docker/docker)，用于将应用程序自动部署为可在云或本地运行的便携式独立容器。 Docker 也是一家[公司](https://www.docker.com/)，它与云、Linux 和 Windows 供应商（包括 Microsoft）协作，致力于推广和发展这项技术。

![显示 Docker 容器可运行位置的关系图。](./media/what-is-docker/docker-containers-run-anywhere.png)

**图 1-2**。 Docker 在混合云的所有层部署容器

如上图所述，Docker 容器可以在任意位置运行：在客户数据中心本地、在外部服务提供商或在 Azure 云中。 Docker 映像容器还可以在 Linux 和 Windows 上本机运行。 但是，Windows 映像仅能在 Windows 主机上运行，Linux 映像可以在 Linux 主机和 Windows 主机上运行（到目前为止，使用 Hyper-V Linux VM），其中主机是指服务器或 VM。

开发人员可以在 Windows、Linux 或 macOS 上使用开发环境。 在开发计算机上，开发人员运行部署了 Docker 映像（包括应用及其依赖项）的 Docker 主机。 在 Linux 或 Mac 上进行开发的开发人员使用基于 Linux 的 Docker 主机，并且他们可以仅为 Linux 容器创建映像。 （在 Mac 上进行开发的开发人员可以从 macOS 中编辑代码或运行 Docker 命令行接口 (CLI)，但在撰写本文时，容器不在 macOS 上直接运行。）在 Windows 上进行开发的开发人员可以为 Linux 或 Windows 容器创建映像。

为了在开发环境中承载容器，并提供其他开发人员工具，Docker 为 Windows 或 macOS 提供了 [Docker 社区版 (CE)](https://www.docker.com/community-edition)。 这些产品安装了承载容器所需的 VM（Docker 主机）。 Docker 还提供 [Docker 企业版 (EE)](https://www.docker.com/enterprise-edition)，该版本专为企业开发而设计，供生成、交付和在生产中运行大型业务关键型应用程序的 IT 团队使用。

若要运行 [Windows 容器](/virtualization/windowscontainers/about/)，有两种类型的运行时可供使用：

- “Windows Server 容器”通过进程和命名空间隔离技术提供应用程序隔离  。 Windows Server 容器与容器主机和主机上运行的所有容器共享内核。

- “Hyper-V 容器”通过在高度优化的虚拟机中运行各容器来扩展 Windows Server 容器提供的隔离  。 在此配置中，容器主机的内核不与 Hyper-V 容器共享，以提供更好的隔离。

这些容器的映像的创建和工作方式均相同。 区别在于，在运行 Hyper-V 容器的映像中创建容器的方式需要使用其他参数。 有关详细信息，请参阅 [Hyper-V 容器](/virtualization/windowscontainers/manage-containers/hyperv-container)。

## <a name="comparing-docker-containers-with-virtual-machines"></a>比较 Docker 容器和虚拟机

图 1-3 显示了 VM 和 Docker 容器之间的比较。

![显示 VM 和容器环境比较的关系图。](./media/what-is-docker/comparison-vms-docker-conatiners.png)

**图 1-3**。 比较传统虚拟机与 Docker 容器

如上图所示，对于 VM，主机服务器中有三个基本层。 从下到上依次为：基础结构、主机操作系统和虚拟机监控程序。 毕竟，每个 VM 都有自己的操作系统和所有必需的库。 另一方面，对于 Docker，主机服务器仅具有基础结构和操作系统。 在此基础上，容器引擎会使容器保持隔离，但允许它们共享单个基本操作系统的服务。

由于容器所需的资源要少得多（例如，它们不需要一个完整的 OS），所以它们易于部署且可快速启动。 这使你能够具有更高的密度，也就是说，这允许你在同一硬件单元上运行更多服务，从而降低了成本。

在同一内核上运行的副作用是，你获得的隔离比 VM 要少。

映像的主要目标是确保在不同部署的同一环境（依赖项）。 也就是说，可以在计算机上调试它，然后将其部署到保证具有相同环境的另一台计算机上。

借助容器映像，可打包应用或服务并采用可靠且可重现的方式对其进行部署。 可以说 Docker 不只是一种技术，还是一种原理和过程。

在使用 Docker 时，你不会听到开发人员说：“为什么它能在我的计算机上使用却不能用在生产中？” 他们只需说“它在 Docker 上运行”，因为打包的 Docker 应用程序可在任何支持的 Docker 环境上执行，而且它在所有部署目标（例如，开发、QA、暂存和生产）上都按预期运行。

## <a name="a-simple-analogy"></a>一个简单的类比

也许一个简单的类比有助于掌握 Docker 的核心概念。

让我们回到 20 世纪 50 年代。 那时，还没有处理器这个词，而复印机无处不在（某种程度上）。

假设你负责按要求快速发出成批的信件、将这些信件邮寄给客户、使用纸张和信封以物理方式寄送到每个客户的地址（那时还没有电子邮件）。

在某个时候，你意识到，这些信件只是由一大组段落组合而成的，根据信件的用途对其进行所需的选取和排列，因此，你设计了一个系统，以快速发送这些信件，希望能大幅提高效率。

这个系统很简单：

1. 先从一副透明薄片开始，每个薄片包含一个段落。

2. 若要发送一组信件，你选择包含所需段落的薄片，然后堆栈并对齐它们，使其外观一致且易于阅读。

3. 最后，你将其置于复印机中并按开始，以生成所需的多个信件。

简而言之，这就是 Docker 的核心理念。

在 Docker 中，每层都是在执行命令（例如，安装程序）后在文件系统所发生的一组更改。

因此，当你在复制层后“查看”文件系统时，你将看到所有文件，包括在安装程序时的层。

你可以将映像视为要在“计算机”中安装的辅助只读硬盘，其中操作系统已经安装。

同样，你可以将容器视为已安装映像硬盘的“计算机”。 与计算机一样，可以打开或关闭容器电源。

>[!div class="step-by-step"]
>[上一页](introduction-to-containers-and-docker.md)
>[下一页](docker-terminology.md)
