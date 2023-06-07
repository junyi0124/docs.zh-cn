---
title: 构建作为 Linux 容器部署到 AKS/Kubernetes 群集中的 ASP.NET Core 应用程序
description: 使用 Microsoft 平台和工具的容器化 Docker 应用程序的生命周期
ms.date: 08/06/2020
ms.openlocfilehash: 831d2372131e20788d0f48190eb8c600aa02485c
ms.sourcegitcommit: 30a686fd4377fe6472aa04e215c0de711bc1c322
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2020
ms.locfileid: "94440824"
---
# <a name="build-aspnet-core-applications-deployed-as-linux-containers-into-an-akskubernetes-orchestrator"></a>构建作为 Linux 容器部署到 AKS/Kubernetes 业务流程协调程序中的 ASP.NET Core 应用程序

Azure Kubernetes 服务 (AKS) 是 Azure 的托管 Kubernetes 业务流程服务，可简化容器部署和管理。

AKS 主要功能如下：

- Azure 托管的控制面板
- 自动升级
- 自我修复
- 用户可配置的缩放
- 针对开发人员和群集操作员提供的更简单的用户体验。

以下示例探讨了如何创建在 Linux 上运行并部署到 Azure 中的 AKS 群集的 ASP.NET Core 3.1 应用程序，同时使用 Visual Studio 2019 完成开发。

## <a name="creating-the-aspnet-core-project-using-visual-studio-2019"></a>使用 Visual Studio 2019 创建 ASP.NET Core 项目

ASP.NET Core 是一个通用开发平台，由 Microsoft 和 GitHub 上的 .NET 社区共同维护。 它跨平台支持 Windows、macOS 和 Linux，并且可用于设备、云和嵌入式/IoT 方案。

此示例使用几个基于 Visual Studio Web API 模板的简单项目，因此你无需太多其他知识即可创建示例。 只需使用标准模板创建项目，该模板包含使用 ASP.NET Core 3.1 技术运行带有 REST API 和带有 Razor Pages 的 Web 应用的小项目的所有元素。

![Visual Studio 中的“添加新项目”窗口，已选择 ASP.NET Core Web 应用。](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/create-aspnet-core-application.png)

**图 4-35**。 在 Visual Studio 2019 中创建 ASP.NET Core Web 应用程序。

要在 Visual Studio 中创建示例项目，请选择“文件” > “新建” > “项目”，选择“Web”项目类型，然后选择“ASP.NET Core Web 应用程序”模板    。 你还可以根据需要搜索模板。

然后输入应用程序名称和位置，如下图所示。

![输入项目名称和位置。](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/enter-project-name-and-location.png)

图 4-36  . 在 Visual Studio 2019 中输入项目名称和位置。

验证是否已选择 ASP.NET Core 3.1 作为框架。 .NET Core 3.1 包含在 Visual Studio 2019 的最新版本中，并在安装 Visual Studio 时自动安装和配置。

![用于选择 ASP.NET Core Web 应用类型的 Visual Studio 对话框，已选择 API 选项。](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/create-web-api-application.png)

图 4-37  . 选择 ASP.NET CORE 3.1 和 Web API 项目类型

注意，现在尚未启用 Docker 支持，只是为了表明它可以在项目创建后完成。

如果有任何以前版本的 .NET Core，则可以从 <https://dotnet.microsoft.com/download> 下载并安装 3.1 版本。

若要显示你可以随时“Docker 化”项目，请立即添加 Docker 支持。 在解决方案资源管理器中右键单击项目节点，然后在上下文菜单中选择“添加” > “Docker 支持” 。

![用于向现有项目添加 Docker 支持的上下文菜单选项：右键单击（在项目上）>“添加”>“Docker 支持”。](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/add-docker-support-to-project.png)

图 4-38  . 向现有项目添加 Docker 支持

若要完成添加 Docker 支持，可以选择 Windows 或 Linux。 在本例中，选择“Linux”。

![为 Dockerfile 选择目标操作系统的选项对话框。](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/select-linux-docker-support.png)

图 4-39  . 选择 Linux 容器。

