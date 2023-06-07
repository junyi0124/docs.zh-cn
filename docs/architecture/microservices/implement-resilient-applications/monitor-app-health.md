---
title: 运行状况监视
description: 了解实现运行状况监视的一种方法。
ms.date: 03/02/2020
ms.openlocfilehash: 3e3e8ec41de1469f0c397d8d80d224dd2f7a2bd2
ms.sourcegitcommit: 0100be20fcf23f61dab672deced70059ed71bb2e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/17/2020
ms.locfileid: "88267888"
---
# <a name="health-monitoring"></a>运行状况监视

运行状况监视可以获取有关容器和微服务状态的近实时信息。 运行状况对于微服务操作的多个方面至关重要，并且在业务流程协调程序分阶段执行部分应用程序升级时尤为重要，稍后将对此进行说明。

通常，基于微服务的应用程序借助检测信号或运行状况检查，使性能监视器、日程安排和业务流程协调程序得以对大量服务进行跟踪监测。 如果服务无法按需或按计划发送某种表示“我处于活动状态”的信号，那么部署更新时，应用程序可能面临风险，或者它可能太晚才发现故障，并且无法阻止可能会导致重大故障的级联故障。

在典型模型中，服务发送有关其状态的报告，聚合这些信息便能整体了解应用程序运行状况。 如果使用的是业务流程协调程序，则可以向业务流程协调程序的群集提供运行状况信息，以便群集可以执行相应操作。 如果使用为应用程序定制的高质量运行状况报告，则可以更轻松地检测到正在运行的应用程序存在的问题并进行修复。

## <a name="implement-health-checks-in-aspnet-core-services"></a>在 ASP.NET Core 服务中实现运行状况检查

开发 ASP.NET Core 微服务或 Web 应用程序时，可以使用 ASP .NET Core 2.2 中发布的内置运行状况检查功能 ([Microsoft.Extensions.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks))。 与许多 ASP.NET 核心功能一样，运行状况检查附带一组服务和中间件。

运行状况检查服务和中间件使用方便，并提供了一些功能，可以用于验证应用程序（如 SQL Server 数据库或远程 API）所需的任何外部资源是否正常工作。 使用该功能时，还可以确定资源正常运行的定义，稍后将会介绍。

若要有效使用此功能，需要先在微服务中配置服务。 其次，需要查询运行状况报告的前端应用程序。 该前端应用程序可能是自定义报告应用程序，也可能是可以根据运行状况状态做出相应反应的业务流程协调程序。

### <a name="use-the-healthchecks-feature-in-your-back-end-aspnet-microservices"></a>在后端 ASP.NET 微服务中使用 HealthChecks 功能

本节介绍在使用 [Microsoft.Extensions.Diagnostics.HealthChecks ](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks)包时，如何在示例 ASP.NET Core 3.1 Web API 应用程序中实现 HealthChecks 功能。 下一节介绍如何在大规模微服务（例如 eShopOnContainers）中实现此功能。

首先，需要定义每个微服务的正常运行状况的必备条件。 在该示例应用程序中，如果可通过 HTTP 访问微服务 API 并且可以使用与其相关的 SQL Server 数据库，可定义该微服务处于正常运行状态。

在 .NET Core 3.1 中，使用内置 API，可以通过以下方式配置服务、为微服务及其依赖的 SQL Server 数据库添加运行状况检查：

```csharp
// Startup.cs from .NET Core 3.1 Web API sample
//
public void ConfigureServices(IServiceCollection services)
{
    //...
    // Registers required services for health checks
    services.AddHealthChecks()
        // Add a health check for a SQL Server database
        .AddCheck(
            "OrderingDB-check",
            new SqlConnectionHealthCheck(Configuration["ConnectionString"]),
            HealthStatus.Unhealthy,
            new string[] { "orderingdb" });
}
```

在上面的代码中，`services.AddHealthChecks()` 方法配置一个基本 HTTP 检查，会返回状态代码 200 以及“正常”  。  此外，`AddCheck()` 扩展方法可配置自定义 `SqlConnectionHealthCheck`，用于检查相关 SQL 数据库的运行状况。

