---
title: Docker 术语
description: 了解使用 Docker 时常用的一些基本术语。
ms.date: 08/06/2020
ms.openlocfilehash: a323075bdd73a117769913ac59426b5bec5df614
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95686446"
---
# <a name="docker-terminology"></a>Docker 术语

本节列出了在深入了解 Docker 之前应熟悉的术语和定义。 有关进一步的定义，请参阅 Docker 提供的详细[术语表](https://docs.docker.com/glossary/)。

**容器映像**：包含创建容器所需的所有依赖项和信息的包。 映像包括所有依赖项（例如框架），以及容器运行时使用的部署和执行配置。 通常情况下，映像派生自多个基础映像，这些基础映像是堆叠在一起形成容器文件系统的层。 创建后，映像不可变。

**Dockerfile**：包含有关如何生成 Docker 映像的说明的文本文件。 与批处理脚本相似，首先第一行将介绍基础映像，然后是关于安装所需程序、复制文件等操作的说明，直至获取所需的工作环境。

**生成**：基于其 Dockerfile 提供的信息和上下文生成容器映像的操作，以及生成映像的文件夹中的其他文件。 可以使用以下 Docker 命令生成映像：

```bash
docker build
```

**容器**：Docker 映像的实例。 容器表示单个应用程序、进程或服务的执行。 它由 Docker 映像的内容、执行环境和一组标准指令组成。 在缩放服务时，可以从相同的映像创建多个容器实例。 或者，批处理作业可以从同一个映像创建多个容器，向每个实例传递不同的参数。

**卷**：提供一个容器可以使用的可写文件系统。 由于映像只可读取，而多数程序需要写入到文件系统，因此卷在容器映像顶部添加了一个可写层，这样程序就可以访问可写文件系统。 程序并不知道它正在访问的是分层文件系统，此文件系统就是往常的文件系统。 卷位于主机系统中，由 Docker 管理。

**标记**：可以应用于映像的标记或标签，以便可以识别同一映像的不同映像或版本（具体取决于版本号或目标环境）。

**多阶段生成**：Docker 17.05 或更高版本的一个功能，可帮助减小最终映像的大小。 例如，包含 SDK 的大型基础映像可用于编译和发布，仅小型运行时基础映像可用于托管应用程序。

**存储库 (repo)** ：相关的 Docker 映像集合，带有指示映像版本的标记。 某些存储库包含特定映像的多个变量，例如包含 SDK（较重）的映像，包含唯一运行时（较轻）的映像，等等。这些变量可以使用标记进行标记。 单个存储库中可包含平台变量，如 Linux 映像和 Windows 映像。

**注册表**：提供存储库访问权限的服务。 大多数公共映像的默认注册表是 [Docker 中心](https://hub.docker.com/)（归作为组织的 Docker 所有）。 注册表通常包含来自多个团队的存储库。 公司通常使用私有注册表来存储和管理其创建的映像。 另一个示例是 Azure 容器注册表。

**多体系结构映像**：对于多体系结构，此功能可根据运行 Docker 的平台简化选择适当映像的过程。 例如，当 Dockerfile 从注册表中请求基础映像 FROM mcr.microsoft.com/dotnet/sdk:3.1 时，它实际上获取 3.1-sdk-nanoserver-1909、3.1-sdk-nanoserver-1809 或 3.1-sdk-buster-slim，具体取决于运行 Docker 的操作系统和版本   。

**Docker 中心**：上传并使用映像的公共注册表。 Docker 中心提供 Docker 映像托管、公共或私有注册表，生成触发器和 Web 挂钩，以及与 GitHub 和 Bitbucket 集成。

**Azure 容器注册表**：用于在 Azure 中使用 Docker 映像及其组件的公共资源。 这提供了与 Azure 中的部署接近的注册表，使你可以控制访问权限，从而可以使用 Azure Active Directory 组和权限。

**Docker 受信任注册表 (DTR)** ：Docker 注册表服务（来自 Docker），可以安装在本地，因此它存在于组织的数据中心和网络中。 这对于应该在企业内部管理的私有映像来说很方便。 Docker 受信任注册表是 Docker 数据中心产品的一部分。 有关详细信息，请参阅 [Docker 受信任注册表 (DTR)](https://www.docker.com/sites/default/files/Docker%20Trusted%20Registry.pdf)。

**Docker 社区版 (CE)** ：适用于 Windows 和 macOS、用于在本地生成、运行和测试容器的开发工具。 适用于 Windows 的 Docker CE 为 Linux 和 Windows 容器提供了开发环境。 Windows 上的 Linux Docker 主机基于 [Hyper-V](https://www.microsoft.com/cloud-platform/server-virtualization) 虚拟机。 适用于 Windows 容器的主机直接基于 Windows。 适用于 Mac 的 Docker CE 基于 Apple 虚拟机监控程序框架和 [xhyve 虚拟机监控程序](https://github.com/mist64/xhyve)，在 Mac OS X 上提供了 Linux Docker 主机虚拟机。适用于 Windows 和 Mac 的 Docker CE 替换了 Docker 工具箱，后者基于 Oracle VirtualBox。

**Docker 企业版 (EE)** ：适用于 Linux 和 Windows 开发的 Docker 工具企业级版本。

**撰写**：一种命令行工具和带有元数据的 YAML 文件格式，用于定义和运行多容器应用程序。 基于多个映像定义单个应用程序，其中包含一个或多个 .yml 文件，这些文件可以根据环境替代值。 在创建定义之后，可以使用单个命令 (docker-compose up) 来部署整个多容器应用程序，该命令在 Docker 主机上为每个映像创建容器。

**群集**：Docker 主机集合像单一虚拟 Docker 主机一样公开，以便应用程序可以扩展到服务分布在群集中多个主机的多个实例。 Docker 群集可以使用 Kubernetes、Azure Service Fabric、Docker Swarm 和 Mesosphere DC/OS创建。

**业务流程协调程序**：简化群集和 Docker 主机管理的工具。 通过命令行接口 (CLI) 或图形 UI，业务流程协调程序能够管理其映像、容器和主机。 可以管理容器网络、配置、负载均衡、服务发现、高可用性、Docker 主机配置等。 业务流程协调程序负责跨节点集合运行、分发、缩放和修复工作负荷。 通常情况下，业务流程协调程序产品是提供群集基础结构的同一产品，如 Kubernetes 和 Azure Service Fabric，以及市场中的其他产品/服务。

>[!div class="step-by-step"]
>[上一页](what-is-docker.md)
>[下一页](docker-containers-images-and-registries.md)
