---
title: 实现断路器模式
description: 了解如何实现断路器模式作为 Http 重试的互补系统。
ms.date: 03/03/2020
ms.openlocfilehash: bebe0b4a622db928175f78f8d3e303d3d7adf170
ms.sourcegitcommit: e3cbf26d67f7e9286c7108a2752804050762d02d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2020
ms.locfileid: "80988880"
---
# <a name="implement-the-circuit-breaker-pattern"></a>实现断路器模式

如前文所述，需要处理某类故障，处理这类故障时，所需的时间并不固定，尝试连接远程服务或资源时就可能会发生这类故障。 处理这类故障可以提高应用程序的稳定性和复原能力。

在分布式环境中，对远程资源和服务的调用可能由于暂时性故障而失败，例如网络连接速度较慢和超时或资源响应缓慢或暂时不可用。 这些故障通常在一段时间之后会自动消失，应配备可靠强大的云应用程序，使用“重试模式”等策略来解决这些故障。

但也可能存在这种情况，由于意外事件引发故障，需要更长的时间来解决故障。 这些故障轻则导致部分连接中断，重则导致服务完全瘫痪。 在这些情况下，应用程序持续重试一个操作可能毫无意义，因为操作不可能成功。

相反，应对应用程序进行编码，使其接受已失败的操作，并相应解决故障。

