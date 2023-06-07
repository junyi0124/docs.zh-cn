---
title: 使用 IHttpClientFactory 实现复原 HTTP 请求
description: 了解如何使用自 .NET Core 2.1 起可用的 IHttpClientFactory 来创建 `HttpClient` 实例，使其更轻松地在应用程序中使用。
ms.date: 08/31/2020
ms.openlocfilehash: 4ebb82395dd685d30846b3549b654abf7c41d43f
ms.sourcegitcommit: 636af37170ae75a11c4f7d1ecd770820e7dfe7bd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/07/2020
ms.locfileid: "91804804"
---
# <a name="use-ihttpclientfactory-to-implement-resilient-http-requests"></a>使用 IHttpClientFactory 实现复原 HTTP 请求

<xref:System.Net.Http.IHttpClientFactory> 是由自 .NET Core 2.1 起可用的固定工厂 `DefaultHttpClientFactory` 实现的协定，用于创建在应用程序中使用的 <xref:System.Net.Http.HttpClient> 实例。

## <a name="issues-with-the-original-httpclient-class-available-in-net-core"></a>.NET Core 中提供的原始 HttpClient 类的相关问题

常见的原始 <xref:System.Net.Http.HttpClient> 类非常易于使用，但在某些情况下，许多开发人员却并未正确使用该类。

虽然此类实现 `IDisposable`，但在 `using` 语句中声明和实例化它并非首选操作，因为释放 `HttpClient` 对象时，基础套接字不会立即释放，这可能会导致套接字耗尽问题。 有关此问题的详细信息，请参阅[你正在以错误方式使用 HttpClient，这将导致软件受损](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/)的博客文章。

因此，`HttpClient` 应进行一次实例化并在应用程序的生命周期中重复使用。 在负载较重的情况下，实例化每个请求的 `HttpClient` 类将耗尽可用的套接字数。 该问题会导致 `SocketException` 错误。 要解决此问题，可能的方法是将 `HttpClient` 对象创建为单一对象或静态对象，请参阅[关于 HttpClient 用法的 Microsoft 文章](../../../csharp/tutorials/console-webapiclient.md)中的说明。 对于生存期较短的控制台应用或一天运行几次的类似应用，这可能是一个不错的解决方案。

