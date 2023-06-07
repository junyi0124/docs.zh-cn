---
title: 基于 Docker 的应用程序的开发流程
description: 从较高的层面对开发基于 Docker 应用程序的选项进行概述。 使用所选的 Visual Studio for Windows、Visual Studio for Mac 或 Visual Studio Code 获得多平台支持（Windows、macOS 和 Linux）。
ms.date: 01/30/2020
ms.openlocfilehash: 799aa6fc742a8fb763ec5a7ae3cf3f70f89bed6d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2020
ms.locfileid: "77502721"
---
# <a name="development-process-for-docker-based-applications"></a>基于 Docker 的应用程序的开发流程

*用你喜欢的方式开发容器化 .NET 应用程序：主要使用集成开发环境 (IDE)，可辅以 Visual Studio 和 Visual Studio Tools for Docker；也可以主要使用 CLI/编辑器，辅以 Docker CLI 和 Visual Studio Code。*

## <a name="development-environment-for-docker-apps"></a>Docker 应用的开发环境

### <a name="development-tool-choices-ide-or-editor"></a>开发工具选项：IDE 或编辑器

无论你更青睐内容丰富、功能强大的 IDE 还是轻量、灵活的级编辑器，Microsoft 都可为你提供用于开发 Docker 应用程序的工具。

**Visual Studio（适用于 Windows）。** 使用 Visual Studio 的基于 Docker 的 .NET Core 3.1 应用程序开发需要 Visual Studio 2019 版本 16.4 或更高版本。 Visual Studio 2019 附带已内置的 Tools for Docker。 通过适用于 Docker 的工具，可以在目标 Docker 环境中开发、运行和验证应用程序。 可以按 F5，直接在 Docker 主机中运行并调试应用程序（单个容器或多个容器），也可以按 Ctrl+F5，编辑并刷新应用程序，而无需重新生成该容器。 要开发基于 Docker 的应用，这是功能最强大的选择。

**Visual Studio for Mac。** 它是一个 IDE，由 Xamarin Studio 演化而来，在 macOS 中运行。 对于 .NET Core 3.1 开发，它需要版本 8.4 或更高版本。 对于使用 macOS 计算机工作而又希望使用功能强大的 IDE 的开发者而言，这应当是理想之选。

**Visual Studio Code 和 Docker CLI**。 如果更青睐支持任何开发语言的轻量级跨平台编辑器，可以使用 Visual Studio Code 和 Docker CLI。 这是针对 macOS、Linux 和 Windows 的跨平台开发方法。 此外，Visual Studio Code 还支持 Docker 扩展（例如适用于 Dockerfile 的 IntelliSense）和在编辑器中运行 Docker 命令的快捷任务。

安装 [Docker Desktop 社区版 (CE)](https://hub.docker.com/search/?type=edition&offering=community)，可以使用单个 Docker CLI 生成适用于 Windows 和 Linux 的应用。

### <a name="additional-resources"></a>其他资源

- **Visual Studio**。 官方网站。 \
  [https://visualstudio.microsoft.com/vs/](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link)

- **Visual Studio Code**. 官方网站。 \
  <https://code.visualstudio.com/download>

- **用于 Windows 的 Docker Desktop 社区版 (CE)**  \
  [https://hub.docker.com/editions/community/docker-ce-desktop-windows](https://hub.docker.com/editions/community/docker-ce-desktop-windows)

- **用于 Mac 的 Docker Desktop 社区版 (CE)**  \
  [https://hub.docker.com/editions/community/docker-ce-desktop-mac](https://hub.docker.com/editions/community/docker-ce-desktop-mac)

## <a name="net-languages-and-frameworks-for-docker-containers"></a>适用于 Docker 容器的 .NET 语言和框架

如本指南的前面几节所述，开发 Docker 容器化 .NET 应用程序时，可以使用 .NET Framework、.NET Core 或开放源 Mono 项目。 面向 Linux 或 Windows 容器时，根据所使用的 .NET 框架，可以用 C\#、F\# 或 Visual Basic 语言进行开发。 有关 .NET 语言的详细信息，请参阅博客文章 [The .NET Language Strategy](https://devblogs.microsoft.com/dotnet/the-net-language-strategy/)（.NET 语言策略）。

>[!div class="step-by-step"]
>[上一页](../architect-microservice-container-applications/scalable-available-multi-container-microservice-applications.md)
>[下一页](docker-app-development-workflow.md)
