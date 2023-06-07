---
title: 适用于 WCF 开发人员的 Docker gRPC
description: 为 ASP.NET Core gRPC 应用程序创建 Docker 映像
ms.date: 01/06/2021
ms.openlocfilehash: f59518a28b0a1dee75c792ba03bd4af826638502
ms.sourcegitcommit: 7ef96827b161ef3fcde75f79d839885632e26ef1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/07/2021
ms.locfileid: "97970084"
---
# <a name="create-docker-images"></a>创建 Docker 映像

本部分介绍如何创建用于 ASP.NET Core gRPC 应用程序的 Docker 映像，并准备在 Docker、Kubernetes 或其他容器环境中运行。 在 GitHub 上的 [dotnet/gRPC/](https://github.com/dotnet-architecture/grpc-for-wcf-developers/tree/master/KubernetesSample) 存储库中提供了使用的示例应用程序，其中包含 ASP.NET Core MVC web 应用和 gRPC 服务。

## <a name="microsoft-base-images-for-aspnet-core-applications"></a>用于 ASP.NET Core 应用程序的 Microsoft 基础映像

Microsoft 提供了一系列用于构建和运行 .NET 应用程序的基本映像。 若要创建 ASP.NET Core 5.0 映像，请使用两个基本映像：

- 用于生成和发布应用程序的 SDK 映像。
- 用于部署的运行时映像。

| 映像 | 说明 |
| ----- | ----------- |
| [mcr.microsoft.com/dotnet/sdk](https://hub.docker.com/_/microsoft-dotnet-sdk/) | 用于生成应用程序 `docker build` 。 不能在生产中使用。 |
| [mcr.microsoft.com/dotnet/aspnet](https://hub.docker.com/_/microsoft-dotnet-aspnet/) | 包含运行时和 ASP.NET Core 依赖项。 用于生产。 |

对于每个映像，都有四个基于不同 Linux 分发的变量，由标记来区分。

| 图像标记 (s)  | Linux | 备注 |
| --------- | ----- | ----- |
| 5.0-buster、5。0 | Debian 10 | 如果未指定操作系统变体，则为默认映像。 |
| 5.0-alpine | Alpine 3.12 | Alpine 基本映像比 Debian 或 Ubuntu 映像小得多。 |
| 5.0-焦点| Ubuntu 20.04 | |

对于 Debian 和 Ubuntu 映像，Alpine 基本映像约 100 MB，比较 200 MB。 某些软件包或库可能在 Alpine 的包管理中不可用。 如果你不确定要使用哪个映像，则应选择默认 Debian。

> [!IMPORTANT]
> 请确保为生成和运行时使用相同的 Linux 变体。 在一个变体上构建和发布的应用程序可能无法在另一个上运行。

## <a name="create-a-docker-image"></a>创建 Docker 映像

Docker 映像由 *Dockerfile* 定义。 此 *Dockerfile* 是一个文本文件，其中包含生成应用程序所需的所有命令，并安装生成或运行该应用程序所需的任何依赖项。 下面的示例演示 ASP.NET Core 5.0 应用程序的最简单的 Dockerfile：

```dockerfile
FROM mcr.microsoft.com/dotnet/sdk:5.0 as build

WORKDIR /src

COPY ./StockKube.sln .
COPY ./src/StockData/StockData.csproj ./src/StockData/
COPY ./src/StockWeb/StockWeb.csproj ./src/StockWeb/

RUN dotnet restore

COPY . .

RUN dotnet publish --no-restore -c Release -o /published src/StockData/StockData.csproj

FROM mcr.microsoft.com/dotnet/aspnet:5.0 as runtime

# Uncomment the line below if running with HTTPS
# ENV ASPNETCORE_URLS=https://+:443

WORKDIR /app

COPY --from=build /published .

ENTRYPOINT [ "dotnet", "StockData.dll" ]
```

Dockerfile 包含两个部分：第一个部分使用 `sdk` 基本映像来生成和发布应用程序; 第二个部分从基中创建运行时映像 `aspnet` 。 这是因为，在运行 `sdk` 时，图像大约为 900 mb，对于运行时映像约 200 mb，其大部分内容是不必要的。

### <a name="the-build-steps"></a>生成步骤

| 步骤 | 说明 |
| ---- | ----------- |
| `FROM ...` | 声明基映像并分配 `builder` 别名。 |
| `WORKDIR /src` | 创建 `/src` 目录并将其设置为当前工作目录。 |
| `COPY . .` | 将主机上当前目录下的所有内容复制到映像上的当前目录。 |
| `RUN dotnet restore` | 还原 (ASP.NET Core 3.0 framework 的所有外部包都已随 SDK) 预先安装。 |
| `RUN dotnet publish ...` | 生成并发布发布版本。 请注意， `--runtime` 标志不是必需的。 |

### <a name="the-runtime-image-steps"></a>运行时映像步骤

| 步骤 | 说明 |
| ---- | ----------- |
| `FROM ...` | 声明新的基本映像。 |
| `WORKDIR /app` | 创建 `/app` 目录并将其设置为当前工作目录。 |
| `COPY --from=builder ...` | 使用第一行中的别名从上一个映像复制已发布的应用程序 `builder` `FROM` 。 |
| `ENTRYPOINT [ ... ]` | 设置要在容器启动时运行的命令。 `dotnet`运行时映像中的命令只能运行 DLL 文件。 |

### <a name="https-in-docker"></a>Docker 中的 HTTPS

用于 Docker 的 Microsoft 基本映像将 `ASPNETCORE_URLS` 环境变量设置为 `http://+:80` ，这意味着在该端口上运行的 Kestrel 不带 HTTPS。 如果对自定义证书使用 HTTPS (如) [自承载 gRPC 应用程序](self-hosted.md) 中所述），则应重写此配置。 在 Dockerfile 的运行时映像创建部分设置环境变量。

```dockerfile
# Runtime image creation
FROM mcr.microsoft.com/dotnet/aspnet:5.0

ENV ASPNETCORE_URLS=https://+:443
```

### <a name="the-dockerignore-file"></a>.Dockerignore 文件

与 `.gitignore` 从源代码管理中排除某些文件和目录的文件非常相似， `.dockerignore` 文件可用于在生成过程中排除文件和目录被复制到映像。 此文件不仅节省时间复制，还可以避免在将 `obj` 目录从 PC 复制到映像时出现的一些错误。 你至少应在文件中添加和的 `bin` 条目 `obj` `.dockerignore` 。

```console
bin/
obj/
```

## <a name="build-the-image"></a>生成映像

对于 `StockKube.sln` 包含两个不同应用程序的解决方案 `StockData` `StockWeb` ，最简单的方法是将每个应用程序的 Dockerfile 放在基目录中。 在这种情况下，若要生成映像，请使用 `docker build` 文件所在的同一目录中的以下命令 `.sln` 。

```console
docker build -t stockdata:1.0.0 -f ./src/StockData/Dockerfile .
```

令困惑命名 `--tag` 标志 (可以缩短为 `-t`) 指定图像的整个名称，包括实际标记（如果指定）。 `.`结束时指定将在其中运行生成的上下文; `COPY` Dockerfile 中命令的当前工作目录。

如果一个解决方案中有多个应用程序，则可以将每个应用程序的 Dockerfile 保存在其自己的文件夹中的 `.csproj` 文件旁边。 你仍应该 `docker build` 从基本目录运行命令，以确保将解决方案和所有项目都复制到映像中。 可以通过使用 `--file` (或) 标志，在当前目录下指定 Dockerfile `-f` 。

```console
docker build -t stockdata:1.0.0 -f ./src/StockData/Dockerfile .
```

## <a name="run-the-image-in-a-container-on-your-machine"></a>在计算机上的容器中运行映像

若要在本地 Docker 实例中运行映像，请使用 `docker run` 命令。

```console
docker run -ti -p 5000:80 stockdata:1.0.0
```

`-ti`标志将当前终端连接到容器的终端，并在交互模式下运行。 `-p 5000:80`发布 (将容器上的端口 80) 链接到 localhost 网络接口上的端口5000。

## <a name="push-the-image-to-a-registry"></a>将映像推送到注册表

验证映像工作后，将其推送到 Docker 注册表，使其在其他系统上可用。 内部网络将需要预配 Docker 注册表。 此活动非常简单，只需运行 [docker 自己的 `registry` 映像](https://docs.docker.com/registry/deploying/) (docker 注册表即可在 docker 容器中运行) ，但可以使用各种更为全面的解决方案。 对于外部共享和云使用，有多种可用的托管注册表，如 [Azure 容器注册表](/azure/container-registry/) 或 [Docker Hub](https://docs.docker.com/docker-hub/repos/)。

若要推送到 Docker Hub，请将映像名称作为你的用户或组织名称作为前缀。

```console
docker tag stockdata:1.0.0 <myorg>/stockdata:1.0.0
docker push <myorg>/stockdata:1.0.0
```

若要推送到专用注册表，请使用注册表主机名和组织名称作为映像名称的前缀。

```console
docker tag stockdata <internal-registry:5000>/<myorg>/stockdata:1.0.0
docker push <internal-registry:5000>/<myorg>/stockdata:1.0.0
```

映像位于注册表中后，可以将其部署到各个 Docker 主机或容器业务流程引擎（如 Kubernetes）。

>[!div class="step-by-step"]
>[上一页](self-hosted.md)
>[下一页](kubernetes.md)
