---
title: 利用容器和协调器
description: 利用 Azure 中的 Docker 容器和 Kubernetes 协调器
ms.date: 05/31/2020
ms.openlocfilehash: 0ca69b71aa7d414a7bc55253b123020d49468dee
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95672508"
---
# <a name="leveraging-containers-and-orchestrators"></a>利用容器和协调器

容器和协调器旨在解决单一部署方法常见的问题。

## <a name="challenges-with-monolithic-deployments"></a>单一部署难题

传统上，大多数应用程序都部署为单个单元。 此类应用程序称为单体架构。 这种将应用程序作为单一单元进行部署的通用方法，即使它们由多个模块或程序集组成，也称为单片体系结构，如图3-1 所示。

![整体体系结构。](./media/monolithic-design.png)

**图 3-1**。 整体体系结构。

尽管它们具有简易性的优点，但单一体系结构面临着许多挑战：

### <a name="deployment"></a>部署

此外，他们还需要重新启动应用程序，如果在部署时没有应用零停机方法，这可能会暂时影响可用性。

### <a name="scaling"></a>扩展

整体应用程序完全承载于单个计算机实例上，通常需要高容量的硬件。 如果单体架构的任何部分需要缩放，则必须将整个应用程序的另一个副本部署到另一台计算机。 使用单体架构时，不能单独缩放应用程序组件-它是全部或无。 缩放不需要缩放的组件会导致资源利用率低下且成本高昂。

### <a name="environment"></a>环境

整体应用程序通常部署到具有预安装的操作系统、运行时和库依赖项的宿主环境。 此环境可能与开发或测试应用程序的环境不匹配。 应用程序环境之间的不一致性是单一部署问题的常见根源。

### <a name="coupling"></a>耦合

单一应用程序可能会在其功能组件间经历较高的耦合。 如果没有硬边界，系统更改通常会产生意外的、代价高昂的副作用。 新功能/修补变得更加棘手、耗时且成本高昂。 更新需要大量测试。 耦合还使得重构组件或在替代实现中交换很困难。 即使是在构建时考虑到了严格的关注点，体系结构 erosion 也会在中设置为具有永不结束 "特殊事例" 的单一基本代码。

### <a name="platform-lock-in"></a>平台锁定

单一应用程序是使用单个技术堆栈构造的。 提供一致性时，这种承诺可能会成为创新的障碍。 新功能和组件将使用应用程序的当前堆栈生成-即使更多新式技术可能是更好的选择。 更长期的风险是技术堆栈过时和过时。 将整个应用程序重新架构到新的、更新式的平台，这一工作既昂贵又有风险。

## <a name="what-are-the-benefits-of-containers-and-orchestrators"></a>容器和协调器的好处是什么？

