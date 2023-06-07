---
title: Docker 应用开发工作流
description: 了解用于开发基于 Docker 的应用程序的工作流的详细信息。 分步深入了解有关优化 Dockerfile 的详细信息，最后了解使用 Visual Studio 时使用的简化工作流。
ms.date: 01/30/2020
ms.openlocfilehash: 4019eed6b814f4c7e8bc4f32758e8cfd7f4c7ec9
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95711170"
---
# <a name="development-workflow-for-docker-apps"></a>Docker 应用开发工作流

应用程序开发生命周期从你的计算机开始，你作为开发人员在该计算机上使用首选语言对应用程序进行编码和本地测试。 使用此工作流，无论选择哪种语言、框架和平台，你都可以开发和测试 Docker 容器，但均在本地执行操作。

每个容器（Docker 映像的实例）都包含以下组成部分：

- 操作系统选择（例如，Linux 发行版、Windows Nano Server 或 Windows Server Core）。

- 开发过程中添加的文件（例如源代码和应用程序二进制文件）。

- 配置信息（例如环境设置和依赖项）。

## <a name="workflow-for-developing-docker-container-based-applications"></a>开发基于 Docker 容器的应用程序的工作流

本节介绍基于 Docker 容器的应用程序的内部循环开发工作流  。 内部循环工作流是指不考虑更广泛的 DevOps 工作流（最多可以包括生产部署），只关注在开发人员的计算机上进行的开发工作。 其中不包括设置环境的初始步骤，因为这些步骤只需进行一次。

应用程序由开发人员自己的服务和附加库（依赖项）组成。 以下是生成 Docker 应用程序时常用的基本步骤，如图 5-1 所示。

:::image type="complex" source="./media/docker-app-development-workflow/life-cycle-containerized-apps-docker-cli.png" alt-text="显示创建容器化应用程序所用的 7 个步骤的示意图。":::
Docker 应用的开发流程：1 - 编写应用代码，2 - 编写 Dockerfile/s，3 - 创建在 Dockerfile/s 上定义的映像，4 -（可选）在 docker-compose.yml 文件中编写服务，5 - 运行容器或 docker-compose 应用，6 - 测试应用或微服务，7 - 推送到存储库并重复操作。
:::image-end:::

**图 5-1**。 开发 Docker 容器化应用的分步工作流

本部分详细介绍了整个流程，并着重通过 Visual Studio 环境解释了每个主要步骤。

