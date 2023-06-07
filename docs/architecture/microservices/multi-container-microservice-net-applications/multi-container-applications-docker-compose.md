---
title: 使用 docker-compose.yml 定义多容器应用程序
description: 如何使用 docker-compose.yml 指定多容器应用程序的微服务组合。
ms.date: 01/30/2020
ms.openlocfilehash: 81303be621da54b7336228585e86d1120a6b7598
ms.sourcegitcommit: ecd9e9bb2225eb76f819722ea8b24988fe46f34c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2020
ms.locfileid: "96739784"
---
# <a name="defining-your-multi-container-application-with-docker-composeyml"></a>使用 docker-compose.yml 定义多容器应用程序

本指南中，[docker-compose.yml](https://docs.docker.com/compose/compose-file/) 文件在[步骤 4.生成多容器 Docker 应用程序时，在 docker-compose.yml 中定义服务](../docker-application-development-process/docker-app-development-workflow.md#step-4-define-your-services-in-docker-composeyml-when-building-a-multi-container-docker-application)部分中介绍。 但还有其他方式可使用 docker-compose 文件，这值得进一步探索。

例如，可明确描述希望如何在 docker-compose.yml 文件中部署多容器应用程序。 此外，也可以描述将如何生成自定义 Docker 映像。 （也可以使用 Docker CLI 生成自定义 Docker 映像。）

基本上，开发人员需定义想要部署的每个容器以及每个容器部署的某些特征。 拥有多容器部署说明文件后，即可通过由 [docker-compose up](https://docs.docker.com/compose/overview/) CLI 命令编制的单个操作部署整个解决方案，也可简单从 Visual Studio 进行部署。 否则，需要在命令行中使用 `docker run` 命令，通过 Docker CLI 经多个步骤逐个部署容器。 因此，docker-compose.yml 中定义的每个服务都必须指定一个映像或生成映像。 其他为可选密钥，且类似于其 `docker run` 命令行对应项。

以下 YAML 代码是 eShopOnContainers 示例的单个 docker-compose.yml 文件的定义，文件可能是全局文件。 这不是来自 eShopOnContainers 的实际 docker-compose 文件。 相反，简化和合并后的单个文件，这不是使用 docker-compose 文件的最佳方式，稍后将对此进行解释。

```yml
version: '3.4'

services:
  webmvc:
    image: eshop/webmvc
    environment:
      - CatalogUrl=http://catalog-api
      - OrderingUrl=http://ordering-api
      - BasketUrl=http://basket-api
    ports:
      - "5100:80"
    depends_on:
      - catalog-api
      - ordering-api
      - basket-api

  catalog-api:
    image: eshop/catalog-api
    environment:
      - ConnectionString=Server=sqldata;Initial Catalog=CatalogData;User Id=sa;Password=[PLACEHOLDER]
    expose:
      - "80"
    ports:
      - "5101:80"
    #extra hosts can be used for standalone SQL Server or services at the dev PC
    extra_hosts:
      - "CESARDLSURFBOOK:10.0.75.1"
    depends_on:
      - sqldata

  ordering-api:
    image: eshop/ordering-api
    environment:
      - ConnectionString=Server=sqldata;Database=Services.OrderingDb;User Id=sa;Password=[PLACEHOLDER]
    ports:
      - "5102:80"
    #extra hosts can be used for standalone SQL Server or services at the dev PC
    extra_hosts:
      - "CESARDLSURFBOOK:10.0.75.1"
    depends_on:
      - sqldata

  basket-api:
    image: eshop/basket-api
    environment:
      - ConnectionString=sqldata
    ports:
      - "5103:80"
    depends_on:
      - sqldata

  sqldata:
    environment:
      - SA_PASSWORD=[PLACEHOLDER]
      - ACCEPT_EULA=Y
    ports:
      - "5434:1433"

  basketdata:
    image: redis
```

此文件中的根密钥是服务。 在该密钥下，可在执行 `docker-compose up` 命令或使用此 docker-compose.yml 文件从 Visual Studio 进行部署时，定义要部署和运行的服务。 在这种情况下，docker-compose.yml 文件定义了多个服务，如以下表所述。

| 服务名称 | 描述 |
|--------------|-------------|
| webmvc       | 容器，包括从服务器端 C\# 使用微服务的 ASP.NET Core MVC 应用程序|
| catalog-api  | 容器，包括 Catalog ASP.NET Core Web API 微服务 |
| ordering-api | 容器，包括 Ordering ASP.NET Core Web API 微服务 |
| sqldata     | 容器，运行适用于 Linux 的 SQL Server，并保存微服务数据库 |
| basket-api   | 容器，包含有 Basket ASP.NET Core Web API 微服务 |
| basketdata  | 容器，运行 REDIS 缓存服务，并将篮数据库作为 REDIS 缓存 |

### <a name="a-simple-web-service-api-container"></a>简单的 Web 服务 API 容器

关注单个容器，catalog-api 容器微服务的定义很简单：

```yml
  catalog-api:
    image: eshop/catalog-api
    environment:
      - ConnectionString=Server=sqldata;Initial Catalog=CatalogData;User Id=sa;Password=[PLACEHOLDER]
    expose:
      - "80"
    ports:
      - "5101:80"
    #extra hosts can be used for standalone SQL Server or services at the dev PC
    extra_hosts:
      - "CESARDLSURFBOOK:10.0.75.1"
    depends_on:
      - sqldata
```

这种容器化服务具有以下基本配置：

- 它基于自定义 eshop/catalog-api  映像。 为简便起见，文件中没有 build: key 设置。 这意味着，必须事先生成（使用 docker build 命令）映像或从任何 Docker 注册表下载（使用 docker pull 命令）映像。

- 它定义了一个名为 ConnectionString 的环境变量，Entity Framework 使用连接字符串用来访问包含目录数据模型的 SQL Server 实例。 在这种情况下，同一 SQL Server 容器拥有多个数据库。 因此，开发 Docker 时所需计算机内存减少。 但是，开发人员也可以为每个微服务数据库部署一个 SQL Server 容器。

- SQL Server 的名称是 sqldata，与运行适用于 Linux 的 SQL Server 实例的容器的名称相同  。 这很方便，因为使用此名称解析（Docker 主机内部）可解析网络地址，这样从其他容器访问容器时，无需了解受访容器的内部 IP。

由于连接字符串由环境变量定义，因此可在不同的时间通过不同机制设置该变量。 例如，在最终主机中部署到生产时，可设置不同的连接字符串，或通过 Azure DevOps Services 中的 CI/CD 管道或喜爱的 DevOps 系统来完成。

- 它公开了端口 80，用于对 Docker 主机中的 catalog-api 服务进行内部访问  。 目前的主机是 Linux VM，因为它以适用于 Linux 的 Docker 映像为基础，但开发人员可配置该容器，使其在 Windows 映像上运行。

- 它将容器上公开的端口 80 转接到 Docker 主机 (Linux VM) 上的端口 5101。

- 它将 Web 服务链接到 sqldata  服务（在容器中运行的 Linux 数据库的 SQL Server 实例）。 指定此依赖项时，在 sqldata 容器启动后，catalog-api 容器才会启动；这一点很重要，因为 catalog-api 需要先启动并运行 SQL Server 数据库。 但是，在许多情况下，这种容器依赖项不足，因为 Docker 只能在容器级别进行检查。 有时服务（在此情况下为 SQL Server）可能还未准备就绪，因此建议在客户端微服务中实施具有指数回退的重试逻辑。 这样一来，如果依赖项容器在短时间内未准备就绪，应用程序仍然可以复原。

- 它被配置为允许访问外部服务器：extra\_hosts 设置允许访问 Docker 主机之外的外部服务器或计算机（即作为开发 Docker 主机的默认 Linux VM 以外的），例如开发 PC 上的本地 SQL Server 实例。

此外，还有其他更高级的 `docker-compose.yml` 设置，我们将在下面的部分介绍。

### <a name="using-docker-compose-files-to-target-multiple-environments"></a>针对多个环境使用 docker-compose 文件

`docker-compose.*.yml` 文件是定义文件，可供多种支持该格式的基础结构使用。 最简单的工具是 docker-compose 命令。

因此，可针对以下主要方案使用 docker-compose 命令。

#### <a name="development-environments"></a>开发环境

开发应用程序时，能够在独立开发环境中运行应用程序非常重要。 开发人员可使用 docker-compose CLI 命令来创建该环境，或使用在后台使用 docker-compose 的 Visual Studio。

docker-compose.yml 文件可配置和记录所有应用程序的服务依赖项（其他服务、缓存、数据库、队列等）。 通过 docker-compose CLI 命令，可使用单个命令 (docker-compose up) 为每个依赖项创建并启动一个或多个容器。

docker-compose.yml 文件是由 Docker 引擎解释的配置文件，但也可作为有关多容器应用程序组成的方便文档文件。

#### <a name="testing-environments"></a>测试环境

任何持续部署 (CD) 或持续集成 (CI) 过程都离不开单元测试和集成测试。 这些自动化测试需要独立的环境，以便它们不受用户或应用程序数据变化的影响。

使用 Docker Compose，可通过命令提示符或脚本中的几条命令非常轻松地创建和摧毁该独立环境，如以下命令：

```console
docker-compose -f docker-compose.yml -f docker-compose-test.override.yml up -d
./run_unit_tests
docker-compose -f docker-compose.yml -f docker-compose-test.override.yml down
```

#### <a name="production-deployments"></a>生产部署

开发人员还可使用 Compose 部署到远程 Docker 引擎。 部署到单个 Docker 主机实例（例如配置了 [Docker Machine](https://docs.docker.com/machine/overview/) 的生产 VM 或服务器）就是一个典型的例子。

如果正在使用任何其他业务流程协调程序（Azure Service Fabric、Kubernetes 等），则可能需要添加如 docker-compose.yml 中的设置和元数据配置设置，但需采用其他业务流程协调程序所需的格式。

在任何情况下，docker-compose 都是用于开发、测试和生产工作流的便捷工具和元数据格式，但生产工作流可能因使用的业务流程协调程序而异。

### <a name="using-multiple-docker-compose-files-to-handle-several-environments"></a>使用多个 docker-compose 文件来处理多个环境

针对不同环境时，应使用多个 compose 文件。 这样可以根据环境创建多个配置变体。

#### <a name="overriding-the-base-docker-compose-file"></a>重写基本 docker-compose 文件

开发人员可使用单个 docker-compose.yml 文件，如前面部分中的简化示例所示。 但是，对于大多数应用程序不建议这样做。

默认情况下，Compose 读取两个文件，docker-compose.yml 和可选的 docker-compose.override.yml 文件。 如图 6-11 所示，使用 Visual Studio 并启用 Docker 支持时，Visual Studio 还会创建另一个用于调试应用程序的 docker compose.vs.debug.g.yml 文件，可在主解决方案文件夹的 obj\\Docker\\ 文件夹中查看此文件。

![Docker Compose 项目中的文件。](./media/multi-container-applications-docker-compose/docker-compose-file-visual-studio.png)

图 6-11  。 Visual Studio 2019 中的 docker-compose 文件

**docker-compose** 项目文件结构：

- *.dockerignore* - 用于忽略文件
- *docker-compose.yml* - 用于撰写微服务
- *docker-compose.override.yml* - 用于配置微服务环境

用户可使用任何编辑器（如 Visual Studio Code 或 Sublime）编辑 docker-compose 文件，并使用 docker-compose up 命令运行该应用程序。

按照约定，docker-compose.yml 文件包含基本配置和其他静态设置。 这意味着服务配置不应该根据所针对的部署环境而改变。

顾名思义，docker-compose.override.yml 文件包含替代基本配置的配置设置，例如依赖于部署环境的配置。 用户也可以拥有具有不同名称的多个重写文件。 重写文件通常包含应用程序所需的但特定于环境或部署的其他信息。

#### <a name="targeting-multiple-environments"></a>针对多个环境

典型的用例是定义多个 compose 文件时，可针对多个环境，如生产、暂存、CI 或开发环境。 为支持这些差异，可将 Compose 配置拆分成多个文件，如图 6-12 所示。

![三个 docker-compose 文件设置为替代基文件的关系图。](./media/multi-container-applications-docker-compose/multiple-docker-compose-files-override-base.png)

图 6-12  。 重写基本 docker-compose.yml 文件中的值的多个 docker-compose 文件

可组合多个 docker compose*.yml 文件来应对不同的环境。 从基本 docker-compose.yml 文件开始。 此基本文件包含的基本设置或静态配置设置不会因环境而更改。 例如，eShopOnContainers 应用将以下 docker-compose.yml 文件（使用少量服务以进行简化）作为基本文件。

```yml
#docker-compose.yml (Base)
version: '3.4'
services:
  basket-api:
    image: eshop/basket-api:${TAG:-latest}
    build:
      context: .
      dockerfile: src/Services/Basket/Basket.API/Dockerfile
    depends_on:
      - basketdata
      - identity-api
      - rabbitmq

  catalog-api:
    image: eshop/catalog-api:${TAG:-latest}
    build:
      context: .
      dockerfile: src/Services/Catalog/Catalog.API/Dockerfile
    depends_on:
      - sqldata
      - rabbitmq

  marketing-api:
    image: eshop/marketing-api:${TAG:-latest}
    build:
      context: .
      dockerfile: src/Services/Marketing/Marketing.API/Dockerfile
    depends_on:
      - sqldata
      - nosqldata
      - identity-api
      - rabbitmq

  webmvc:
    image: eshop/webmvc:${TAG:-latest}
    build:
      context: .
      dockerfile: src/Web/WebMVC/Dockerfile
    depends_on:
      - catalog-api
      - ordering-api
      - identity-api
      - basket-api
      - marketing-api

  sqldata:
    image: mcr.microsoft.com/mssql/server:2017-latest

  nosqldata:
    image: mongo

  basketdata:
    image: redis

  rabbitmq:
    image: rabbitmq:3-management

```

基本 docker-compose.yml 文件中的值不应因目标部署环境不同而不同。

例如，如果专注于 webmvc 服务定义，则可以看到无论目标环境如何，这些信息都相同。 会看到以下信息：

- 服务名称：webmvc。

- 容器的自定义映像：eshop/webmvc。

- 生成自定义 Docker 映像的命令，指示要使用的 Dockerfile。

- 其他服务依赖项，所以此容器在其他依赖项容器启动之前不会启动。

开发人员可进行其他配置，但重要的一点是，在基本 docker-compose.yml 文件中，只需设置各环境的共同信息。 然后，在 docker-compose.override.yml 或用于生产或暂存的类似文件中，应配置特定于每个环境的配置。

通常，docker-compose.override.yml 适用于开发环境，如 eShopOnContainers 中的以下示例所示：

```yml
#docker-compose.override.yml (Extended config for DEVELOPMENT env.)
version: '3.4'

services:
# Simplified number of services here:

  basket-api:
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=http://0.0.0.0:80
      - ConnectionString=${ESHOP_AZURE_REDIS_BASKET_DB:-basketdata}
      - identityUrl=http://identity-api
      - IdentityUrlExternal=http://${ESHOP_EXTERNAL_DNS_NAME_OR_IP}:5105
      - EventBusConnection=${ESHOP_AZURE_SERVICE_BUS:-rabbitmq}
      - EventBusUserName=${ESHOP_SERVICE_BUS_USERNAME}
      - EventBusPassword=${ESHOP_SERVICE_BUS_PASSWORD}
      - AzureServiceBusEnabled=False
      - ApplicationInsights__InstrumentationKey=${INSTRUMENTATION_KEY}
      - OrchestratorType=${ORCHESTRATOR_TYPE}
      - UseLoadTest=${USE_LOADTEST:-False}

    ports:
      - "5103:80"

  catalog-api:
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=http://0.0.0.0:80
      - ConnectionString=${ESHOP_AZURE_CATALOG_DB:-Server=sqldata;Database=Microsoft.eShopOnContainers.Services.CatalogDb;User Id=sa;Password=[PLACEHOLDER]}
      - PicBaseUrl=${ESHOP_AZURE_STORAGE_CATALOG_URL:-http://localhost:5202/api/v1/catalog/items/[0]/pic/}
      - EventBusConnection=${ESHOP_AZURE_SERVICE_BUS:-rabbitmq}
      - EventBusUserName=${ESHOP_SERVICE_BUS_USERNAME}
      - EventBusPassword=${ESHOP_SERVICE_BUS_PASSWORD}
      - AzureStorageAccountName=${ESHOP_AZURE_STORAGE_CATALOG_NAME}
      - AzureStorageAccountKey=${ESHOP_AZURE_STORAGE_CATALOG_KEY}
      - UseCustomizationData=True
      - AzureServiceBusEnabled=False
      - AzureStorageEnabled=False
      - ApplicationInsights__InstrumentationKey=${INSTRUMENTATION_KEY}
      - OrchestratorType=${ORCHESTRATOR_TYPE}
    ports:
      - "5101:80"

  marketing-api:
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=http://0.0.0.0:80
      - ConnectionString=${ESHOP_AZURE_MARKETING_DB:-Server=sqldata;Database=Microsoft.eShopOnContainers.Services.MarketingDb;User Id=sa;Password=[PLACEHOLDER]}
      - MongoConnectionString=${ESHOP_AZURE_COSMOSDB:-mongodb://nosqldata}
      - MongoDatabase=MarketingDb
      - EventBusConnection=${ESHOP_AZURE_SERVICE_BUS:-rabbitmq}
      - EventBusUserName=${ESHOP_SERVICE_BUS_USERNAME}
      - EventBusPassword=${ESHOP_SERVICE_BUS_PASSWORD}
      - identityUrl=http://identity-api
      - IdentityUrlExternal=http://${ESHOP_EXTERNAL_DNS_NAME_OR_IP}:5105
      - CampaignDetailFunctionUri=${ESHOP_AZUREFUNC_CAMPAIGN_DETAILS_URI}
      - PicBaseUrl=${ESHOP_AZURE_STORAGE_MARKETING_URL:-http://localhost:5110/api/v1/campaigns/[0]/pic/}
      - AzureStorageAccountName=${ESHOP_AZURE_STORAGE_MARKETING_NAME}
      - AzureStorageAccountKey=${ESHOP_AZURE_STORAGE_MARKETING_KEY}
      - AzureServiceBusEnabled=False
      - AzureStorageEnabled=False
      - ApplicationInsights__InstrumentationKey=${INSTRUMENTATION_KEY}
      - OrchestratorType=${ORCHESTRATOR_TYPE}
      - UseLoadTest=${USE_LOADTEST:-False}
    ports:
      - "5110:80"

  webmvc:
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=http://0.0.0.0:80
      - PurchaseUrl=http://webshoppingapigw
      - IdentityUrl=http://10.0.75.1:5105
      - MarketingUrl=http://webmarketingapigw
      - CatalogUrlHC=http://catalog-api/hc
      - OrderingUrlHC=http://ordering-api/hc
      - IdentityUrlHC=http://identity-api/hc
      - BasketUrlHC=http://basket-api/hc
      - MarketingUrlHC=http://marketing-api/hc
      - PaymentUrlHC=http://payment-api/hc
      - SignalrHubUrl=http://${ESHOP_EXTERNAL_DNS_NAME_OR_IP}:5202
      - UseCustomizationData=True
      - ApplicationInsights__InstrumentationKey=${INSTRUMENTATION_KEY}
      - OrchestratorType=${ORCHESTRATOR_TYPE}
      - UseLoadTest=${USE_LOADTEST:-False}
    ports:
      - "5100:80"
  sqldata:
    environment:
      - SA_PASSWORD=[PLACEHOLDER]
      - ACCEPT_EULA=Y
    ports:
      - "5433:1433"
  nosqldata:
    ports:
      - "27017:27017"
  basketdata:
    ports:
      - "6379:6379"
  rabbitmq:
    ports:
      - "15672:15672"
      - "5672:5672"

```

在此示例中，开发替代配置向主机公开一些端口，使用重定向 URL 定义环境变量，并为开发环境指定连接字符串。 这些设置都只针对开发环境。

运行 `docker-compose up`（或从 Visual Studio 启动它时）时，该命令会自动读取替代内容，就像它已合并这两个文件。

假设需要为生产环境使用具有不同配置值、端口或连接字符串的另一个 Compose 文件。 可创建另一个重写文件，如具有不同设置和环境变量的名为 `docker-compose.prod.yml` 的文件。 该文件可能存储在不同 Git 存储库中，或由其他团队管理和保护。

#### <a name="how-to-deploy-with-a-specific-override-file"></a>如何使用特定重写文件进行部署

若要使用多个重写文件或具有不同名称的重写文件，可通过 docker-compose 命令使用 -f 选项并指定文件。 按照合并文件在命令行上的指定顺序进行撰写。 以下示例演示如何使用重写文件进行部署。

```console
docker-compose -f docker-compose.yml -f docker-compose.prod.yml up -d
```

#### <a name="using-environment-variables-in-docker-compose-files"></a>在 docker-compose 文件中使用环境变量

如前面的示例所示，开发人员可方便地从环境变量中获取配置信息，尤其是在生产环境中。 可使用语法 ${MY\_VAR} 在 docker-compose 文件中引用环境变量。 docker-compose.prod.yml 文件中的以下行显示如何引用环境变量的值。

```yml
IdentityUrl=http://${ESHOP_PROD_EXTERNAL_DNS_NAME_OR_IP}:5105
```

创建和初始化环境变量的方式不同，具体取决于主机环境（Linux、Windows、云集群等）。 但是，便捷方法是使用 .env 文件。 docker-compose 文件支持在 .env 文件中声明默认环境变量。 这些环境变量的值是默认值。 但开发人员在每个环境（主机操作系统或群集中的环境变量）中定义的值可重写这些默认值。 将此 .env 文件放置在执行 docker-compose 命令的文件夹中。

以下示例展示了 .env 文件，例如用于 eShopOnContainers 应用程序的 [.env](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/.env) 文件。

```sh
# .env file

ESHOP_EXTERNAL_DNS_NAME_OR_IP=localhost

ESHOP_PROD_EXTERNAL_DNS_NAME_OR_IP=10.121.122.92
```

Docker-compose 要求 .env 文件中的每行都是 \<variable\>=\<value\> 格式。

运行时环境中设置的值始终会重写 .env 文件中定义的值。 同样，通过命令行参数传递的值也会重写 .env 文件中设置的默认值。

#### <a name="additional-resources"></a>其他资源

- **Docker Compose 概述** \
    <https://docs.docker.com/compose/overview/>

- **多个 Compose 文件** \
    [https://docs.docker.com/compose/extends/\#multiple-compose-files](https://docs.docker.com/compose/extends/#multiple-compose-files)

### <a name="building-optimized-aspnet-core-docker-images"></a>生成优化的 ASP.NET Core Docker 映像

如果查看 Internet 上源代码的 Docker 和 .NET Core ，则会发现 Dockerfiles 会将源代码源复制到容器，展现生成 Docker 映像的简单性。 这些示例表明，使用简单配置，即可拥有 Docker 映像，同时应用程序还会带有环境。 以下示例显示在此情况下的简单 Dockerfile。

```dockerfile
FROM mcr.microsoft.com/dotnet/sdk:3.1
WORKDIR /app
ENV ASPNETCORE_URLS http://+:80
EXPOSE 80
COPY . .
RUN dotnet restore
ENTRYPOINT ["dotnet", "run"]
```

这样的 Dockerfile 为有效 Dockerfile。 但开发人员可以大幅优化映像，尤其是生产映像。

开发人员会在容器和微服务模型中不断启动容器。 使用容器时，通常不会重启睡眠容器，因为该容器为一次性容器。 业务流程协调程序（如 Kubernetes 和 Azure Service Fabric）只创建映像的新实例。 这意味着，需要在生成应用程序时，通过预编译应用程序进行优化，这样可加快实例化过程。 当容器启动时，它应已准备好运行。 请勿在运行时使用 `dotnet restore` 和 `dotnet build` CLI 命令进行还原和编译，如有关 .NET Core 和 Docker 的博客文章中所述。

.NET 团队一直致力于使 .NET Core 和 ASP.NET Core 成为容器优化的框架。 .NET Core 不仅是一个内存占用少的轻量级框架；该团队还致力于针对三种主要方案优化了 Docker 映像，并在版本 2.1 及更高版本中，在 dotnet/core  的 Docker 中心注册表中发布了这些映像：

1. **开发**：实现快速迭代和调试更改为优先事项，大小次之。

2. **生成**：编译应用程序是优先事项，此映像包括二进制文件和其他可优化二进制文件的依赖项。

3. **生产**：重点是实现快速部署和容器启动，所以这些映像仅限于二进制文件和运行应用程序所需的内容。

.NET 团队在 [dotnet/core](https://hub.docker.com/_/microsoft-dotnet/)（位于 Docker Hub）中提供了四个基本变体：

1. **sdk**：用于开发和生成方案
1. **aspnet**：用于 ASP.NET 生产方案
1. **runtime**：用于 .NET 生产方案
1. **runtime-deps**：用于 [自包含应用程序](../../../core/deploying/index.md#publish-self-contained)的生产方案

为了更快启动，运行时映像还会自动将 aspnetcore\_url 设置为端口 80，并使用 Ngen 创建程序集的本机映像缓存。

#### <a name="additional-resources"></a>其他资源

- **使用 ASP.NET Core 生成优化的 Docker 映像**
  <https://docs.microsoft.com/archive/blogs/stevelasker/building-optimized-docker-images-with-asp-net-core>

- **为 .NET Core 应用程序生成 Docker 映像**
  [https://docs.microsoft.com/dotnet/core/docker/building-net-docker-images](/aspnet/core/host-and-deploy/docker/building-net-docker-images)

> [!div class="step-by-step"]
> [上一页](data-driven-crud-microservice.md)
> [下一页](database-server-container.md)
