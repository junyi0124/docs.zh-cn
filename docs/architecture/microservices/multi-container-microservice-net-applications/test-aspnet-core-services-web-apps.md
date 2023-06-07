---
title: 测试 ASP.NET Core 服务和 Web 应用
description: 适用于容器化 .NET 应用程序的 .NET 微服务体系结构 | 探索用于在容器中测试 ASP.NET Core 服务和 Web 应用的体系结构。
ms.date: 08/07/2020
ms.openlocfilehash: 67872668781d8ae5d79bf360aee73f744cf4404b
ms.sourcegitcommit: 635a0ff775d2447a81ef7233a599b8f88b162e5d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97633944"
---
# <a name="testing-aspnet-core-services-and-web-apps"></a>测试 ASP.NET Core 服务和 Web 应用

控制器是 ASP.NET Core API 服务和 ASP.NET MVC Web 应用程序的核心部分。 因此，应该对它们会按照应用程序所计划的那样正常运行有信心。 自动测试可以给你这种信心，还可在生产前检测错误。

需要根据有效或无效输入测试控制器的运行状况，并根据其执行的业务操作的结果测试控制器的响应情况。 但是，应对微服务进行这些类型的测试：

- 单元测试。 这可确保应用程序的各个组件按预期工作。 断言测试组件 API。

- 集成测试。 这可确保对于数据库等外部项目，组件交互按预期工作。 断言可以测试组件 API、UI 或数据库 I/O、登录等操作的副作用。

- 每个微服务的的功能测试。 这可确保从用户角度，应用程序按预期运行。

- 服务测试。 这可确保测试端到端服务用例，包括同时测试多个服务。 对于此类型的测试，需要首先准备环境。 在这种情况下，这意味着启动服务（例如，通过使用 docker-compose up）。

### <a name="implementing-unit-tests-for-aspnet-core-web-apis"></a>实现 ASP.NET Core Web API 的单元测试

单元测试涉及在测试应用程序部分时，使之与它的基础结构和依赖关系相隔离。 单元测试控制器逻辑时，仅测试单个操作或方法的内容，不测试其依赖项或框架自身的行为。 单位测试不检测部件间交互的问题，这是集成测试的用途。

单元测试控制器操作时，请确保仅关注其行为。 控制器单元测试可避免筛选器、路由或模型绑定（请求数据到 ViewModel 或 DTO 的映射）等情况。 由于仅专注测试一项内容，单位测试通常编写简单、运行迅速。 正确编写的单位测试集无需过多开销即可经常运行。

单元测试基于 xUnit.net、MSTest、Moq 或 NUnit 等测试框架实现。 对于 eShopOnContainers 示例应用程序，我们使用 xUnit。

为 Web API 控制器编写单元测试时，通过使用 C\# 中的新关键字直接实例化控制器类，以便尽快运行测试。 下面的示例演示如何将 [xUnit](https://xunit.net/) 作为测试框架执行此操作。

```csharp
[Fact]
public async Task Get_order_detail_success()
{
    //Arrange
    var fakeOrderId = "12";
    var fakeOrder = GetFakeOrder();

    //...

    //Act
    var orderController = new OrderController(
        _orderServiceMock.Object,
        _basketServiceMock.Object,
        _identityParserMock.Object);

    orderController.ControllerContext.HttpContext = _contextMock.Object;
    var actionResult = await orderController.Detail(fakeOrderId);

    //Assert
    var viewResult = Assert.IsType<ViewResult>(actionResult);
    Assert.IsAssignableFrom<Order>(viewResult.ViewData.Model);
}
```

### <a name="implementing-integration-and-functional-tests-for-each-microservice"></a>为每个微服务实现集成和功能测试

如上所述，集成测试和功能测试具有不同的用途和目标。 但是，测试 ASP.NET Core 控制器时实现两者的方式类似，所以在此部分中，我们着重介绍集成测试。

集成测试可确保应用程序的组件在组装后正常运行。 ASP.NET Core 使用单元测试框架和可用于处理请求（无网络费用）的内置测试 web 主机支持集成测试。

与单元测试不同，集成测试经常产生应用程序基础结构问题，例如数据库、文件系统、网络资源或 web 请求和响应。 单元测试使用虚设或 mock 对象解决这些问题。 但集成测试的目的是确保系统按预期正常运行，因此对于集成测试，不能使用虚设或 mock 对象。 可以改为使用基础结构，如数据库访问或从其他服务中调用服务。

与单元测试相比，集成测试执行较多的代码片段，且集成测试依赖基础结构元素，因此它的速度比单元测试慢几个数量级。 因此，最好限制编写和运行的集成测试数量。

ASP.NET Core 包括可用于处理 HTTP 请求且无网络费用的内置测试 web 主机，这意味着可比使用真正的 Web 主机时更快地运行这些测试。 NuGet 组件中，测试 web 主机 (TestServer) 作为 Microsoft.AspNetCore.TestHost 提供。 可将其添加到集成测试项目，并用于托管 ASP.NET Core 应用程序。

如以下代码所示，创建 ASP.NET Core 控制器的集成测试时，通过测试主机实例化控制器。 这相当于 HTTP 请求，但它的运行速度更快。