如果不恰当地使用 Http 重试，可能会在自己的软件中产生拒绝服务 ([DoS](https://en.wikipedia.org/wiki/Denial-of-service_attack)) 攻击。 由于微服务出现故障或执行缓慢，多个客户端可能会反复重试失败的请求。 这会产生极高的风险，导致针对失败的服务的流量成倍增加。

因此，需要某种防御屏障，以便在不断尝试无意义时停止过多的请求。 此防御屏障正是断路器。

断路器模式与“重试模式”的目的和用途不同。 “重试模式”让应用程序能够重试某项操作，并预期该操作最终会成功。 断路器模式阻止应用程序执行很可能会失败的操作。 应用程序可以合并这两种模式。 但重试逻辑应能敏锐觉察断路器返回的任何异常，并在断路器指示故障并非暂时性故障时放弃重试尝试。

## <a name="implement-circuit-breaker-pattern-with-ihttpclientfactory-and-polly"></a>使用 `IHttpClientFactory` 和 Polly 实现断路器模式

在实现重试操作时，建议的方法为断路器使用 Polly 这种经过验证的 .NET 库以及它与 `IHttpClientFactory` 的本机集成。

在使用 `IHttpClientFactory` 时，将断路器策略添加到 `IHttpClientFactory` 传出中间件管道就像将单个增量代码片段添加到已有代码一样简单。

此处在用于 HTTP 调用重试的代码中添加的唯一内容是用于将断路器策略添加到策略列表的代码（如以下增量代码所示，该代码是 ConfigureServices() 方法的一部分）。

```csharp
//ConfigureServices()  - Startup.cs
services.AddHttpClient<IBasketService, BasketService>()
        .SetHandlerLifetime(TimeSpan.FromMinutes(5))  //Sample. Default lifetime is 2 minutes
        .AddHttpMessageHandler<HttpClientAuthorizationDelegatingHandler>()
        .AddPolicyHandler(GetRetryPolicy())
        .AddPolicyHandler(GetCircuitBreakerPolicy());
```

`AddPolicyHandler()` 方法将策略添加至将要使用的 `HttpClient` 对象。 在这种情况下，它会为断路器添加 Polly 策略。

为了获取更为模块化的方法，会在一个名为 `GetCircuitBreakerPolicy()` 的单独方法中定义断路器策略，如下列代码所示：

```csharp
static IAsyncPolicy<HttpResponseMessage> GetCircuitBreakerPolicy()
{
    return HttpPolicyExtensions
        .HandleTransientHttpError()
        .CircuitBreakerAsync(5, TimeSpan.FromSeconds(30));
}
```

上述代码示例配置了断路器策略，因此，如果在重试 Http 请求时出现五个连续故障，则会中断或断开线路。 此时，电路将断开 30 秒：在此期间，断路器会立即中止呼叫，而不是拨打电话。  该策略自动将[相关异常和 HTTP 状态代码](/aspnet/core/fundamentals/http-requests#handle-transient-faults)解释为故障。  

如果一个特定资源出现问题，且该资源部署在不同于执行 HTTP 调用的客户端应用程序或服务的环境中，则还应使用断路器将请求重定向到回退基础结构。 这样一来，如果数据中心发生故障，但该故障只影响后端微服务，而不影响客户端应用程序，则客户端应用程序可以重定向到回退服务。 Polly 正在规划新策略，用于自动实现此[故障转移策略](https://github.com/App-vNext/Polly/wiki/Polly-Roadmap#failover-policy)方案。

所有这些功能均适用于从 .NET 代码内部管理故障转移的情况，与由 Azure 自动管理的情况相反（位置透明化）。

从使用情况角度来看，在使用 HttpClient 时，无需添加新内容，因为该代码与结合使用 `HttpClient` 和 `IHttpClientFactory` 时所使用的代码相同，如之前部分所述。

## <a name="test-http-retries-and-circuit-breakers-in-eshoponcontainers"></a>在 eShopOnContainers 中测试 Http 重试和断路器

每当在 Docker 主机中启动 eShopOnContainers 解决方案时，它需要启动多个容器。 启动和初始化某些容器时会比较慢，如 SQL Server 容器。 尤其是首次在 Docker 中部署 eShopOnContainers 应用程序时，因为它需要设置映像和数据库。 某些容器的启动速度比其他容器慢，这可能导致其余服务一开始会引发 HTTP 异常，即使在 docker-compose 级别设置容器之间的依赖关系也会如此，如前面部分中所述。 容器之间的那些 docker-compose 依赖关系只存在于进程级别。 可能容器的入口点进程已启动，但 SQL Server 可能还不能响应查询。 这是一连串错误导致的结果，应用程序在尝试使用该特定容器时可能引发异常。

在应用程序部署到云时，也可能在启动时看到这种类型的错误。 在这种情况下，业务流程协调程序可能会将容器从一个节点或 VM 移动到另一个（即启动新实例），以平衡群集节点中的容器数。

在启用所有容器时，“eShopOnContainers”解决这些问题的方法是使用前文所述的重试模式。

### <a name="test-the-circuit-breaker-in-eshoponcontainers"></a>在 eShopOnContainers 中测试断路器

有几种方法可以中断/打开线路，并使用 eShopOnContainers 对其进行测试。

一种方法是在断路器策略中将允许的重试次数减少为 1 次，并将整个解决方案重新部署到 Docker。 如果设为一次重试，部署过程中 HTTP 请求失败的可能性就会很大，断路器会打开，然后收到错误。

另一种方法是使用 Basket  微服务中实现的自定义中间件。 启用此中间件后，它会捕获所有 HTTP 请求并返回状态代码 500。 可以通过向失败 URI 发出 GET 请求来启用此中间件，如下所示：

- `GET http://localhost:5103/failing`\
  此请求返回中间件的当前状态。 如果启用了中间件，则请求返回状态代码 500。 如果禁用了中间件，则无响应。

- `GET http://localhost:5103/failing?enable`\
  此请求启用中间件。

- `GET http://localhost:5103/failing?disable`\
  此请求禁用中间件。

例如，应用程序运行后，可立即通过在任何浏览器中使用以下 URI 发出请求，来启用中间件。 请注意，订单微服务使用端口 5103。

`http://localhost:5103/failing?enable`

然后可以使用 URI `http://localhost:5103/failing` 检查状态，如图 8-5 中所示。

![查看中间件模拟失败状态的屏幕截图。](./media/implement-circuit-breaker-pattern/failing-middleware-simulation.png)

**图 8-5**。 正在检查“失败”的 ASP.NET 中间件的状态 – 在此例中为禁用状态。

在这种情况下，每当调用市场篮微服务时，微服务就会使用状态代码 500 进行响应。

中间件开始运行后，便可尝试从 MVC web 应用程序下订单。 由于请求失败，线路将打开。

在下面的示例中，可以看到 MVC web 应用程序在用于下订单的逻辑中具有一个捕获块。  如果代码捕获了一个开路异常，会向用户显示一条友好消息，请用户等待。

```csharp
public class CartController : Controller
{
    //…
    public async Task<IActionResult> Index()
    {
        try
        {
            var user = _appUserParser.Parse(HttpContext.User);
            //Http requests using the Typed Client (Service Agent)
            var vm = await _basketSvc.GetBasket(user);
            return View(vm);
        }
        catch (BrokenCircuitException)
        {
            // Catches error when Basket.api is in circuit-opened mode
            HandleBrokenCircuitException();
        }
        return View();
    }

    private void HandleBrokenCircuitException()
    {
        TempData["BasketInoperativeMsg"] = "Basket Service is inoperative, please try later on. (Business message due to Circuit-Breaker)";
    }
}
```

摘要如下。 重试策略尝试数次发出 HTTP 请求，并获取 HTTP 错误。 当重试次数达到断路器策略设置的最大次数时（此例中为 5），应用程序会引发 BrokenCircuitException。 结果是一条友好消息，如图 8-6 中所示。

![出现市场篮服务无效错误的 MVC Web 应用的屏幕截图。](./media/implement-circuit-breaker-pattern/basket-service-inoperative.png)

**图 8-6**。 断路器向 UI 返回一个错误

可以实现其他逻辑来指定何时打开/中断线路。 或如果有一个回退数据中心或冗余的后端系统，可尝试对另一个后端微服务发出 HTTP 请求。

最后，针对 `CircuitBreakerPolicy` 的另一种可能操作是使用 `Isolate`（强制打开线路并保持为打开状态）和 `Reset`（再次关闭路线）。 可使用这些操作构建一个实用程序 HTTP 终结点，用于在策略上直接调用隔离和重置。  还可以在生产中以合适的安全程度使用这种 HTTP 终结点，用于临时隔离下游系统，比如在想要升级系统的时候。 或可手动打开线路，以保护疑似发生故障的下游系统。

## <a name="additional-resources"></a>其他资源

- **断路器模式**\
  [https://docs.microsoft.com/azure/architecture/patterns/circuit-breaker](/azure/architecture/patterns/circuit-breaker)

>[!div class="step-by-step"]
>[上一页](implement-http-call-retries-exponential-backoff-polly.md)
>[下一页](monitor-app-health.md)