使用编辑器/CLI 开发方法（例如，Visual Studio Code 和 macOS 或 Windows 上的 Docker CLI）时，需了解每个步骤，通常需要比使用 Visual Studio 了解得更详细。 有关在 CLI 环境中进行开发的详细信息，请参阅电子书[Containerized Docker Application lifecycle with Microsoft Platforms and Tools](https://aka.ms/dockerlifecycleebook/)（容器化 Docker 应用程序生命周期与 Microsoft 平台和工具）。

使用 Visual Studio 2019 时，许多步骤都无需开发人员执行，可显著提高工作效率。 使用 Visual Studio 2019 开发多容器应用程序时更是如此。 例如，只需单击一下，Visual Studio 就会将 `Dockerfile` 和 `docker-compose.yml` 文件添加到含有应用程序配置的项目。 在 Visual Studio 中运行应用程序时，系统会生成 Docker 映像并在 Docker 中直接运行多容器应用程序；开发人员甚至还能同时调试多个容器。 这些功能可大大提高开发速度。

但即使 Visual Studio 可以自动执行这些步骤，这也并不意味着开发人员不需要了解 Docker 的工作原理。 因此，下面的指南详细介绍了每个步骤。

![步骤 1 的映像。](./media/docker-app-development-workflow/step-1-code-your-app.png)

## <a name="step-1-start-coding-and-create-your-initial-application-or-service-baseline"></a>步骤 1。 开始编码并创建初始应用程序或服务基线

开发 Docker 应用程序的方式与开发不带 Docker 的应用程序的方式类似。 二者的区别在于，开发 Docker 应用程序时，是在本地环境中部署和测试在 Docker 容器中运行的应用程序或服务（由 Docker 设置的 Linux VM，或者如果使用 Windows 容器，则直接是 Windows）。

### <a name="set-up-your-local-environment-with-visual-studio"></a>使用 Visual Studio 设置本地环境

首先，请务必按以下说明所述安装适用于 Windows 的 [Docker 社区版 (CE)](https://docs.docker.com/docker-for-windows/)：

[适用于 Windows 的 Docker CE 入门](https://docs.docker.com/docker-for-windows/)

此外，需要安装了“.NET Core 跨平台开发”工作负载的 Visual Studio 2019 版本 16.4 或更高版本，如图 5-2 所示  。

![“.NET Core 跨平台”开发选择的屏幕截图。](./media/docker-app-development-workflow/dotnet-core-cross-platform-development.png)

**图 5-2**。 在 Visual Studio 2019 设置过程中，选择“.NET Core 跨平台开发”工作负载 

即使尚未在应用程序中启用 Docker，开发人员也可以开始在普通 .NET 中编写应用程序（如果打算使用容器，则通常在 .NET Core 中进行），并在 Docker 中进行部署和测试。 但建议尽快开始使用 Docker，因为这才是真实环境，并且能快速发现存在的问题。 推荐这样做是因为通过 Visual Studio 可轻松使用 Docker，开发人员能够快速明白，在 Visual Studio 中能调试多容器应用程序就是一个很好的示例。

### <a name="additional-resources"></a>其他资源

- 适用于 Windows 的 Docker CE 入门   \
  <https://docs.docker.com/docker-for-windows/>

- **Visual Studio 2019** \
  [https://visualstudio.microsoft.com/downloads/](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)

![步骤 2 的映像。](./media/docker-app-development-workflow/step-2-write-dockerfile.png)

## <a name="step-2-create-a-dockerfile-related-to-an-existing-net-base-image"></a>步骤 2。 创建与现有 .NET 基础映像相关的 Dockerfile

要生成自定义映像，需为每个自定义映像提供一个 Dockerfile；无论是从 Visual Studio 自动部署，还是使用 Docker CLI（docker run 和 docker-compose 命令）手动部署，也需为每个要部署的容器提供一个 Dockerfile。 如果应用程序只包含一个自定义服务，则只需要一个 Dockerfile。 如果应用程序包含多个服务（如在微服务体系结构中），则每个服务都需要一个 Dockerfile。

将 Dockerfile 放在应用程序或服务的根文件夹中。 Dockerfile 包含多个命令，可指示 Docker 如何在容器中设置和运行应用程序或服务。 开发人员可在代码中手动创建 Dockerfile，并将其与 .NET 依赖项一起添加到项目中。

借助 Visual Studio 及其 Docker 工具，只需单击几次鼠标即可完成此任务。 在 Visual Studio 2019 中新建项目时，可看到一个名为“启用 Docker 支持”的选项，如图 5-3 所示  。

![显示“启用 Docker 支持”复选框的屏幕截图。](./media/docker-app-development-workflow/enable-docker-support-check-box.png)

**图 5-3**。 在 Visual Studio 2019 中创建新的 ASP.NET Core 项目时，启用 Docker 支持

也可以如图 5-4 所示，右键单击“解决方案资源管理器”中的项目，并选择“添加” > “Docker 支持...”，从而在现有 ASP.NET Core Web 应用项目中启用 Docker 支持    。

![显示“添加”菜单中“Docker 支持”选项的屏幕截图。](./media/docker-app-development-workflow/add-docker-support-option.png)

**图 5-4**。 在现有 Visual Studio 2019 项目中启用 Docker 支持

此操作可将 Dockerfile 添加到具有所需配置的项目，并且仅可用于 ASP.NET Core 项目  。

与其类似，Visual Studio 也可以为具有“添加 > 容器业务流程协调程序支持...”选项的整个解决方案添加 `docker-compose.yml` 文件  。在步骤 4 中，我们将更详细地探讨此选项。

### <a name="using-an-existing-official-net-docker-image"></a>使用现有的官方 .NET Docker 映像

开发人员通常以基础映像（从 [Docker 中心](https://hub.docker.com/)注册表等官方存储库中获取）为基础生成容器的自定义映像。 确切地说，在 Visual Studio 中启用 Docker 支持后就可以执行此操作。 Dockerfile 将使用现有的 `dotnet/core/aspnet` 映像。

之前我们介绍了根据开发人员选择的框架和操作系统可使用的 Docker 映像和存储库。 例如，如果想使用 ASP.NET Core（Linux 或 Windows），则使用的映像是 `mcr.microsoft.com/dotnet/aspnet:3.1`。 因此，只需指定用于容器的基础 Docker 映像即可。 为此，将 `FROM mcr.microsoft.com/dotnet/aspnet:3.1` 添加到 Dockerfile。 Visual Studio 会自动执行此操作，但若要更新版本，则需更新此值。

使用从 Docker 中心获取的带版本号的官方 .NET 映像存储库可确保能在所有计算机（包括用于开发、测试和生产的计算机）上使用相同的语言功能。

以下示例显示了 ASP.NET Core 容器的示例 Dockerfile。

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:3.1
ARG source
WORKDIR /app
EXPOSE 80
COPY ${source:-obj/Docker/publish} .
ENTRYPOINT ["dotnet", " MySingleContainerWebApp.dll "]
```

在此情况下，该映像以官方 ASP.NET Core Docker 映像的版本 3.1（适用于 Linux 和 Windows 的多体系结构）为基础。 这是 `FROM mcr.microsoft.com/dotnet/aspnet:3.1` 设置。 （有关此基础映像的详细信息，请参阅 [ASP.NET Core Docker 映像](https://hub.docker.com/_/microsoft-dotnet-aspnet/)页面。）在 Dockerfile 中，还需指示 Docker 侦听要在运行时使用的 TCP 端口（示例中由 EXPOSE 设置配置的 TPC 端口为 80）。

可在 Dockerfile 中指定其他配置设置，具体取决于使用的语言和框架。 例如，带有 `["dotnet", "MySingleContainerWebApp.dll"]` 的 ENTRYPOINT 行可指示 Docker 运行 .NET Core 应用程序。 如果使用 SDK 和 .NET Core CLI (dotnet CLI) 来生成和运行 .NET 应用程序，则此设置会有所不同。 底部的 ENTRYPOINT 行和其他设置会有所不同，具体取决于为应用程序选择的语言和平台。

### <a name="additional-resources"></a>其他资源

- 为 .NET Core 应用程序生成 Docker 映像   \
  [https://docs.microsoft.com/dotnet/core/docker/building-net-docker-images](/aspnet/core/host-and-deploy/docker/building-net-docker-images)

- **生成开发人员自己的映像**。 请查看官方 Docker 文档。\
  <https://docs.docker.com/engine/tutorials/dockerimages/>

- 保持 .NET 容器映像的最新状态   \
  <https://devblogs.microsoft.com/dotnet/staying-up-to-date-with-net-container-images/>

- 将 .NET 与 Docker 一起使用 - DockerCon 2018 更新   \
  <https://devblogs.microsoft.com/dotnet/using-net-and-docker-together-dockercon-2018-update/>

### <a name="using-multi-arch-image-repositories"></a>使用多体系结构映像存储库

单个存储库中可包含平台变量，如 Linux 映像和 Windows 映像。 借助此功能，Microsoft （基础映像创建者）等供应商可创建涵盖多个平台（即 Linux 和 Windows）的单个存储库。 例如，Docker 中心注册表中提供的 [dotnet/core](https://hub.docker.com/_/microsoft-dotnet/) 存储库可使用相同的存储库名称为 Linux 和 Windows Nano Server 提供支持。

如果指定了标签，请明确指定一个平台，如下所示：

- `mcr.microsoft.com/dotnet/aspnet:3.1-buster-slim` \
  目标：Linux 上的 .NET Core 3.1 仅运行时

- `mcr.microsoft.com/dotnet/aspnet:3.1-nanoserver-1909` \
  目标：Windows Nano Server 上的 .NET Core 3.1 仅运行时

但如果指定相同的映像名称，即使使用的标记相同，多体系结构映像（如 `aspnet` 映像）也将使用 Linux 或 Windows 版本，具体取决于部署的 Docker 主机操作系统，如下例所示：

- `mcr.microsoft.com/dotnet/aspnet:3.1` \
  多体系结构：Linux 或 Windows Nano Server 上的 .NET Core 3.1 仅运行时，具体取决于 Docker 主机操作系统

这样一来，从 Windows 主机拉取映像时，也会拉取 Windows 变体，并且从 Linux 主机拉取同一映像名称时，也会拉取 Linux 变体。

### <a name="multi-stage-builds-in-dockerfile"></a>Dockerfile 中的多阶段生成

Dockerfile 类似于批处理脚本。 类似于在必须从命令行设置计算机时你会执行的操作。

从设置初始上下文的基本映像开始，就像位于主机操作系统上的启动文件系统。 它不是操作系统，但是可以将其视为近似于容器内的操作系统。

执行每个命令行都会在文件系统上创建新的层，其中包含对上一个层的更改，这样一来，合并时可以生成相应的文件系统。

由于每个新的层都位于上一层之上，并且随着每个命令的执行，生成的图像大小也会增加，因此如果必须添加生成和发布应用程序所需的 SDK，那么映像可能会变得非常大。

在此情况下，多阶段生成（从 Docker 17.05 及更高版本开始）可以发挥奇妙的作用。

核心理念如下：可以将 Dockerfile 执行过程分为几个阶段，其中一个阶段为初始映像，后续阶段是一个或多个命令，最后阶段确定最终的映像大小。

简言之，可以使用多阶段生成将创建拆分为不同的“阶段”，然后只采用中间阶段的相关目录来组建最终的映像。 使用此功能的一般策略为：

1. 将基础 SDK 映像（无论大小）用于生成应用程序并将其发布到文件夹所需的一切内容，然后

2. 使用基础、仅运行时的小型映像，并从上一阶段复制发布文件夹，以生成小的最终映像。

了解多阶段的最好方法可能是逐行仔细浏览 Dockerfile，因此让我们开始浏览初始 Dockerfile（由 Visual Studio 向项目中添加 Docker 支持时所创建，我们稍后将对其进行一些优化）。

初始 Dockerfile 可能如下所示：

```dockerfile
 1  FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base
 2  WORKDIR /app
 3  EXPOSE 80
 4
 5  FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build
 6  WORKDIR /src
 7  COPY src/Services/Catalog/Catalog.API/Catalog.API.csproj …
 8  COPY src/BuildingBlocks/HealthChecks/src/Microsoft.AspNetCore.HealthChecks …
 9  COPY src/BuildingBlocks/HealthChecks/src/Microsoft.Extensions.HealthChecks …
10  COPY src/BuildingBlocks/EventBus/IntegrationEventLogEF/ …
11  COPY src/BuildingBlocks/EventBus/EventBus/EventBus.csproj …
12  COPY src/BuildingBlocks/EventBus/EventBusRabbitMQ/EventBusRabbitMQ.csproj …
13  COPY src/BuildingBlocks/EventBus/EventBusServiceBus/EventBusServiceBus.csproj …
14  COPY src/BuildingBlocks/WebHostCustomization/WebHost.Customization …
15  COPY src/BuildingBlocks/HealthChecks/src/Microsoft.Extensions …
16  COPY src/BuildingBlocks/HealthChecks/src/Microsoft.Extensions …
17  RUN dotnet restore src/Services/Catalog/Catalog.API/Catalog.API.csproj
18  COPY . .
19  WORKDIR /src/src/Services/Catalog/Catalog.API
20  RUN dotnet build Catalog.API.csproj -c Release -o /app
21
22  FROM build AS publish
23  RUN dotnet publish Catalog.API.csproj -c Release -o /app
24
25  FROM base AS final
26  WORKDIR /app
27  COPY --from=publish /app .
28  ENTRYPOINT ["dotnet", "Catalog.API.dll"]
```

以下为每一行的详细信息：

- **第 1 行：** 使用“小型”仅运行时基础映像开始一个阶段，将其称为“基础”，以供参考  。

- **第 2 行：** 在映像中创建 /app 目录  。

- **第 3 行：** 公开端口 80  。

- **第 5 行：** 使用“大型”映像开始用于生成/发布的新阶段。 将其称为“生成”  ，以供参考。

- **第 6 行：** 在映像中创建目录 /src  。

- **第 7 行：** 在第 16 行，复制引用的 .csproj 项目文件，以便之后能够还原包  。

- **第 17 行：** 还原 Catalog.API 项目和引用项目的包  。

- **第 18 行：** 将解决方案的所有目录树（.dockerignore 文件中包含的文件/目录除外）复制到映像中的 /src 目录    。

- **第 19 行：** 将当前文件夹更改为 Catalog.API 项目  。

- **第 20 行：** 生成项目（和其他项目依赖项）并输出到映像中的 /app 目录  。

- **第 22 行：** 开始一个从“生成”继续的新阶段。 将它称为“发布”  以进行引用。

- **第 23 行：** 发布项目（和依赖项）并输出到映像中的 /app 目录  。

- **第 25 行：** 开始一个从“基础”继续的新阶段，并将其称为“最终”   。

- **第 26 行：** 将当前目录更改为 /app  。

- **第 27 行：** 将 /app 目录从阶段“发布”复制到当前目录   。

- **第 28 行：** 定义启动容器时要运行的命令。

现在让我们探索一些用于提高整个流程性能（以 eShopOnContainer 为例，这意味着在 Linux 容器中构建完整的解决方案需要大约 22 分钟或更长时间）的优化。

你将利用 Docker 的层缓存功能，该功能非常简单：如果基础映像和命令与之前执行的相同，那么它可以直接使用生成的层，而不需要执行命令，从而节省一些时间。

因此，让我们重点关注“生成”阶段，第 5-6 行基本相同，但是第 7-17 行与 eShopOnContainer 中每个服务不同，因此它们必须每次都执行命令，但是如果将第 7-16 行更改为： 

```dockerfile
COPY . .
```

然后，对于每项服务都相同，它会复制整个解决方案并创建一个更大的层，但是：

1. 仅在第一次时（以及如果文件发生更改，则在重新构建时）执行复制进程，而且复制进程会使用所有其他服务的缓存，并且

2. 由于较大的映像发生在中间阶段，因此不会影响最终的映像大小。

下一个重要的优化涉及在第 17 行中执行的 `restore` 命令，其对于 eShopOnContainers 的每项服务也是不同的。 如果把该行更改为：

```dockerfile
RUN dotnet restore
```

它将还原整个解决方案的包，但它只还原一次，而不是当前策略中的 15 次。

然而，`dotnet restore` 仅当文件夹中有单个项目或解决方案文件时才会运行，因此实现这一点有点复杂，解决此问题且无需涉及太多细节的方法是：

1. 将以下命令行添加到 .dockerignore： 

   - `*.sln`，以忽略主文件夹树中的所有解决方案文件

   - `!eShopOnContainers-ServicesAndWebApps.sln`，以仅添加此解决方案文件。

2. 将 `/ignoreprojectextensions:.dcproj` 参数添加到 `dotnet restore`，因此它也忽略 docker-compose 项目，且仅还原 eShopOnContainers-ServicesAndWebApps 解决方案的包。

在最后的优化中，第 20 行是多余的，因为第 23 行也生成应用程序，并且实质上它在第 20 行之后运行，所以这又是一个耗时的命令。

生成的文件为：

```dockerfile
 1  FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base
 2  WORKDIR /app
 3  EXPOSE 80
 4
 5  FROM mcr.microsoft.com/dotnet/sdk:3.1 AS publish
 6  WORKDIR /src
 7  COPY . .
 8  RUN dotnet restore /ignoreprojectextensions:.dcproj
 9  WORKDIR /src/src/Services/Catalog/Catalog.API
10  RUN dotnet publish Catalog.API.csproj -c Release -o /app
11
12  FROM base AS final
13  WORKDIR /app
14  COPY --from=publish /app .
15  ENTRYPOINT ["dotnet", "Catalog.API.dll"]
```

### <a name="creating-your-base-image-from-scratch"></a>从头开始创建基础映像

开发人员可以从头开始创建自己的 Docker 基础映像。 不推荐刚开始使用 Docker 的开发人员使用此方案，但如果想为自己的基础映像设置特定位，则可采用此方案。

### <a name="additional-resources"></a>其他资源

- **多体系结构 .NET Core 映像**。\
  <https://github.com/dotnet/announcements/issues/14>

- **创建基础映像**。 请查看官方 Docker 文档。\
  <https://docs.docker.com/develop/develop-images/baseimages/>

![步骤 3 的映像。](./media/docker-app-development-workflow/step-3-create-dockerfile-defined-images.png)

## <a name="step-3-create-your-custom-docker-images-and-embed-your-application-or-service-in-them"></a>步骤 3。 创建自定义 Docker 映像并将应用程序或服务嵌入其中

需为应用程序中的每项服务创建一个相关映像。 如果应用程序由单个服务或 Web 应用程序组成，则只需创建一个映像。

请注意，Visual Studio 中会自动生成 Docker 映像。 以下步骤仅适用于编辑器/CLI 工作流，下文清楚解释了各步骤的工作原理。

你作为开发人员在推送完整功能或更改源代码管理系统（例如 GitHub）前，需要在本地进行开发和测试。 这意味着开发人员需要创建 Docker 映像并向本地 Docker 主机（Windows 或 Linux VM）部署容器，并运行、测试和调试这些本地容器。

若要使用 Docker CLI 和 Dockerfile 在本地环境中创建自定义映像，可使用 docker build 命令，如图 5-5 所示。

![显示 docker 生成命令的控制台输出的屏幕截图。](./media/docker-app-development-workflow/run-docker-build-command.png)

**图 5-5**。 创建自定义 Docker 映像

（可选），可先运行 `dotnet publish`，生成包含所需 .NET 库和二进制文件的可部署文件夹，然后使用 `docker build` 命令，而不是直接从项目文件夹运行 docker build。

这将创建名为 `cesardl/netcore-webapi-microservice-docker:first` 的 Docker 映像。 此处的 :first 是表示特定版本的标记。 为组合 Docker 应用程序创建自定义映像时，可为每个映像重复执行此步骤。

应用程序由多个容器组成时（即多容器应用程序），还可使用 `docker-compose up --build` 命令，借助相关 docker-compose.yml 文件中公开的元数据，只需一个命令即可生成所有相关映像。

使用 docker images 命令可查找本地存储库中的现有映像，如图 5-6 所示。

![命令 docker 映像的控制台输出，其中显示现有映像。](./media/docker-app-development-workflow/view-existing-images-with-docker-images.png)

**如 5-6**。 使用 docker images 命令查看现有映像

### <a name="creating-docker-images-with-visual-studio"></a>使用 Visual Studio 创建 Docker 映像

使用 Visual Studio 创建具有 Docker 支持的项目时，不会显示创建映像。 而是在按下 F5（或 Ctrl-F5）运行已 docker 化的应用程序或服务时创建映像   。 Visual Studio 会自动执行此操作，开发人员不会看到该过程，但务必要了解其原理。

![可选步骤 4 的映像。](./media/docker-app-development-workflow/step-4-define-services-docker-compose-yml.png)

## <a name="step-4-define-your-services-in-docker-composeyml-when-building-a-multi-container-docker-application"></a>步骤 4。 生成多容器 Docker 应用程序时，在 docker-compose.yml 中定义服务

借助 [docker-compose.yml](https://docs.docker.com/compose/compose-file/) 文件，开发人员可定义一组相关服务，通过部署命令将其部署为组合应用程序。 它还配置其依赖项关系和运行时配置。

若要使用 docker-compose.yml 文件，则需在主解决方案文件夹或根解决方案文件夹中创建该文件，其内容与以下示例中的内容类似：

```yml
version: '3.4'

services:

  webmvc:
    image: eshop/web
    environment:
      - CatalogUrl=http://catalog-api
      - OrderingUrl=http://ordering-api
    ports:
      - "80:80"
    depends_on:
      - catalog-api
      - ordering-api

  catalog-api:
    image: eshop/catalog-api
    environment:
      - ConnectionString=Server=sqldata;Port=1433;Database=CatalogDB;…
    ports:
      - "81:80"
    depends_on:
      - sqldata

  ordering-api:
    image: eshop/ordering-api
    environment:
      - ConnectionString=Server=sqldata;Database=OrderingDb;…
    ports:
      - "82:80"
    extra_hosts:
      - "CESARDLBOOKVHD:10.0.75.1"
    depends_on:
      - sqldata

  sqldata:
    image: mcr.microsoft.com/mssql/server:latest
    environment:
      - SA_PASSWORD=Pass@word
      - ACCEPT_EULA=Y
    ports:
      - "5433:1433"
```

此 docker-compose.yml 文件是简化合并版。 其中包含始终需要的每个容器的静态配置数据（如自定义映像的名称），以及视部署环境而定的配置信息（如连接字符串）。 后面的章节将介绍如何将 docker-compose.yml 配置拆分为多个 docker-compose 文件，并根据环境和执行类型（调试或发布）覆盖值。

docker-compose.yml 示例文件中定义了四项服务：`webmvc` 服务（一个 Web 应用程序）、两个微服务（`ordering-api` 和 `basket-api`）和一个数据源容器（作为容器运行的基于 SQL Server for Linux 的 `sqldata`）。 每项服务都将部署为一个容器，因此每项服务都需要一个 Docker 映像。

docker-compose.yml 文件不仅指定正在使用的容器，还指定如何单独配置各容器。 例如，.yml 文件中的 `webmvc` 容器定义：

- 使用预生成的 `eshop/web:latest` 映像。 但也可以借助基于 docker-compose 文件中 build: 部分的其他配置，在执行 docker-compose 的过程中配置要生成的映像。

- 初始化两个环境变量（CatalogUrl 和 OrderingUrl）。

- 将容器上的公开端口 80 转接到主机上的外部端口 80。

- 通过 depends_on 设置将 Web 应用链接到目录和排序服务。 此操作会让该服务处于等待状态，直到启用这些服务。

稍后介绍如何实现微服务和多容器应用时，我们会再次回顾 docker-compose.yml 文件。

### <a name="working-with-docker-composeyml-in-visual-studio-2019"></a>在 Visual Studio 2019 中使用 docker-compose.yml

除了向项目添加 Dockerfile，如前所述，Visual Studio 2017（从版本 15.8 开始）还可以向解决方案添加对 Docker Compose 的业务流程协调程序支持。

添加容器业务流程协调程序支持（如图 5-7 所示）时，Visual Studio 将首次为项目创建 Dockerfile，并在解决方案中使用几个全局 `docker-compose*.yml` 文件创建新的（服务部分）项目，然后将项目添加到这些文件。 随后可打开 docker-compose.yml 文件并对其进行更新，增加新的功能。

必须为要添加到 docker-compose.yml 文件的每个项目重复执行此操作。

在撰写此文时，Visual Studio 支持 Docker Compose 和 Kubernetes/Helm 业务流程协调程序   。

![显示项目上下文菜单中的“容器业务流程协调程序支持”选项的屏幕截图。](./media/docker-app-development-workflow/add-container-orchestrator-support-option.png)

**图 5-7**。 右键单击 ASP.NET Core 项目，在 Visual Studio 2019 中添加对 Docker 的支持

在 Visual Studio 中添加业务流程协调程序支持后，解决方案资源管理器中会出现一个新节点（位于 `docker-compose.dcproj` 项目文件中），其中包含添加的 docker-compose.yml 文件，如图 5-8 所示。

![解决方案资源管理器中 docker-compose 节点的屏幕截图。](./media/docker-app-development-workflow/docker-compose-tree-node.png)

**图 5-8**。 在 Visual Studio 2019 解决方案资源管理器中添加的 docker-compose 树节点 

可使用 `docker-compose up` 命令，通过单个 docker-compose.yml 文件部署多容器应用程序。 但 Visual Studio 按组添加，因此开发人员可根据环境（开发或生产）和执行类型（发布或调试）来覆盖值。 稍后将介绍此功能。

![步骤 5 的映像。](./media/docker-app-development-workflow/step-5-run-containers-compose-app.png)

## <a name="step-5-build-and-run-your-docker-application"></a>步骤 5。 生成并运行 Docker 应用程序

如果应用程序只有一个容器，则可通过将其部署到 Docker 主机（虚拟机或物理服务器）来运行该程序。 但如果应用程序包含多项服务，则可使用单个 CLI 命令 (`docker-compose up)`) 或使用 Visual Studio（会在其中使用该命令）将其部署为组合应用程序。 接下来介绍这两种不同的选项。

### <a name="option-a-running-a-single-container-application"></a>选项 A：运行单容器应用程序

#### <a name="using-docker-cli"></a>使用 Docker CLI

可使用 `docker run` 命令运行 Docker 容器，如图 5-9 所示：

```console
docker run -t -d -p 80:5000 cesardl/netcore-webapi-microservice-docker:first
```

上面的命令将在每次运行时从指定的映像创建新的容器实例。 可以使用 `--name` 参数为容器指定名称，然后使用 `docker start {name}`（或者使用容器 ID 或自动名称）运行现有的容器实例。

![使用 docker run 命令运行 Docker 容器的屏幕截图。](./media/docker-app-development-workflow/use-docker-run-command.png)

**图 5-9**。 使用 docker run 命令运行 Docker 容器

此时，该命令将容器的内部端口 5000 绑定到主机的端口 80。 这意味着主机在侦听端口 80，并将其转接到容器上的端口 5000。

显示的哈希为容器 ID，如果不使用 `--name` 选项，还会为它分配随机可读的名称。

#### <a name="using-visual-studio"></a>使用 Visual Studio

如果尚未添加容器业务流程协调程序支持，也可按下 Ctrl-F5 在 Visual Studio 中运行单容器应用，还可使用 F5 在容器中调试应用程序   。 使用 docker run 在本地运行容器。

### <a name="option-b-running-a-multi-container-application"></a>选项 B：运行多容器应用程序

在大多数企业方案中，Docker 应用程序由多项服务组成，这意味着需要运行多容器应用程序，如图 5-10 所示。

![具有多个 Docker 容器的 VM](./media/docker-app-development-workflow/vm-with-docker-containers-deployed.png)

**图 5-10**。 部署了 Docker 容器的 VM

#### <a name="using-docker-cli"></a>使用 Docker CLI

若要使用 Docker CLI 运行多容器应用程序，请使用 `docker-compose up` 命令。 此命令使用解决方案级别的 docker-compose.yml 文件来部署多容器应用程序  。 图 5-11 显示了从主解决方案目录（包含 docker-compose.yml 文件）运行命令的结果。

![运行 docker-compose up 命令时的屏幕视图](./media/docker-app-development-workflow/results-docker-compose-up.png)

**图 5-11**。 运行 docker-compose up 命令时的示例结果

运行 docker-compose up 命令后，应用程序及其相关容器将部署到 Docker 主机中，如图 5-10 所示。

#### <a name="using-visual-studio"></a>使用 Visual Studio

使用 Visual Studio 2019 运行多容器应用程序十分简单。 按 Ctrl-F5 可运行，按 F5 可调试，按照惯例，将 docker-compose 项目设置为启动项目    。  Visual Studio 处理所有需要的设置，所以你可以按照惯例创建断点，并使用已附加的调试程序调试最终在“远程服务器”中运行的独立进程，就是这样。

如前所述，每次向解决方案中的项目添加对 Docker 的支持时，都会在全局（解决方案级别）docker-compose.yml 文件中配置该项目，因此开发人员可以同时运行或调试整个解决方案。 Visual Studio 将为每个启用了 Docker 解决方案支持的项目启动一个容器，并代为执行所有内部步骤（发布 dotnet、生成 Docker 等）。

如果想大致看看所有步骤，请查看文件：

`{root solution folder}\obj\Docker\docker-compose.vs.debug.g.yml`

此处的重点是，在 Visual Studio 2019 中，按下 F5 键可执行另一项 Docker 命令，如图 5-12 所示  。 此选项允许开发人员在解决方案级别运行 docker-compose.yml 文件中定义的所有容器，从而运行或调试多容器应用程序。 可调试多容器解决方案就意味着，开发人员可设置多个断点，每个端点都位于不同的项目（容器）中，这样从 Visual Studio 进行调试时，会在不同项目中定义的断点处停止，并在不同容器上运行。

![调试工具栏运行 docker-compose 项目的屏幕截图。](./media/docker-app-development-workflow/debug-toolbar-docker-compose-project.png)

**图 5-12**。 在 Visual Studio 2019 中运行多容器应用

### <a name="additional-resources"></a>其他资源

- 将 ASP.NET 容器部署到远程 Docker 主机   \
  <https://docs.microsoft.com/visualstudio/containers/hosting-web-apps-in-docker>

### <a name="a-note-about-testing-and-deploying-with-orchestrators"></a>有关使用业务流程协调程序进行测试和部署的注意事项

使用 docker-compose up 和 docker run 命令（或在 Visual Studio 中运行和调试容器）足以在开发环境中测试容器。 但不应该将这种方法用于生产部署，在生产部署中应该以业务流程协调程序（例如 [Kubernetes](https://kubernetes.io/) 或 [Service Fabric](https://azure.microsoft.com/services/service-fabric/)）为目标。 如果正在使用 Kubernetes，那么必须使用 [pod](https://kubernetes.io/docs/concepts/workloads/pods/pod/) 来组织容器和[服务](https://kubernetes.io/docs/concepts/services-networking/service/)，使其互联。 还可以使用[部署](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)来组织 pod 的创建和修改。

![步骤 6 的映像。](./media/docker-app-development-workflow/step-6-test-app-microservices.png)

## <a name="step-6-test-your-docker-application-using-your-local-docker-host"></a>步骤 6。 使用本地 Docker 主机测试 Docker 应用程序

这一步骤会因应用程序的用途而有所不同。 对于部署为单个容器或服务的简单 .NET Core Web 应用程序而言，在 Docker 主机上打开浏览器并导航到该站点即可访问该服务，如图 5-13 所示。 （如果 Dockerfile 中的配置将容器映射到除主机上的 80 端口以外的任何端口，请在 URL 中包含该主机端口。）

![来自 localhost/API/values 的响应的屏幕截图。](./media/docker-app-development-workflow/test-docker-app-locally-localhost.png)

**图 5-13**。 使用 localhost 在本地测试 Docker 应用程序的示例

如果 localhost 未指向 Docker 主机 IP（默认情况下，使用 Docker CE 时应指向主机 IP），若要导航到服务，请使用计算机网卡的 IP 地址。

请注意，在此处讨论的特定容器示例中，浏览器中的此 URL 使用的是端口 80。 但该请求会在内部被重定向到端口 5000，因为 docker run 命令之前进行了此部署，如上一步中所述。

还可从终端使用 curl 测试应用程序，如图 5-14 所示。 对于 Windows 上安装的 Docker，除计算机的实际 IP 地址以外，默认的 Docker 主机 IP 始终为 10.0.75.1。

![通过 curl 获取 http://10.0.75.1/API/values 的控制台输出。](./media/docker-app-development-workflow/test-docker-app-locally-curl.png)

**图 5-14**。 使用 curl 在本地测试 Docker 应用程序的示例

### <a name="testing-and-debugging-containers-with-visual-studio-2019"></a>使用 Visual Studio 2019 测试和调试容器

使用 Visual Studio 2019 运行和调试容器时，调试 .NET 应用程序的方式与运行不带容器的应用程序的方式类似。

### <a name="testing-and-debugging-without-visual-studio"></a>不使用 Visual Studio 进行测试和调试

如果使用编辑器/CLI 方法进行开发，调试容器会更加困难，可能要通过生成跟踪来进行调试。

### <a name="additional-resources"></a>其他资源

- 在本地 Docker 容器中调试应用   \
  [https://docs.microsoft.com/visualstudio/containers/edit-and-refresh](/visualstudio/containers/edit-and-refresh)

- **Steve Lasker。Build, Debug, Deploy ASP.NET Core Apps with Docker**（使用 Docker 生成、调试、部署 ASP.NET Core 应用）。 视频。 \
  <https://channel9.msdn.com/Events/Visual-Studio/Visual-Studio-2017-Launch/T115>

## <a name="simplified-workflow-when-developing-containers-with-visual-studio"></a>使用 Visual Studio 可简化开发容器的工作流

实际上，使用 Visual Studio 进行开发的工作流比使用编辑器/CLI 方法的工作流简单得多。 Visual Studio 隐藏或简化了 Docker 需要执行的与 Dockerfile 和 docker-compose.yml 文件相关的大部分步骤，如图 5-15 所示。

:::image type="complex" source="./media/docker-app-development-workflow/simplified-life-cycle-containerized-apps-docker-cli.png" alt-text="显示了创建应用所用的五个简化步骤的关系图。":::
Docker 应用的开发流程：1 - 编写应用代码，2 - 编写 Dockerfile/s，3 - 创建在 Dockerfile/s 上定义的映像，4 -（可选）在 docker-compose.yml 文件中编写服务，5 - 运行容器或 docker-compose 应用，6 - 测试应用或微服务，7 - 推送到存储库并重复操作。
:::image-end:::

**图 5-15**。 使用 Visual Studio 可简化开发工作流

此外，只需执行一次第 2 步（向项目添加对 Docker 的支持）。 因此，该工作流与使用 .NET 进行任何其他开发时的普通开发任务的工作流类似。 开发人员需要了解其背后的工作原理（映像生成过程，使用的基础映像，容器的部署等），有时还需要编辑 Dockerfile 或 docker-compose.yml 文件以自定义行为。 但 Visual Studio 大大简化了大部分工作，提高了工作效率。

### <a name="additional-resources"></a>其他资源

- **使用 Visual Studio (2017) 进行 Steve Lasker .NET Docker 开发** \
  <https://channel9.msdn.com/Events/Visual-Studio/Visual-Studio-2017-Launch/T111>

## <a name="using-powershell-commands-in-a-dockerfile-to-set-up-windows-containers"></a>在 DockerFile 中使用 PowerShell 命令来设置 Windows 容器

[Windows 容器](/virtualization/windowscontainers/about/index)允许开发人员将现有 Windows 应用程序转换为 Docker 映像，并使用与 Docker 生态系统其余部分相同的工具进行部署。 若要使用 Windows 容器，请在 Dockerfile 中运行 PowerShell 命令，如以下示例所示：

```dockerfile
FROM mcr.microsoft.com/windows/servercore
LABEL Description="IIS" Vendor="Microsoft" Version="10"
RUN powershell -Command Add-WindowsFeature Web-Server
CMD [ "ping", "localhost", "-t" ]
```

本示例中使用的是 Windows Server Core 基础映像（FROM 设置），并使用 PowerShell 命令（RUN 设置）来安装 IIS。 同样，也可使用 PowerShell 命令来设置其他组件，如 ASP.NET 4.x、.NET Framework 4.6 或任何其他 Windows 软件。 例如，Dockerfile 中的以下命令可设置 ASP.NET 4.5：

```dockerfile
RUN powershell add-windowsfeature web-asp-net45
```

### <a name="additional-resources"></a>其他资源

- **aspnet-docker/Dockerfile。** 在 dockerfiles 中运行以包含 Windows 功能的 Powershell 命令示例。\
  <https://github.com/Microsoft/aspnet-docker/blob/master/4.7.1-windowsservercore-ltsc2016/runtime/Dockerfile>

>[!div class="step-by-step"]
>[上一页](index.md)
>[下一页](../multi-container-microservice-net-applications/index.md)
