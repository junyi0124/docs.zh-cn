---
title: 官方 .NET Docker 映像
description: 适用于容器化 .NET 应用程序的 .NET 微服务体系结构 | 官方 .NET Docker 映像
ms.date: 01/30/2020
ms.openlocfilehash: 9339c21897749db854262e280321658ce1f541cb
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95675511"
---
# <a name="official-net-docker-images"></a>官方 .NET Docker 映像

官方 .NET Docker 映像是由 Microsoft 创建和优化的 Docker 映像。 这些映像在 [Docker 中心](https://hub.docker.com/u/microsoft/)的 Microsoft 存储库中公开提供。 每个存储库可以包含多个映像，具体取决于 .NET 版本以及操作系统和版本（Linux Debian、Linux Alpine、Windows Nano Server、Windows Server Core 等）。

自 .NET Core 2.1 起，包括 ASP.NET Core 在内的所有 .NET Core 映像都在 .NET Core 映像存储库的 Docker Hub (<https://hub.docker.com/_/microsoft-dotnet/>) 提供。

自 2018 年 5 月起，Microsoft 映像将[联合到 Microsoft 容器注册表](https://azure.microsoft.com/blog/microsoft-syndicates-container-catalog/)中。 官方目录仍然仅在 Docker 中心中提供，可在此处找到更新的地址以拉取映像。

大多数映像存储库提供广泛的标记，以帮助选择特定的框架版本以及 OS（Linux 发行版或 Windows 版本）。

## <a name="net-core-and-docker-image-optimizations-for-development-versus-production"></a>适用于开发与生产的 .NET Core 和 Docker 映像优化

为开发人员生成 Docker 映像时，Microsoft 侧重于以下主要方案：

- 用于开发和生成 .NET Core 应用的映像  。

- 用于运行 .NET Core 应用的映像  。

为什么是多个映像？ 因为在开发、生成和运行容器化应用程序时，通常具有不同的优先级。 通过为这些单独的任务提供不同的映像，Microsoft 有助于优化开发、生成和部署应用程序的单独进程。

### <a name="during-development-and-build"></a>在开发和生成过程中

在开发期间，重要的是可循环访问更改的速度以及调试更改的能力。 与更改代码的能力和快速查看更改相比，映像的大小不是那么重要。 某些工具和“build-agent 容器”在开发和生成进程中使用 .NET Core 开发映像 (mcr.microsoft.com/dotnet/sdk:3.1)。 在 Docker 容器中生成时，重要方面是为了编译应用所需要的元素。 这包括编译器和任何其他 .NET 依赖项。

为什么此类型的生成映像很重要？ 不能将此映像部署到生产中。 相反，它是用于生成放置在生产映像中的内容的映像。 此映像将用于持续集成 (CI) 环境，或在使用 Docker 多阶段生成时用于生成环境。

### <a name="in-production"></a>生产中

在生产中重要的是基于生产 .NET Core 映像部署和启动容器的速度。 因此，基于 mcr.microsoft.com/dotnet/aspnet:3.1 的仅运行时映像很小，可方便地通过网络从 Docker 注册表快速传输到 Docker 主机。 已准备运行内容，以此实现从启动容器到处理结果的最快时间。 在 Docker 模型中，不需要编译 C\# 代码，但在使用生成容器运行 dotnet 生成或 dotnet 发布时需要。

在此优化的映像中，只放置运行应用程序所需的二进制文件和其他内容。 例如，由 `dotnet publish` 创建的内容仅包含已编译的.NET 二进制文件、映像、.js 和 .css 文件。 随着时间推移，将推出包含预实时编译（在运行时进行从中间语言到本机语言的编译）包的映像。

虽然 .NET Core 和 ASP.NET Core 映像有多个版本，但它们全都共享一个或多个层，包括基本层。 因此，存储映像所需的磁盘空间量很小；它仅包含自定义映像和其基础映像之间的增量。 结果，从注册表中提取映像速度会很快。

在 Docker 中心浏览 .NET 映像存储库时，会发现已使用标记将多个映像版本进行分类或标记。 这些标记有助于决定使用哪一个，具体取决于需要的版本，如下表所示：

| Image | 注释 |
|-------|----------|
| mcr.microsoft.com/dotnet/aspnet:3.1 | ASP.NET Core，包含仅运行时和 ASP.NET Core 优化，适用于 Linux 和 Windows（多体系结构） |
| mcr.microsoft.com/dotnet/sdk:3.1 | .NET Core，包含 SDK，适用于 Linux 和 Windows（多体系结构） |

> [!div class="step-by-step"]
> [上一页](net-container-os-targets.md)
> [下一页](../architect-microservice-container-applications/index.md)
