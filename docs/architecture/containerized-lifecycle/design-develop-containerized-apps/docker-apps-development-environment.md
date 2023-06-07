---
title: Docker 应用的开发环境
description: 了解支持 Docker 开发生命周期的最重要的开发工具选项。
ms.date: 08/06/2020
ms.openlocfilehash: 07b42b2bd05ab16ba0fbf61863b050ee2c9e242b
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87916034"
---
# <a name="development-environment-for-docker-apps"></a>Docker 应用的开发环境

## <a name="development-tools-choices-ide-or-editor"></a>开发工具选择：IDE 或编辑器

无论你更青睐内容丰富、功能强大的 IDE 还是灵活轻量的编辑器，Microsoft 在开发 Docker 应用程序时都能满足你的需求。

### <a name="visual-studio-code-and-docker-cli-cross-platform-tools-for-mac-linux-and-windows"></a>Visual Studio Code 和 Docker CLI（适用于 Mac、Linux 和 Windows 的跨平台工具）

如果更青睐支持任何开发语言的轻量级跨平台编辑器，可以使用 Visual Studio Code 和 Docker CLI。 这些产品提供简单而可靠的体验，这对简化开发人员工作流至关重要。 安装“用于 Mac 的 Docker”或“用于 Windows 的 Docker”（开发环境），Docker 开发人员即可使用单个 Docker CLI 为 Windows 或 Linux（运行时环境）构建应用。 此外，Visual Studio Code 还支持 Docker 扩展（例如适用于 Dockerfile 的 IntelliSense）和在编辑器中运行 Docker 命令的快捷任务。

> [!NOTE]
> 要下载 Visual Studio Code，请转到 <https://code.visualstudio.com/download>。
>
> 要下载用于 Mac 和 Windows 的 Docker，请转到 <https://www.docker.com/products/docker>。

### <a name="visual-studio-with-docker-tools-windows-development-machine"></a>附带 Docker 工具的 Visual Studio（Windows 开发计算机）

建议使用 Visual Studio 2019 并启用内置 Docker 工具。 使用 Visual Studio，可以直接在所选的 Docker 环境中开发、运行和验证应用程序。 按 F5，即可直接在 Docker 主机中调试应用程序（单个容器或多个容器）；也可以按 Ctrl+F5，无需重新生成容器，即可编辑并刷新应用。 对于创建用于 Linux 或 Windows 的 Docker 容器的 Windows 开发人员，这是最简单且功能最强大的选择。

### <a name="visual-studio-for-mac-mac-development-machine"></a>Visual Studio for Mac（Mac 开发计算机）

开发基于 Docker 的应用程序时，可以使用 [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link)。 与 Visual Studio Code for Mac 相比，Visual Studio for Mac 提供了更丰富的 IDE。

## <a name="language-and-framework-choices"></a>选择语言和框架

可以使用 Microsoft 工具和大多数现代语言开发 Docker 应用程序。 以下是初始列表，但不局限于该表：

- .NET Core 和 ASP.NET Core
- Node.js
- Go
- Java
- Ruby
- Python

基本上可以使用由 Linux 或 Windows 中的 Docker 支持的任何现代语言。

>[!div class="step-by-step"]
>[上一页](deploy-azure-kubernetes-service.md)
>[下一页](docker-apps-inner-loop-workflow.md)