`AddCheck()` 方法添加具有指定名称的新运行状况检查和类型 `IHealthCheck` 的实现。 可以使用 AddCheck 方法添加多个运行状况检查，使微服务在其所有检查均获得“正常”结果后才会提供“正常”状态。

`SqlConnectionHealthCheck` 是自定义类，可实现 `IHealthCheck`，它使用连接字符串作为构造函数参数，并执行简单查询，检查与 SQL 数据库的连接是否成功。 如果查询执行成功，将返回 `HealthCheckResult.Healthy()`，如果失败，将返回包含实际异常的 `FailureStatus`。

```csharp
// Sample SQL Connection Health Check
public class SqlConnectionHealthCheck : IHealthCheck
{
    private static readonly string DefaultTestQuery = "Select 1";

    public string ConnectionString { get; }

    public string TestQuery { get; }

    public SqlConnectionHealthCheck(string connectionString)
        : this(connectionString, testQuery: DefaultTestQuery)
    {
    }

    public SqlConnectionHealthCheck(string connectionString, string testQuery)
    {
        ConnectionString = connectionString ?? throw new ArgumentNullException(nameof(connectionString));
        TestQuery = testQuery;
    }

    public async Task<HealthCheckResult> CheckHealthAsync(HealthCheckContext context, CancellationToken cancellationToken = default(CancellationToken))
    {
        using (var connection = new SqlConnection(ConnectionString))
        {
            try
            {
                await connection.OpenAsync(cancellationToken);

                if (TestQuery != null)
                {
                    var command = connection.CreateCommand();
                    command.CommandText = TestQuery;

                    await command.ExecuteNonQueryAsync(cancellationToken);
                }
            }
            catch (DbException ex)
            {
                return new HealthCheckResult(status: context.Registration.FailureStatus, exception: ex);
            }
        }

        return HealthCheckResult.Healthy();
    }
}
```

请注意，在上面的代码中，`Select 1` 是用于检查数据库运行状况的查询。 若要监视微服务的可用性，Kubernetes 等业务流程协调程序可通过发送微服务测试请求来定期执行运行状况检查。 请务必让数据库查询保持高效，以便快速执行这些操作而不会导致更高的资源使用率。

最后，添加对 URL 路径 `/hc` 进行响应的中间件：

```csharp
// Startup.cs from .NET Core 3.1 Web Api sample
//
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    //…
    app.UseEndpoints(endpoints =>
    {
        //...
        endpoints.MapHealthChecks("/hc");
        //...
    });
    //…
}
```

调用 `<yourmicroservice>/hc` 终结点时，它将运行 Startup 类的 `AddHealthChecks()` 方法中配置的所有运行状况检查，并显示结果。

### <a name="healthchecks-implementation-in-eshoponcontainers"></a>eShopOnContainers 中的 HealthChecks 实现

eShopOnContainers 中的微服务依赖多个服务来执行其任务。 例如，eShopOnContainers 中的 `Catalog.API` 微服务依赖许多服务，例如，Azure Blob 存储、SQL Server 和 RabbitMQ。 因此，它使用 `AddCheck()` 方法添加了多个运行状况检查。 对于每个依赖服务，需要添加自定义 `IHealthCheck` 实现，用于定义其各自的运行状况状态。

开放源代码项目 [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) 可通过为基于 .NET Core 3.1 构建的每个企业服务提供自定义运行状况检查实现，解决此问题。 每个运行状况检查都可作为单独的 NuGet 包，可轻松添加到项目。 eShopOnContainers 将其广泛用于其所有微服务。

例如，在 `Catalog.API` 微服务中，添加了以下 NuGet 包：

![AspNetCore.Diagnostics.HealthChecks NuGet 包的屏幕截图。](./media/monitor-app-health/aspnet-core-diagnostics-health-checks.png)

**图 8-7**。 使用 AspNetCore.Diagnostics.HealthChecks 在 Catalog.API 中实现的运行状况检查

在以下代码中，将为每项依赖服务添加运行状况检查实现，然后配置中间件：

