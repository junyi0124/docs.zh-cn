---
title: 使用 IHostedService 和 BackgroundService 类在微服务中实现后台任务
description: 用于容器化 .NET 应用程序的 .NET 微服务体系结构 | 了解使用 IHostedService 和 BackgroundService 在微服务 .NET Core 中实现后台任务的新选项。
ms.date: 08/14/2020
ms.openlocfilehash: 279f9e0093deafab51e63d72dce233c8e9466a55
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91173349"
---
# <a name="implement-background-tasks-in-microservices-with-ihostedservice-and-the-backgroundservice-class"></a>使用 IHostedService 和 BackgroundService 类在微服务中实现后台任务

任何应用程序中都可能需要使用后台任务和计划作业，无论应用程序是否遵循微服务体系结构模式均是如此。 使用微服务体系结构的区别在于，你可以在一个单独的用于托管的进程/容器中实现后台任务，以便根据需要对其纵向缩减/扩展。

一般在 .NET Core 中，我们将这些类型的任务称为托管服务  ，因为它们是托管在主机/应用程序/微服务中的服务/逻辑。 请注意，在这种情况下，托管服务仅表示具有后台任务逻辑的类。

自 .NET Core 2.0 开始，该框架提供名为 <xref:Microsoft.Extensions.Hosting.IHostedService> 的新接口，有助于轻松实现托管服务。 基本理念是，可以注册多个后台任务（托管服务），在 Web 主机或主机运行时在后台运行，如图 6-26 所示。

![比较 ASP.NET Core IWebHost 和 .NET Core IHost 的关系图。](./media/background-tasks-with-ihostedservice/ihosted-service-webhost-vs-host.png)

**图 6-26**. 在 WebHost 与主机中使用 IHostedService

ASP.NET Core 1.x 和 2.x 支持 `IWebHost`，适用于 Web 应用中的后台进程。 .NET Core 2.1 及更高版本支持 `IHost`，适用于带有纯控制台应用的后台进程。 请注意 `WebHost` 和 `Host` 之间产生的差异。

ASP.NET Core 2.0 中的 `WebHost`（实现 `IWebHost` 的基类）是用于为进程提供 HTTP 服务器功能的基础结构项目，例如，在实现 MVC Web 应用或 Web API 服务时。 它提供 ASP.NET Core 中所有新的基础结构优点，使用户能够使用依赖关系注入，在请求管道中插入中间件等。 `WebHost` 将这些相同的 `IHostedServices` 用于后台任务。

.NET Core 2.1 中引入了 `Host`（实现 `IHost` 的基类）。 基本上，`Host` 能让用户拥有与 `WebHost`（依赖项注入、托管服务等）相似的基础结构，但在这种情况下，只需拥有一个简单轻便的进程作为主机，与 MVC、Web API 或 HTTP 服务器功能无关。

因此，可以选择一个专用主机进程或使用 `IHost` 创建一个来专门处理托管服务，例如仅用于托管 `IHostedServices` 的微服务，或者也可以选择性地扩展现有的 ASP.NET Core `WebHost`，例如现有的 ASP.NET Core Web API 或 MVC 应用。

每种方法都有优缺点，具体取决于业务和可伸缩性需求。 重要的是，如果后台任务与 HTTP (`IWebHost`) 无关，则应使用 `IHost`。

## <a name="registering-hosted-services-in-your-webhost-or-host"></a>在 WebHost 或主机中注册托管服务

让我们进一步深化对 `IHostedService` 接口的了解，因为它在 `WebHost` 或 `Host` 中的使用非常相似。

SignalR 是使用托管服务的项目的一个示例，但也可以将其用于更简单的操作，如：

- 轮询数据库以查找更改的后台任务。
- 定期更新某些缓存的计划任务。
- 允许任务在后台线程上执行的 QueueBackgroundWorkItem 实现。
- 在 Web 应用后台处理消息队列中的消息，同时共享 `ILogger` 等公共服务。
- 从 `Task.Run()` 开始的后台任务。

基本上，可以将所有这些操作卸载至实现 `IHostedService` 的后台任务。

