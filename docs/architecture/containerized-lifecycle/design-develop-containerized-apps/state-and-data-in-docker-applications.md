---
title: Docker 应用程序中的状态和数据
description: 了解在容器化应用程序中用于保存状态的可用选项。
ms.date: 08/06/2020
ms.openlocfilehash: d55519e9340ec06588c2dae3e7363d03f263ce39
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91163461"
---
# <a name="state-and-data-in-docker-applications"></a>Docker 应用程序中的状态和数据

在大多数情况下，可以将容器视为进程的实例。 进程不维护持久状态。 虽然容器可以写入其本地存储，但假设一个实例将无限期地存在，就像假设内存中的单个位置将是持久的一样。 与进程一样，容器映像应被假定有多个实例且最终将终止；如果使用容器业务流程协调程序对其进行管理，那么应假定它们可能从一个节点或 VM 移动到另一个节点或 VM。

以下解决方案用于管理 Docker 应用程序中的持久性数据：

从 Docker 主机，作为 [Docker 卷](https://docs.docker.com/engine/admin/volumes/)：

- **卷**存储在由 Docker 管理的主机文件系统区域中。

- **绑定装载**可映射到主机文件系统中的任何文件夹，因此不能通过 Docker 进程控制访问，且访问可带来安全风险，原因在于容器可访问敏感的 OS 文件夹。

- **tmpfs 装载**类似仅存在于主机内存中的虚拟文件夹，并且绝不会写入文件系统。

从远程存储：

- [Azure 存储](https://azure.microsoft.com/documentation/services/storage/)提供可异地分发存储，为容器提供良好的持久性解决方案。

- 远程关系数据库（如 [Azure SQL 数据库](https://azure.microsoft.com/services/sql-database/)）、NoSQL 数据库（如 [Azure Cosmos DB](/azure/cosmos-db/introduction)）或缓存服务（如 [Redis](https://redis.io/)）。

从 Docker 容器：

- Docker 提供一个名为“覆盖文件系统”的功能  。 此功能实现了写入时复制任务，该任务将更新后的信息存储到容器的根文件系统中。 该信息“基于”容器所基于的原始映像。 如果从系统中删除容器，则这些更改将丢失。 因此，虽然可以在其本地存储中保存容器的状态，但根据此功能设计系统将与容器设计的前提相冲突，而容器设计在默认情况下为无状态。

- 但是，Docker 卷现在是处理 Docker 中的本地数据的首选方式。 如需了解有关容器中的存储的详细信息，请查看 [Docker 存储驱动程序](https://docs.docker.com/engine/userguide/storagedriver/)和[关于映像、容器和存储驱动程序](https://docs.docker.com/engine/userguide/storagedriver/imagesandcontainers/)。

下面提供了有关这些选项的其他详细信息。

**卷**是从主机 OS 映射到容器目录的目录。 当容器中的代码访问该目录时，该访问实际上是对主机操作系统上的一个目录的访问。 此目录不与容器自身的生存期绑定，且该目录由 Docker 管理并独立于主机计算机的核心功能。 因此，数据卷用于将独立于容器的生命周期的数据持久保存。 如果从 Docker 主机中删除容器或图像，不会删除数据卷中保留的数据。

可以命名卷，也可以让卷保持匿名（默认设置）。 命名卷是**数据卷容器**的演变，它使得在不同容器之间共享数据变得容易。 卷还支持卷驱动程序，使你可将数据存储在远程主机上等。

绑定装载早已可用且可将任何文件夹映射到容器中的装入点  。 绑定挂载的限制比卷的限制多，并存在一些重要安全问题，因此，卷是建议选项。

`tmpfs` 装载是仅存在于主机内存中的虚拟文件夹，并且绝不会写入文件系统  。 它们快速且安全，但使用内存且仅适用于非持久性数据。

如图 4-5 所示，常规 Docker 卷可以存储在容器本身之外，但是在主机服务器或 VM 的物理边界内。 但是，Docker 容器不能从一个主机服务器或 VM 访问另一个主机服务器或 VM 的卷。 换而言之，如果使用这些卷，那么不能管理在不同 Docker 主机上运行的容器之间共享的数据，尽管可通过支持远程主机的卷驱动程序来实现这一功能。

![显示存储在容器外部的 Docker 卷的示意图。](./media/state-and-data-in-docker-applications/container-based-application-external-data-sources.png)

**图 4-5**。 基于容器的应用程序的卷和外部数据源

此外，当 Docker 容器由业务流程协调程序管理时，容器可能会在主机之间“移动”，这取决于群集所执行的优化。 因此，不建议将数据卷用于业务数据。 但它们是一种良好的机制，可以处理跟踪文件、暂存文件或类似的不会影响业务数据一致性的文件。

远程数据源和缓存工具（如 Azure SQL 数据库、Azure Cosmos DB）或远程缓存（如 Redis）可以在容器化应用程序中使用，与开发时没有容器的使用方式相同  。 这是存储业务应用程序数据的一种行之有效的方法。

Azure 存储。  业务数据通常需要放在外部资源或数据库中，如 Azure 存储。 Azure 存储在云中提供以下服务：

- Blob 存储存储非结构化的对象数据。 Blob 可以是任何类型的文本或二进制数据，如文档或媒体文件（图像、音频和视频文件）。 Blob 存储也称为对象存储。

- 文件存储使用标准 SMB 协议为旧版应用程序提供共享存储。 Azure 虚拟机和云服务可以通过装载的共享在应用程序组件之间共享文件数据。 本地应用程序可以通过文件服务 REST API 访问共享中的文件数据。

- 表存储存储结构化数据集。 表存储是 NoSQL 键属性数据存储，它允许快速开发和快速访问大量数据。

关系数据库和 NoSQL 数据库。  外部数据库有很多选择，包括关系数据库（如 SQL Server、PostgreSQL、Oracle）或 NoSQL 数据库（如 Azure Cosmos DB、MongoDB）等。本指南不会对这些数据库加以解释，因为它们完全是不同的主题。

>[!div class="step-by-step"]
>[上一页](monolithic-applications.md)
>[下一页](soa-applications.md)