在长期运行的进程中使用 `HttpClient` 的共享实例时，开发人员会遇到另一个问题。 在将 HttpClient 实例化为单一实例或静态对象的情况下，它无法处理 DNS 更改，如 dotnet/runtime GitHub 存储库的此[问题](https://github.com/dotnet/runtime/issues/18348)中所述。

但是，问题实际上不是 `HttpClient` 本身，而是 [HttpClient 的默认构造函数](/dotnet/api/system.net.http.httpclient.-ctor?view=netcore-3.1#System_Net_Http_HttpClient__ctor)，因为它创建了一个新的实际 <xref:System.Net.Http.HttpMessageHandler> 实例，该实例具有上面提到的“套接字耗尽”和 DNS 更改问题  。

若要解决上述问题并使 `HttpClient` 实例可管理，.NET Core 2.1 引入了 <xref:System.Net.Http.IHttpClientFactory> 接口，该接口可用于在应用中通过依赖关系注入 (DI) 来配置和创建 `HttpClient` 实例。 它还提供基于 Polly 的中间件的扩展，以利用 HttpClient 中的委托处理程序。

[Polly](http://www.thepollyproject.org/) 是瞬态故障处理库，它可以通过流畅且线程安全的方式使用一些预定义的策略，帮助开发人员为其应用程序增加弹性。

## <a name="benefits-of-using-ihttpclientfactory"></a>使用 IHttpClientFactory 的好处

同时实现 <xref:System.Net.Http.IHttpMessageHandlerFactory> 的 <xref:System.Net.Http.IHttpClientFactory> 当前实现具有以下优势：

- 提供一个中心位置，用于命名和配置逻辑 `HttpClient` 对象。 例如，可以配置预配置的客户端（服务代理）以访问特定微服务。
- 通过后列方式整理出站中间件的概念：在 `HttpClient` 中委托处理程序并实现基于 Polly 的中间件以利用 Polly 的复原策略。
- `HttpClient` 已经具有委托处理程序的概念，这些委托处理程序可以链接在一起，处理出站 HTTP 请求。 将 HTTP 客户端注册到工厂后，可使用一个 Polly 处理程序将 Polly 策略用于重试、断路器等。
- 管理 <xref:System.Net.Http.HttpMessageHandler> 的生存期，避免在自行管理 `HttpClient` 生存期时出现上述问题。

> [!TIP]
> 由于关联的 `HttpMessageHandler` 由工厂管理，因此可安全释放由 DI 注入的 `HttpClient` 实例。 事实上，注入的 `HttpClient` 实例是从 DI 角度区分范围的  。

> [!NOTE]
> `IHttpClientFactory` (`DefaultHttpClientFactory`) 实现与 `Microsoft.Extensions.DependencyInjection` NuGet 包中的 DI 实现紧密关联。 有关使用其他 DI 容器的详细信息，请参阅此 [GitHub 讨论](https://github.com/dotnet/extensions/issues/1345)。

## <a name="multiple-ways-to-use-ihttpclientfactory"></a>IHttpClientFactory 的多种用法

可以通过多种方法在应用程序中使用 `IHttpClientFactory`：

- 基本用法
- 使用命名客户端
- 使用类型化客户端
- 使用生成的客户端

为简洁起见，本指南介绍了使用 `IHttpClientFactory` 的最结构化的方法，即使用类型化客户端（服务代理模式）。 不过，所有选项均已记录，并且当前在此[涵盖 `IHttpClientFactory` 用法的文章](/aspnet/core/fundamentals/http-requests#consumption-patterns)中列出。

## <a name="how-to-use-typed-clients-with-ihttpclientfactory"></a>如何结合使用类型化客户端和 IHttpClientFactory

那么，什么是“类型化客户端”？ 它只是为某些特定用途预配置的 `HttpClient`。 此配置可以包括特定值，如基本服务器、HTTP 标头或超时。

下图显示了如何将类型化客户端与 `IHttpClientFactory` 结合使用：

![展示如何将类型化客户端与 IHttpClientFactory 结合使用的图表。](./media/use-httpclientfactory-to-implement-resilient-http-requests/client-application-code.png)

**图 8-4**。 结合使用 `IHttpClientFactory` 和类型化客户端类。

在上图中，`ClientService`（由控制器或客户端代码使用）使用由注册的 `IHttpClientFactory` 创建的 `HttpClient`。 此工厂将池的 `HttpMessageHandler` 分配给 `HttpClient`。 当使用扩展方法 <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient%2A> 在 DI 容器中注册 `IHttpClientFactory` 时，可以使用 Polly 策略配置 `HttpClient`。

要配置上述结构，请通过安装包含 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection> 的 <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient%2A> 扩展方法的 `Microsoft.Extensions.Http` NuGet 包，在应用程序中添加 <xref:System.Net.Http.IHttpClientFactory>。 此扩展方法用于注册内部 `DefaultHttpClientFactory` 类，后者用作接口 `IHttpClientFactory` 的单一实例。 它定义 <xref:Microsoft.Extensions.Http.HttpMessageHandlerBuilder> 的临时配置。 此消息处理程序（<xref:System.Net.Http.HttpMessageHandler> 对象）获取自池，可供从工厂返回的 `HttpClient` 使用。

在下一个代码中，可以看到如何使用 `AddHttpClient()` 注册需要使用 `HttpClient` 的类型化客户端（服务代理）。

```csharp
// Startup.cs
//Add http client services at ConfigureServices(IServiceCollection services)
services.AddHttpClient<ICatalogService, CatalogService>();
services.AddHttpClient<IBasketService, BasketService>();
services.AddHttpClient<IOrderingService, OrderingService>();
```

按先前代码中所示注册客户端服务，使 `DefaultClientFactory` 为每个服务创建一个标准 `HttpClient`。

还可以在注册中添加特定于实例的配置（例如，配置基址），并添加一些弹性策略，如以下代码所示：

```csharp
services.AddHttpClient<ICatalogService, CatalogService>(client =>
{
    client.BaseAddress = new Uri(Configuration["BaseUrl"]);
})
    .AddPolicyHandler(GetRetryPolicy())
    .AddPolicyHandler(GetCircuitBreakerPolicy());
```

就本示例而言，可以在下一个代码中看到以上策略之一：

```csharp
static IAsyncPolicy<HttpResponseMessage> GetRetryPolicy()
{
    return HttpPolicyExtensions
        .HandleTransientHttpError()
        .OrResult(msg => msg.StatusCode == System.Net.HttpStatusCode.NotFound)
        .WaitAndRetryAsync(6, retryAttempt => TimeSpan.FromSeconds(Math.Pow(2, retryAttempt)));
}
```

可以在[下一篇文章](implement-http-call-retries-exponential-backoff-polly.md)中找到有关使用 Polly 的更多详细信息。

### <a name="httpclient-lifetimes"></a>HttpClient 生存期

每次从 `IHttpClientFactory` 获取 `HttpClient` 对象时，都会返回一个新实例。 但每个 `HttpClient` 使用 `IHttpClientFactory` 汇集和重用的 `HttpMessageHandler`，以减少资源消耗，只要 `HttpMessageHandler` 的生存期尚未过期。

由于每个处理程序通常都管理自己的基础 HTTP 连接，所以有必要汇集处理程序；创建的处理程序数量如果多于必需的数量，则可能导致连接延迟。 部分处理程序还保持连接无期限地打开，这样可以防止处理程序对 DNS 更改作出反应。

池中的 `HttpMessageHandler` 对象的生存期就是池中的 `HttpMessageHandler` 实例可重用的时间长度。 默认值为两分钟，但可基于每个类型化客户端重写此值。 要重写该值，请在创建客户端时在返回的 <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder> 上调用 `SetHandlerLifetime()`，如以下代码所示：

```csharp
//Set 5 min as the lifetime for the HttpMessageHandler objects in the pool used for the Catalog Typed Client
services.AddHttpClient<ICatalogService, CatalogService>()
    .SetHandlerLifetime(TimeSpan.FromMinutes(5));
```

每个类型化客户端都可自行配置处理程序生存期值。 将生存期设置为 `InfiniteTimeSpan` 可禁用处理程序到期。

### <a name="implement-your-typed-client-classes-that-use-the-injected-and-configured-httpclient"></a>实现使用注入的和配置的 HttpClient 的类型化客户端类

在上一步中，需要定义类型化客户端类，如示例代码中的类，“BasketService”、“CatalogService”、“OrderingService”等。类型化客户端是一个类，它接受 `HttpClient` 对象（通过其构造函数注入），并用它来调用某些远程 HTTP 服务。 例如：

```csharp
public class CatalogService : ICatalogService
{
    private readonly HttpClient _httpClient;
    private readonly string _remoteServiceBaseUrl;

    public CatalogService(HttpClient httpClient)
    {
        _httpClient = httpClient;
    }

    public async Task<Catalog> GetCatalogItems(int page, int take,
                                               int? brand, int? type)
    {
        var uri = API.Catalog.GetAllCatalogItems(_remoteServiceBaseUrl,
                                                 page, take, brand, type);

        var responseString = await _httpClient.GetStringAsync(uri);

        var catalog = JsonConvert.DeserializeObject<Catalog>(responseString);
        return catalog;
    }
}
```

Typed Client（在本例中为 `CatalogService`）由 DI（依赖项注入）激活，这意味着除 `HttpClient` 外，它还可接受其构造函数中的任何注册服务。

Typed Client 实际上是一个临时对象，这意味着每当需要实例时，就会创建一个新的实例。 它会在每次构造时接收一个新的 `HttpClient` 实例。 但是，池中的 `HttpMessageHandler` 对象是由多个 `HttpClient` 实例重复使用的对象。

### <a name="use-your-typed-client-classes"></a>使用类型化客户端类

最后，在实现了类型化类后，可使用 `AddHttpClient()` 来注册和配置它们。 之后，可在 DI 注入服务的任何位置使用它们。 例如，在 Razor 页面代码或 MVC Web 应用的控制器中，如 eShopOnContainers 的以下代码中：

```csharp
namespace Microsoft.eShopOnContainers.WebMVC.Controllers
{
    public class CatalogController : Controller
    {
        private ICatalogService _catalogSvc;

        public CatalogController(ICatalogService catalogSvc) =>
                                                           _catalogSvc = catalogSvc;

        public async Task<IActionResult> Index(int? BrandFilterApplied,
                                               int? TypesFilterApplied,
                                               int? page,
                                               [FromQuery]string errorMsg)
        {
            var itemsPage = 10;
            var catalog = await _catalogSvc.GetCatalogItems(page ?? 0,
                                                            itemsPage,
                                                            BrandFilterApplied,
                                                            TypesFilterApplied);
            //… Additional code
        }

        }
}
```

到目前为止，以上代码段只显示了执行常规 HTTP 请求的示例。 但以下部分会展示“神奇之处”，它将介绍由 `HttpClient` 发出的所有 HTTP 请求如何具有可复原策略，例如带指数退避的重试、断路器、使用身份验证令牌的安全功能，甚至其他任何自定义功能。 所有这些都可通过向已注册的 Typed Client 添加策略和委派处理程序来完成。

## <a name="additional-resources"></a>其他资源

- 在 .NET Core 中使用 HttpClientFactory   
  [https://docs.microsoft.com/aspnet/core/fundamentals/http-requests](/aspnet/core/fundamentals/http-requests)

- `dotnet/extensions` GitHub 存储库中的 HttpClientFactory 源代码   
  <https://github.com/dotnet/extensions/tree/v3.1.8/src/HttpClientFactory>

- **Polly（.NET 的恢复和暂时性故障处理库）**  
  <http://www.thepollyproject.org/>
  
- 使用无依赖项注入的 IHttpClientFactory（GitHub 问题）   
  <https://github.com/dotnet/extensions/issues/1345>

>[!div class="step-by-step"]
>[上一页](implement-resilient-entity-framework-core-sql-connections.md)
>[下一页](implement-http-call-retries-exponential-backoff-polly.md)
