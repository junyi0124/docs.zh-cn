---
title: Kubernetes-WCF 开发人员 gRPC
description: 在 Kubernetes 群集中运行 ASP.NET Core gRPC 服务。
ms.date: 12/15/2020
ms.openlocfilehash: 0b457d6fa982b5f5b983194d4aedbff0eb782f36
ms.sourcegitcommit: 655f8a16c488567dfa696fc0b293b34d3c81e3df
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/06/2021
ms.locfileid: "97938049"
---
# <a name="kubernetes"></a>Kubernetes

尽管可以在 Docker 主机上手动运行容器，但对于可靠的生产系统，更好的做法是使用容器业务流程引擎来管理群集中多个服务器上运行的多个实例。 提供各种容器业务流程引擎，包括 Kubernetes、Docker Swarm 和 Apache Mesos。 但在这些引擎中，Kubernetes 的使用幅度远远超出最大，因此，本章将重点介绍这一点。

Kubernetes 包括以下功能：

- **计划** 在群集内的多个节点上运行容器，确保可用资源的平衡使用，在存在中断的情况下使容器保持运行，并处理对新版本的映像或新配置的滚动更新。
- **运行状况检查** 监视容器，以确保继续服务。
- **DNS & 服务发现** 处理群集中服务之间的路由。
- **入口** 公开选定的服务，通常在这些服务的实例之间提供负载平衡。
- **资源管理** 将存储等外部资源附加到容器。

