---
title: 创建简单的数据驱动 CRUD 微服务
description: 适用于容器化 .NET 应用程序的 .NET 微服务体系结构 | 了解如何在微服务应用程序的上下文中创建简单的 CRUD（数据驱动）微服务。
ms.date: 08/14/2020
ms.openlocfilehash: 27c9b331573ff08ea16c756552818df285156282
ms.sourcegitcommit: ecd9e9bb2225eb76f819722ea8b24988fe46f34c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2020
ms.locfileid: "96739864"
---
# <a name="creating-a-simple-data-driven-crud-microservice"></a>创建简单的数据驱动 CRUD 微服务

本部分概述如何创建一个简单的微服务，用于对数据源执行创建、读取、更新和删除 (CRUD) 操作。

## <a name="designing-a-simple-crud-microservice"></a>设计简单的 CRUD 微服务

从设计的角度来看，这种类型的容器化微服务是非常简单的。 这有可能是由于要解决的问题很简单，也可能是由于其实现只是一个概念证明。

![显示简单 CRUD 微服务内部设计模式的关系图。](./media/data-driven-crud-microservice/internal-design-simple-crud-microservices.png)

图 6-4  。 简单 CRUD 微服务的内部设计

这种由数据驱动的简单服务的一个例子是 eShopOnContainers 示例应用程序的目录微服务。 此类型的服务在单个 ASP.NET Core Web API 项目中实现其全部功能，该项目包含用于其数据模型、业务逻辑和数据访问代码的类。 它还将其相关数据存储在 SQL Server 上运行的数据库（作为另一个用于开发/测试的容器）中，但也可以存储在任何常规 SQL Server 主机上，如图 6-5 中所示。

![显示数据驱动/CRUD 微服务容器的关系图。](./media/data-driven-crud-microservice/simple-data-driven-crud-microservice.png)

图 6-5  。 简单的数据驱动 CRUD 微服务设计

