---
title: Windows 上的 Visual Studio Tools for Docker
description: 了解 Visual Studio 2017 15.7 版及更高版本中可用的 Docker 工具。
ms.date: 08/06/2020
ms.custom: vs-dotnet
ms.openlocfilehash: ae20ebf7c3c27d7f2ebe51c33719b82048f86241
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032185"
---
# <a name="use-docker-tools-in-visual-studio-on-windows"></a>在 Windows 上的 Visual Studio 中使用 Docker 工具

使用 Visual Studio 2017 15.7 版及更高版本中附带的 Docker 工具的开发人员工作流与使用 Visual Studio Code 和 Docker CLI（事实上，它在同一 Docker CLI 基础上构建）类似，但前者更容易上手，因为它简化了流程并提高了构建、运行和创建任务的工作效率。 还可以通过 Visual Studio 中常用的 `F5` 和 `Ctrl+F5` 键运行和调试容器。 如果在解决方案级别的同一 `docker-compose.yml` 文件中定义其容器，甚至可以调试整个解决方案。

## <a name="configure-your-local-environment"></a>配置本地环境

使用最新版本的用于 Windows 的 Docker，开发 Docker 应用程序容易得多，因为设置非常简单，如以下参考资料中所述。

> [!TIP]
> 要了解有关安装用于 Windows 的 Docker 的详细信息，请转到 (<https://docs.docker.com/docker-for-windows/>)。

## <a name="docker-support-in-visual-studio"></a>Visual Studio 中的 Docker 支持

可以将两个级别的 Docker 支持添加到项目中。 在 ASP.NET Core 项目中，启用 Docker 支持，即可将 `Dockerfile` 文件添加项目中。 下一级是容器业务流程支持，它会向项目添加解决方案级别的 `Dockerfile`（如果它尚不存在）和 `docker-compose.yml` 文件。 默认情况下，在 Visual Studio 2017 15.0 到 15.7 版中添加了通过 Docker Compose 支持的容器业务流程支持。 容器业务流程支持是 Visual Studio 2017 15.8 版或更高版本中的一个可选功能。 Visual Studio 2019 及更高版本也支持 Kubernetes/Helm 部署。

“添加”>“Docker 支持”和“添加”>“容器业务流程协调程序支持”命令位于“解决方案资源管理器”中 ASP.NET Core 项目的项目节点的右键单击菜单（或上下文菜单）上，如图 4-31 所示：

![Visual Studio 中的“添加 Docker 支持”菜单选项](media/add-docker-support-menu.png)

**图 4-31**。 向 Visual Studio 2019 项目添加 Docker 支持

### <a name="add-docker-support"></a>添加 Docker 支持

除了上一节中所示的向现有应用程序添加 Docker 支持的选项之外，你也可以通过以下方法启用 Docker 支持：在项目创建期间，在“新建项目”对话框中单击“确定”后打开的“新建 ASP.NET Core Web 应用程序”对话框中选择“启用 Docker 支持”（如图 4-32 所示）   。

![在 Visual Studio 中为新的 ASP.NET Core Web 应用启用 Docker 支持](media/enable-docker-support-visual-studio.png)

**图 4-32**。 在 Visual Studio 2019 中创建项目期间启用 Docker 支持

添加或启用 Docker 支持时，Visual Studio 将向项目添加 Dockerfile 文件，其中包括对解决方案中所有必需项目的引用。

### <a name="add-container-orchestration-support"></a>添加容器业务流程支持

想要构建多容器解决方案时，请为项目添加容器业务流程支持。 这样就可以同时运行和调试一组容器（整个解决方案）（如果已在同一个 docker-compose.yml 文件中定义这些容器）。

要添加容器业务流程支持，请右键单击“解决方案资源管理器”中的解决方案或项目节点，然后选择“添加”>“容器业务流程支持”。 然后选择“Kubernetes/Helm”或“Docker Compose”以管理容器 。

向项目添加容器业务流程支持后，会看到添加到项目的 Dockerfile 文件以及添加到“解决方案资源管理器”中的某个解决方案的 docker-compose 文件夹，如图 4-33 所示 ：

![Visual Studio 解决方案资源管理器中的 Docker 文件](media/docker-support-solution-explorer.png)

**图 4-33**。 Visual Studio 2019 解决方案资源管理器中的 Docker 文件

如果 docker-compose.yml 已存在，Visual Studio 只需向其添加配置代码所需的行。

## <a name="configure-docker-tools"></a>配置 Docker 工具

从主菜单中，选择“工具”>“选项”，然后展开“容器工具”>“设置”。 随即出现容器工具设置。

![Visual Studio Docker 工具选项，其中显示三个页面：文章文本中详细介绍的“常规”、“单个项目”和“Docker Compose”。](media/visual-studio-docker-tools-options.png)

**图 4-34**。 Docker 工具选项

下表可帮助确定如何设置这些选项。

| 页面/设置                                |  默认设置   | 说明                                                                                                                                                                                                                                                                                                                                                                                                           |
| ------------------------------------------- | :----------------: | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **常规页**                            |
| 根据需要安装 Docker Desktop            |     提示我      |
| 根据需要启动 Docker Desktop              |     提示我      |
| 信任 ASP.NET Core SSL 证书          |     提示我      | 如果没有（使用 `dotnet dev-certs https --trust`）将 localhost SSL 证书标记为受信任，则 Visual Studio 将在每次运行项目时提示你。                                                                                                                                                                                                                                                    |
| **单个项目页**                     |
| 在项目打开时拉取所需的 Docker 映像 |        True        | 为了在运行项目时提高性能，Visual Studio 将在后台启动 Docker 拉取操作，以便在准备好运行代码时，映像已下载或正在下载。 如果只需加载项目和浏览代码，可以将其关闭，以避免下载不需要的容器映像。 这可能会降低打开项目的用户体验。 |
| 在项目加载时拉取更新后的 Docker 映像  | .NET Core 项目 | 将更新拉到现有映像，以获取项目打开时的最新更新。 这可能会降低打开项目的用户体验。                                                                                                                                                                                                                                                                                          |
| 在项目关闭时删除容器          |        True        | 在关闭项目时进行清理，这可能会减慢关闭项目的用户体验，但是无论如何通常都是很快的。                                                                                                                                                                                                                                                                                                            |
| 在项目打开时运行容器              |        True        | 为了在运行项目时提高性能，Visual Studio 将启动解决方案中的所有容器。 这可能会降低打开项目的用户体验。                                                                                                                                                                                                                                                        |
| **Docker Compose**                          |                    | Docker Compose 页包含与单个项目页相同的设置，但它们适用于多容器解决方案。                                                                                                                                                                                                                                                                                           |

> [!WARNING]
> 如果 localhost SSL 证书不受信任，并且你将选项设置为“绝不”，则 HTTPS Web 请求可能会在运行时在应用或服务中失败。 在这种情况下，请将值重新设置为“提示我”，或最好使用命令 `dotnet dev-certs https --trust` 信任开发计算机中的证书。

> [!TIP]
> 有关服务实现以及 Visual Studio Tools for Docker 用法的更多详细信息，请阅读以下文章：
>
> 在本地 Docker 容器中调试应用：<https://docs.microsoft.com/visualstudio/containers/edit-and-refresh>
>
> 使用 Visual Studio 将 ASP.NET 容器部署到容器注册表：<https://docs.microsoft.com/visualstudio/containers/hosting-web-apps-in-docker>

> [!div class="step-by-step"]
> [上一页](docker-apps-inner-loop-workflow.md)
> [下一页](set-up-windows-containers-with-powershell.md)