本章详细介绍如何将 ASP.NET Core gRPC 服务和使用该服务的网站部署到 Kubernetes 群集中。 使用的示例应用程序可在 GitHub 上的 [dotnet/grpc/for wcf 开发人员](https://github.com/dotnet-architecture/grpc-for-wcf-developers/tree/master/KubernetesSample) 存储库中找到。

## <a name="kubernetes-terminology"></a>Kubernetes 术语

Kubernetes 使用 *desired state configuration*： API 用于描述 *pod、**部署* 和 *服务* 等对象，而 *控制平面* 负责实现 *群集* 中所有 *节点* 上的所需状态。 Kubernetes 群集具有运行 *KUBERNETES API* 的 *主* 节点，可以通过编程方式或使用 `kubectl` 命令行工具进行通信。 `kubectl` 可以通过命令行参数创建和管理对象，但它最适合包含 Kubernetes 对象的声明数据的 YAML 文件。

### <a name="kubernetes-yaml-files"></a>Kubernetes YAML 文件

每个 Kubernetes YAML 文件将至少有三个顶级属性：

```yaml
apiVersion: v1
kind: Namespace
metadata:
  # Object properties
```

`apiVersion`属性用于指定 (哪个版本以及该文件的目标) 。 `kind`属性指定 YAML 表示的对象的类型。 `metadata`属性包含对象属性，如 `name` 、 `namespace` 和 `labels` 。

大多数 Kubernetes YAML 文件还将有一个 `spec` 描述创建对象所需的资源和配置的部分。

### <a name="pods"></a>Pod

Pod 是 Kubernetes 中的基本执行单位。 它们可以运行多个容器，但也可用于运行单个容器。 Pod 还包括容器所需的所有存储资源以及网络 IP 地址。

### <a name="services"></a>服务

服务是元对象，用于描述 pod (或 pod) 集，并提供一种在群集内访问这些对象的方式，如使用群集 DNS 服务将服务名称映射到一组 pod IP 地址。

### <a name="deployments"></a>部署

部署是 pod 的 *所需状态* 对象。 如果手动创建 pod，则它将在终止时重新启动。 部署用于告知群集哪些 pod 和这些 pod 的副本数在当前时间运行。

### <a name="other-objects"></a>其他对象

箱、服务和部署只是三个最基本的对象类型。 Kubernetes 群集管理了许多其他对象类型。 有关详细信息，请参阅 [Kubernetes 概念](https://kubernetes.io/docs/concepts/) 文档。

### <a name="namespaces"></a>命名空间

Kubernetes 群集旨在扩展到数百或数千个节点并运行相似的服务数量。 若要避免对象名称之间的冲突，请使用命名空间将对象组合成较大的应用程序的一部分。 Kubernetes 自己的服务在 `default` 命名空间中运行。 所有用户对象都应在其自己的命名空间中创建，以避免与群集中的默认对象或其他租户发生冲突。

## <a name="get-started-with-kubernetes"></a>Kubernetes 入门

如果你运行的是适用于 Windows 的 Docker Desktop 或适用于 Mac 的 Docker Desktop，则 Kubernetes 已可用。 只需在 "**设置**" 窗口的 " **Kubernetes** " 部分中将其启用：

![在 Docker Desktop 中启用 Kubernetes](media/kubernetes/enable-kubernetes-docker-desktop-v2.png)

若要在 Linux 上运行本地 Kubernetes 群集，请考虑使用 [minikube](https://github.com/kubernetes/minikube)或 [MicroK8s](https://microk8s.io/) （如果 linux 分发支持 [snap](https://snapcraft.io/)）。

若要确认群集是否正在运行且可访问，请运行 `kubectl version` 以下命令：

```console
kubectl version
Client Version: version.Info{Major:"1", Minor:"19", GitVersion:"v1.19.3", GitCommit:"1e11e4a2108024935ecfcb2912226cedeafd99df", GitTreeState:"clean", BuildDate:"2020-10-14T12:50:19Z", GoVersion:"go1.15.2", Compiler:"gc", Platform:"windows/amd64"}
Server Version: version.Info{Major:"1", Minor:"19", GitVersion:"v1.19.3", GitCommit:"1e11e4a2108024935ecfcb2912226cedeafd99df", GitTreeState:"clean", BuildDate:"2020-10-14T12:41:49Z", GoVersion:"go1.15.2", Compiler:"gc", Platform:"linux/amd64"}
```

在此示例中， `kubectl` CLI 和 Kubernetes 服务器都运行的是版本1.14.6。 的每个版本 `kubectl` 都应支持服务器的上一个和下一个版本，因此 `kubectl` 1.14 也适用于服务器版本1.13 和1.15。

## <a name="run-services-on-kubernetes"></a>在 Kubernetes 上运行服务

该示例应用程序的 `kube` 目录包含三个 YAML 文件。 该 `namespace.yml` 文件声明自定义命名空间： `stocks` 。 该 `stockdata.yml` 文件声明 gRPC 应用程序的部署和服务， `stockweb.yml` 文件声明使用 gRPC 服务的 ASP.NET CORE 5.0 MVC web 应用程序的部署和服务。

若要将 `YAML` 文件与配合使用 `kubectl` ，请运行 `apply -f` 以下命令：

```console
kubectl apply -f object.yml
```

`apply`此命令将检查 YAML 文件的有效性并显示从 API 收到的任何错误，但不会等待，直到创建了该文件中声明的所有对象，因为此步骤可能需要一些时间。 使用 `kubectl get` 带有相关对象类型的命令检查群集中的对象创建。

### <a name="the-namespace-declaration"></a>命名空间声明

命名空间声明非常简单，只需分配 `name` ：

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: stocks
```

使用 `kubectl` 应用 `namespace.yml` 文件，并确认已成功创建命名空间：

```console
> kubectl apply -f namespace.yml
namespace/stocks created

> kubectl get namespaces
NAME              STATUS   AGE
stocks            Active   2m53s
```

### <a name="the-stockdata-application"></a>StockData 应用程序

该 `stockdata.yml` 文件声明了两个对象：一个部署和一个服务。

#### <a name="the-stockdata-deployment"></a>StockData 部署

YAML 文件的部署部分 `spec` 为部署本身提供，其中包括所需的副本数，以及 `template` 部署要创建和管理的 Pod 对象的。 请注意，部署对象由 API 管理 `apps` ，如中所指定 `apiVersion` ，而不是主 Kubernetes API。

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: stockdata
  namespace: stocks
spec:
  selector:
    matchLabels:
      run: stockdata
  replicas: 1
  template:
    metadata:
      labels:
        run: stockdata
    spec:
      containers:
      - name: stockdata
        image: stockdata:1.0.0
        imagePullPolicy: Never
        resources:
          limits:
            cpu: 100m
            memory: 100Mi
        ports:
        - containerPort: 80
```

`spec.selector`属性用于将正在运行的 pod 与部署相匹配。 Pod 的 `metadata.labels` 属性必须与属性匹配， `matchLabels` 否则 API 调用将失败。

`template.spec`部分声明要运行的容器。 当你使用本地 Kubernetes 群集（如 Docker Desktop 提供的群集）时，可以指定在本地生成的映像，只要它们具有版本标记即可。

> [!IMPORTANT]
> 默认情况下，Kubernetes 将始终检查并尝试请求新映像。 如果在其任何已知存储库中都找不到该映像，则创建 Pod 会失败。 若要使用本地映像，请将设置 `imagePullPolicy` 为 `Never` 。

`ports`属性指定应在 Pod 上发布的容器端口。 `stockservice`映像在标准 HTTP 端口上运行服务，因此端口80已发布。

`resources`部分将资源限制应用于在 Pod 中运行的容器。 这是一种很好的做法，因为它可以防止单个 Pod 消耗节点上的所有可用 CPU 或内存。

> [!NOTE]
> ASP.NET Core 5.0 经过优化和优化，可在资源受限的容器中运行。 `dotnet/core/aspnet`Docker 映像设置环境变量，以告知 `dotnet` 运行时它在容器中。

#### <a name="the-stockdata-service"></a>StockData 服务

YAML 文件的服务部分声明了向群集中的 pod 提供访问权限的服务。

```yaml
apiVersion: v1
kind: Service
metadata:
  name: stockdata
  namespace: stocks
spec:
  ports:
  - port: 80
  selector:
    run: stockdata
```

`spec` `selector` `Pods` 在这种情况下，服务使用属性来匹配运行，在此示例中，查找具有标签的 pod `run: stockdata` 。 匹配的 pod 上指定的已 `port` 命名的服务发布。 命名空间中运行的其他盒 `stocks` 可以使用作为地址访问此服务上的 HTTP `http://stockdata` 。 在其他命名空间中运行的 pod 可以使用 `http://stockdata.stocks` 主机名。 您可以通过使用 [网络策略](https://kubernetes.io/docs/concepts/services-networking/network-policies/)来控制跨命名空间的服务访问。

#### <a name="deploy-the-stockdata-application"></a>部署 StockData 应用程序

使用 `kubectl` 应用 `stockdata.yml` 文件，并确认已创建部署和服务：

```console
> kubectl apply -f .\stockdata.yml
deployment.apps/stockdata created
service/stockdata created

> kubectl get deployment stockdata --namespace stocks
NAME        READY   UP-TO-DATE   AVAILABLE   AGE
stockdata   1/1     1            1           17s

> kubectl get service stockdata --namespace stocks
NAME        TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
stockdata   ClusterIP   10.97.132.103   <none>        80/TCP    33s
```

### <a name="the-stockweb-application"></a>StockWeb 应用程序

该 `stockweb.yml` 文件声明 MVC 应用程序的部署和服务。

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: stockweb
  namespace: stocks
spec:
  selector:
    matchLabels:
      run: stockweb
  replicas: 1
  template:
    metadata:
      labels:
        run: stockweb
    spec:
      containers:
      - name: stockweb
        image: stockweb:1.0.0
        imagePullPolicy: Never
        resources:
          limits:
            cpu: 100m
            memory: 100Mi
        ports:
        - containerPort: 80
        env:
        - name: StockData__Address
          value: "http://stockdata"
        - name: DOTNET_SYSTEM_NET_HTTP_SOCKETSHTTPHANDLER_HTTP2UNENCRYPTEDSUPPORT
          value: "true"

---

apiVersion: v1
kind: Service
metadata:
  name: stockweb
  namespace: stocks
spec:
  type: NodePort
  ports:
  - port: 80
  selector:
    run: stockweb
```

#### <a name="environment-variables"></a>环境变量

`env`部署对象的部分指定要在运行映像的容器中设置的环境变量 `stockweb:1.0.0` 。

此 **`StockData__Address`** 环境变量将映射到 `StockData:Address` 配置设置，感谢 EnvironmentVariables 配置提供程序。 此设置在名称之间使用双下划线分隔部分。 该地址使用 `stockdata` 在同一 Kubernetes 命名空间中运行的服务的服务名称。

**`DOTNET_SYSTEM_NET_HTTP_SOCKETSHTTPHANDLER_HTTP2UNENCRYPTEDSUPPORT`** 环境变量设置一个 <xref:System.AppContext> 开关，该开关可为启用未加密的 HTTP/2 连接 <xref:System.Net.Http.HttpClient> 。 此环境变量的作用与在代码中设置开关相同，如下所示：

```csharp
AppContext.SetSwitch("System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);
```

如果使用交换机的环境变量，则可以轻松地根据运行应用程序的上下文来更改上下文。

#### <a name="service-types"></a>服务类型

`type: NodePort`属性用于使 web 应用程序可以从群集外部访问。 此属性类型会导致 Kubernetes 将服务上的端口80发布到群集外部网络套接字上的任意端口。 您可以使用命令查找分配的端口 `kubectl get service` 。

不 `stockdata` 能从群集外部访问该服务，因此它使用默认类型 `ClusterIP` 。

生产系统最有可能使用集成的负载均衡器向外部使用者公开公共应用程序。 以这种方式公开的服务应使用 `LoadBalancer` 类型。

有关服务类型的详细信息，请参阅 [Kubernetes 发布服务](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types) 文档。

#### <a name="deploy-the-stockweb-application"></a>部署 StockWeb 应用程序

使用 `kubectl` 应用 `stockweb.yml` 文件，并确认已创建部署和服务：

```console
> kubectl apply -f .\stockweb.yml
deployment.apps/stockweb created
service/stockweb created

> kubectl get deployment stockweb --namespace stocks
NAME       READY   UP-TO-DATE   AVAILABLE   AGE
stockweb   1/1     1            1           8s

> kubectl get service stockweb --namespace stocks
NAME       TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
stockweb   NodePort   10.106.141.5   <none>        80:32564/TCP   13s
```

此命令的输出 `get service` 显示 HTTP 端口已发布到 `32564` 外部网络上的端口。 对于 Docker Desktop，此 IP 地址将为 localhost。 可以通过浏览到来访问应用程序 `http://localhost:32564` 。

### <a name="test-the-application"></a>测试应用程序

StockWeb 应用程序显示从简单的请求-答复服务中检索到的 NASDAQ 股票列表。 对于此演示，每行还显示返回它的服务实例的唯一 ID。

![StockWeb 屏幕快照](media/kubernetes/stockweb-screenshot.png)

如果服务的副本数 `stockdata` 增加，则可能希望 **服务器** 值从行更改为行，但事实上，始终从同一实例返回所有100记录。 如果每隔几秒刷新一次页面，则服务器 ID 保持不变。 为何发生这种情况？ 此处提供两个因素。

首先，Kubernetes 服务发现系统默认使用轮循机制负载平衡。 第一次查询 DNS 服务器时，它将返回服务的第一个匹配的 IP 地址。 下一次，它将返回列表中的下一个 IP 地址，依此类推，直到结束。 此时，它会循环返回到开始。

其次， `HttpClient` 用于 StockWeb 应用程序的 gRPC 客户端的由 [ASP.NET Core HttpClientFactory](../microservices/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests.md)创建和管理，该客户端的单个实例用于每次调用页面。 客户端只进行一次 DNS 查找，因此所有请求都将路由到相同的 IP 地址。 由于 `HttpClientHandler` 出于性能方面的原因，已缓存的多个请求都将使用相同的 IP 地址，直到缓存的 DNS 条目过期，或由于某种原因释放了处理程序实例。

结果是，默认情况下，对 gRPC 服务的请求不会在群集中该服务的所有实例间平衡。 不同的使用者将使用不同的实例，但这不能保证请求的良好分布或资源的平衡使用。

下一章 [服务网格](service-mesh.md)将解决此问题。

>[!div class="step-by-step"]
>[上一页](docker.md)
>[下一页](service-mesh.md)