上一个关系图显示了逻辑 Catalog 微服务包括其 Catalog 数据库，该数据库无论是否与其位于同一 Docker 主机均可。 若该数据库位于同一 Docker 主机，可能更利于开发，但不利于生产。 开发这种类型的服务时，只需要 [ASP.NET Core](/aspnet/core/) 和数据访问 API 或 ORM（如 [Entity Framework Core](/ef/core/index)）。 还可以通过 [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) 自动生成 [Swagger](https://swagger.io/) 元数据，用于提供有关服务功能的说明，如下一部分中所述。

请注意，在 Docker 容器中运行 SQL Server 这样的数据库服务器十分适用于开发环境，因为可以设置和运行全部所需的依赖关系，而无需在云中或本地预配数据库。 这在运行集成测试时十分方便。 但是对于生产环境，则不建议在容器中运行数据库服务器，因为这种方法通常无法实现高可用性。 对于 Azure 中的生产环境，建议使用 Azure SQL 数据库或任何其他可提供高可用性和高扩展性的数据库技术。 例如，对于 NoSQL 方法，可选择 CosmosDB。

最后，通过编辑 Dockerfile 和 docker-compose.yml 元数据文件，可配置此容器的映像的创建方式—它使用哪种基映像，以及设计内部和外部名称及 TCP 端口等设置。

## <a name="implementing-a-simple-crud-microservice-with-aspnet-core"></a>使用 ASP.NET Core 实现简单 CRUD 微服务

要使用 .NET Core 和 Visual Studio 实现简单 CRUD 微服务，需先创建一个简单的 ASP.NET Core Web API 项目（在 .NET Core 上运行以便可在 Linux Docker 主机上运行），如图 6 6 所示。

![显示项目设置的 Visual studio 的屏幕截图。](./media/data-driven-crud-microservice/create-asp-net-core-web-api-project.png)

图 6-6  。 在 Visual Studio 2019 中创建 ASP.NET Core Web API 项目

要创建 ASP.NET Core Web API 项目，首先需选择 ASP.NET Core Web 应用程序，然后选择 API 类型。 创建该项目之后，便可使用 Entity Framework API 或其他 API 实现 MVC 控制器，与任何其他 Web API 项目中的操作一样。 在新的 Web API 项目中，可以看到该微服务中的唯一依赖关系在 ASP.NET Core 本身上。 在内部，Microsoft.AspNetCore.All 依赖项内引用的是实体框架和许多其他 .NET Core NuGet 包，如图 6-7 所示  。

![VS 显示 Catalog.Api 的 NuGet 依赖项的屏幕截图。](./media/data-driven-crud-microservice/simple-crud-web-api-microservice-dependencies.png)

图 6-7  。 简单 CRUD Web API 微服务中的依赖关系

API 项目包括对 Microsoft.AspNetCore.App NuGet 包的引用，其中包括对所有基本包的引用。 它也可能包含其他一些包。

### <a name="implementing-crud-web-api-services-with-entity-framework-core"></a>使用 Entity Framework Core 实现 CRUD Web API 服务

Entity Framework (EF) Core 是轻量化、可扩展和跨平台版的常用 Entity Framework 数据访问技术。 EF Core 是一种支持 .NET 开发人员使用 .NET 对象处理数据库的对象关系映射程序 (ORM)。

目录微服务使用 EF 和 SQL Server 提供程序，因为其数据库在具有 SQL Server for Linux Docker 映像的容器中运行。 但也可将数据库部署到任何其他 SQL Server 中，如本地 Windows 或 Azure SQL DB。 唯一需要更改的是 ASP.NET Web API 微服务中的连接字符串。

#### <a name="the-data-model"></a>数据模型

使用 EF Core 时，数据访问是通过使用模型来执行的。 模型由（域模型）实体类和表示与数据库的会话的派生上下文 (DbContext) 组成，用于查询和保存数据。 可从现有数据库生成模型，手动编码模型使之与数据库相匹配，或采用代码优先方法（通过此方法可在模型随时间推移发生更改后轻松地对于数据库进行相应改进），使用 EF 迁移技术基于模型创建数据库。 对于目录微服务，已使用后一种方法。 可在以下代码示例中看到 CatalogItem 实体类的示例，这是一个简单的普通旧 CLR 对象 ([POCO](https://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) 实体类。

```csharp
public class CatalogItem
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Description { get; set; }
    public decimal Price { get; set; }
    public string PictureFileName { get; set; }
    public string PictureUri { get; set; }
    public int CatalogTypeId { get; set; }
    public CatalogType CatalogType { get; set; }
    public int CatalogBrandId { get; set; }
    public CatalogBrand CatalogBrand { get; set; }
    public int AvailableStock { get; set; }
    public int RestockThreshold { get; set; }
    public int MaxStockThreshold { get; set; }

    public bool OnReorder { get; set; }
    public CatalogItem() { }

    // Additional code ...
}
```

还需要一个 DbContext 来表示与数据库的会话。 对于目录微服务，CatalogContext 类派生自 DbContext 基类，如下例中所示：

```csharp
public class CatalogContext : DbContext
{
    public CatalogContext(DbContextOptions<CatalogContext> options) : base(options)
    { }
    public DbSet<CatalogItem> CatalogItems { get; set; }
    public DbSet<CatalogBrand> CatalogBrands { get; set; }
    public DbSet<CatalogType> CatalogTypes { get; set; }

    // Additional code ...
}
```

可具有更多 `DbContext` 实现。 例如，在示例 Catalog.API 微服务中，还有一个名为 `CatalogContextSeed` 的 `DbContext`，它会在首次尝试访问数据库时自动填充示例数据。 此方法对于演示数据以及自动化测试方案很有用。

在 `DbContext` 中，使用 `OnModelCreating` 方法来自定义对象/数据库实体映射和其他[EF 扩展点](https://devblogs.microsoft.com/dotnet/implementing-seeding-custom-conventions-and-interceptors-in-ef-core-1-0/)。

##### <a name="querying-data-from-web-api-controllers"></a>从 Web API 控制器查询数据

通常使用语言集成查询 (LINQ) 从数据库检索实体类的实例，如下例所示：

```csharp
[Route("api/v1/[controller]")]
public class CatalogController : ControllerBase
{
    private readonly CatalogContext _catalogContext;
    private readonly CatalogSettings _settings;
    private readonly ICatalogIntegrationEventService _catalogIntegrationEventService;

    public CatalogController(
        CatalogContext context,
        IOptionsSnapshot<CatalogSettings> settings,
        ICatalogIntegrationEventService catalogIntegrationEventService)
    {
        _catalogContext = context ?? throw new ArgumentNullException(nameof(context));
        _catalogIntegrationEventService = catalogIntegrationEventService
            ?? throw new ArgumentNullException(nameof(catalogIntegrationEventService));

        _settings = settings.Value;
        context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;
    }

    // GET api/v1/[controller]/items[?pageSize=3&pageIndex=10]
    [HttpGet]
    [Route("items")]
    [ProducesResponseType(typeof(PaginatedItemsViewModel<CatalogItem>), (int)HttpStatusCode.OK)]
    [ProducesResponseType(typeof(IEnumerable<CatalogItem>), (int)HttpStatusCode.OK)]
    [ProducesResponseType((int)HttpStatusCode.BadRequest)]
    public async Task<IActionResult> ItemsAsync(
        [FromQuery]int pageSize = 10,
        [FromQuery]int pageIndex = 0,
        string ids = null)
    {
        if (!string.IsNullOrEmpty(ids))
        {
            var items = await GetItemsByIdsAsync(ids);

            if (!items.Any())
            {
                return BadRequest("ids value invalid. Must be comma-separated list of numbers");
            }

            return Ok(items);
        }

        var totalItems = await _catalogContext.CatalogItems
            .LongCountAsync();

        var itemsOnPage = await _catalogContext.CatalogItems
            .OrderBy(c => c.Name)
            .Skip(pageSize * pageIndex)
            .Take(pageSize)
            .ToListAsync();

        itemsOnPage = ChangeUriPlaceholder(itemsOnPage);

        var model = new PaginatedItemsViewModel<CatalogItem>(
            pageIndex, pageSize, totalItems, itemsOnPage);

        return Ok(model);
    }
    //...
}
```

##### <a name="saving-data"></a>保存数据

使用实体类的实例在数据库中创建、删除和修改数据。 可向 Web API 控制器添加代码，如以下硬编码示例（在此例中为模拟数据）所示。

```csharp
var catalogItem = new CatalogItem() {CatalogTypeId=2, CatalogBrandId=2,
                                     Name="Roslyn T-Shirt", Price = 12};
_context.Catalog.Add(catalogItem);
_context.SaveChanges();
```

##### <a name="dependency-injection-in-aspnet-core-and-web-api-controllers"></a>ASP.NET Core 和 Web API 控制器中的依赖注入

在 ASP.NET Core 中可非常方便地使用依赖关系注入 (DI)。 无需设置第三方控制反转 (IoC) 容器，但如果愿意，可将喜欢的 IoC 容器插入 ASP.NET Core 基础结构。 在本例中，这意味着可通过控制器构造函数直接插入所需的 EF DBContext 或其他存储库。

在前面提到的 `CatalogController` 类中，`CatalogContext`（继承自 `DbContext`）类型与 `CatalogController()` 构造函数中的其他必需对象一起注入。

需在 Web API 项目中设置一个重要配置，即向服务的 IoC 容器注册 DbContext 类。 通常在 `Startup` 类中执行此操作，方法是在 `ConfigureServices()` 方法中调用 `services.AddDbContext<CatalogContext>()` 方法，如以下简化的示例中所示  ：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Additional code...
    services.AddDbContext<CatalogContext>(options =>
    {
        options.UseSqlServer(Configuration["ConnectionString"],
        sqlServerOptionsAction: sqlOptions =>
        {
            sqlOptions.MigrationsAssembly(
                typeof(Startup).GetTypeInfo().Assembly.GetName().Name);

           //Configuring Connection Resiliency:
           sqlOptions.
               EnableRetryOnFailure(maxRetryCount: 5,
               maxRetryDelay: TimeSpan.FromSeconds(30),
               errorNumbersToAdd: null);

       });

       // Changing default behavior when client evaluation occurs to throw.
       // Default in EFCore would be to log warning when client evaluation is done.
       options.ConfigureWarnings(warnings => warnings.Throw(
           RelationalEventId.QueryClientEvaluationWarning));
   });

  //...
}
```

### <a name="additional-resources"></a>其他资源

- **查询数据** \
  [https://docs.microsoft.com/ef/core/querying/index](/ef/core/querying/index)

- **保存数据** \
  [https://docs.microsoft.com/ef/core/saving/index](/ef/core/saving/index)

## <a name="the-db-connection-string-and-environment-variables-used-by-docker-containers"></a>Docker 容器使用的数据库连接字符串和环境变量

可使用 ASP.NET Core 设置并向 settings.json 文件添加 ConnectionString 属性，如下例中所示：

```json
{
    "ConnectionString": "Server=tcp:127.0.0.1,5433;Initial Catalog=Microsoft.eShopOnContainers.Services.CatalogDb;User Id=sa;Password=[PLACEHOLDER]",
    "ExternalCatalogBaseUrl": "http://localhost:5101",
    "Logging": {
        "IncludeScopes": false,
        "LogLevel": {
            "Default": "Debug",
            "System": "Information",
            "Microsoft": "Information"
        }
    }
}
```

Settings.json 文件中可为 ConnectionString 属性或任何其他属性设置默认值。 但使用 Docker 时，这些属性会被 docker-compose.override.yml 文件中指定的环境变量的值替代。

在 docker-compose.yml 或 docker-compose.override.yml 文件中，可初始化这些环境变量，以便 Docker 将其设置为 OS 环境变量，如以下 docker-compose.override.yml 文件中所示（本示例中连接字符串和其他行会换行，但在你自己的文件中不换行）。

```yml
# docker-compose.override.yml

#
catalog-api:
  environment:
    - ConnectionString=Server=sqldata;Database=Microsoft.eShopOnContainers.Services.CatalogDb;User Id=sa;Password=[PLACEHOLDER]
    # Additional environment variables for this service
  ports:
    - "5101:80"
```

相较于项目或微服务级别的配置文件，解决方法级别的 docker-compose.yml 文件不仅更灵活，而且如果使用从部署工具（如 Azure DevOps Services Docker 部署任务）设置的值替换在 docker-compose 文件中声明的环境变量，使用该文件会更安全。

最后，可使用配置 \["ConnectionString"\] 从代码中获取该值，如前面的代码示例中的 ConfigureServices 方法中所示。

但对于生产环境，可能需要寻找其他方法来存储连接字符串等机密。 使用 [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 是管理应用程序机密的绝佳方式。

Azure Key Vault 有助于存储和保护云应用程序和服务使用的加密密钥和机密。 机密是指需严格控制的任何内容（如 API 密钥、连接字符串和密码等），而使用记录、设置过期日期和管理访问 *等* 均涵盖于严控范围内。

Azure Key Vault 允许对应用程序机密的使用情况进行非常详尽地控制，无需让任何人知晓这些内容。 甚至可轮换机密以增强安全性，且不会对开发或操作造成中断。

必须在组织的 Active Directory 中注册应用程序，如此才可使用 Key Vault。

有关更多详细信息，请查看 *Key Vault 概念文档*。

### <a name="implementing-versioning-in-aspnet-web-apis"></a>在 ASP.NET Web API 中实现版本控制

随着业务需求的改变，可能会添加新的资源集，资源之间的关系可能会改变，资源中数据的结构也可能被修改。 更新 Web API 以适应新要求是一个相对比较简单的过程，但必须考虑这些更改会为使用 Web API 的客户端应用程序带来怎样的影响。 虽然设计和实现 Web API 的开发人员对该 API 具有完全控制，但对由远程运营的第三方组织所构建的客户端应用程序却不具有同等程度的控制。

通过版本控制，Web API 能够指示其公开的功能和资源。 客户端应用程序然后便可向特定版本的功能或资源提交请求。 有几种方法可用于实现版本控制：

- URI 版本管理

- 查询字符串版本控制

- 标头版本控制

查询字符串和 URI 版本控制实现起来最简单。 标头版本控制是一个好方法。 但标头版本控制不如 URI 版本控制简单明确。 由于 URL 版本控制最简单明晰，eShopOnContainers 示例应用程序使用 URI 版本控制。

使用 URI 版本控制时，如 eShopOnContainers 示例应用程序所示，每次修改 Web API 或更改资源的架构时，都需向每个资源的 URI 中添加版本号。 现有 URI 应维持原有工作方式，返回符合架构的资源，该架构与请求的版本匹配。

如下面的代码示例中所示，可通过在 Web API 控制器中使用 Route 属性来设置版本，让版本信息在 URI 中一目了然（在本例中为 v1）。

```csharp
[Route("api/v1/[controller]")]
public class CatalogController : ControllerBase
{
    // Implementation ...
```

此版本控制机制很简单，取决于将请求路由到相应终结点的服务器。 但如果使用 REST，为实现更精密的版本控制和使用最佳方法，应使用超媒体并实现 [HATEOAS（将超文本用作应用状态的引擎）](/azure/architecture/best-practices/api-design#use-hateoas-to-enable-navigation-to-related-resources)。

### <a name="additional-resources"></a>其他资源

- **Scott Hanselman.ASP.NET Core RESTful Web API versioning made easy**（简化 ASP.NET Core RESTful Web API 版本控制） \
  <https://www.hanselman.com/blog/ASPNETCoreRESTfulWebAPIVersioningMadeEasy.aspx>

- **RESTful Web API 版本控制** \
  <https://docs.microsoft.com/azure/architecture/best-practices/api-design#versioning-a-restful-web-api>

- **Roy Fielding。版本控制、超媒体和 REST** \
  <https://www.infoq.com/articles/roy-fielding-on-versioning>

## <a name="generating-swagger-description-metadata-from-your-aspnet-core-web-api"></a>从 ASP.NET Core Web API 生成 Swagger 描述元数据

[Swagger](https://swagger.io/) 是一个常用开源框架，由包含大量工具的大型“生态系统”提供支持，可帮助设计、生成、存档和使用 RESTful API。 它正逐渐变为 API 描述性元数据域的标准。 应在所有类型的微服务中，无论是数据驱动的微服务还是多个高级域驱动微服务，包含 Swagger 描述性元数据（如下一部分中所述）。

Swagger 的核心是 Swagger 规范，它是 JSON 或 YAML 文件中的 API 描述性元数据。 该规范为 API 创建 RESTful 协定，并以人类和机器均可识别的格式详细描述其资源和操作，以简化开发、发现和集成。

该规范是 OpenAPI 规范 (OAS) 的基础，开发于开放、透明和协作化的社区，旨在让 RESTful 接口定义的方式实现标准化。

该规范在如何发现服务以及如何理解其功能方面对其结构进行定义。 有关详细信息，包括 Web 编辑器以及 Spotify、Uber、Slack 和 Microsoft 等公司的 Swagger 规范的示例，请访问 Swagger 站点 (<https://swagger.io>)。

### <a name="why-use-swagger"></a>为何使用 Swagger？

为 API 生成 Swagger 元数据的主要原因如下。

**为让其他产品能够自动使用和集成 API**。 许多产品和[商业工具](https://swagger.io/commercial-tools/)以及许多[库和框架](https://swagger.io/open-source-integrations/)均支持 Swagger。 Microsoft 具有高级产品和工具，可自动使用基于 Swagger 的 API，举例如下：

- [AutoRest](https://github.com/Azure/AutoRest)。 可自动生成用于调用 Swagger 的 .NET 客户端类。 可通过 CLI 使用此工具，它也与 Visual Studio 集成以便能够通过 GUI 轻松使用。

- [Microsoft Flow](https://flow.microsoft.com/)。 可自动[使用 API 并将其集成](https://flow.microsoft.com/blog/integrating-custom-api/)到高级 Microsoft Flow 工作流，且无需具备编程技能。

- [Microsoft PowerApps](https://powerapps.microsoft.com/)。 可自动从通过 [PowerApps Studio](https://powerapps.microsoft.com/build-powerapps/) 生成的 [PowerApps 移动应用](https://powerapps.microsoft.com/blog/register-and-use-custom-apis-in-powerapps/)使用 API，且无需具备编程技能。

- [Azure 应用服务逻辑应用](/azure/app-service-logic/app-service-logic-what-are-logic-apps)。 可自动[使用 API 并将其集成到 Azure 应用服务逻辑应用](/azure/app-service-logic/app-service-logic-custom-hosted-api)，且无需具备编程技能。

**能够自动生成 API 文档**。 创建大规模 RESTful API 时，如基于微服务的复杂应用程序，需要处理大量终结点，同时请求和响应负载中会使用各种不同的数据模型。 具备正确的文档和稳定可靠的 API 资源管理器（通过 Swagger 获取），是 API 成功发挥功能和开发人员成功使用它的关键所在。

Microsoft Flow、PowerApps 和 Azure 逻辑应用通过使用 Swagger 的元数据来了解如何使用和连接 API。

基于 *swagger-ui*，可通过多种方式以功能 API 帮助页面的形式自动生成针对 ASP.NET Core REST API 应用程序的 Swagger 元数据。

或许，最常用的方式是当前 [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers) 中使用的 [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle.AspNetCore)，我们将在本指南中对此进行详细介绍，但除此以外，也可选择使用 [NSwag](https://github.com/RSuter/NSwag)，它可以生成 Typescript 和 C\# API 客户端，也可基于 Swagger 或 OpenAPI 规范生成 C\# 控制器，甚至可使用 [NSwagStudio](https://github.com/RSuter/NSwag/wiki/NSwagStudio) 扫描包含这些控制器的 .dll 来生成控制器。

### <a name="how-to-automate-api-swagger-metadata-generation-with-the-swashbuckle-nuget-package"></a>如何使用 Swashbuckle NuGet 包自动生成 API Swagger 元数据

手动（在 JSON 或 YAML 文件中）生成 Swagger 元数据可能会耗时较长且较为枯燥。 但可使用 [Swashbuckle NuGet 包](https://aka.ms/swashbuckledotnetcore)动态生成 Swagger API 元数据，让 ASP.NET Web API 服务自动发现 API。

Swashbuckle 为 ASP.NET Web API 项目自动生成 Swagger 元数据。 它支持 ASP.NET Core Web API 项目和传统 ASP.NET Web API 以及任何其他风格，如 Azure API 应用、Azure 移动应用和基于 ASP.NET 的 Azure Service Fabric 微服务等。 它还支持容器上部署的普通 Web API，就像为引用应用程序提供支持那样。

Swashbuckle 将 API 资源管理器和 Swagger 或 [swagger ui](https://github.com/swagger-api/swagger-ui) 结合起来，为 API 使用者提供更丰富的发现和文档体验。 除 Swagger 元数据生成器引擎外，Swashbuckle 还包含 swagger-ui 的嵌入版本，可在安装 Swashbuckle 后自动提供。

这意味除 API 外，又有了一个好用的发现 UI，可帮助开发人员使用 API。 它只需要少量代码和维护，因为它自动生成，这让用户能够专注于生成 API。 API 资源管理器的结果如图 6-8 所示。

![显示 eShopOContainers API 的 Swagger API 资源管理器的屏幕截图。](./media/data-driven-crud-microservice/swagger-metadata-eshoponcontainers-catalog-microservice.png)

图 6-8  。 基于 Swagger 元数据的 Swashbuckle API 资源管理器—eShopOnContainers 目录微服务

Swashbuckle 生成的 Swagger UI API 文档包括所有已发布操作。 此处，API 资源管理器不是最重要的部分。 一旦具有可在 Swagger 元数据中进行自我描述的 Web API 后，便可从基于 Swagger 的工具中无缝使用 API，这些工具包括可面向多个平台的客户端代理类代码生成器。 例如，之前提到过，[AutoRest](https://github.com/Azure/AutoRest) 可自动生成 .NET 客户端类。 同时还有 [swagger-codegen](https://github.com/swagger-api/swagger-codegen) 等其他工具可用，这些工具可用于自动生成 API 客户端库、服务器存根（stub）和文档的代码。

目前，Swashbuckle 在用于 ASP.NET Core 应用程序的高级元包 [Swashbuckle.AspNetCore](https://www.nuget.org/packages/Swashbuckle.AspNetCore) 下包含五个内部 NuGet 包。

在 Web API 项目中安装这些 NuGet 包后，需在 Startup 类中配置 Swagger，如以下简化的  代码所示：

```csharp
public class Startup
{
    public IConfigurationRoot Configuration { get; }
    // Other startup code...

    public void ConfigureServices(IServiceCollection services)
    {
        // Other ConfigureServices() code...

        // Add framework services.
        services.AddSwaggerGen(options =>
        {
            options.DescribeAllEnumsAsStrings();
            options.SwaggerDoc("v1", new OpenApiInfo
            {
                Title = "eShopOnContainers - Catalog HTTP API",
                Version = "v1",
                Description = "The Catalog Microservice HTTP API. This is a Data-Driven/CRUD microservice sample"
            });
        });

        // Other ConfigureServices() code...
    }

    public void Configure(IApplicationBuilder app,
        IHostingEnvironment env,
        ILoggerFactory loggerFactory)
    {
        // Other Configure() code...
        // ...
        app.UseSwagger()
            .UseSwaggerUI(c =>
            {
                c.SwaggerEndpoint("/swagger/v1/swagger.json", "My API V1");
            });
    }
}
```

完成此操作后，便可启动应用程序，并使用类似以下的 URL 浏览以下 Swagger JSON 和 UI 终结点：

```console
  http://<your-root-url>/swagger/v1/swagger.json

  http://<your-root-url>/swagger/
```

之前已展示由 Swashbuckle 为类似于 `http://<your-root-url>/swagger` 的 URL 生成的 UI。 图 6-9 中还展示了如何测试 API 方法。

![显示可用测试工具的 Swagger UI 的屏幕截图。](./media/data-driven-crud-microservice/swashbuckle-ui-testing.png)

图 6-9  。 Swashbuckle UI 测试目录/项目 API 方法

Swagger UI API 详细信息显示了一个响应示例，可用于执行真正的 API，这对于开发人员发现非常有用。 图 6-10 显示了当使用 [Postman](https://www.getpostman.com/) 请求 `http://<your-root-url>/swagger/v1/swagger.json` 时，从 eShopOnContainers 微服务（工具在下方使用此微服务）中生成的 Swagger JSON 元数据。

![显示 Swagger JSON 元数据的示例 Postman UI 的屏幕截图。](./media/data-driven-crud-microservice/swagger-json-metadata.png)

图 6-10  。 Swagger JSON 元数据

就是这么简单。 由于是自动生成的，向 API 中添加更多功能后，Swagger 元数据会增长。

### <a name="additional-resources"></a>其他资源

- **使用 Swagger 的 ASP.NET Web API 帮助页** \
  [https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger)

- **Swashbuckle 和 ASP.NET Core 入门** \
  [https://docs.microsoft.com/aspnet/core/tutorials/getting-started-with-swashbuckle](/aspnet/core/tutorials/getting-started-with-swashbuckle)

- **NSwag 和 ASP.NET Core 入门** \
  [https://docs.microsoft.com/aspnet/core/tutorials/getting-started-with-nswag](/aspnet/core/tutorials/getting-started-with-nswag)

> [!div class="step-by-step"]
> [上一页](microservice-application-design.md)
> [下一页](multi-container-applications-docker-compose.md)
