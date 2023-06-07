---
title: Docker 简介
description: 本文简要概述 .NET Core 应用程序上下文中的 Docker。
ms.date: 03/20/2019
ms.custom: mvc
ms.openlocfilehash: 6303c16e914c86f3a431a9e5367e6feb8ff88c9e
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "95724737"
---
# <a name="introduction-to-net-and-docker"></a>.NET 和 Docker 简介

.NET Core 可以在 Docker 容器中轻松运行。 容器提供一种轻量级方法，用于将应用程序与主机系统的其他组件分隔，以便仅共享内核，并使用提供给应用程序的资源。 如果你不熟悉 Docker，强烈建议通读 Docker 的[概述文档](https://docs.docker.com/engine/docker-overview/)。

若要详细了解如何安装 Docker，请参阅 [Docker 桌面：社区版](https://www.docker.com/products/docker-desktop)的下载页面。

## <a name="docker-basics"></a>Docker 基础知识

首先来熟悉几个概念。 Docker 客户端具有可用于管理映像和容器的 CLI。 如前文所述，应花时间通读 [Docker 概述](https://docs.docker.com/engine/docker-overview/)文档。

### <a name="images"></a>映像

映像是构成容器基础的文件系统更改的有序集合。 映像不具有状态，并且是只读的。 大多情况下，一个映像以另一个映像为基础，但具有一些自定义设置。 例如，为应用程序创建新映像时，可将其基于已包含 .NET Core 运行时的现有映像。

由于容器是从映像创建的，因此，映像具有一组在容器启动时运行的运行参数（例如启动可执行文件）。

### <a name="containers"></a>容器

容器是映像的可运行实例。 生成映像时，部署应用程序和依赖项。 然后可以实例化多个容器，且每个容器相互独立。 每个容器实例具有其自己的文件系统、内存和网络接口。

### <a name="registries"></a>注册表

容器注册表是映像存储库的集合。 映像可以基于注册表映像。 可以直接从注册表中的映像创建容器。 [Docker 容器、映像和注册表之间的关系](../../architecture/microservices/container-docker-introduction/docker-containers-images-registries.md)是[构建和生成容器化应用程序或微服务](../../architecture/microservices/architect-microservice-container-applications/index.md)时的一个重要概念。 此方法大大缩短了开发和部署之间的时间。

Docker 具有一个托管在 [Docker 中心](https://hub.docker.com/)的公共注册表，可供用户使用。 [.NET Core 相关映像](https://hub.docker.com/_/microsoft-dotnet/)均在 Docker 中心列出。

[Microsoft 容器注册表 (MCR)](/azure/container-registry) 是 Microsoft 提供的容器映像的官方来源。 MCR 构建在 Azure CDN 之上，可提供用于全局复制的映像。 但是，MCR 没有面向公众的网站，了解有关 Microsoft 提供的容器映像的主要方法是通过 [Microsoft Docker 中心页面](https://hub.docker.com/_/microsoft-dotnet/)。

### <a name="dockerfile"></a>Dockerfile

Dockerfile 是定义可创建映像的一组指令的文件。 Dockerfile 中的每个指令创建映像中的一个层。 大多数情况下，在重新生成映像时，只会重新生成已发生更改的层。 可以将 Dockerfile 分发给其他人，便于他们采用你创建映像的方式重新创建一个新映像。 尽管可以分发有关如何创建映像的指令，但分发映像的主要方式是将其发布到注册表。

## <a name="net-core-images"></a>.NET Core 映像

官方 .NET Core Docker 映像发布到 Microsoft 容器注册表 (MCR)，用户可以在 [Microsoft.NET Core Docker 中心存储库](https://hub.docker.com/_/microsoft-dotnet/)中找到这些映像。 每个存储库包含 .NET（SDK 或运行时）和可以使用的操作系统的不同组合的映像。

Microsoft 提供适合特定场景的映像。 例如，[ASP.NET Core 存储库](https://hub.docker.com/_/microsoft-dotnet-aspnet/)提供针对在生产环境中运行 ASP.NET Core 应用生成的映像。

## <a name="azure-services"></a>Azure 服务

各种 Azure 服务都支持容器。 为应用程序创建 Docker 映像并将其部署到以下服务之一：

- [Azure Kubernetes 服务 (AKS)](https://azure.microsoft.com/services/kubernetes-service/)\
使用 kubernetes 缩放和编排 Linux 容器。

- [Azure 应用服务](https://azure.microsoft.com/services/app-service/containers/)\
在 PaaS 环境中使用 Linux 容器部署 Web 应用或 API。

- [Azure 容器实例](https://azure.microsoft.com/services/container-instances/)\
将容器托管在云中，而不使用任何高级管理服务。

- [Azure Batch](https://azure.microsoft.com/services/batch/)\
使用容器运行重复的计算作业。

- [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/)\
使用 Windows Server 容器直接迁移 .NET 应用程序并将其现代化为微服务。

- [Azure 容器注册表](https://azure.microsoft.com/services/container-registry/)\
在所有类型的 Azure 部署中存储和管理容器映像。

## <a name="next-steps"></a>后续步骤

- [了解如何容器化 .NET Core 应用。](build-container.md)
- [了解如何容器化 ASP.NET Core 应用程序。](/aspnet/core/host-and-deploy/docker/building-net-docker-images)
- [试学“ASP.NET Core 微服务”教程。](https://dotnet.microsoft.com/learn/web/aspnet-microservice-tutorial/intro)
- [了解 Visual Studio 中的容器工具](/visualstudio/containers/overview)
