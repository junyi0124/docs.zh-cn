---
title: 使用 Web API 实现微服务应用层
description: 了解依赖关系注入和转存进程模式及其在 Web API 应用层中的实现详细信息。
ms.date: 08/17/2020
ms.openlocfilehash: 45121026e06c55258a16f41aa801c06808a6919f
ms.sourcegitcommit: 721c3e4bdbb1ea0bb420818ec944c538fe5c513a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96437792"
---
# <a name="implement-the-microservice-application-layer-using-the-web-api"></a>使用 Web API 实现微服务应用层

## <a name="use-dependency-injection-to-inject-infrastructure-objects-into-your-application-layer"></a>使用依赖关系注入将基础结构对象注入到应用层中

如前所述，可以在要生成的项目（程序集）中实现应用层，例如在 Web API 项目或 MVC web 应用项目中。 如果使用 ASP.NET Core 构建微服务，应用程序层通常是 Web API 库。 如果要从自定义应用程序层代码中分离来自 ASP.NET Core 的内容（其基础结构以及你的控制器），还可将应用程序层置于单独的类库，但这是可选操作。

例如，订购微服务的应用层代码直接在 Ordering.API 项目（ASP.NET Core Web API 项目）中实现，如图 7-23 所示。

:::image type="complex" source="./media/microservice-application-layer-implementation-web-api/ordering-api-microservice.png" alt-text="解决方案资源管理器中 Ordering.API 微服务的屏幕截图。":::
Ordering.API 微服务的解决方案资源管理器视图，显示“应用程序”文件夹下的子文件夹：行为、命令、DomainEventHandler、IntegrationEvent、模型、查询和验证。
:::image-end:::

**图 7-23**。 Ordering.API ASP.NET Core Web API 项目中的应用程序层

ASP.NET Core 包含一个简单的[内置 IoC 容器](/aspnet/core/fundamentals/dependency-injection)（表示为 接口），默认情况下，该容器支持构造函数注入，ASP.NET 可通过 DI 提供某些服务。 ASP.NET Core 使用“服务”这一术语来表示将通过 DI 注入的你注册的类型。 可以在应用程序的 Startup 类中的 ConfigureServices 方法中配置内置容器的服务。 依赖项会在类型需要以及在 IoC 容器中注册的服务中实现。

通常需要注入实现基础结构对象的依赖项。 一个要注入的典型的依赖项是存储库。 但可注入任何其他你拥有的基础结构依赖项。 对于较简单的实现，可直接注入 Unit of Work 模式对象（EF DbContext 对象），因为 DBContext 也是基础结构持久性对象的实现。

在下面的示例中，可以看到 .NET 如何通过构造函数注入所需的存储库对象。 此类是一个命令处理程序，我们将在下一部分中对其进行介绍。

```csharp
public class CreateOrderCommandHandler
        : IRequestHandler<CreateOrderCommand, bool>
{
    private readonly IOrderRepository _orderRepository;
    private readonly IIdentityService _identityService;
    private readonly IMediator _mediator;
    private readonly IOrderingIntegrationEventService _orderingIntegrationEventService;
    private readonly ILogger<CreateOrderCommandHandler> _logger;

    // Using DI to inject infrastructure persistence Repositories
    public CreateOrderCommandHandler(IMediator mediator,
        IOrderingIntegrationEventService orderingIntegrationEventService,
        IOrderRepository orderRepository,
        IIdentityService identityService,
        ILogger<CreateOrderCommandHandler> logger)
    {
        _orderRepository = orderRepository ?? throw new ArgumentNullException(nameof(orderRepository));
        _identityService = identityService ?? throw new ArgumentNullException(nameof(identityService));
        _mediator = mediator ?? throw new ArgumentNullException(nameof(mediator));
        _orderingIntegrationEventService = orderingIntegrationEventService ?? throw new ArgumentNullException(nameof(orderingIntegrationEventService));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));
    }

    public async Task<bool> Handle(CreateOrderCommand message, CancellationToken cancellationToken)
    {
        // Add Integration event to clean the basket
        var orderStartedIntegrationEvent = new OrderStartedIntegrationEvent(message.UserId);
        await _orderingIntegrationEventService.AddAndSaveEventAsync(orderStartedIntegrationEvent);

        // Add/Update the Buyer AggregateRoot
        // DDD patterns comment: Add child entities and value-objects through the Order Aggregate-Root
        // methods and constructor so validations, invariants and business logic
        // make sure that consistency is preserved across the whole aggregate
        var address = new Address(message.Street, message.City, message.State, message.Country, message.ZipCode);
        var order = new Order(message.UserId, message.UserName, address, message.CardTypeId, message.CardNumber, message.CardSecurityNumber, message.CardHolderName, message.CardExpiration);

        foreach (var item in message.OrderItems)
        {
            order.AddOrderItem(item.ProductId, item.ProductName, item.UnitPrice, item.Discount, item.PictureUrl, item.Units);
        }

        _logger.LogInformation("----- Creating Order - Order: {@Order}", order);

        _orderRepository.Add(order);

        return await _orderRepository.UnitOfWork
            .SaveEntitiesAsync(cancellationToken);
    }
}

```