```csharp
// Startup.cs from Catalog.api microservice
//
public static IServiceCollection AddCustomHealthCheck(this IServiceCollection services, IConfiguration configuration)
{
    var accountName = configuration.GetValue<string>("AzureStorageAccountName");
    var accountKey = configuration.GetValue<string>("AzureStorageAccountKey");

    var hcBuilder = services.AddHealthChecks();

    hcBuilder
        .AddSqlServer(
            configuration["ConnectionString"],
            name: "CatalogDB-check",
            tags: new string[] { "catalogdb" });

    if (!string.IsNullOrEmpty(accountName) && !string.IsNullOrEmpty(accountKey))
    {
        hcBuilder
            .AddAzureBlobStorage(
                $"DefaultEndpointsProtocol=https;AccountName={accountName};AccountKey={accountKey};EndpointSuffix=core.windows.net",
                name: "catalog-storage-check",
                tags: new string[] { "catalogstorage" });
    }
    if (configuration.GetValue<bool>("AzureServiceBusEnabled"))
    {
        hcBuilder
            .AddAzureServiceBusTopic(
                configuration["EventBusConnection"],
                topicName: "eshop_event_bus",
                name: "catalog-servicebus-check",
                tags: new string[] { "servicebus" });
    }
    else
    {
        hcBuilder
            .AddRabbitMQ(
                $"amqp://{configuration["EventBusConnection"]}",
                name: "catalog-rabbitmqbus-check",
                tags: new string[] { "rabbitmqbus" });
    }

    return services;
}
```

最后，添加 HealthCheck 中间件，用于侦听“/hc”终结点：

```csharp
// HealthCheck middleware
app.UseHealthChecks("/hc", new HealthCheckOptions()
{
    Predicate = _ => true,
    ResponseWriter = UIResponseWriter.WriteHealthCheckUIResponse
});
```

### <a name="query-your-microservices-to-report-about-their-health-status"></a>查询微服务，以报告其运行状况状态

按照本文所述配置好运行状况检查后，每当在 Docker 上运行该微服务，便可直接通过浏览器检查其运行状况是否正常。 必须在 Docker 主机中发布容器端口，以便通过外部 Docker 主机 IP 或 `localhost` 访问该容器，如图 8-8 所示。

![运行状况检查返回的 JSON 响应的屏幕截图。](./media/monitor-app-health/health-check-json-response.png)

**图 8-8**。 通过浏览器检查单个服务的运行状况状态

在该测试中，可以看到 `Catalog.API` 微服务（在端口 5101 上运行）处于正常运行状态，并以 JSON 形式返回 HTTP 状态 200 和状态信息。 该服务还检查了其 SQL Server 数据库依赖项和 RabbitMQ 的运行状况，以便运行状况检查报告显示其运行正常。

## <a name="use-watchdogs"></a>使用监视程序

监视程序是一项单独服务，可以监视多个服务的运行和负载状况，并借助前面介绍的 `HealthChecks` 库进行查询，从而报告微服务的运行状况。 这有助于防止某些根据单个服务视图检测不到的错误。 监视程序也是托管代码的好选择，可以在没有用户交互的情况下根据已知情况执行修正操作。

eShopOnContainers 示例包含一个网页，该网页显示了示例运行状况检查报告，如图 8-9 所示。 这是最简单的监视程序，因为它只显示 eShopOnContainers 中的微服务和 Web 应用程序的状态。 通常，监视程序在检测到不正常运行状态时还会执行相应操作。

