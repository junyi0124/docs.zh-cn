---
title: 通过 Ocelot 实现 API 网关
description: 了解如何通过 Ocelot 实现 API 网关以及如何在基于容器的环境中使用 Ocelot。
ms.date: 03/02/2020
ms.openlocfilehash: 6d9229228e228b664a602ce9a682d435505a8107
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95718093"
---
# <a name="implement-api-gateways-with-ocelot"></a>通过 Ocelot 实现 API 网关

> [!IMPORTANT]
> 引用微服务应用程序 [ eShopOnContainers ](https://github.com/dotnet-architecture/eShopOnContainers) 当前正在使用 [Envoy](https://www.envoyproxy.io/) 提供的功能来实现 API 网关，而不是之前引用的 [Ocelot](https://github.com/ThreeMammals/Ocelot)。
> 我们之所以选择这种设计，是因为 Envoy 对 WebSocket 协议的内置支持，这是 eShopOnContainers 中实现的新 gRPC 服务间通信所必需的。
> 但是，我们在指南中保留了此部分，因此，你可以将 Ocelot 视为适用于生产级方案的简单且功能强大的轻型 API 网关。
> 此外，最新的 Ocelot 版本在其 JSON 架构中包含中断性变更。 请考虑使用版本低于 v16.0.0 的 Ocelot 或使用密钥路由，而不使用重新路由。

## <a name="architect-and-design-your-api-gateways"></a>构建和设计 API 网关

以下体系结构关系图显示了如何在 eShopOnContainers 中通过 Ocelot 实现 API 网关。

![显示 eShopOnContainers 体系结构的图示。](./media/implement-api-gateways-with-ocelot/eshoponcontainers-architecture.png)

图 6-28。 使用 API 网关的 eShopOnContainers 体系结构

该图显示了如何使用“用于 Windows 的 Docker”或“用于 Mac 的 Docker”将整个应用程序部署到单个 Docker 主机或开发电脑。 然而，虽然部署到业务流程协调程序中的方法与之相似，但图中的容器都可在业务流程协调程序中横向扩展。

此外，应从业务流程协调程序卸载基础结构资产（如数据库、缓存和消息代理），并将其部署到基础结构的高可用系统，如 Azure SQL 数据库、Azure Cosmos DB、Azure Redis、Azure 服务总线或本地 HA 群集解决方案。

此外，在关系图中还可以看到，在开发和部署微服务以及自己的相关 API 网关时，拥有多个 API 网关后，多个开发团队可进行自治（在本例中为“营销”功能和“购物”功能）。

如果有单一 API 网关，这意味着多个开发团队可更新单个点，由此可将所有微服务与应用程序的某一个部分相结合。

更进一步来说，在设计时，有时也可将精细 API 网关限于单个业务微服务，具体取决于所选体系结构。 使用由业务或域指示的 API 网关边界将有助于更好地设计。

例如，由于精细 API 网关的概念类似于 UI 复合服务，因此 API 网关层中的精细粒度特别适用于基于微服务的高级复合 UI 应用程序。

上一节[创建基于微服务的复合 UI](../architect-microservice-container-applications/microservice-based-composite-ui-shape-layout.md) 中深入介绍许多详细信息。

对于许多中型和大型应用程序，关键在于使用自定义生成的 API 网关产品，这通常是一种不错的方法，但不能作为单一聚合器或唯一的中央自定义 API 网关，除非该 API 网关允许多个开发团队在多个独立配置区域创建自主微服务。

### <a name="sample-microservicescontainers-to-reroute-through-the-api-gateways"></a>通过 API 网关重新路由的示例微服务/容器

例如，eShopOnContainers 具有大约六项必须通过 API 网关发布的内部微服务类型，如下图所示。

![显示其子文件夹的“服务”文件夹的屏幕截图。](./media/implement-api-gateways-with-ocelot/eshoponcontainers-microservice-folders.png)

图 6-29。 Visual Studio 的 eShopOnContainers 解决方案中的微服务文件夹

对于“标识”服务，尽管借助 Ocelot，也可以将其纳入重新路由列表，但在设计过程中会将其从 API 网关路由中排除，因为它是系统中唯一的横切关注点。

目前，所有这些服务都是作为 ASP.NET Core Web API 服务实现的，可以从代码中了解到。 我们主要来看一项微服务，比如“目录”微服务代码。

![显示 Catalog.API 项目内容的解决方案资源管理器的屏幕截图。](./media/implement-api-gateways-with-ocelot/catalog-api-microservice-folders.png)

图 6-30。 Web API 微服务示例（“目录”微服务）

可看到“目录”微服务是典型的 ASP.NET Core Web API 项目，其中包含多个控制器和方法，如以下代码所示。

```csharp
[HttpGet]
[Route("items/{id:int}")]
[ProducesResponseType((int)HttpStatusCode.BadRequest)]
[ProducesResponseType((int)HttpStatusCode.NotFound)]
[ProducesResponseType(typeof(CatalogItem),(int)HttpStatusCode.OK)]
public async Task<IActionResult> GetItemById(int id)
{
    if (id <= 0)
    {
        return BadRequest();
    }
    var item = await _catalogContext.CatalogItems.
                                          SingleOrDefaultAsync(ci => ci.Id == id);
    //…

    if (item != null)
    {
        return Ok(item);
    }
    return NotFound();
}
```

HTTP 请求将最终运行访问微服务数据库的那种 C# 代码以及任何其他必需的操作。

对于微服务 URL，当容器部署在本地开发电脑（本地 Docker 主机）中时，每个微服务的容器始终有一个内部端口（通常是端口 80），在其 dockerfile 中指定，如以下 dockerfile 中所示：

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base
WORKDIR /app
EXPOSE 80
```

代码中显示的端口 80 位于 Docker 主机内部，因此客户端应用无法访问。

客户端应用只能访问使用 `docker-compose` 进行部署时发布的外部端口（如有）。

部署到生产环境时，不应发布这些外部端口。 这正是要使用 API 网关的原因，这样可避免客户端应用与微服务直接通信。

但是，在开发时，却要直接访问微服务/容器并通过 Swagger 运行它。 这就是为什么在 eShopOnContainers 中，即使 API 网关或客户端应用不使用外部端口，仍要指定外部端口。

以下是“目录”微服务的 `docker-compose.override.yml` 文件示例：

```yml
catalog-api:
  environment:
    - ASPNETCORE_ENVIRONMENT=Development
    - ASPNETCORE_URLS=http://0.0.0.0:80
    - ConnectionString=YOUR_VALUE
    - ... Other Environment Variables
  ports:
    - "5101:80"   # Important: In a production environment you should remove the external port (5101) kept here for microservice debugging purposes.
                  # The API Gateway redirects and access through the internal port (80).
```

可以看到如何在 docker-compose.override.yml 配置中将端口 80 作为“目录”容器的内部端口，将端口 5101 作为外部访问的端口。 但是，使用 API 网关时，应用程序不应使用此端口，端口仅用于调试、运行和测试“目录”微服务。

通常不会将 docker-compose 部署到生产环境中，因为适合微服务的生产部署环境是 Kubernetes 或 Service Fabric 等业务流程协调程序。 部署到这些环境时，会使用不同的配置文件，部署时不直接发布微服务的任何外部端口，但始终要使用 API 网关中的反向代理。

在本地 Docker 主机中运行“目录”微服务。 从 Visual Studio 运行完整的 eShopOnContainers 解决方案（它将运行 docker-compose 文件中的所有服务）；在 CMD 或放置 `docker-compose.yml` 和 `docker-compose.override.yml` 的文件夹中的 PowerShell 中运行以下 docker-compose 命令启动“目录”微服务。

```console
docker-compose run --service-ports catalog-api
```

此命令仅运行 catalog-api 服务容器以及 docker-compose.yml 中指定的依赖项。 此事例中为 SQL Server 容器和 RabbitMQ 容器。

然后，可直接访问“目录”微服务，并直接通过该“外部”端口由 Swagger UI 查看其方法，此事例中为 `http://localhost:5101/swagger`：

![显示 Catalog.API REST API 的 Swagger UI 的屏幕截图。](./media/implement-api-gateways-with-ocelot/test-catalog-microservice.png)

图 6-31。 通过其 Swagger UI 测试“目录”微服务

此时，可在 Visual Studio 中的 C# 代码中设置断点，使用 Swagger UI 中公开的方法测试微服务，最后使用 `docker-compose down` 命令清理所有内容。

但是，在这种情况下，通过外部端口 5101 直接访问与微服务的通信正是你希望在应用程序中避免的。 可设置 API 网关的间接附加级别来避免该情况（此事例中为 Ocelot）。 这样一来，客户端应用将不会直接访问微服务。

## <a name="implementing-your-api-gateways-with-ocelot"></a>通过 Ocelot 实现 API 网关

Ocelot 基本上是一组可按特定顺序应用的中间件。

Ocelot 仅适用于 ASP.NET Core。 包的最新版本以 `.NETCoreApp 3.1` 为目标，因此不适用于 .NET Framework 应用程序。

使用 Visual Studio 中的 [Ocelot NuGet 包](https://www.nuget.org/packages/Ocelot/)在 ASP.NET Core 项目中安装 Ocelot 及其依赖项。

```powershell
Install-Package Ocelot
```

在 eShopOnContainers 中，其 API 网关实现是一个非常简单的 ASP.NET Core WebHost 项目，而 Ocelot 的中间件可处理所有 API 网关功能，如下图所示：

![显示 Ocelot API 网关项目的解决方案资源管理器的屏幕截图。](./media/implement-api-gateways-with-ocelot/ocelotapigw-base-project.png)

图 6-32。 eShopOnContainers 中的 OcelotApiGw 基础项目

此 ASP.NET Core WebHost 项目基本上由两个简单文件生成：`Program.cs` 和 `Startup.cs`。

Program.cs 只需要创建和配置典型的 ASP.NET Core BuildWebHost。

```csharp
namespace OcelotApiGw
{
    public class Program
    {
        public static void Main(string[] args)
        {
            BuildWebHost(args).Run();
        }

        public static IWebHost BuildWebHost(string[] args)
        {
            var builder = WebHost.CreateDefaultBuilder(args);

            builder.ConfigureServices(s => s.AddSingleton(builder))
                    .ConfigureAppConfiguration(
                          ic => ic.AddJsonFile(Path.Combine("configuration",
                                                            "configuration.json")))
                    .UseStartup<Startup>();
            var host = builder.Build();
            return host;
        }
    }
}
```

对于 Ocelot，必须通过 `AddJsonFile()` 方法向生成器提供 `configuration.json` 文件。 通常可使用不同端口在 `configuration.json` 中指定所有 API 网关 Re-Route，即具有特定端口的外部终结点和相关内部终结点。

```json
{
    "ReRoutes": [],
    "GlobalConfiguration": {}
}
```

配置具有两个部分。 一组 ReRoute 和一个 GlobalConfiguration。 ReRoute 是指示 Ocelot 如何处理上游请求的对象。 全局配置允许替代 ReRoute 特定设置。 如果不想管理大量的 ReRoute 特定设置，可采用此方法。

以下是 eShopOnContainers 中某个 API 网关的 [ReRoute 配置文件](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/ApiGateways/Web.Bff.Shopping/apigw/configuration.json)的简化示例。

```json
{
  "ReRoutes": [
    {
      "DownstreamPathTemplate": "/api/{version}/{everything}",
      "DownstreamScheme": "http",
      "DownstreamHostAndPorts": [
        {
          "Host": "catalog-api",
          "Port": 80
        }
      ],
      "UpstreamPathTemplate": "/api/{version}/c/{everything}",
      "UpstreamHttpMethod": [ "POST", "PUT", "GET" ]
    },
    {
      "DownstreamPathTemplate": "/api/{version}/{everything}",
      "DownstreamScheme": "http",
      "DownstreamHostAndPorts": [
        {
          "Host": "basket-api",
          "Port": 80
        }
      ],
      "UpstreamPathTemplate": "/api/{version}/b/{everything}",
      "UpstreamHttpMethod": [ "POST", "PUT", "GET" ],
      "AuthenticationOptions": {
        "AuthenticationProviderKey": "IdentityApiKey",
        "AllowedScopes": []
      }
    }

  ],
    "GlobalConfiguration": {
      "RequestIdKey": "OcRequestId",
      "AdministrationPath": "/administration"
    }
  }
```

Ocelot API 网关的主要功能是接收传入的 HTTP 请求并将其转发到下游服务，目前作为另一 HTTP 请求。 Ocelot 将一个请求到另一请求的路由描述为 ReRoute。

例如，我们主要来看上方 configuration.json（即“购物篮”微服务的配置）中的某个 ReRoute。

```json
{
      "DownstreamPathTemplate": "/api/{version}/{everything}",
      "DownstreamScheme": "http",
      "DownstreamHostAndPorts": [
        {
          "Host": "basket-api",
          "Port": 80
        }
      ],
      "UpstreamPathTemplate": "/api/{version}/b/{everything}",
      "UpstreamHttpMethod": [ "POST", "PUT", "GET" ],
      "AuthenticationOptions": {
        "AuthenticationProviderKey": "IdentityApiKey",
        "AllowedScopes": []
      }
}
```

DownstreamPathTemplate、Scheme 和 DownstreamHostAndPorts 构成要将此请求转发到的内部微服务 URL。

端口是服务使用的内部端口。 使用容器时，在其 dockerfile 中指定端口。

`Host` 是一个服务名称，取决于使用的服务名称解析。 使用 docker-compose 时，服务名称由 Docker 主机提供，它使用 docker-compose 文件中提供的服务名称。 如果使用 Kubernetes 或 Service Fabric 等业务流程协调程序，则应通过每个业务流程协调程序提供的 DNS 或名称解析来解析该名称。

DownstreamHostAndPorts 是一个数组，包含要将请求转发到的任何下游服务的主机和端口。 通常这只包含一个条目，但有时可能想要将均衡请求加载到下游服务，而通过 Ocelot 即可添加多个条目，然后选择负载均衡器。 但是如果使用 Azure 和任何业务流程协调程序，那么通过云和业务流程协调程序基础结构进行负载均衡可能会更好。

UpstreamPathTemplate 是一个 URL，Ocelot 将其用来识别用于客户端中给定请求的 DownstreamPathTemplate。 最后，使用了 UpstreamHttpMethod，因此 Ocelot 可区分对相同 URL 的不同的请求（GET、POST、PUT）。

此时，即可拥有使用一个或[多个合并 configuration.json 文件](https://ocelot.readthedocs.io/en/latest/features/configuration.html#merging-configuration-files)的单个 Ocelot API 网关，也可将[配置存储在 Consul KV 存储中](https://ocelot.readthedocs.io/en/latest/features/configuration.html#store-configuration-in-consul)。

但正如体系结构和设计部分中所介绍的那样，如果真的想拥有自主微服务，那么最好将单一 API 网关拆分成多个 API 网关和/或 BFF（用于前端的后端）。 为此，我们来看看如何通过 Docker 容器实现该方法。

### <a name="using-a-single-docker-container-image-to-run-multiple-different-api-gateway--bff-container-types"></a>使用单个 Docker 容器映像运行多个不同类型的 API 网关/BFF 容器

在 eShopOnContainers 中，为 Ocelot API 网关使用单个 Docker 容器映像。但是在运行时，通过提供不同的 configuration.json 文件为每种类型的 API 网关/BFF 创建不同的服务/容器，并使用 docker 卷为每个服务访问不同的 PC 文件夹。

![所有 API 网关的单个 Ocelot 网关 Docker 映像的图示。](./media/implement-api-gateways-with-ocelot/reusing-single-ocelot-docker-image.png)

图 6-33。 在多个 API 网关类型中重用单个 Ocelot Docker 映像

在 eShopOnContainers 中，使用名为“OcelotApiGw”的项目和 docker-compose.yml 文件中指定的映像名称“eshop/ocelotapigw”创建“Generic Ocelot API Gateway Docker Image”。 然后在部署到 Docker 时，会有四个从同一 Docker 映像创建的 API 网关容器，如以下从 docker-compose.yml 文件中提取的内容所示。

```yml
  mobileshoppingapigw:
    image: eshop/ocelotapigw:${TAG:-latest}
    build:
      context: .
      dockerfile: src/ApiGateways/ApiGw-Base/Dockerfile

  mobilemarketingapigw:
    image: eshop/ocelotapigw:${TAG:-latest}
    build:
      context: .
      dockerfile: src/ApiGateways/ApiGw-Base/Dockerfile

  webshoppingapigw:
    image: eshop/ocelotapigw:${TAG:-latest}
    build:
      context: .
      dockerfile: src/ApiGateways/ApiGw-Base/Dockerfile

  webmarketingapigw:
    image: eshop/ocelotapigw:${TAG:-latest}
    build:
      context: .
      dockerfile: src/ApiGateways/ApiGw-Base/Dockerfile
```

另外，如 docker-compose.override.yml 文件所示，这些 API 网关容器之间唯一的区别是 Ocelot 配置文件，该文件对每个服务容器的配置文件都不同，并且在运行时通过 Docker 卷指定。

```yml
mobileshoppingapigw:
  environment:
    - ASPNETCORE_ENVIRONMENT=Development
    - IdentityUrl=http://identity-api
  ports:
    - "5200:80"
  volumes:
    - ./src/ApiGateways/Mobile.Bff.Shopping/apigw:/app/configuration

mobilemarketingapigw:
  environment:
    - ASPNETCORE_ENVIRONMENT=Development
    - IdentityUrl=http://identity-api
  ports:
    - "5201:80"
  volumes:
    - ./src/ApiGateways/Mobile.Bff.Marketing/apigw:/app/configuration

webshoppingapigw:
  environment:
    - ASPNETCORE_ENVIRONMENT=Development
    - IdentityUrl=http://identity-api
  ports:
    - "5202:80"
  volumes:
    - ./src/ApiGateways/Web.Bff.Shopping/apigw:/app/configuration

webmarketingapigw:
  environment:
    - ASPNETCORE_ENVIRONMENT=Development
    - IdentityUrl=http://identity-api
  ports:
    - "5203:80"
  volumes:
    - ./src/ApiGateways/Web.Bff.Marketing/apigw:/app/configuration
```

由于上面的代码，并且如下方 Visual Studio Explorer 所示，定义每个特定业务/BFF API 网关所需的唯一文件只有 configuration.json 文件，因为这四个 API 网关基于相同 Docker 映像。

![显示具有 configuration.json 文件的所有 API 网关的屏幕截图。](./media/implement-api-gateways-with-ocelot/ocelot-configuration-files.png)

图 6-34。 使用 Ocelot 定义每个 API 网关/BFF 所需的唯一文件是配置文件

将 API 网关拆分成多个 API 网关后，专注于不同微服务子集的不同开发团队可使用独立 Ocelot 配置文件来管理自己的 API 网关。 同时重用相同的 Ocelot Docker 映像。

现在，如果通过 API 网关运行 eShopOnContainers（在打开 eShopOnContainers-ServicesAndWebApps.sln 解决方案或运行“docker-compose up”时，默认包含在 VS 中），将执行以下示例路由。

例如，访问 webshoppingapigw API 网关提供的上游 URL `http://localhost:5202/api/v1/c/catalog/items/2/` 时，将从 Docker 主机中的内部下游 URL `http://catalog-api/api/v1/2` 获取相同结果，如以下浏览器所示。

![显示通过 API 网关进行响应的浏览器屏幕截图。](./media/implement-api-gateways-with-ocelot/access-microservice-through-url.png)

图 6-35。 通过 API 网关提供的 URL 访问微服务

由于测试或调试原因，如果想不通过 API 网关直接访问目录 Docker 容器（仅在开发环境中），考虑到“catalog-api”是 Docker 主机内部的 DNS 解析（由 docker-compose 服务名称处理服务发现），直接访问容器的唯一方法是通过 docker-compose.override.yml 中发布的外部端口，该端口仅用于开发测试，例如以下浏览器中的 `http://localhost:5101/api/v1/Catalog/items/1`。

![显示对 Catalog.api 的直接响应的浏览器屏幕截图。](./media/implement-api-gateways-with-ocelot/direct-access-microservice-testing.png)

图 6-36。 出于测试目的直接访问微服务

但由于已配置应用程序，因此它会通过 API 网关访问所有微服务，而非通过直接端口“快捷方式”。

### <a name="the-gateway-aggregation-pattern-in-eshoponcontainers"></a>eShopOnContainers 中的网关聚合模式

如上所述，可通过代码使用自定义服务灵活实现请求聚合。 还可使用 [Ocelot 中的请求聚合功能](https://ocelot.readthedocs.io/en/latest/features/requestaggregation.html#request-aggregation)实现请求聚合，但可能无法满足你对灵活性的要求。 因此，在 eShopOnContainers 中实现聚合的首选方法是为每个聚合器提供显式 ASP.NET Core Web API 服务。

根据该方法，在考虑到早前所示的简化全局体系结构关系图中未显示的聚合器服务时，API 网关复合关系图实际上得到了更多扩展。

在下图中，还可了解聚合器服务如何与其相关 API 网关协同工作。

![显示聚合器服务的 eShopOnContainers 体系结构的图示。](./media/implement-api-gateways-with-ocelot/eshoponcontainers-architecture-aggregator-services.png)

图 6-37。 使用聚合器服务的 eShopOnContainers 体系结构

进一步放大视图，在下图的“购物”业务区中，可以看到在使用 API 网关中的聚合器服务时，客户端应用和微服务之间的干扰减少了。

![显示放大的 eShopOnContainers 体系结构的图示。](./media/implement-api-gateways-with-ocelot/zoom-in-vision-aggregator-services.png)

图 6-38。 聚合器服务的放大影像

可以看到，当关系图显示可能来自 API 网关的请求时，它可能变得复杂。 虽然可以看到如何简化蓝色箭头，但从客户端应用角度来看，通过减少通信中的干扰和延迟来使用聚合器模式时，尤其是远程应用（移动和 SPA 应用）的用户体验最终可获得显著改善。

对于“营销”业务范围和微服务，它是一个简单的用例，因此不需要使用聚合器，但如果需要，也可使用。

### <a name="authentication-and-authorization-in-ocelot-api-gateways"></a>Ocelot API 网关中的身份验证和授权

在 Ocelot API 网关中，可使用提供身份验证令牌的 [IdentityServer](https://identityserver.io/)，在 API 网关外部或内部设置身份验证服务，例如 ASP.NET Core Web API 服务。

由于 eShopOnContainers 使用的多个 API 网关具有基于 BFF 和业务范围的边界，因此“标识/身份验证”服务排除在 API 网关之外，如下图中黄色高亮部分所示。

![显示 API 网关下的标识微服务的图示。](./media/implement-api-gateways-with-ocelot/eshoponcontainers-identity-service-position.png)

图 6-39。 eShopOnContainers 中的“标识”服务的位置

但是，Ocelot 还支持在 API 网关边界内设置“标识/身份验证”微服务，如另一图所示。

![显示 Ocelot API 网关中的身份验证的图示。](./media/implement-api-gateways-with-ocelot/ocelot-authentication.png)

图 6-40。 Ocelot 中的身份验证

如上图所示，当标识微服务位于 API 网关 (AG) 下时：1) AG 从标识微服务请求身份验证令牌，2) 标识微服务将令牌返回到 AG，3-4) AG 使用身份验证令牌向微服务发出请求。 由于 eShopOnContainers 应用程序已将 API 网关拆分为多个 BFF（用于前端的后端）和业务范围 API 网关，因此另一种选择是为横切关注点创建其他 API 网关。 对于基于更复杂的微服务且具有多个横切关注点微服务的架构，这种选择更加合理。 由于 eShopOnContainers 中只有一个横切关注点，为简单起见，决定仅从 API 网关领域处理安全服务。

在任何情况下，如果应用受到 API 网关级别的保护，则尝试使用任何安全的微服务时，首先会访问 Ocelot API 网关的身份验证模块。 这将重定向 HTTP 请求，访问“标识”或“身份验证”微服务以获取访问令牌，这样便可通过 access_token 访问受保护的服务。

使用身份验证在 API 网关级别保护服务的方法是在 configuration.json 的相关设置中设置 AuthenticationProviderKey。

```json
    {
      "DownstreamPathTemplate": "/api/{version}/{everything}",
      "DownstreamScheme": "http",
      "DownstreamHostAndPorts": [
        {
          "Host": "basket-api",
          "Port": 80
        }
      ],
      "UpstreamPathTemplate": "/api/{version}/b/{everything}",
      "UpstreamHttpMethod": [],
      "AuthenticationOptions": {
        "AuthenticationProviderKey": "IdentityApiKey",
        "AllowedScopes": []
      }
    }
```

Ocelot 运行时，会查看 ReRoutes AuthenticationOptions.AuthenticationProviderKey 并检查是否有使用给定密钥注册的验证提供程序。 如果没有，那么 Ocelot 将不会启动。 如果有，那么 ReRoute 将在执行时使用该提供程序。

由于通过 `authenticationProviderKey = "IdentityApiKey"` 配置了 Ocelot WebHost，只要该服务的任何请求不具备身份验证令牌，就需要进行身份验证。

```csharp
namespace OcelotApiGw
{
    public class Startup
    {
        private readonly IConfiguration _cfg;

        public Startup(IConfiguration configuration) => _cfg = configuration;

        public void ConfigureServices(IServiceCollection services)
        {
            var identityUrl = _cfg.GetValue<string>("IdentityUrl");
            var authenticationProviderKey = "IdentityApiKey";
                         //…
            services.AddAuthentication()
                .AddJwtBearer(authenticationProviderKey, x =>
                {
                    x.Authority = identityUrl;
                    x.RequireHttpsMetadata = false;
                    x.TokenValidationParameters = new Microsoft.IdentityModel.Tokens.TokenValidationParameters()
                    {
                        ValidAudiences = new[] { "orders", "basket", "locations", "marketing", "mobileshoppingagg", "webshoppingagg" }
                    };
                });
            //...
        }
    }
}
```

然后，还需要在微服务等任何要访问的资源（如以下“购物篮”微服务控制器）上通过 [Authorize] 属性设置授权。

```csharp
namespace Microsoft.eShopOnContainers.Services.Basket.API.Controllers
{
    [Route("api/v1/[controller]")]
    [Authorize]
    public class BasketController : Controller
    {
      //...
    }
}
```

ValidAudience（如“basket”）通过 Startup 类的 ConfigureServices() 中的 `AddJwtBearer()` 与每个微服务中定义的受众相关联，如下方代码所示。

```csharp
// prevent from mapping "sub" claim to nameidentifier.
JwtSecurityTokenHandler.DefaultInboundClaimTypeMap.Clear();

var identityUrl = Configuration.GetValue<string>("IdentityUrl");

services.AddAuthentication(options =>
{
    options.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;

}).AddJwtBearer(options =>
{
    options.Authority = identityUrl;
    options.RequireHttpsMetadata = false;
    options.Audience = "basket";
});
```

如果尝试使用基于 API 网关的 ReRoute URL（例如 `http://localhost:5202/api/v1/b/basket/1`）访问任何安全的微服务（如“购物篮”微服务），除非提供有效令牌，否则会出现“401 未授权”。 另一方面，如果 ReRoute URL 未经身份验证，Ocelot 将调用与其关联的任何下游方案（内部微服务 URL）。

Ocelot 的 ReRoute 层中的授权。  Ocelot 支持在进行身份验证后评估的基于声明的授权。 通过将以下行添加到 ReRoute 配置中，可以在路由级别设置授权。

```json
"RouteClaimsRequirement": {
    "UserType": "employee"
}
```

在该示例中，调用授权中间件时，Ocelot 将查找用户在令牌中是否具有声明类型“UserType”并且该声明的值是否为“employee”。 如果不是，则不向用户授权，并且响应为 403 禁止访问。

## <a name="using-kubernetes-ingress-plus-ocelot-api-gateways"></a>使用 Kubernetes 入口和 Ocelot API 网关

使用 Kubernetes 时（如在 Azure Kubernetes 服务群集中），通常会根据 Nginx 通过 [Kubernetes 入口层](https://kubernetes.io/docs/concepts/services-networking/ingress/)统一所有 HTTP 请求。

在 Kubernetes 中，如果不使用任何入口方法，那么服务和 Pod 的 IP 只能由群集网络路由。

但是，如果使用入口方法，则 Internet 和服务（包括 API 网关）间会有一个中间层，充当反向代理。

入口为规则的集合，允许入站连接到达群集服务。 入口通常配置为提供外部可访问的 URL、负载均衡流量、SSL 终止等服务。 用户通过将入口资源发布到 API 服务器来请求入口。

在 eShopOnContainers 中，在本地进行开发并仅使用开发计算机作为 Docker 主机时，不会使用任何入口，只使用多个 API 网关。

但是，当面向基于 Kubernetes 的“生产”环境时，eShopOnContainers 会在 API 网关前使用入口。 这样一来，客户端仍可调用相同基 URL，但请求会路由到多个 API 网关或 BFF。

API 网关只是呈现服务的前端或外观，Web 应用通常不在其呈现范围内。 此外，API 网关可能会隐藏某些内部微服务。

然而，入口只重定向 HTTP 请求，而不会试图隐藏任何微服务或 Web 应用。

在 Web 应用程序前的 Kubernetes 中加入入口 Nginx 层和几个 Ocelot API 网关/BFF 是理想的体系结构，如下图所示。

![显示入口层如何适合 AKS 环境的图示。](./media/implement-api-gateways-with-ocelot/eshoponcontainer-ingress-tier.png)

图 6-41。 部署到 Kubernetes 时 eShopOnContainers 中的入口层

Kubernetes 入口充当流向应用的所有流量的反向代理，包括通常在 Api 网关范围之外的 Web 应用程序。 将 eShopOnContainers 部署到 Kubernetes 时，它只通过入口公开一些服务或终结点，基本上是以下列出的 URL 上的后缀：

- `/` 用于客户端 SPA Web 应用程序
- `/webmvc` 用于客户端 MVC Web 应用程序
- `/webstatus` 用于显示 status/healthchecks 的客户端 Web 应用
- `/webshoppingapigw` 用于 Web BFF 和购物业务流程
- `/webmarketingapigw` 用于 Web BFF 和营销业务流程
- `/mobileshoppingapigw` 用于移动 BFF 和购物业务流程
- `/mobilemarketingapigw` 用于移动 BFF 和营销业务流程

部署到 Kubernetes 时，每个 Ocelot API 网关为运行 API 网关的每个 pod 使用不同的“configuration.json”文件。 这些“configuration.json”文件是通过装载（最初使用 deploy.ps1 脚本）卷提供的，该卷是基于名为“ocelot”的 Kubernetes 配置映射创建的。 每个容器将其相关配置文件装载到名为 `/app/configuration` 的容器文件夹中。

在 eShopOnContainers 的源代码文件中，可在 `k8s/ocelot/` 文件夹中找到原始的“configuration.json”文件。 每个 BFF/APIGateway 都有一个文件。

## <a name="additional-cross-cutting-features-in-an-ocelot-api-gateway"></a>Ocelot API 网关中的其他横切功能

使用 Ocelot API 网关时，还有其他重要的功能等待你来研究和使用，如以下链接所述。

- 客户端中集成 Ocelot 与 Consul 或 Eureka 的服务发现 \
  <https://ocelot.readthedocs.io/en/latest/features/servicediscovery.html>

- API 网关层中的缓存 \
  <https://ocelot.readthedocs.io/en/latest/features/caching.html>

- API 网关层中的日志记录 \
  <https://ocelot.readthedocs.io/en/latest/features/logging.html>

- API 网关层中的服务质量（重试次数和断路器） \
  <https://ocelot.readthedocs.io/en/latest/features/qualityofservice.html>

- 速率限制 \
  [https://ocelot.readthedocs.io/en/latest/features/ratelimiting.html](https://ocelot.readthedocs.io/en/latest/features/ratelimiting.html )

> [!div class="step-by-step"]
> [上一页](background-tasks-with-ihostedservice.md)
> [下一页](../microservice-ddd-cqrs-patterns/index.md)