类使用注入的存储库执行事务和保持状态更改。 类是命令处理程序、ASP.NET Core Web API 控制器方法，还是 [DDD 应用程序服务](https://lostechies.com/jimmybogard/2008/08/21/services-in-domain-driven-design/)，这并不重要。 它最终是一个简单类，该类使用存储库、域实体和其他应用程序协调，这与命令处理程序相似。 依赖项注入的工作原理对于所有所述的类都是相同的，如基于构造函数使用 DI 的示例。

### <a name="register-the-dependency-implementation-types-and-interfaces-or-abstractions"></a>注册依赖项实现类型和接口或抽象

使用通过构造函数注入的对象前，需要知道在何处注册接口和类，这些接口和类生成通过 DI 注入应用程序类的对象。 （如基于构造函数的 DI，如前面所示。）

#### <a name="use-the-built-in-ioc-container-provided-by-aspnet-core"></a>使用由 ASP.NET Core 提供的内置 IoC 容器

使用 ASP.NET Core 提供的内置 IoC 容器时，在 Startup.cs 文件中注册要注入ConfigureServices 方法的类型，如以下代码所示：

```csharp
// Registration of types into ASP.NET Core built-in container
public void ConfigureServices(IServiceCollection services)
{
    // Register out-of-the-box framework services.
    services.AddDbContext<CatalogContext>(c =>
        c.UseSqlServer(Configuration["ConnectionString"]),
        ServiceLifetime.Scoped);

    services.AddMvc();
    // Register custom application dependencies.
    services.AddScoped<IMyCustomRepository, MyCustomSQLRepository>();
}
```

在 IoC 容器中注册类型时最常用的模式是注册一对类型 - 一个接口及其相关实现类。 当通过任何构造函数请求 IoC 容器中的对象时，请求特定接口类型的对象。 例如，在前面的示例中，最后一行说明当构造函数具有 IMyCustomRepository（接口或抽象）依赖项时，IoC 容器将注入 MyCustomSQLServerRepository 实现类的实例。

#### <a name="use-the-scrutor-library-for-automatic-types-registration"></a>将 Scrutor 库用于自动类型注册

在 .NET Core 中使用 DI 时，可能需要扫描程序集，并自动按约定注册其类型。 当前 ASP.NET Core 中未提供此功能。 但是，可以使用 [Scrutor](https://github.com/khellang/Scrutor) 库。 如果需要在 IoC 容器中注册许多类型，使用该方法很方便。

#### <a name="additional-resources"></a>其他资源

- **Matthew King。向 Scrutor 注册服务** \
  <https://www.mking.net/blog/registering-services-with-scrutor>

- **Kristian Hellang。Scrutor。** GitHub 存储库。 \
  <https://github.com/khellang/Scrutor>

#### <a name="use-autofac-as-an-ioc-container"></a>使用 Autofac 作为 IoC 容器

还可使用其他 IoC 容器，并将其插入 ASP.NET Core 管道，就像 eShopOnContainers（使用 [Autofac](https://autofac.org/)）中的订购微服务一样。 使用 Autofac 时通常通过模块注册类型，这可根据类型位置，在多个文件之间拆分注册类型，就像可在多个类库中分布应用程序类型一样。

例如，下面是 [Ordering.API Web API](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/Services/Ordering/Ordering.API) 项目的 [Autofac 应用程序模块](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Infrastructure/AutofacModules/ApplicationModule.cs)，包含要注入的类型。

```csharp
public class ApplicationModule : Autofac.Module
{
    public string QueriesConnectionString { get; }
    public ApplicationModule(string qconstr)
    {
        QueriesConnectionString = qconstr;
    }

    protected override void Load(ContainerBuilder builder)
    {
        builder.Register(c => new OrderQueries(QueriesConnectionString))
            .As<IOrderQueries>()
            .InstancePerLifetimeScope();
        builder.RegisterType<BuyerRepository>()
            .As<IBuyerRepository>()
            .InstancePerLifetimeScope();
        builder.RegisterType<OrderRepository>()
            .As<IOrderRepository>()
            .InstancePerLifetimeScope();
        builder.RegisterType<RequestManager>()
            .As<IRequestManager>()
            .InstancePerLifetimeScope();
   }
}
```

Autofac 还具有用于[按名称约定扫描程序集和注册类型](https://autofac.readthedocs.io/en/latest/register/scanning.html)的功能。

注册过程和概念类似于向内置 ASP.NET Core IoC 容器注册类型，但使用 Autofac 时，语法略有不同。

在示例代码中，抽象 IOrderRepository 与实现类 OrderRepository 一同注册。 这意味着每当构造函数通过 IOrderRepository 抽象或接口声明依赖项时，IoC 容器会注入 OrderRepository 类的实例。

实例作用域类型确定实例在相同服务或依赖项的请求之间的共享方式。 发出依赖项请求时，IoC 容器会返回以下项：

- 每个生存期范围的一个实例（在 ASP.NET Core IoC 容器中称为“已设置范围”）。

- 每个依赖项的一个实例（在 ASP.NET Core IoC 容器中称为“暂时”）。

- 使用 IoC 容器的跨所有对象共享的一个实例（在 ASP.NET Core IoC 容器中称为“单一实例”）。

#### <a name="additional-resources"></a>其他资源

- **ASP.NET Core 中的依赖关系注入简介** \
  [https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection](/aspnet/core/fundamentals/dependency-injection)

- **Autofac。** 官方文档。 \
  <https://docs.autofac.org/en/latest/>

- **比较 ASP.NET Core IoC 容器服务生存期和 Autofac IoC 容器实例作用域 - Cesar de la Torre。** \
  <https://devblogs.microsoft.com/cesardelatorre/comparing-asp-net-core-ioc-service-life-times-and-autofac-ioc-instance-scopes/>

## <a name="implement-the-command-and-command-handler-patterns"></a>实现命令和命令处理程序模式

在上一部分中的“DI 通过构造函数”示例中，IoC 容器通过类中的构造函数注入存储库。 但是，它们究竟是在哪里注入的？ 在简单的 Web API（例如 eShopOnContainers 中的目录微服务）中，它们在控制器构造函数的 MVC 控制器级别注入（作为 ASP.NET Core 请求管道的一部分）。 但是，在本部分的初始代码（eShopOnContainers 中来自 Ordering.API 服务的 [CreateOrderCommandHandler](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Application/Commands/CreateOrderCommandHandler.cs) 类）中，依赖项注入通过特定命令处理程序的构造函数完成。 让我们来了解一下什么是命令处理程序，以及使用它的好处。

命令模式在本质上与本指南之前介绍的 CQRS 模式相关。 CQRS 具有两个功能。 第一个功能是查询，通过 [Dapper](https://github.com/StackExchange/dapper-dot-net) 微 ORM 使用简化的查询，我们已经在前文中介绍过了。 第二个功能是命令（这是事务的起点），以及服务外的输入通道。

如图 7-24 所示，该模式基于接受客户端的命令、根据域模式规则进行处理，最后保持事务状态。

![显示从客户端到数据库的高级别数据流的关系图。](./media/microservice-application-layer-implementation-web-api/high-level-writes-side.png)

**图 7-24**。 CQRS 模式中的命令高级别视图或“事务端”

图 7-24 显示 UI 应用通过 API 向 `CommandHandler` 发送命令以更新数据库，此命令取决于域模型和基础结构。

### <a name="the-command-class"></a>命令类

命令是让系统执行更改系统状态的操作的请求。 命令具有命令性，且应仅处理一次。

由于命令具有命令性，所以通常采用命令语气使用谓词（如“create”或“update”）命名，命令可能包括聚合类型，例如 CreateOrderCommand。 与事件不同，命令不是过去发生的事实，它只是一个请求，因此可以拒绝它。

命令可能源自 UI，由用户发出请求而产生，也可能来自进程管理器，由进程管理器指导聚合执行操作而产生。

命令的一个重要特征是它应该由单一接收方处理，且仅处理一次。 这是因为命令是要在应用程序中执行的单个操作或事务。 例如，同一个订单创建命令的处理次数不应超过一次。 这是命令和事件之间的一个重要区别。 事件可能会经过多次处理，因为许多系统或微服务可能会对该事件感兴趣。

此外，请注意，如果命令不是幂等，命令仅会处理一次。 如果命令可执行多次且结果不变（由于命令的本质或系统处理命令的方式），则命令是幂等。

建议将命令和更新设置为幂等，如果在域的业务规则和固定协定下有意义。 例如，我们使用同一个示例，如果出于任何原因（重试逻辑、黑客攻击等），相同的 CreateOrder 命令多次到达系统，应能识别并确保不会创建多个订单。 为此，需要在操作中附加一些类型的标识，确定是否已处理命令或更新。

可将命令发送给单个接收方，但不会发布命令。 发布适用于声明事实的事件 - 事件已发生，事件接收方可能对其感兴趣。 对于事件，发布服务器无需在意哪些接收方获取事件或对其进行什么操作。 但域或集成事件是一个例外，前面章节已有介绍。

命令通过包含数据字段或集合（其中包含执行命令所需的所有信息）的类实现。 命令是一种特殊的数据传输对象 (DTO)，专门用于请求更改或事务。 命令本身完全基于处理命令所需的信息，别无其他。

下面的示例显示了简化的 `CreateOrderCommand` 类。 这是 eShopOnContainers 的订购微服务中使用的不可变命令。

```csharp
// DDD and CQRS patterns comment: Note that it is recommended to implement immutable Commands
// In this case, its immutability is achieved by having all the setters as private
// plus only being able to update the data just once, when creating the object through its constructor.
// References on Immutable Commands:
// http://cqrs.nu/Faq
// https://docs.spine3.org/motivation/immutability.html
// http://blog.gauffin.org/2012/06/griffin-container-introducing-command-support/
// https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/how-to-implement-a-lightweight-class-with-auto-implemented-properties

[DataContract]
public class CreateOrderCommand
    : IRequest<bool>
{
    [DataMember]
    private readonly List<OrderItemDTO> _orderItems;

    [DataMember]
    public string UserId { get; private set; }

    [DataMember]
    public string UserName { get; private set; }

    [DataMember]
    public string City { get; private set; }

    [DataMember]
    public string Street { get; private set; }

    [DataMember]
    public string State { get; private set; }

    [DataMember]
    public string Country { get; private set; }

    [DataMember]
    public string ZipCode { get; private set; }

    [DataMember]
    public string CardNumber { get; private set; }

    [DataMember]
    public string CardHolderName { get; private set; }

    [DataMember]
    public DateTime CardExpiration { get; private set; }

    [DataMember]
    public string CardSecurityNumber { get; private set; }

    [DataMember]
    public int CardTypeId { get; private set; }

    [DataMember]
    public IEnumerable<OrderItemDTO> OrderItems => _orderItems;

    public CreateOrderCommand()
    {
        _orderItems = new List<OrderItemDTO>();
    }

    public CreateOrderCommand(List<BasketItem> basketItems, string userId, string userName, string city, string street, string state, string country, string zipcode,
        string cardNumber, string cardHolderName, DateTime cardExpiration,
        string cardSecurityNumber, int cardTypeId) : this()
    {
        _orderItems = basketItems.ToOrderItemsDTO().ToList();
        UserId = userId;
        UserName = userName;
        City = city;
        Street = street;
        State = state;
        Country = country;
        ZipCode = zipcode;
        CardNumber = cardNumber;
        CardHolderName = cardHolderName;
        CardExpiration = cardExpiration;
        CardSecurityNumber = cardSecurityNumber;
        CardTypeId = cardTypeId;
        CardExpiration = cardExpiration;
    }


    public class OrderItemDTO
    {
        public int ProductId { get; set; }

        public string ProductName { get; set; }

        public decimal UnitPrice { get; set; }

        public decimal Discount { get; set; }

        public int Units { get; set; }

        public string PictureUrl { get; set; }
    }
}
```

基本上，命令类包含通过使用域模型对象执行业务事务所需的所有数据。 因此，命令是包含只读数据、不包含行为的数据结构。 命令的名称指示其用途。 在 C# 等许多语言中，命令表示为类，但它们不是真正的面向对象意义上的真的类。

命令的另一个特征是不变性，因为它们的预期用途是由域模型直接处理。 它们不需要在预计的生存期内更改。 在 C# 类中，可通过不使用任何可更改内部状态的资源库或其他方法，实现不变性。

请记住，如果打算或希望命令经过序列化/反序列化过程，则这些属性必须具有一个专用资源库和 `[DataMember]`（或 `[JsonProperty]`）属性。 否则，反序列化程序将无法使用所需的值在目标上重建对象。 如果该类的构造函数带有所有属性的参数，并且使用通常的 camelCase 命名约定，并且可以将该构造函数注释为 `[JsonConstructor]`，则也可以使用真正的只读属性。 但是，此选项需要更多代码。

例如，用于创建订单的命令类可能与你要创建的订单在数据上类似，但你可能不需要相同的属性。 例如，`CreateOrderCommand` 命令没有订单 ID，因为订单尚未创建。

许多命令类可能很简单，只需要一些有关需要更改的状态的字段。 如果只需要使用类似以下的命令将订单状态从“处理中”更改为“已付款”或“已发货”，则是这种情况：

```csharp
[DataContract]
public class UpdateOrderStatusCommand
    :IRequest<bool>
{
    [DataMember]
    public string Status { get; private set; }

    [DataMember]
    public string OrderId { get; private set; }

    [DataMember]
    public string BuyerIdentityGuid { get; private set; }
}
```

一些开发人员将其 UI 请求对象从命令 DTO 中分离，但这只是一种个人偏好。 这种分离既枯燥又没有太大价值，对象几乎都是相同的形状。 例如，在 eShopOnContainers 中，一些命令直接来自客户端。

### <a name="the-command-handler-class"></a>命令处理程序类

应为每个命令实现特定命令处理程序类。 这是该模式的工作原理，是应用命令对象、域对象和基础结构存储库对象的情景。 对于 CQRS 和 DDD ，命令处理程序实际上是应用程序层的核心。 但是，域类中应包含所有域逻辑 - 在聚合根（根实体）、子实体或[域服务](https://lostechies.com/jimmybogard/2008/08/21/services-in-domain-driven-design/)中，但不在命令处理程序中（命令处理程序是应用程序层中的类）。

命令处理程序类为上一节提到的单一责任原则 (SRP) 的实现方式提供了强大的基石。

命令处理程序收到命令，并从使用的聚合获取结果。 结果应为成功执行命令，或者异常。 出现异常时，系统状态应保持不变。

命令处理程序通常执行以下步骤：

- 它接收 DTO 等命令对象（从[转存进程](https://en.wikipedia.org/wiki/Mediator_pattern)或其他基础结构对象）。

- 它会验证命令是否有效（如果转存进程未验证）。

- 它会实例化作为当前命令目标的聚合根实例。

- 它会在聚合根实例上执行方法，从命令获得所需数据。

- 它将聚合的新状态保持到相关数据库。 这最后一个操作是实际的事务。

通常情况下，命令处理程序处理由聚合根（根实体）驱动的单个聚合。 如果多个聚合应受到单个命令接收的影响，可使用域事件跨多个聚合传播状态或操作。

请注意，处理命令时，所有域逻辑应在域模型（聚合）内，完全封装并准备好进行单元测试。 命令处理程序的作用是从数据库获取域模型，最后指示基础结构层（存储库）在模型更改完成后保存更改。 此方法的优点是，你可在独立的、完全封装的、丰富行为域模型中重构域逻辑，无需在应用程序或基础结构层中更改代码，命令处理程序、Web API、存储库等是管道级别。

如果命令处理程序很复杂，包含过多逻辑，则可能存在代码异味。 请查看它们，如果发现域逻辑，则重构代码，将域行为移动到域对象（聚合根和子实体）的方法。

作为命令处理程序类的示例，下面的代码演示本章开头介绍的同一个 `CreateOrderCommandHandler` 类。 这个示例还强调了 Handle 方法以及域模型对象/聚合的操作。

```csharp
public class CreateOrderCommandHandler
        : IRequestHandler<CreateOrderCommand, bool>
{
    private readonly IOrderRepository _orderRepository;
    private readonly IIdentityService _identityService;
    private readonly IMediator _mediator;
    private readonly IOrderingIntegrationEventService _orderingIntegrationEventService;
    private readonly ILogger<CreateOrderCommandHandler> _logger;

    // Using DI to inject infrastructure persistence Repositories
    public CreateOrderCommandHandler(IMediator mediator,
        IOrderingIntegrationEventService orderingIntegrationEventService,
        IOrderRepository orderRepository,
        IIdentityService identityService,
        ILogger<CreateOrderCommandHandler> logger)
    {
        _orderRepository = orderRepository ?? throw new ArgumentNullException(nameof(orderRepository));
        _identityService = identityService ?? throw new ArgumentNullException(nameof(identityService));
        _mediator = mediator ?? throw new ArgumentNullException(nameof(mediator));
        _orderingIntegrationEventService = orderingIntegrationEventService ?? throw new ArgumentNullException(nameof(orderingIntegrationEventService));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));
    }

    public async Task<bool> Handle(CreateOrderCommand message, CancellationToken cancellationToken)
    {
        // Add Integration event to clean the basket
        var orderStartedIntegrationEvent = new OrderStartedIntegrationEvent(message.UserId);
        await _orderingIntegrationEventService.AddAndSaveEventAsync(orderStartedIntegrationEvent);

        // Add/Update the Buyer AggregateRoot
        // DDD patterns comment: Add child entities and value-objects through the Order Aggregate-Root
        // methods and constructor so validations, invariants and business logic
        // make sure that consistency is preserved across the whole aggregate
        var address = new Address(message.Street, message.City, message.State, message.Country, message.ZipCode);
        var order = new Order(message.UserId, message.UserName, address, message.CardTypeId, message.CardNumber, message.CardSecurityNumber, message.CardHolderName, message.CardExpiration);

        foreach (var item in message.OrderItems)
        {
            order.AddOrderItem(item.ProductId, item.ProductName, item.UnitPrice, item.Discount, item.PictureUrl, item.Units);
        }

        _logger.LogInformation("----- Creating Order - Order: {@Order}", order);

        _orderRepository.Add(order);

        return await _orderRepository.UnitOfWork
            .SaveEntitiesAsync(cancellationToken);
    }
}
```

以下是命令处理程序应执行的附加步骤：

- 使用命令数据对聚合根的方法和行为进行操作。

- 在域对象中，执行事务时引发域事件，但这从命令处理程序角度看是透明的。

- 如果聚合操作结果成功且在完成事务后，引发集成事件。 （可能还会由存储库等基础结构类引发。）

#### <a name="additional-resources"></a>其他资源

- **Mark Seemann。在边界上，应用程序不是面向对象的** \
  <https://blog.ploeh.dk/2011/05/31/AttheBoundaries,ApplicationsareNotObject-Oriented/>

- **命令和事件** \
  <https://cqrs.nu/Faq/commands-and-events>

- **命令处理程序有什么用途？** \
  <https://cqrs.nu/Faq/command-handlers>

- **Jimmy Bogard。域命令模式 - 处理程序** \
  <https://jimmybogard.com/domain-command-patterns-handlers/>

- **Jimmy Bogard。域命令模式 - 验证** \
  <https://jimmybogard.com/domain-command-patterns-validation/>

## <a name="the-command-process-pipeline-how-to-trigger-a-command-handler"></a>命令处理管道：如何触发命令处理程序

下一个问题是如何调用命令处理程序。 可从每个相关的 ASP.NET Core 控制器手动调用。 但是，此方法过于耦合，并不理想。

建议的其他两个主要选项是：

- 通过内存中转存进程模式项目。

- 在控制器和处理程序之间使用异步消息队列。

### <a name="use-the-mediator-pattern-in-memory-in-the-command-pipeline"></a>在命令管道中使用转存进程模式（内存中）

如图 7-25 所示，在 CQRS 方法中使用类似于内存中总线的智能转存进程，该进程非常智能，可基于要接收的命令或 DTO 的类型重定向到正确的命令处理程序。 组件之间的黑色箭头表示对象（许多情况下，通过 DI 注入）之间的依赖关系及其相关交互。

![显示从客户端到数据库的详细数据流的关系图。](./media/microservice-application-layer-implementation-web-api/mediator-cqrs-microservice.png)

**图 7-25**。 在单个 CQRS 微服务进程中使用转存进程模式

上图显示了图 7-24 的放大图：ASP.NET Core 控制器将命令发送到 MediatR 的命令管道，使它们到达相应的处理程序。

使用转存进程模式的原因是对于企业应用程序，处理请求可能很复杂。 你需要添加具有开放数量的整合问题，例如登录、验证、审核和安全性。 在这些情况下，可以依赖转存进程管道（请参阅[转存进程模式](https://en.wikipedia.org/wiki/Mediator_pattern)），以提供应对这些额外行为或整合问题的方法。

转存进程是封装此进程方式的对象。它基于状态、命令处理程序调用方式或提供给处理程序的负载，协调执行。 借助转存进程组件，可通过应用修饰器（或[管道行为](https://github.com/jbogard/MediatR/wiki/Behaviors)从 [MediatR 3](https://www.nuget.org/packages/MediatR/3.0.0) 开始），采用集中且透明的方式，应用整合问题。 有关更多信息，请参见[修饰器模式](https://en.wikipedia.org/wiki/Decorator_pattern)。

修饰器和行为类似于[面向方面编程 (AOP)](https://en.wikipedia.org/wiki/Aspect-oriented_programming)，仅应用于由转存进程组件管理的特定进程管道。 AOP 中实现整合问题的方面基于编译时注入的 aspect weaver 或基于对象调用截获应用。 这两种典型 AOP 方法的工作方式有时“就像是魔术”，因为不容易了解 AOP 的工作方式。 处理严重问题或 bug 时，AOP 可能难以调试。 另一方面，这些修饰器/行为是显式的，且仅在转存进程的上下文中应用，因此调试更可预测、更轻松。

例如，在 eShopOnContainers 订购微服务中，实现了两个示例行为：一个 [LogBehavior](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Behaviors/LoggingBehavior.cs) 类和一个 [ValidatorBehavior](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Behaviors/ValidatorBehavior.cs) 类。 下一节通过演示 eShopOnContainers 如何使用 [MediatR](https://www.nuget.org/packages/MediatR) [行为](https://github.com/jbogard/MediatR/wiki/Behaviors)介绍了行为的实现。

### <a name="use-message-queues-out-of-proc-in-the-commands-pipeline"></a>在命令的管道中使用消息队列（进程外）

另一种选择是使用基于中转站或消息队列的异步消息，如图 7-26 所示。 可在命令处理程序前，将此选项与转存进程组件合并。

![使用 HA 消息队列显示数据流的关系图。](./media/microservice-application-layer-implementation-web-api/add-ha-message-queue.png)

**图 7-26**。 通过 CQRS 命令使用消息队列（进程外和进程间通信）

还可以通过高可用性消息队列处理命令的管道，以将命令传递到相应的处理程序。 使用消息队列接受命令可能会进一步复杂化命令管道，因为很可能需要将管道拆分为通过外部消息队列连接的两个进程。 如果需要基于异步消息传送，提高可伸缩性和性能，则仍应使用此方法。 请思考图 7-26 的情况，控制器将命令消息发布到队列，然后返回。 然后命令处理程序按自己的步调处理消息。 这是队列的一大优点：消息队列可在需要超高可伸缩性时（例如股票或具有大量传入数据的任何其他方案）充当缓冲区。

但是，由于消息队列具有异步性质，你需要了解如何与客户端应用程序就命令进程的成功或失败进行通信。 一般来说，应永远不要使用“发后不理”命令。 每个业务应用程序需要了解命令是否处理成功，或至少了解是否已验证和接受。

因此，相较于运行事务后返回操作结果的进程内命令进程，如果要在验证提交到异步队列的命令消息后响应客户端，这会增加系统复杂性。 使用队列时，可能需要通过其他操作结果消息返回命令进程结果，这将需要在系统中使用其他组件和自定义通信。

此外，异步命令是单向命令，这在许多情况下可能不是必要的，如下文中 Burtsev Alexey 和 Greg Young 之间有趣的[在线对话](https://groups.google.com/forum/#!msg/dddcqrs/xhJHVxDx2pM/WP9qP8ifYCwJ)中所介绍的：

> \[Burtsev Alexey\] 我发现有人在许多代码中使用异步命令处理或单向命令消息传送，但这样做是不合理的（他们并没有执行长操作或外部异步代码，他们甚至没有跨应用程序边界使用消息总线）。 他们为什么要引入不必要的复杂性？ 实际上我至今没有看到过使用阻止命令处理程序的 CQRS 代码示例，但是它在大多数情况下是有效的。
>
> \[Greg Young\] \[...\] 异步命令并不存在；它实际上是另一种事件。 如果我必须接受你发送给我的信息并在我不同意时引发事件，则这不再是你告诉我执行某个操作（\[即，这不是命令\]）。 这是你告诉我一些操作已完成。 虽然最初似乎只有细微差异，但会产生多方面影响。

异步命令会极大地增加系统的复杂性，因为没有指示失败的简单方法。 因此，除非需要缩放要求或在特殊情况下需要通过消息传达内部微服务，否则不建议使用异步命令。 在这些情况下，必须设计针对失败的单独报告和恢复系统。

在 eShopOnContainers 的初始版本中，我们决定使用同步命令处理，从 HTTP 请求启动，由转存进程模式驱动。 这样一来，可轻松地返回进程的成功或失败，如 [CreateOrderCommandHandler](https://github.com/dotnet-architecture/eShopOnContainers/blob/netcore1.1/src/Services/Ordering/Ordering.API/Application/Commands/CreateOrderCommandHandler.cs) 实现中一样。

在任何情况下，这应该是基于你的应用程序或微服务的业务要求的决定。

## <a name="implement-the-command-process-pipeline-with-a-mediator-pattern-mediatr"></a>通过转存进程模式 (MediatR) 实现命令进程管道

作为示例实现，本指南建议基于转存进程模式使用进程内管道，驱动命令引入和路由命令（内存中）到正确的命令处理程序。 本指南还建议应用[行为](https://github.com/jbogard/MediatR/wiki/Behaviors)以分隔整合问题。

有关 .NET Core 中的实现，可使用多个开发源代码库来实现转存进程模式。 本指南中使用的库是 [MediatR](https://github.com/jbogard/MediatR) 开放源代码库（由 Jimmy Bogard 创建），但也可使用其他方法。 MediatR 是一个小型的简单库，可处理命令等内存中消息，同时应用修饰器或行为。

使用转存进程模式有助于减小耦合度，并隔离请求工作的问题，同时自动连接到执行该工作的处理程序（在此情况下为命令处理程序）。

本指南中 Jimmy Bogard 介绍了使用转存进程模式的另一个好处：

> 我认为在这里值得提一下测试，它提供了针对系统行为的良好一致窗口。 请求传入，响应传出。我们发现这对生成行为一致的测试很有用。

首先，让我们看一下示例 WebAPI 控制器，你会在其中使用转存进程对象。 如果你没有使用转存进程对象，则需要为此控制器注入所有依赖项，例如记录器对象等。 因此，构造函数可能很复杂。 但是，如果你使用转存进程对象，控制器的构造函数可以简单很多，只需几个依赖项而不是许多依赖项（如果你针对每个整合操作使用一个依赖项），如以下示例所示：

```csharp
public class MyMicroserviceController : Controller
{
    public MyMicroserviceController(IMediator mediator,
                                    IMyMicroserviceQueries microserviceQueries)
    {
        // ...
    }
}
```

你会发现转存进程可提供简洁、精益的 Web API 控制器构造函数。 此外，在控制器方法中，将命令发送到转存进程对象的代码几乎只有一行：

```csharp
[Route("new")]
[HttpPost]
public async Task<IActionResult> ExecuteBusinessOperation([FromBody]RunOpCommand
                                                               runOperationCommand)
{
    var commandResult = await _mediator.SendAsync(runOperationCommand);

    return commandResult ? (IActionResult)Ok() : (IActionResult)BadRequest();
}
```

### <a name="implement-idempotent-commands"></a>实现幂等命令

在 eShopOnContainers 中，比上述更高级的示例是从订购微服务提交 CreateOrderCommand 对象。 但由于订购业务进程有点复杂，所以在我们的示例中，其实是从购物篮微服务开始，提交 CreateOrderCommand 对象的操作从名为 [UserCheckoutAcceptedIntegrationEventHandler](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/IntegrationEvents/EventHandling/UserCheckoutAcceptedIntegrationEventHandler.cs) 的集成事件处理程序（而不是从客户端应用调用的简单 WebAPI 控制器，如之前较简单示例所示）执行。

不过，将命令提交到 MediatR 的操作非常类似，如下面的代码所示。

```csharp
var createOrderCommand = new CreateOrderCommand(eventMsg.Basket.Items,
                                                eventMsg.UserId, eventMsg.City,
                                                eventMsg.Street, eventMsg.State,
                                                eventMsg.Country, eventMsg.ZipCode,
                                                eventMsg.CardNumber,
                                                eventMsg.CardHolderName,
                                                eventMsg.CardExpiration,
                                                eventMsg.CardSecurityNumber,
                                                eventMsg.CardTypeId);

var requestCreateOrder = new IdentifiedCommand<CreateOrderCommand,bool>(createOrderCommand,
                                                                        eventMsg.RequestId);
result = await _mediator.Send(requestCreateOrder);
```

但是，这种情况还是有点高级，因为我们也要实现幂等命令。 CreateOrderCommand 进程应该是幂等的，所以如果出于任何原因（如重试），通过网络复制相同消息，将仅处理同一个业务订单一次。

这是通过以下方式实现的：包装业务命令（在此情况下为 CreateOrderCommand），将其嵌入通用 IdentifiedCommand（通过来自网络的每个消息的 ID 跟踪，必须是幂等的）。

在以下代码中，你会发现 IdentifiedCommand 仅仅是包含 ID 和已包装业务命令对象的 DTO。

```csharp
public class IdentifiedCommand<T, R> : IRequest<R>
    where T : IRequest<R>
{
    public T Command { get; }
    public Guid Id { get; }
    public IdentifiedCommand(T command, Guid id)
    {
        Command = command;
        Id = id;
    }
}
```

名为 [IdentifiedCommandHandler.cs](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Commands/IdentifiedCommandHandler.cs) 的 IdentifiedCommand 的 CommandHandler 将基本上检查消息中的 ID 是否已存在于表格中。 如果已存在，将不会再次处理该命令，因此它充当幂等命令。 基础结构代码由以下 `_requestManager.ExistAsync` 方法调用执行。

```csharp
// IdentifiedCommandHandler.cs
public class IdentifiedCommandHandler<T, R> : IRequestHandler<IdentifiedCommand<T, R>, R>
        where T : IRequest<R>
{
    private readonly IMediator _mediator;
    private readonly IRequestManager _requestManager;
    private readonly ILogger<IdentifiedCommandHandler<T, R>> _logger;

    public IdentifiedCommandHandler(
        IMediator mediator,
        IRequestManager requestManager,
        ILogger<IdentifiedCommandHandler<T, R>> logger)
    {
        _mediator = mediator;
        _requestManager = requestManager;
        _logger = logger ?? throw new System.ArgumentNullException(nameof(logger));
    }

    /// <summary>
    /// Creates the result value to return if a previous request was found
    /// </summary>
    /// <returns></returns>
    protected virtual R CreateResultForDuplicateRequest()
    {
        return default(R);
    }

    /// <summary>
    /// This method handles the command. It just ensures that no other request exists with the same ID, and if this is the case
    /// just enqueues the original inner command.
    /// </summary>
    /// <param name="message">IdentifiedCommand which contains both original command & request ID</param>
    /// <returns>Return value of inner command or default value if request same ID was found</returns>
    public async Task<R> Handle(IdentifiedCommand<T, R> message, CancellationToken cancellationToken)
    {
        var alreadyExists = await _requestManager.ExistAsync(message.Id);
        if (alreadyExists)
        {
            return CreateResultForDuplicateRequest();
        }
        else
        {
            await _requestManager.CreateRequestForCommandAsync<T>(message.Id);
            try
            {
                var command = message.Command;
                var commandName = command.GetGenericTypeName();
                var idProperty = string.Empty;
                var commandId = string.Empty;

                switch (command)
                {
                    case CreateOrderCommand createOrderCommand:
                        idProperty = nameof(createOrderCommand.UserId);
                        commandId = createOrderCommand.UserId;
                        break;

                    case CancelOrderCommand cancelOrderCommand:
                        idProperty = nameof(cancelOrderCommand.OrderNumber);
                        commandId = $"{cancelOrderCommand.OrderNumber}";
                        break;

                    case ShipOrderCommand shipOrderCommand:
                        idProperty = nameof(shipOrderCommand.OrderNumber);
                        commandId = $"{shipOrderCommand.OrderNumber}";
                        break;

                    default:
                        idProperty = "Id?";
                        commandId = "n/a";
                        break;
                }

                _logger.LogInformation(
                    "----- Sending command: {CommandName} - {IdProperty}: {CommandId} ({@Command})",
                    commandName,
                    idProperty,
                    commandId,
                    command);

                // Send the embedded business command to mediator so it runs its related CommandHandler
                var result = await _mediator.Send(command, cancellationToken);

                _logger.LogInformation(
                    "----- Command result: {@Result} - {CommandName} - {IdProperty}: {CommandId} ({@Command})",
                    result,
                    commandName,
                    idProperty,
                    commandId,
                    command);

                return result;
            }
            catch
            {
                return default(R);
            }
        }
    }
}
```

由于 IdentifiedCommand 充当业务命令的信封，当业务命令由于不是重复 ID 而需要处理时，它会获取此内部业务命令，然后将其重新提交到转存进程，正如从 [IdentifiedCommandHandler.cs](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Commands/IdentifiedCommandHandler.cs) 运行 `_mediator.Send(message.Command)` 时显示的代码的最后一部分所示。

执行该操作时，它会链接和运行业务命令处理程序（在此情况下是针对订购数据库运行事务的 [CreateOrderCommandHandler](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Commands/CreateOrderCommandHandler.cs)），如以下代码所示。

```csharp
// CreateOrderCommandHandler.cs
public class CreateOrderCommandHandler
        : IRequestHandler<CreateOrderCommand, bool>
{
    private readonly IOrderRepository _orderRepository;
    private readonly IIdentityService _identityService;
    private readonly IMediator _mediator;
    private readonly IOrderingIntegrationEventService _orderingIntegrationEventService;
    private readonly ILogger<CreateOrderCommandHandler> _logger;

    // Using DI to inject infrastructure persistence Repositories
    public CreateOrderCommandHandler(IMediator mediator,
        IOrderingIntegrationEventService orderingIntegrationEventService,
        IOrderRepository orderRepository,
        IIdentityService identityService,
        ILogger<CreateOrderCommandHandler> logger)
    {
        _orderRepository = orderRepository ?? throw new ArgumentNullException(nameof(orderRepository));
        _identityService = identityService ?? throw new ArgumentNullException(nameof(identityService));
        _mediator = mediator ?? throw new ArgumentNullException(nameof(mediator));
        _orderingIntegrationEventService = orderingIntegrationEventService ?? throw new ArgumentNullException(nameof(orderingIntegrationEventService));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));
    }

    public async Task<bool> Handle(CreateOrderCommand message, CancellationToken cancellationToken)
    {
        // Add Integration event to clean the basket
        var orderStartedIntegrationEvent = new OrderStartedIntegrationEvent(message.UserId);
        await _orderingIntegrationEventService.AddAndSaveEventAsync(orderStartedIntegrationEvent);

        // Add/Update the Buyer AggregateRoot
        // DDD patterns comment: Add child entities and value-objects through the Order Aggregate-Root
        // methods and constructor so validations, invariants and business logic
        // make sure that consistency is preserved across the whole aggregate
        var address = new Address(message.Street, message.City, message.State, message.Country, message.ZipCode);
        var order = new Order(message.UserId, message.UserName, address, message.CardTypeId, message.CardNumber, message.CardSecurityNumber, message.CardHolderName, message.CardExpiration);

        foreach (var item in message.OrderItems)
        {
            order.AddOrderItem(item.ProductId, item.ProductName, item.UnitPrice, item.Discount, item.PictureUrl, item.Units);
        }

        _logger.LogInformation("----- Creating Order - Order: {@Order}", order);

        _orderRepository.Add(order);

        return await _orderRepository.UnitOfWork
            .SaveEntitiesAsync(cancellationToken);
    }
}
```

### <a name="register-the-types-used-by-mediatr"></a>注册由 MediatR 使用的类型

为了让 MediatR 识别命令处理程序类，需要在 IoC 容器中注册转存进程类和命令处理程序。 默认情况下，MediatR 使用 Autofac 作为 IoC 容器，但还可以使用内置的 ASP.NET Core IoC 容器或受 MediatR 支持的其他容器。

下面的代码演示如何在使用 Autofac 模块时注册转存进程的类型和命令。

```csharp
public class MediatorModule : Autofac.Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterAssemblyTypes(typeof(IMediator).GetTypeInfo().Assembly)
            .AsImplementedInterfaces();

        // Register all the Command classes (they implement IRequestHandler)
        // in assembly holding the Commands
        builder.RegisterAssemblyTypes(typeof(CreateOrderCommand).GetTypeInfo().Assembly)
                .AsClosedTypesOf(typeof(IRequestHandler<,>));
        // Other types registration
        //...
    }
}
```

MediatR 的魅力就在于此。

由于每个命令处理程序都实现了泛型 `IRequestHandler<T>` 接口，因此使用 `RegisteredAssemblyTypes` 方法注册程序集时，所有标记为 `IRequestHandler` 的类型也将注册到它们的 `Commands` 中。 例如：

```csharp
public class CreateOrderCommandHandler
  : IRequestHandler<CreateOrderCommand, bool>
{
```

这是将命令与命令处理程序关联的代码。 此处理程序仅仅是简单类，但它继承自 `RequestHandler<T>`，MediatR 确保其使用正确负载（命令）调用。

## <a name="apply-cross-cutting-concerns-when-processing-commands-with-the-behaviors-in-mediatr"></a>使用 MediatR 中的行为处理命令时，应用整合问题

还需要执行一个操作：将整合问题应用到转存进程管道。 还可在 Autofac 注册模块代码的末尾查看其如何注册行为类型，特别是 LoggingBehavior 类和 ValidatorBehavior 类。 但是还可以添加其他自定义行为。

```csharp
public class MediatorModule : Autofac.Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterAssemblyTypes(typeof(IMediator).GetTypeInfo().Assembly)
            .AsImplementedInterfaces();

        // Register all the Command classes (they implement IRequestHandler)
        // in assembly holding the Commands
        builder.RegisterAssemblyTypes(
                              typeof(CreateOrderCommand).GetTypeInfo().Assembly).
                                   AsClosedTypesOf(typeof(IRequestHandler<,>));
        // Other types registration
        //...
        builder.RegisterGeneric(typeof(LoggingBehavior<,>)).
                                                   As(typeof(IPipelineBehavior<,>));
        builder.RegisterGeneric(typeof(ValidatorBehavior<,>)).
                                                   As(typeof(IPipelineBehavior<,>));
    }
}
```

[LoggingBehavior](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Behaviors/LoggingBehavior.cs) 类可像以下代码那样实现 - 记录执行的命令处理程序的信息，以及是否成功。

```csharp
public class LoggingBehavior<TRequest, TResponse>
         : IPipelineBehavior<TRequest, TResponse>
{
    private readonly ILogger<LoggingBehavior<TRequest, TResponse>> _logger;
    public LoggingBehavior(ILogger<LoggingBehavior<TRequest, TResponse>> logger) =>
                                                                  _logger = logger;

    public async Task<TResponse> Handle(TRequest request,
                                        RequestHandlerDelegate<TResponse> next)
    {
        _logger.LogInformation($"Handling {typeof(TRequest).Name}");
        var response = await next();
        _logger.LogInformation($"Handled {typeof(TResponse).Name}");
        return response;
    }
}
```

只需通过实现此行为类并在管道（在上面的 MediatorModule 中）中注册，所有通过 MediatR 处理的命令都会记录有关执行的信息。

eShopOnContainers 订购微服务还会应用基本验证的第二个行为，即依赖 [FluentValidation](https://github.com/JeremySkinner/FluentValidation) 库的 [ValidatorBehavior](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Behaviors/ValidatorBehavior.cs) 类，如以下代码所示：

```csharp
public class ValidatorBehavior<TRequest, TResponse>
         : IPipelineBehavior<TRequest, TResponse>
{
    private readonly IValidator<TRequest>[] _validators;
    public ValidatorBehavior(IValidator<TRequest>[] validators) =>
                                                         _validators = validators;

    public async Task<TResponse> Handle(TRequest request,
                                        RequestHandlerDelegate<TResponse> next)
    {
        var failures = _validators
            .Select(v => v.Validate(request))
            .SelectMany(result => result.Errors)
            .Where(error => error != null)
            .ToList();

        if (failures.Any())
        {
            throw new OrderingDomainException(
                $"Command Validation Errors for type {typeof(TRequest).Name}",
                        new ValidationException("Validation exception", failures));
        }

        var response = await next();
        return response;
    }
}
```

如果验证失败，则此处的行为会引发异常，但是也可以返回结果对象，在成功时包含命令结果，或在未成功时包含验证消息。 这样便可以更轻松地向用户显示验证结果。

然后将根据 [FluentValidation](https://github.com/JeremySkinner/FluentValidation) 库为通过 CreateOrderCommand 传递的数据创建验证，如以下代码所示：

```csharp
public class CreateOrderCommandValidator : AbstractValidator<CreateOrderCommand>
{
    public CreateOrderCommandValidator()
    {
        RuleFor(command => command.City).NotEmpty();
        RuleFor(command => command.Street).NotEmpty();
        RuleFor(command => command.State).NotEmpty();
        RuleFor(command => command.Country).NotEmpty();
        RuleFor(command => command.ZipCode).NotEmpty();
        RuleFor(command => command.CardNumber).NotEmpty().Length(12, 19);
        RuleFor(command => command.CardHolderName).NotEmpty();
        RuleFor(command => command.CardExpiration).NotEmpty().Must(BeValidExpirationDate).WithMessage("Please specify a valid card expiration date");
        RuleFor(command => command.CardSecurityNumber).NotEmpty().Length(3);
        RuleFor(command => command.CardTypeId).NotEmpty();
        RuleFor(command => command.OrderItems).Must(ContainOrderItems).WithMessage("No order items found");
    }

    private bool BeValidExpirationDate(DateTime dateTime)
    {
        return dateTime >= DateTime.UtcNow;
    }

    private bool ContainOrderItems(IEnumerable<OrderItemDTO> orderItems)
    {
        return orderItems.Any();
    }
}

```

可以创建其他验证。 这是实现命令验证的一种简洁、巧妙的方法。

类似地，可实现其他方面的其他行为或要应用到命令的整合问题（需要处理它们时）。

#### <a name="additional-resources"></a>其他资源

##### <a name="the-mediator-pattern"></a>转存进程模式

- **转存进程模式** \
  [https://en.wikipedia.org/wiki/Mediator\_pattern](https://en.wikipedia.org/wiki/Mediator_pattern)

##### <a name="the-decorator-pattern"></a>修饰器模式

- **修饰器模式** \
  [https://en.wikipedia.org/wiki/Decorator\_pattern](https://en.wikipedia.org/wiki/Decorator_pattern)

##### <a name="mediatr-jimmy-bogard"></a>MediatR (Jimmy Bogard)

- **MediatR。** GitHub 存储库。 \
  <https://github.com/jbogard/MediatR>

- **结合使用 CQRS 与 MediatR 和 AutoMapper** \
  <https://lostechies.com/jimmybogard/2015/05/05/cqrs-with-mediatr-and-automapper/>

- **简化控制器：POST 和命令。** \
  <https://lostechies.com/jimmybogard/2013/12/19/put-your-controllers-on-a-diet-posts-and-commands/>

- **解决转存进程管道的整合问题** \
  <https://lostechies.com/jimmybogard/2014/09/09/tackling-cross-cutting-concerns-with-a-mediator-pipeline/>

- **CQRS 和 REST：完美组合** \
  <https://lostechies.com/jimmybogard/2016/06/01/cqrs-and-rest-the-perfect-match/>

- **MediatR 管道示例** \
  <https://lostechies.com/jimmybogard/2016/10/13/mediatr-pipeline-examples/>

- **MediatR 和 ASP.NET Core 的垂直切片测试固定例程** \
  <https://lostechies.com/jimmybogard/2016/10/24/vertical-slice-test-fixtures-for-mediatr-and-asp-net-core/>

- **适用于 Microsoft 依赖注入的 MediatR 扩展已发布** \
  <https://lostechies.com/jimmybogard/2016/07/19/mediatr-extensions-for-microsoft-dependency-injection-released/>

##### <a name="fluent-validation"></a>Fluent 验证

- **Jeremy Skinner。FluentValidation.** GitHub 存储库。 \
  <https://github.com/JeremySkinner/FluentValidation>

> [!div class="step-by-step"]
> [上一页](microservice-application-layer-web-api-design.md)
> [下一页](../implement-resilient-applications/index.md)