第1章介绍了容器。 我们重点介绍了云本机计算基础 (CNCF 如何) 排名容器化，这是其 [云本机线索映射](https://raw.githubusercontent.com/cncf/trailmap/master/CNCF_TrailMap_latest.png) 指南的第一步，企业始于云原生旅程。 在本部分中，我们将介绍容器的优点。

Docker 是最常用的容器管理平台。 它可与 Linux 或 Windows 上的容器一起使用。 容器提供独立但可重复的应用程序环境，这些环境在任何系统上以相同的方式运行。 这一方面使它们完美地用于开发和托管云本机服务。 容器彼此隔离。 同一主机硬件上的两个容器可以具有不同版本的软件，而不会导致冲突。

容器由简单的基于文本的文件定义，这些文件将变为项目项目并签入到源代码管理中。 尽管完全服务器和虚拟机需要手动操作才能进行更新，但容器非常易于控制版本。 可使用自动工具作为生成管道的一部分，开发、测试和部署在容器中运行的应用。

容器是不可变的。 定义容器后，可以完全相同的方式重新创建和运行。 这种不可变性适用于基于组件的设计。 如果应用程序的某些部分的演化不同于其他部分，那么当你只是部署最常发生变化的部件时，为什么会重新部署整个应用？ 应用的不同功能和交叉切削问题可以分解为单独的单位。 图3-2 显示了单一应用程序如何通过委托某些功能来利用容器和微服务。 应用程序本身中的其余功能也已容器化。

![分解单一应用程序以在后端使用微服务。](./media/cloud-native-design.png)

**图 3-2**。 分解应用以接纳微服务。

每个云本机服务都在单独的容器中生成和部署。 每个都可以根据需要进行更新。 可以在节点上托管单个服务，每个服务都有相应的资源。 每个服务的运行环境是不可变的，在开发、测试和生产环境之间共享，并且易于版本化。 应用程序的不同区域之间的耦合与服务间的调用或消息（而不是单体架构中的编译时依赖关系）显式发生。 你还可以选择最能充分利用给定功能的技术，无需更改应用程序的其余部分。

容器化服务需要自动管理。 手动管理一大组独立部署的容器是不可行的。 例如，请考虑以下任务：

- 如何在多台计算机的群集中预配容器实例？
- 部署完成后，容器的发现和通信方式如何呢？
- 容器如何按需进行缩放？
- 如何监视每个容器的运行状况？
- 如何针对硬件和软件故障保护容器？
- 如何将实时应用程序的容器升级到零停机时间？

容器协调器解决这些问题和其他问题。

在云中，Kubernetes 已成为事实容器 orchestrator。 它是由云本机计算基础 (CNCF) 管理的开源平台。 Kubernetes 自动化了计算机群集上容器化工作负荷的部署、缩放和操作问题。 但是，安装和管理 Kubernetes 非常复杂。

更好的方法是使用 Kubernetes 作为云供应商提供的托管服务。 Azure 云的功能是一个完全托管的 Kubernetes 平台，该平台名为 [Azure Kubernetes Service (AKS) ](https://azure.microsoft.com/services/kubernetes-service/)。 AKS 抽象了管理 Kubernetes 的复杂性和操作开销。 使用 Kubernetes 作为云服务;Microsoft 负责管理和支持它。 AKS 还与其他 Azure 服务和开发工具紧密集成。

AKS 是一种基于群集的技术。 联合虚拟机或节点的池部署到 Azure 云。 它们共同构成了一个高度可用的环境或群集。 群集将作为无缝的单一实体出现到你的云本机应用程序。 在这种情况下，AKS 会按照均匀分布负载的预定义策略，在这些节点上部署容器化服务。

## <a name="what-are-the-scaling-benefits"></a>缩放的优点是什么？

基于容器构建的服务可以利用 Kubernetes 等业务流程工具提供的缩放权益。 设计容器仅知道自己的情况。 如果有多个需要协同工作的容器，则应将其组织在更高的级别。 组织大量的容器及其共享依赖项（如网络配置）是业务流程工具在其中节省时间的地方！ Kubernetes 在一组容器上创建一个抽象层，然后将它们 *组织到 pod*。 Pod 在称为 *节点* 的辅助角色计算机上运行。 此组织结构称为 *群集*。 图3-3 显示了 Kubernetes 群集的不同组件。

![Kubernetes 群集组件。 ](./media/kubernetes-cluster-components.png)
**图 3-3**。 Kubernetes 群集组件。

缩放容器化工作负荷是容器协调器的一项重要功能。 AKS 支持跨两个维度的自动缩放：容器实例和计算节点。 它们共同使 AKS 能够快速有效地响应需求高峰并增加额外资源。 本章稍后将讨论 AKS 中的扩展。

### <a name="declarative-versus-imperative"></a>声明性与命令式

Kubernetes 支持声明性和命令性配置。 命令式方法涉及运行各种命令，这些命令告诉 Kubernetes 如何执行每个步骤。 运行此映像。 删除此 pod。 公开此端口。 使用声明性方法，你可以创建一个名为清单的配置文件，以描述你需要的内容，而不是要执行的操作。 Kubernetes 读取清单并将所需的结束状态转换为实际结束状态。

命令性命令非常适用于学习和交互式试验。 但是，你将需要以声明方式创建 Kubernetes 清单文件，以将基础结构作为代码方法，从而提供可靠且可重复的部署。 清单文件将成为项目项目，并在 CI/CD 管道中用于自动执行 Kubernetes 部署。

如果已使用命令性命令配置了群集，则可以使用导出声明性清单 `kubectl get svc SERVICENAME -o yaml > service.yaml` 。 此命令生成类似于下面所示的清单：

```yaml
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: "2019-09-13T13:58:47Z"
  labels:
    component: apiserver
    provider: kubernetes
  name: kubernetes
  namespace: default
  resourceVersion: "153"
  selfLink: /api/v1/namespaces/default/services/kubernetes
  uid: 9b1fac62-d62e-11e9-8968-00155d38010d
spec:
  clusterIP: 10.96.0.1
  ports:
  - name: https
    port: 443
    protocol: TCP
    targetPort: 6443
  sessionAffinity: None
  type: ClusterIP
status:
  loadBalancer: {}
```

使用声明性配置时，你可以在使用 `kubectl diff -f FOLDERNAME` 配置文件所在的文件夹上使用来预览将进行的更改。 确定要应用所做的更改后，运行 `kubectl apply -f FOLDERNAME` 。 添加 `-R` 以递归方式处理文件夹层次结构。

你还可以将声明性配置用于其他 Kubernetes 功能，其中一项是部署。 声明性部署可帮助管理版本、更新和缩放。 它们指示 Kubernetes 部署控制器如何部署新更改、扩大负载或回滚到以前的修订版本。 如果群集不稳定，则声明性部署会自动将群集恢复到所需状态。 例如，如果某个节点出现故障，部署机制将重新部署替代以实现所需状态

使用声明性配置，可将基础结构表示为可签入并在应用程序代码中进行版本控制的代码。 它通过生成和部署管道为连续部署提供改进的更改控制和更好的支持。

## <a name="what-scenarios-are-ideal-for-containers-and-orchestrators"></a>哪些方案适用于容器和协调器？

以下方案非常适合使用容器和协调器。

### <a name="applications-requiring-high-uptime-and-scalability"></a>需要高运行时间和可伸缩性的应用程序

具有高运行时间和伸缩性要求的单个应用程序是使用微服务、容器和协调器的云本机体系结构的理想选择。 它们可在容器中进行开发，在版本控制环境中进行测试，并部署到生产环境中，但停机时间为零。 使用 Kubernetes 群集可确保这样的应用程序也可以按需缩放，并从节点故障中自动恢复。

### <a name="large-numbers-of-applications"></a>大量应用程序

部署和维护大量应用程序的组织从容器和协调器中获益。 设置容器化环境和 Kubernetes 群集的前期工作主要是一项固定成本。 部署、维护和更新单个应用程序的成本会因应用程序数量而异。 除了少量的应用程序，手动维护自定义应用程序的复杂性超出了使用容器和协调器实现解决方案的成本。

## <a name="when-should-you-avoid-using-containers-and-orchestrators"></a>何时应避免使用容器和协调器？

如果无法按照 Twelve-Factor 应用原则构建应用程序，应考虑避免容器和协调器。 在这些情况下，请考虑基于 VM 的托管平台，或可能是某个混合系统。 利用它，你始终可以在单独的容器中或甚至无服务器的功能中关闭某些功能。

## <a name="development-resources"></a>开发资源

本部分显示了可帮助你开始使用容器和协调器作为下一个应用程序的开发资源的简短列表。 如果正在寻找有关如何设计云本机微服务体系结构应用的指南，请阅读此书籍的随附 .net [微服务：容器化 .Net 应用程序的体系结构](https://dotnet.microsoft.com/download/thank-you/microservices-architecture-ebook)。

### <a name="local-kubernetes-development"></a>本地 Kubernetes 开发

Kubernetes 部署在生产环境中提供了极大的价值，但也可以在开发计算机上本地运行。 尽管你可能会独立地处理单独的微服务，但有时你可能需要在本地运行整个系统，就像部署到生产环境时它会运行一样。 可以通过以下几种工具来帮助： Minikube 和 Docker 桌面。 Visual Studio 还提供了用于 Docker 开发的工具。

### <a name="minikube"></a>Minikube

什么是 Minikube？ Minikube 项目显示 "Minikube 在 macOS、Linux 和 Windows 上实现本地 Kubernetes 群集"。 它的主要目标是 "成为本地 Kubernetes 应用程序开发的最佳工具，并支持所有适合的 Kubernetes 功能"。 安装 Minikube 与 Docker 分离，但 Minikube 支持不同于 Docker Desktop 支持的虚拟机监控程序。 Minikube 目前支持以下 Kubernetes 功能：

- DNS
- NodePorts
- ConfigMaps 和机密
- 仪表板
- 容器运行时： Docker、rkt、CRI 和 containerd
- 启用 (CNI 的容器网络接口) 
- 流入量

安装 Minikube 后，可以通过运行命令快速开始使用它 `minikube start` ，这会下载映像并启动本地 Kubernetes 群集。 启动群集后，你可以使用标准 Kubernetes 命令与它交互 `kubectl` 。

### <a name="docker-desktop"></a>Docker Desktop

你还可以直接从 Windows 上的 Docker Desktop 使用 Kubernetes。 如果你使用的是 Windows 容器，则这是唯一的选择，对于非 Windows 容器也是不错的选择。 图3-4 显示了如何在运行 Docker Desktop 时启用本地 Kubernetes 支持。

![在 Docker Desktop 中配置 Kubernetes](./media/docker-desktop-kubernetes.png)

**图 3-4**。 在 Docker Desktop 中配置 Kubernetes。

Docker Desktop 是在本地配置和运行容器化应用程序的最常用工具。 使用 Docker Desktop 时，可以针对将部署到生产环境中的一组 Docker 容器映像进行本地开发。 Docker Desktop 设计为在本地生成、测试和交付容器化应用。 它支持 Linux 和 Windows 容器。 将映像推送到映像注册表（如 Azure 容器注册表或 Docker 中心）后，AKS 可将其提取并部署到生产环境。

### <a name="visual-studio-docker-tooling"></a>Visual Studio Docker 工具

Visual Studio 支持基于 web 的应用程序的 Docker 开发。 创建新的 ASP.NET Core 应用程序时，可以选择使用 Docker 支持来配置该应用程序，如图3-5 所示。

![Visual Studio 启用 Docker 支持](./media/visual-studio-enable-docker-support.png)

**图 3-5**。 Visual Studio 启用 Docker 支持

如果选择此选项，则会 `Dockerfile` 在其根中创建项目，该项目可用于在 Docker 容器中生成和托管应用。 图 3-6 中显示了一个示例 Dockerfile

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:3.1-buster AS build
WORKDIR /src
COPY ["eShopWeb/eShopWeb.csproj", "eShopWeb/"]
RUN dotnet restore "eShopWeb/eShopWeb.csproj"
COPY . .
WORKDIR "/src/eShopWeb"
RUN dotnet build "eShopWeb.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "eShopWeb.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "eShopWeb.dll"]
```

**图 3-6**。 Visual Studio 生成的 Dockerfile

应用运行时的默认行为也配置为使用 Docker。 图3-7 显示了在添加 Docker 支持创建的新 ASP.NET Core 项目中可用的不同运行选项。

![Visual Studio Docker 运行选项](./media/visual-studio-docker-run-options.png)

**图 3-7**。 Visual Studio Docker 运行选项

除了本地开发外， [Azure Dev Spaces](/azure/dev-spaces/) 为多个开发人员提供了一种方便的方法，以便在 Azure 中使用自己的 Kubernetes 配置。 如图3-7 所示，还可以在 Azure Dev Spaces 中运行该应用程序。

此外，随时可以向现有 ASP.NET Core 应用程序添加 Docker 支持。 在 Visual Studio 解决方案资源管理器中，右键单击项目并选择 "**添加**  >  **Docker 支持**"，如图3-8 所示。

![Visual Studio 添加 Docker 支持](./media/visual-studio-add-docker-support.png)

**图 3-8**。 向 Visual Studio 添加 Docker 支持

还可以添加容器业务流程支持，如图3-8 所示。 默认情况下，orchestrator 将使用 Kubernetes 和 Helm。 选择 orchestrator 后，会将一个 `azds.yaml` 文件添加到项目根，并 `charts` 添加一个文件夹，其中包含用于配置应用程序并将其部署到 Kubernetes 的 Helm 图表。 图3-9 显示新项目中生成的文件。

![Visual Studio 添加 Orchestrator 支持](./media/visual-studio-add-orchestrator-support.png)

**图 3-9**。 向 Visual Studio 添加业务流程支持

### <a name="visual-studio-code-docker-tooling"></a>Visual Studio Code Docker 工具

有许多可用于支持 Docker 开发的 Visual Studio Code 扩展。

Microsoft [为 Visual Studio Code 扩展提供 Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)。 此扩展简化了向应用程序添加容器支持的过程。 它基架需要的文件，生成 Docker 映像，并使你能够在容器中调试应用程序。 扩展功能是一个可视化资源管理器，可让你轻松地对容器和映像执行操作，如启动、停止、检查、删除等。 扩展还支持 Docker Compose 使你能够将多个正在运行的容器作为一个单元进行管理。

>[!div class="step-by-step"]
>[上一页](scale-applications.md)
>[下一页](leverage-serverless-functions.md)