通过这些简单的步骤，你就可以在 Linux 容器上运行 ASP.NET Core 3.1 应用程序。

同样，你还可以添加一个非常简单的 WebApp 项目（图 4-40）来使用 Web API 终结点，但此处不进行详细介绍。

然后，为 WebApi 项目添加业务流程协调程序支持，如下图 4-40 所示。

![正在向 WebApi 项目添加业务流程协调程序支持](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/add-orchestrator-support.png)

图 4-40  . 正在向 WebApi 项目添加业务流程协调程序支持。

当你选择适用于本地开发的 `Docker Compose` 选项时，Visual Studio 会添加包含 docker-compose 文件的 docker-compose 项目，如图 4-41 中所示。

![Docker-compose 已添加到解决方案中](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/docker-compose-project-in-visual-studio.png)

图 4-41  . 正在向 WebApi 项目添加业务流程协调程序支持。

添加的初始文件与以下文件类似：

`docker-compose.yml`

```yml
version: "3.4"

services:
  webapi:
    image: ${DOCKER_REGISTRY-}webapi
    build:
      context: .
      dockerfile: WebApi/Dockerfile

  webapp:
    image: ${DOCKER_REGISTRY-}webapp
    build:
      context: .
      dockerfile: WebApp/Dockerfile
```

`docker-compose.override.yml`

```yml
version: "3.4"

services:
  webapi:
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=https://+:443;http://+:80
    ports:
      - "80"
      - "443"
    volumes:
      - ${APPDATA}/Microsoft/UserSecrets:/root/.microsoft/usersecrets:ro
      - ${APPDATA}/ASP.NET/Https:/root/.aspnet/https:ro
  webapp:
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=https://+:443;http://+:80
    ports:
      - "80"
      - "443"
    volumes:
      - ${APPDATA}/Microsoft/UserSecrets:/root/.microsoft/usersecrets:ro
      - ${APPDATA}/ASP.NET/Https:/root/.aspnet/https:ro
```

若要使用 Docker Compose 运行应用，你只需对 `docker-compose.override.yml` 进行一些调整即可

```yml
services:
  webapi:
    #...
    ports:
      - "51080:80"
      - "51443:443"
    #...
  webapp:
    environment:
      #...
      - WebApiBaseAddress=http://webapi
    ports:
      - "50080:80"
      - "50443:443"
    #...
```

现在，你可以通过以下三种方式运行应用程序：按 F5 键、使用“播放”按钮或按 Ctrl+F5 键，从而选择 docker-compose 项目，如图 4-42 所示  。

![使用 Visual Studio 运行 docker-compose 项目](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/running-docker-compose-with-visual-studio.png)

图 4-42  . 正在向 WebApi 项目添加业务流程协调程序支持。

按照说明运行 docker-compose 应用程序时，你将获得：

1. 根据 docker-compose 文件构建的映像和创建的容器。
2. 浏览器将在 `docker-compose` 项目的“属性”对话框中配置的地址中打开。
3. “容器”窗口打开（在 Visual Studio 2019 版本 16.4 及更高版本中）。
4. 调试器支持解决方案中的所有项目，如下图所示。

浏览器已打开：

![正在运行 Web 应用的浏览器视图](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/browser-opened.png)

图 4-43  . 具有在多个容器上运行的应用程序的浏览器窗口。

容器窗口：

![Visual Studio“容器”窗口](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/visual-studio-containers-window.png)

图 4-44  . Visual Studio“容器”窗口

通过“容器”窗口，你可以查看正在运行的容器、浏览可用的映像、查看环境变量、日志和端口映射、检查文件系统、附加调试器，或者在容器环境中打开终端窗口。

如你所见，Visual Studio 2019 与 Docker 之间的集成完全面向开发人员的工作效率。

当然，你也可以使用 `docker images` 命令列出图像。 你应看到使用 Visual Studio 2019 自动部署项目时创建的带有 `dev` 标记的 `webapi` 和 `webapp` 映像。

```console
docker images
```

![来自 Docker 映像命令的控制台输出，显示了一个包含以下内容的列表：存储库、标记、映像 ID、创建（日期）和大小。](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/docker-images-command.png)

