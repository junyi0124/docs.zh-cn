---
title: 使用 .NET 容器时定位的操作系统
description: 适用于容器化 .NET 应用程序的 .NET 微服务体系结构 | 使用 .NET 容器时定位的操作系统
ms.date: 01/30/2020
ms.openlocfilehash: 1f52dcb4da6509be7e771af353daea9cfc97306c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95688453"
---
# <a name="what-os-to-target-with-net-containers"></a>使用 .NET 容器时定位的操作系统

由于 Docker 支持多种操作系统，且鉴于 .NET Framework 和 .NET Core 之间的差异，应根据所使用的框架，面向特定操作系统和特定版本。

对于 Windows，可使用 Windows Server Core 或 Windows Nano Server。 这两种 Windows 版本分别提供 .NET Framework 和 .NET Core 各自所需的特征（Windows Server Core 中的 IIS 与 Nano Server 中自承载的 web 服务器，如 Kestrel）。

对于 Linux，正式的 .NET Docker 映像（如 Debian）中提供并支持多个发行版本。

图 3-1 显示基于所使用的 .NET framework，可使用的操作系统版本。

![显示要与哪些 .NET 容器一起使用的操作系统的关系图。](./media/net-container-os-targets/targeting-operating-systems.png)

**图 3-1。** 根据 .NET framework 确定要面向的操作系统

部署旧的 .NET Framework 应用程序时，必须以与旧应用程序和 IIS 兼容但具有更大映像的 Windows Server Core 为目标。 部署 .NET Core 应用程序时，可以针对已经过云优化、使用 Kestrel、更小且启动速度更快的 Windows Nano Server。 还可以面向 Linux，支持 Debian、Alpine 和其他操作系统。 也使用 Kestrel、更小且启动速度更快。

如果想使用不同的 Linux 发行版本或要使用 Microsoft 不支持的映像版本，还可以创建自己的 Docker 映像。 例如，可以使用 ASP.NET Core 创建一个在传统 .NET Framework 和 Windows Server Core 上运行的映像，但这不是常见的 Docker 方案。

> [!IMPORTANT]
> 与完整的 Windows 映像相比，使用 Windows Server Core 映像时，你可能会发现缺少某些 DLL。 可以通过创建自定义 Server Core 映像，在映像构建时添加缺失的文件来解决此问题，如本 [GitHub 注释](https://github.com/microsoft/dotnet-framework-docker/issues/299#issuecomment-511537448)中所述。

将映像名称添加到 Dockerfile 文件后，可根据所使用的标记选择操作系统和版本，如下例所示：

| 图像 | 注释 |
|-------|----------|
| mcr.microsoft.com/dotnet/runtime:3.1 | .NET Core 3.1 多体系结构：支持 Linux 和 Windows Nano Server，具体取决于 Docker 主机。 |
| mcr.microsoft.com/dotnet/aspnet:3.1 | ASP.NET Core 3.1 多体系结构：支持 Linux 和 Windows Nano Server，具体取决于 Docker 主机。 <br/> ASP.NET Core 的 aspnetcore 映像具有多个优化。 |
| mcr.microsoft.com/dotnet/aspnet:3.1-buster-slim | Linux Debian 发行版上的 .NET Core 3.1 仅运行时 |
| mcr.microsoft.com/dotnet/aspnet:3.1-nanoserver-1809 | Windows Nano Server（Windows Server 版本 1809）上的 .NET Core 3.1 仅运行时 |

## <a name="additional-resources"></a>其他资源

- **由于缺少 WindowsCodecsExt.dll 而导致 BitmapDecoder 产生故障（GitHub 问题）**  
  <https://github.com/microsoft/dotnet-framework-docker/issues/299>

> [!div class="step-by-step"]
> [上一页](container-framework-choice-factors.md)
> [下一页](official-net-docker-images.md)