幸运的是，[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) 还提供 [AspNetCore.HealthChecks.UI](https://www.nuget.org/packages/AspNetCore.HealthChecks.UI/) NuGet 包，可用于通过已配置的 URI 显示运行状况检查结果。

![运行状况检查 UI eShopOnContainers 运行状况状态的屏幕截图。](./media/monitor-app-health/health-check-status-ui.png)

**图 8-9**。 eShopOnContainers 中的示例运行状况检查报告

总之，此监视器服务可查询每个微服务的“/hc”终结点。 这将执行其中定义的所有运行状况检查，并根据这些检查返回总体运行状况状态。 可通过几个配置项和两行代码（需要添加到监视器服务的 Startup.cs 中）轻松使用 HealthChecksUI。

运行状况检查 UI 的示例配置文件：

```json
// Configuration
{
  "HealthChecks-UI": {
    "HealthChecks": [
      {
        "Name": "Ordering HTTP Check",
        "Uri": "http://localhost:5102/hc"
      },
      {
        "Name": "Ordering HTTP Background Check",
        "Uri": "http://localhost:5111/hc"
      },
      //...
    ]}
}
```

用于添加 HealthChecksUI 的 Startup.cs 文件：

```csharp
// Startup.cs from WebStatus(Watch Dog) service
//
public void ConfigureServices(IServiceCollection services)
{
    //…
    // Registers required services for health checks
    services.AddHealthChecksUI();
}
//…
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    //…
    app.UseHealthChecksUI(config => config.UIPath = "/hc-ui");
    //…
}
```

## <a name="health-checks-when-using-orchestrators"></a>在使用业务流程协调程序的情况下执行运行状况检查

若要监视微服务的可用性，Kubernetes 和 Service Fabric 等业务流程协调程序可通过发送微服务测试请求来定期执行运行状况检查。 若业务流程协调程序确定某服务/容器运行不正常，它会停止向该实例路由请求。 通常，它还会为该容器创建新实例。

例如，大多数业务流程协调程序可以使用运行状况检查来管理不会停机的部署。 仅当服务/容器的状态变为正常时，业务流程协调程序才会开始将流量路由到服务/容器实例。

当业务流程协调程序执行应用程序升级时，运行状况监视尤为重要。 某些业务流程协调程序（如 Azure Service Fabric）会分阶段更新服务，例如，它们可能为每个应用程序升级更新五分之一的群集面。 同时升级的一组节点称为“升级域”  。 完成每个升级域的升级操作并面向用户提供后，在部署进程移至下一升级域之前，该升级域必须通过运行状况检查。

服务运行状况的另一方面是报告服务的指标。 这是某些业务流程协调程序（如 Service Fabric）运行状况模型的高级功能。 因为要借助指标来平衡资源使用情况，所以在使用业务流程协调程序时，指标非常重要。 指标也可以是系统运行状况的指示器。 例如，某个应用程序包含多个微服务，并且每个实例都会报告每秒的请求数 (RPS) 指标。 如果某一服务使用的资源（内存、处理器等）比另一个服务多，则业务流程协调程序会在群集中移动服务实例，尝试使资源使用保持均衡状态。

请注意，Azure Service Fabric 提供自己的[运行状况监视模型](/azure/service-fabric/service-fabric-health-introduction)，该模型比简单的运行状况检查更高级。

## <a name="advanced-monitoring-visualization-analysis-and-alerts"></a>高级监视：可视化、分析和警报

监视的最后一部分是对事件流进行可视化、报告服务性能、以及在检测到问题时发出警报。 可以使用不同的解决方案来进行这方面的监视。

可以使用显示服务状态的简单自定义应用程序，例如介绍 [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) 时显示的自定义页。 或者，可以使用更高级的工具（如 [Azure Monitor](https://azure.microsoft.com/services/monitor/)）基于事件流发出警报。

最后，如果要存储所有事件流，可以使用 Microsoft Power BI 或其他解决方案（如 Kibana 或 Splunk）来可视化数据。

## <a name="additional-resources"></a>其他资源

- **适用于 ASP.NET Core 的 HealthChecks 和 HealthChecks UI** \
  <https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks>

- **Service Fabric 运行状况监视简介** \
  [https://docs.microsoft.com/azure/service-fabric/service-fabric-health-introduction](/azure/service-fabric/service-fabric-health-introduction)

- **Azure Monitor** \
  <https://azure.microsoft.com/services/monitor/>

>[!div class="step-by-step"]
>[上一页](implement-circuit-breaker-pattern.md)
>[下一页](../secure-net-microservices-web-applications/index.md)
