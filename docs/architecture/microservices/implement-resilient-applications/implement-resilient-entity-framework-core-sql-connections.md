---
title: 实现复原 Entity Framework Core SQL 连接
description: 了解如何实现复原 Entity Framework Core SQL 连接。 在云中使用 Azure SQL 数据库时，此技术尤为重要。
ms.date: 10/16/2018
ms.openlocfilehash: cae3550ce301750949b042957d5d10f0167e614c
ms.sourcegitcommit: 88fbb019b84c2d044d11fb4f6004aec07f2b25b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/05/2021
ms.locfileid: "97899556"
---
# <a name="implement-resilient-entity-framework-core-sql-connections"></a>实现复原 Entity Framework Core SQL 连接

对于 Azure SQL DB，Entity Framework Core (EF) 早已提供了内部数据库连接复原和重试逻辑。 但如果想要[复原 EF Core 连接](/ef/core/miscellaneous/connection-resiliency)，则需要为每个 <xref:Microsoft.EntityFrameworkCore.DbContext> 连接启用 Entity Framework 执行策略。

例如，EF Core 连接级别的下列代码可启用复原 SQL 连接，此连接在连接失败时会重试。

```csharp
// Startup.cs from any ASP.NET Core Web API
public class Startup
{
    // Other code ...
    public IServiceProvider ConfigureServices(IServiceCollection services)
    {
        // ...
        services.AddDbContext<CatalogContext>(options =>
        {
            options.UseSqlServer(Configuration["ConnectionString"],
            sqlServerOptionsAction: sqlOptions =>
            {
                sqlOptions.EnableRetryOnFailure(
                maxRetryCount: 10,
                maxRetryDelay: TimeSpan.FromSeconds(30),
                errorNumbersToAdd: null);
            });
        });
    }
//...
}
```

## <a name="execution-strategies-and-explicit-transactions-using-begintransaction-and-multiple-dbcontexts"></a>使用 BeginTransaction 和多个 DbContext 的执行策略和显式事务

在 EF Core 连接中启用重试时，使用 EF Core 执行的每项操作都会成为其自己的可重试操作。 如果发生暂时性故障，每个查询和 `SaveChanges` 的每次调用都会作为一个单元进行重试。

但是，如果你的代码使用 `BeginTransaction` 启动事务，则自行定义一组需要作为一个单元来处理的操作。 如果发生故障，则必须回滚事务中的所有内容。

如果在使用 EF 执行策略（重试策略）时尝试执行该事务，并从多个 DbContext 调用 `SaveChanges`，则会收到与下列情况类似的异常：

> System.InvalidOperationException：已配置的执行策略“SqlServerRetryingExecutionStrategy”不支持用户启动的事务。 使用由“DbContext.Database.CreateExecutionStrategy()”返回的执行策略执行事务（作为一个可回溯单元）中的所有操作。

该解决方案通过代表所有需要执行的委托来手动调用 EF 执行策略。 如果发生暂时性故障，执行策略会再次调用委托。 例如，以下代码演示了在更新产品时，如何使用两个 DbContext（\_ catalogContext 和 IntegrationEventLogContext）在 eShopOnContainers 中实现该操作，然后保存需要使用不同 DbContext 的 ProductPriceChangedIntegrationEvent 对象。

```csharp
public async Task<IActionResult> UpdateProduct(
    [FromBody]CatalogItem productToUpdate)
{
    // Other code ...

    var oldPrice = catalogItem.Price;
    var raiseProductPriceChangedEvent = oldPrice != productToUpdate.Price;

    // Update current product
    catalogItem = productToUpdate;

    // Save product's data and publish integration event through the Event Bus
    // if price has changed
    if (raiseProductPriceChangedEvent)
    {
        //Create Integration Event to be published through the Event Bus
        var priceChangedEvent = new ProductPriceChangedIntegrationEvent(
          catalogItem.Id, productToUpdate.Price, oldPrice);

       // Achieving atomicity between original Catalog database operation and the
       // IntegrationEventLog thanks to a local transaction
       await _catalogIntegrationEventService.SaveEventAndCatalogContextChangesAsync(
           priceChangedEvent);

       // Publish through the Event Bus and mark the saved event as published
       await _catalogIntegrationEventService.PublishThroughEventBusAsync(
           priceChangedEvent);
    }
    // Just save the updated product because the Product's Price hasn't changed.
    else
    {
        await _catalogContext.SaveChangesAsync();
    }
}
```

第一个 <xref:Microsoft.EntityFrameworkCore.DbContext> 是 `_catalogContext`，第二个 `DbContext` 在 `_catalogIntegrationEventService` 对象中。 通过使用 EF 执行策略在所有 `DbContext` 对象之间执行“提交”操作。

若要实现此多个 `DbContext` 提交，`SaveEventAndCatalogContextChangesAsync` 要使用 `ResilientTransaction` 类，如以下代码所示：

```csharp
public class CatalogIntegrationEventService : ICatalogIntegrationEventService
{
    //…
    public async Task SaveEventAndCatalogContextChangesAsync(
        IntegrationEvent evt)
    {
        // Use of an EF Core resiliency strategy when using multiple DbContexts
        // within an explicit BeginTransaction():
        // https://docs.microsoft.com/ef/core/miscellaneous/connection-resiliency
        await ResilientTransaction.New(_catalogContext).ExecuteAsync(async () =>
        {
            // Achieving atomicity between original catalog database
            // operation and the IntegrationEventLog thanks to a local transaction
            await _catalogContext.SaveChangesAsync();
            await _eventLogService.SaveEventAsync(evt,
                _catalogContext.Database.CurrentTransaction.GetDbTransaction());
        });
    }
}
```

基本上，`ResilientTransaction.ExecuteAsync` 方法从传递的 `DbContext` (`_catalogContext`) 开始一个事务，然后让 `EventLogService` 使用该事务保存来自 `IntegrationEventLogContext` 的更改，然后提交整个事务。

```csharp
public class ResilientTransaction
{
    private DbContext _context;
    private ResilientTransaction(DbContext context) =>
        _context = context ?? throw new ArgumentNullException(nameof(context));

    public static ResilientTransaction New (DbContext context) =>
        new ResilientTransaction(context);

    public async Task ExecuteAsync(Func<Task> action)
    {
        // Use of an EF Core resiliency strategy when using multiple DbContexts
        // within an explicit BeginTransaction():
        // https://docs.microsoft.com/ef/core/miscellaneous/connection-resiliency
        var strategy = _context.Database.CreateExecutionStrategy();
        await strategy.ExecuteAsync(async () =>
        {
            await using var transaction = await _context.Database.BeginTransactionAsync();
            await action();
            await transaction.CommitAsync();
        });
    }
}
```

## <a name="additional-resources"></a>其他资源

- **ASP.NET MVC 应用程序中的 EF 的连接复原和命令截获** \
  [https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application)

- **Cesar de la Torre。使用复原 Entity Framework Core SQL 连接和事务** \
  <https://devblogs.microsoft.com/cesardelatorre/using-resilient-entity-framework-core-sql-connections-and-transactions-retries-with-exponential-backoff/>

>[!div class="step-by-step"]
>[上一页](implement-retries-exponential-backoff.md)
>[下一页](use-httpclientfactory-to-implement-resilient-http-requests.md)