图 4-45  . Docker 映像视图

## <a name="register-the-solution-in-an-azure-container-registry-acr"></a>在 Azure 容器注册表 (ACR) 中注册解决方案

你可以将映像上传到 [Azure 容器注册表 (ACR)](https://azure.microsoft.com/services/container-registry/)，但你也可以使用 Docker Hub 或任何其他注册表，因此可以将映像从该注册表部署到 AKS 群集。

### <a name="create-an-acr-instance"></a>创建 ACR 实例

从 az cli 运行以下命令：

```powershell
az acr create --name exploredocker --resource-group explore-docker-aks-rg --sku basic --admin-enabled
```

> [!NOTE]
> 容器注册表名称（例如 `exploredocker`）在 Azure 中必须唯一，并且必须包含 5-50 个字母数字字符。 如需了解详情，请参阅[创建容器注册表](/azure/container-registry/container-registry-get-started-azure-cli#create-a-container-registry)

### <a name="create-the-image-in-release-mode"></a>在发布模式下创建映像

现在，你将通过更改为“发布”，在“发布”模式（准备生产）中创建映像，如图 4-46 所示，并像以前一样运行应用程序 。

![VS 中的工具栏选项，用于在发布模式下生成。](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/select-release-mode.png)

图 4-46  . 选择发布模式

如果执行 `docker images` 命令，则你将看到创建的两个映像：一个用于 `debug` (dev) 模式，另一个用于 `release` (latest) 模式 。

### <a name="create-a-new-tag-for-the-image"></a>为映像创建新标记

需要使用注册表的 `loginServer` 名称标记每个容器映像。 在将容器映像推送到映像注册表时，使用此标记进行路由。

可以从 Azure 门户中查看 `loginServer` 名称，从 Azure 容器注册表中获取信息

![位于右上角的 Azure 容器注册表名称的浏览器视图。](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/loginServer-name.png)

图 4-47  . 注册表名称的视图

或者通过运行以下命令：

```console
az acr list --resource-group <resource-group-name> --query "[].{acrLoginServer:loginServer}" --output table
```

![来自上述命令的控制台输出。](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/az-cli-loginServer-name.png)

图 4-48  . 使用 az cli 获取注册表的名称

这两种情况都可获得名称。 在本示例中，为 `exploredocker.azurecr.io`。

现在，可以使用以下命令标记映像，获取最新映像（发布映像）：

```console
docker tag <image-name>:latest <login-server-name>/<image-name>:v1
```

运行 `docker tag` 命令后，使用 `docker images` 命令列出映像，你应看到带有新标记的映像。

![来自 Docker 映像命令的控制台输出。](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/tagged-docker-images-list.png)

图 4-49  . 带标记的映像视图

### <a name="push-the-image-into-the-azure-acr"></a>将映像推送到 Azure ACR

登录到 Azure 容器注册表

```console
az acr login --name exploredocker
```

使用以下命令将映像推送到 Azure ACR：

```console
docker push <login-server-name>/<image-name>:v1
```

此命令需要一段时间上传映像，但会在此过程中为你提供反馈。 在下图中，你可以看到一个映像已完成输出，而另一个映像正在输出。

![来自 Docker 推送命令的控制台输出。](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/uploading-docker-images-complete.png)

图 4-50。 来自推送命令的控制台输出。

若要将多容器应用部署到 AKS 群集中，你需要一些清单 `.yaml` 文件，这些文件的大部分属性都是从 `docker-compose.yml` 和 `docker-compose.override.yml` 文件中获取的。

#### `deploy-webapi.yml`

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webapi
  labels:
    app: weather-forecast
spec:
  replicas: 1
  selector:
    matchLabels:
      service: webapi
  template:
    metadata:
      labels:
        app: weather-forecast
        service: webapi
    spec:
      containers:
        - name: webapi
          image: exploredocker.azurecr.io/webapi:v1
          imagePullPolicy: IfNotPresent
          ports:
            - containerPort: 80
              protocol: TCP
          env:
            - name: ASPNETCORE_URLS
              value: http://+:80
---
apiVersion: v1
kind: Service
metadata:
  name: webapi
  labels:
    app: weather-forecast
    service: webapi
spec:
  ports:
    - port: 80
      targetPort: 80
      protocol: TCP
  selector:
    service: webapi
```

#### `deploy-webapp.yml`

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webapp
  labels:
    app: weather-forecast
spec:
  replicas: 1
  selector:
    matchLabels:
      service: webapp
  template:
    metadata:
      labels:
        app: weather-forecast
        service: webapp
    spec:
      containers:
        - name: webapp
          image: exploredocker.azurecr.io/webapp:v1
          imagePullPolicy: IfNotPresent
          ports:
            - containerPort: 80
              protocol: TCP
          env:
            - name: ASPNETCORE_URLS
              value: http://+:80
            - name: WebApiBaseAddress
              value: http://webapi
---
apiVersion: v1
kind: Service
metadata:
  name: webapp
  labels:
    app: weather-forecast
    service: webapp
spec:
  type: LoadBalancer
  ports:
    - port: 80
      targetPort: 80
      protocol: TCP
  selector:
    service: webapp
```

> [!NOTE]
> 以前的 `.yml` 文件仅使用 `ASPNETCORE_URLS` 参数启用 `HTTP` 端口，以避免示例应用中出现证书缺失的问题。

> [!TIP]
> 可在本指南的[部署到 Azure Kubernetes 服务 (AKS)](deploy-azure-kubernetes-service.md) 部分中了解如何为此示例创建 AKS 群集  。

现已准备好使用 kubectl 进行部署，但首先必须使用以下命令获取 AKS 群集中的凭据：

```console
az aks get-credentials --resource-group explore-docker-aks-rg --name explore-docker-aks
```

![来自上述命令的控制台输出：将“explore-docker-aks”合并为 C:\Users\Miguel.kube\config 中的当前上下文](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/getting-aks-credentials.png)

图 4-51。 从 AKS 获取凭证到 kubectl 环境。

还必须允许 AKS 群集使用以下命令从 ACR 中拉取映像：

```console
az aks update --name explore-docker-aks --resource-group explore-docker-aks-rg --attach-acr exploredocker
```

上一个命令可能需要几分钟才能完成。 然后，使用 `kubectl apply` 命令启动部署，然后使用 `kubectl get all` 列出集群对象。

```console
kubectl apply -f deploy-webapi.yml
kubectl apply -f deploy-webapp.yml

kubectl get all
```

![上述命令的控制台输出：已应用部署。 已创建服务。](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/kubectl-apply-command.png)

图 4-52。 部署到 Kubernetes

需要等待一段时间，直到负载均衡器获取外部 IP，并使用 `kubectl get services` 进行检查，然后该应用程序应该在该地址可用，如下图所示：

![部署到 AKS 的应用程序的浏览器视图](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/aks-deployed-application.png)

图 4-53。 部署到 Kubernetes

部署完成后，你可以通过 ssh 隧道使用本地代理访问 [Kubernetes Web UI](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)。

首先，你必须使用以下命令创建 ClusterRoleBinding：

```console
kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
```

然后使用以下命令启动代理：

```console
az aks browse --resource-group exploredocker-aks-rg --name explore-docker-aks
```

浏览器窗口应在 `http://127.0.0.1:8001` 处打开，其视图与下图类似：

![Kubernetes 仪表板的浏览器视图，显示了部署、Pod、副本集和服务。](media/build-aspnet-core-applications-linux-containers-aks-kubernetes/kubernetes-cluster-information.png)

图 4-54。 查看 Kubernetes 群集信息

现在，你已在 Linux 容器中运行 ASP.NET Core 应用程序，并将其部署到了 Azure 上的 AKS 群集。

> [!NOTE]
> 有关使用 Kubernetes 进行部署的更多信息，请参阅：<https://kubernetes.io/docs/reference/kubectl/cheatsheet/>

> [!div class="step-by-step"]
> [上一页](set-up-windows-containers-with-powershell.md)
> [下一页](../docker-devops-workflow/index.md)