向 `WebHost` 或 `Host` 添加一个或多个 `IHostedServices` 的方式是，通过 ASP.NET Core `WebHost`（或 .NET Core 2.1 及更高版本中的 `Host`）中的 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionHostedServiceExtensions.AddHostedService%2A>  扩展方法对它们进行注册。 基本上，必须在常见的 `Startup` 类的 `ConfigureServices()` 方法中注册托管服务，如以下典型的 ASP.NET WebHost 中的代码所示。

```csharp
public IServiceProvider ConfigureServices(IServiceCollection services)
{
    //Other DI registrations;

    // Register Hosted Services
    services.AddHostedService<GracePeriodManagerService>();
    services.AddHostedService<MyHostedServiceB>();
    services.AddHostedService<MyHostedServiceC>();
    //...
}
```

在该代码中，`GracePeriodManagerService` 托管服务是来自 eShopOnContainers 中的订购业务微服务的真实代码，而另外两个只是两个额外示例。

`IHostedService` 后台任务的执行与应用程序（就此而言，为主机或微服务）的生存期相协调。 当应用程序启动时注册任务，当应用程序关闭时，有机会执行某些正常操作或清理。

始终可以启动后台线程来运行任何任务，而无需使用 `IHostedService`。 不同之处就在于应用的关闭时间，此时会直接终止线程，而没有机会执行正常的清理操作。

## <a name="the-ihostedservice-interface"></a>IHostedService 接口