```csharp
public class PrimeWebDefaultRequestShould
{
    private readonly TestServer _server;
    private readonly HttpClient _client;

    public PrimeWebDefaultRequestShould()
    {
        // Arrange
        _server = new TestServer(new WebHostBuilder()
           .UseStartup<Startup>());
           _client = _server.CreateClient();
    }

    [Fact]
    public async Task ReturnHelloWorld()
    {
        // Act
        var response = await _client.GetAsync("/");
        response.EnsureSuccessStatusCode();
        var responseString = await response.Content.ReadAsStringAsync();
        // Assert
        Assert.Equal("Hello World!", responseString);
    }
}
```

#### <a name="additional-resources"></a>其他资源

- **Steve Smith.测试控制器** (ASP.NET Core) \
    [https://docs.microsoft.com/aspnet/core/mvc/controllers/testing](/aspnet/core/mvc/controllers/testing)

- **Steve Smith.集成测试** (ASP.NET Core) \
    [https://docs.microsoft.com/aspnet/core/test/integration-tests](/aspnet/core/test/integration-tests)

- **在 .NET Core 中使用 dotnet test 的单元测试** \
    [https://docs.microsoft.com/dotnet/core/testing/unit-testing-with-dotnet-test](../../../core/testing/unit-testing-with-dotnet-test.md)

- **xUnit.net**。 官方网站。 \
    <https://xunit.net/>

-  单元测试基本信息。 \
    [https://docs.microsoft.com/visualstudio/test/unit-test-basics](/visualstudio/test/unit-test-basics)

- **Moq**。 GitHub 存储库。 \
    <https://github.com/moq/moq>

- **NUnit**。 官方网站。 \
    <https://nunit.org/>

### <a name="implementing-service-tests-on-a-multi-container-application"></a>在多容器应用程序上实现服务测试

如前所述，在测试多容器应用程序时，所有微服务需要在 Docker 主机或容器群集内运行。 包含有关一系列微服务的多个操作的端到端服务测试要求通过运行 docker-compose up（如果使用业务流程协调程序，则运行同等机制），在 Docker 主机中部署和启动整个应用程序。 整个应用程序及其所有服务运行后，可以执行端到端集成和功能测试。

可以使用多种方法。 在用于部署应用程序的 docker-compose.yml 文件 中，可在解决方案级别扩展入口点，以使用 [dotnet 测试](../../../core/tools/dotnet-test.md)。 还可以使用将在设置为目标的映像中运行测试的其他 compose 文件。 通过使用包括容器上的微服务和数据库的其他集成测试 compose 文件，可确保运行测试前相关数据始终重置为其初始状态。

compose 应用程序运行后，如果运行 Visual Studio，可利用断点和异常。 或者可在 Azure DevOps Services 或支持 Docker 容器的其他任何 CI/CD 系统中的 CI 管道中自动运行集成测试。

## <a name="testing-in-eshoponcontainers"></a>在 eShopOnContainers 中进行测试

参考应用程序 (eShopOnContainers) 测试最近进行了重构，现在有四种类别：

1. 单元  测试，只是普通的旧式常规单元测试，包含在 {MicroserviceName}.UnitTests  项目中

2. 微服务功能/集成测试  ，具有涉及每个微服务的基础结构但相互独立的测试用例，包含在 {MicroserviceName}.FunctionalTests  项目中。

3. **应用程序功能/集成测试**，侧重于微服务集成，具有执行多个微服务的测试用例。 这些测试位于项目 Application.FunctionalTests  中。

单元测试和集成测试在微服务项目中的测试文件夹中进行组织，而应用程序和负载测试是在根文件夹下单独管理的，如图 6-25 所示。

![VS 指出解决方案中某些测试项目的屏幕截图。](./media/test-aspnet-core-services-web-apps/eshoponcontainers-test-folder-structure.png)

**图 6-25**。 eShopOnContainers 中的测试文件夹结构

微服务和应用程序功能/集成测试使用常规测试运行程序从 Visual Studio 运行，但首先需要启动所需基础结构服务（通过一组包含在解决方案测试文件夹中的 docker-compose 文件）：

docker-compose-test.yml 

```yml
version: '3.4'

services:
  redis.data:
    image: redis:alpine
  rabbitmq:
    image: rabbitmq:3-management-alpine
  sqldata:
    image: mcr.microsoft.com/mssql/server:2017-latest
  nosqldata:
    image: mongo
```

docker-compose-test.override.yml 

```yml
version: '3.4'

services:
  redis.data:
    ports:
      - "6379:6379"
  rabbitmq:
    ports:
      - "15672:15672"
      - "5672:5672"
  sqldata:
    environment:
      - SA_PASSWORD=Pass@word
      - ACCEPT_EULA=Y
    ports:
      - "5433:1433"
  nosqldata:
    ports:
      - "27017:27017"
```

因此，若要运行功能/集成测试，必须首先从解决方案测试文件夹运行此命令：

```console
docker-compose -f docker-compose-test.yml -f docker-compose-test.override.yml up
```

正如你所见，这些 docker-compose 文件仅启动 Redis、RabbitMQ、SQL Server 和 MongoDB 微服务。

### <a name="additional-resources"></a>其他资源

- eShopOnContainers 上的单元和集成测试 \
    <https://github.com/dotnet-architecture/eShopOnContainers/wiki/Unit-and-integration-testing>

- eShopOnContainers  上的负载测试 \
    <https://github.com/dotnet-architecture/eShopOnContainers/wiki/Load-testing>

> [!div class="step-by-step"]
> [上一页](subscribe-events.md)
> [下一页](background-tasks-with-ihostedservice.md)