当注册 `IHostedService` 时，.NET Core 会在应用程序启动和停止期间分别调用 `IHostedService` 类型的 `StartAsync()` 和 `StopAsync()` 方法。 有关详细信息，请参阅 [IHostedService 接口](/aspnet/core/fundamentals/host/hosted-services?tabs=visual-studio&view=aspnetcore-3.1#ihostedservice-interface)

如你所想，可以创建 IHostedService 的多个实现，并在 `ConfigureService()` 方法中将它们注册到 DI 容器中，如前所示。 所有这些托管服务将随应用程序/微服务一起启动和停止。

作为开发人员，当主机触发 `StopAsync()` 方法时，需负责处理服务的停止操作。

## <a name="implementing-ihostedservice-with-a-custom-hosted-service-class-deriving-from-the-backgroundservice-base-class"></a>使用从 BackgroundService 基类派生的自定义托管服务类来实现 IHostedService

可以从头开始创建自定义托管服务类并实现 `IHostedService`，因为在使用 .NET Core 2.0 时需执行这些操作。

但是，由于大多数后台任务在取消令牌管理和其他典型操作方面都有类似的需求，因此有一个非常方便且可以从中进行派生的抽象基类，名为 `BackgroundService`（自 .NET Core 2.1 起提供）。

该类提供设置后台任务所需的主要工作。

下一个代码是在 .NET Core 中实现的抽象 BackgroundService 基类。

```csharp
// Copyright (c) .NET Foundation. Licensed under the Apache License, Version 2.0.
/// <summary>
/// Base class for implementing a long running <see cref="IHostedService"/>.
/// </summary>
public abstract class BackgroundService : IHostedService, IDisposable
{
    private Task _executingTask;
    private readonly CancellationTokenSource _stoppingCts =
                                                   new CancellationTokenSource();

    protected abstract Task ExecuteAsync(CancellationToken stoppingToken);

    public virtual Task StartAsync(CancellationToken cancellationToken)
    {
        // Store the task we're executing
        _executingTask = ExecuteAsync(_stoppingCts.Token);

        // If the task is completed then return it,
        // this will bubble cancellation and failure to the caller
        if (_executingTask.IsCompleted)
        {
            return _executingTask;
        }

        // Otherwise it's running
        return Task.CompletedTask;
    }

    public virtual async Task StopAsync(CancellationToken cancellationToken)
    {
        // Stop called without start
        if (_executingTask == null)
        {
            return;
        }

        try
        {
            // Signal cancellation to the executing method
            _stoppingCts.Cancel();
        }
        finally
        {
            // Wait until the task completes or the stop token triggers
            await Task.WhenAny(_executingTask, Task.Delay(Timeout.Infinite,
                                                          cancellationToken));
        }

    }

    public virtual void Dispose()
    {
        _stoppingCts.Cancel();
    }
}
```

从上一抽象基类派生时，得益于该继承的实现，用户只需在自定义的托管服务类中实现 `ExecuteAsync()` 方法，如以下来自 eShopOnContainers 的简化代码所示，该代码轮询数据库并在需要时将集成事件发布到事件总线中。

```csharp
public class GracePeriodManagerService : BackgroundService
{
    private readonly ILogger<GracePeriodManagerService> _logger;
    private readonly OrderingBackgroundSettings _settings;

    private readonly IEventBus _eventBus;

    public GracePeriodManagerService(IOptions<OrderingBackgroundSettings> settings,
                                     IEventBus eventBus,
                                     ILogger<GracePeriodManagerService> logger)
    {
        // Constructor's parameters validations...
    }

    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        _logger.LogDebug($"GracePeriodManagerService is starting.");

        stoppingToken.Register(() =>
            _logger.LogDebug($" GracePeriod background task is stopping."));

        while (!stoppingToken.IsCancellationRequested)
        {
            _logger.LogDebug($"GracePeriod task doing background work.");

            // This eShopOnContainers method is querying a database table
            // and publishing events into the Event Bus (RabbitMQ / ServiceBus)
            CheckConfirmedGracePeriodOrders();

            await Task.Delay(_settings.CheckUpdateTime, stoppingToken);
        }

        _logger.LogDebug($"GracePeriod background task is stopping.");
    }

    .../...
}
```

在 eShopOnContainers 的此特定情况下，它正在执行一个应用程序方法，该方法查询数据库表以查找具有特定状态的订单，并且在应用更改时，它会通过事件总线（其下可以使用 RabbitMQ 或 Azure 服务总线）发布集成事件。

当然，也可以改为运行任何其他业务的后台任务。

默认情况下，取消令牌会设置为 5 秒超时，但可以在使用 `IWebHostBuilder` 的 `UseShutdownTimeout` 扩展构建 `WebHost` 时更改该值。 这意味着我们的服务预计将在 5 秒内取消，否则会更突然地终止。

以下代码会将该时间更改为 10 秒。

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

### <a name="summary-class-diagram"></a>摘要类图

下图显示了实现 IHostedServices 时涉及的类和接口的直观摘要。

![显示 IWebHost 和 IHost 可以托管多个服务的关系图。](./media/background-tasks-with-ihostedservice/class-diagram-custom-ihostedservice.png)

**图 6-27**。 显示多个与 IHostedService 相关的类和接口的类图

类图：IWebHost 和 IHost 可以托管许多服务，这些服务从实现 IHostedService 的 BackgroundService 继承。

### <a name="deployment-considerations-and-takeaways"></a>部署注意事项和要点

请务必注意，部署 ASP.NET Core `WebHost` 或 .NET Core `Host` 的方式可能会影响最终解决方案。 例如，如果在 IIS 或常规 Azure 应用服务上部署 `WebHost`，由于应用池回收，主机可能会被关闭。 但是，如果将主机作为容器部署到 Kubernetes 等业务流程协调程序中，则可以控制主机的实时实例数量。 此外，还可以考虑云中专门针对这些方案的其他方法，例如 Azure Functions。 最后，如果需要服务一直处于运行状态并在 Windows Server 上部署，可以使用 Windows 服务。

但即使对于部署到应用池中的 `WebHost`，也存在如重新填充或刷新应用程序的内存中缓存这样的情况，这仍然适用。

`IHostedService` 接口为在 ASP.NET Core Web 应用程序（在 .NET Core 2.0 及更高版本中）或任何进程/主机（从使用 `IHost` 的 .NET Core 2.1 开始）中启动后台任务提供了一种便捷方式。 其主要优势在于，当主机本身将要关闭时，可以有机会进行正常取消以清理后台任务的代码。

## <a name="additional-resources"></a>其他资源

- **在 ASP.NET Core/Standard 2.0 中生成计划任务** \
  <https://blog.maartenballiauw.be/post/2017/08/01/building-a-scheduled-cache-updater-in-aspnet-core-2.html>

- **在 ASP.NET Core 2.0 中实现 IHostedService** \
  <https://www.stevejgordon.co.uk/asp-net-core-2-ihostedservice>

- **使用 ASP.NET Core 2.1 的 GenericHost 示例** \
  <https://github.com/aspnet/Hosting/tree/release/2.1/samples/GenericHostSample>

> [!div class="step-by-step"]
> [上一页](test-aspnet-core-services-web-apps.md)
> [下一页](implement-api-gateways-with-ocelot.md)
