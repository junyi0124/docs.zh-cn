---
title: 测试 ASP.NET Core MVC 应用
description: 使用 ASP.NET Core 和 Azure 构建新式 Web 应用程序 | 测试 ASP.NET Core MVC 应用
author: ardalis
ms.author: wiwagn
ms.date: 12/01/2020
ms.openlocfilehash: b253cfb90487cc462b0f3b8a7564c97ad403aa06
ms.sourcegitcommit: 45c7148f2483db2501c1aa696ab6ed2ed8cb71b2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/08/2020
ms.locfileid: "96851394"
---
# <a name="test-aspnet-core-mvc-apps"></a>测试 ASP.NET Core MVC 应用

> “如果你不喜欢对产品进行单元测试，很可能你的客户也不喜欢这样做。” 
 > \_- 匿名-

任何复杂程度的软件在响应更改方面皆可能意外失败。 因此，更改后，除最普通（或关键性最低）的应用程序外，其他所有应用程序均需测试。 手动测试是最慢、最不可靠且最昂贵的软件测试方式。 遗憾的是，如果应用程序在设计上不具备可测试性，这可能是唯一可用的方式。 为了遵循[第 4 章](architectural-principles.md)中列出的体系结构原则而编写的应用程序应为单元可测试的。 ASP.NET Core 应用程序支持自动集成和功能测试。

## <a name="kinds-of-automated-tests"></a>自动测试类型

软件应用程序自动测试具有多种类型。 最简单最低级别的测试是单元测试。 级别稍高的测试包括集成测试和功能测试。 其他类型的测试不在本文讨论范围之内，例如 UI 测试、负载测试、压力测试和版本验收测试。

### <a name="unit-tests"></a>单元测试

单元测试可测试应用程序逻辑的单个部分。 可通过否定列举方式对其进行进一步描述。 单元测试并不测试代码如何处理依赖项或基础结构 - 这是集成测试的用途。 单元测试不测试编写代码所用的框架，你应假定其适用，如果不适用，请报告 bug 并编写代码解决。 单元测试完全在内存或进程中运行。 它不会与文件系统、网络或数据库通信。 单元测试仅测试代码。

因为单元测试仅测试单个代码单元，且无外部依赖项，所以执行速度应该非常快。 因此，可在数秒内运行数百个单元测试的测试套件。 请经常运行单元测试，最好是在每次推送到共享源代码管理存储库前运行，当然还要在生成服务器上的每个自动生成时运行它们。

### <a name="integration-tests"></a>集成测试

尽管建议封装与数据库和文件系统等基础结构交互的代码，但是仍会剩下一些此类代码，你可能需要对其进行测试。 应用程序依赖项完全解析时，还应验证代码层是否按预期方式交互。 此功能是集成测试的目的。 与单元测试相比，由于集成测试通常依赖外部依赖项和基础结构，因此其设置速度较慢，难度较大。 因此，应避免测试可以通过集成测试中的单元测试进行测试的项。 如果可通过单元测试测试给定方案，请使用单元测试进行测试。 如果不能，再考虑使用集成测试。

与单元测试相比，集成测试通常具有更复杂的设置和拆卸过程。 例如，针对实际数据库运行的集成测试，在每次运行测试前，需要通过一种方式将数据库返回到已知状态。 随着不断添加新测试以及生产数据库架构不断发展，这些测试脚本的大小和复杂性会逐渐增加。 对于许多大型系统，将签入共享源代码管理更改前，在开发人员工作站上运行一整套集成测试并不实际。 这种情况下，可在生成服务器上运行集成测试。

### <a name="functional-tests"></a>功能测试

集成测试从开发者角度编写，用于验证系统的一些组件是否能共同正常运行。 功能测试从用户角度编写，用于基于其要求验证系统的正确性。 下面的节选提供了一个有用的类比，有助于理解功能测试与单元测试的区别：

> “很多时候，开发系统类似于修建房屋。 虽然这个类比并不完全准确，但是我们可将其延伸，用以理解单元测试与功能测试的区别。 单元测试类似于巡查房屋建筑工地的建筑检查员。 建筑检查员专注于房屋的各种内部系统、地基、框架、电路、管道等。 他确保（测试）房屋各部分功能正常且安全，即符合建筑规范。 在这种情景下，功能测试类似于出现在同一建筑工地上的房主。 他假定房屋内部系统一切正常，建筑检查员履行了其检查职责。 房主关心的是住在这个房屋里的体验。 他关心房屋外观如何、每个房间是否大小合适、房屋是否能满足家庭需要，以及窗外是否有好的风景。 也就是说，房主对房屋执行功能测试。 他是站在用户角度。 建筑检察员则是对房屋进行单元测试。 他站在建筑商角度。”

源：[单元测试与功能测试](https://www.softwaretestingtricks.com/2007/01/unit-testing-versus-functional-tests.html)

我认为：“作为开发人员，我们的失败可能体现在两方面：我们构建应用的方式错误，或者我们的应用不能满足客户需求。” 单元测试可确保构建应用的方式正确；功能测试可确保我们的应用可满足客户需求。

由于功能测试在系统级别运行，所以可能需要一定程度的 UI 自动化。 与集成测试一样，它们通常也适用于某种类型的测试基础结构。 此活动使其比单元测试和集成测试更慢、更易发生故障。 应仅运行确保系统按用户期望运行所需数量的功能测试。

### <a name="testing-pyramid"></a>测试金字塔

Martin Fowler 提出了测试金字塔概念，如图 9-1 所示。

![测试金字塔](./media/image9-1.png)

**图 9-1**。 测试金字塔

该金字塔的不同层次及其相对大小表示不同的测试类别，以及应为应用程序编写的测试数量。 如图所示，建议以大量单元测试作为基层，中间以较小的集成测试层作为支持，顶端为更小的功能测试层。 理想情况下，每层应仅包含较低层中无法充分执行的测试。 确定特定方案所需的测试类型时，请考虑此测试金字塔。

### <a name="what-to-test"></a>要测试的内容

对缺乏编写自动测试经验的开发人员而言，一个经常遇到的问题是确定测试内容。 不妨从测试条件逻辑开始。 如果方法中包含基于条件语句（if-else、switch 等）更改的行为，应能至少编写数个用于在某些条件下确定行为是否正确的测试。 如果代码存在错误条件，建议通过代码为“主逻辑”编写至少一个测试（不含任何错误），并为“副逻辑”编写至少一个测试（含错误或非典型结果），以确保应用程序在错误情况下按预期运行。 最后，重点测试可能失败的项，而不是关注代码覆盖率等指标。 一般而言，高代码覆盖率比低代码覆盖率更好。 但是，与仅仅为了改善测试代码覆盖率指标而编写自动属性测试相比，编写复杂的业务关键型方法的测试更有意义。

## <a name="organizing-test-projects"></a>组织测试项目

可按最适合你的方式组织测试项目。 最好按照类型（单元测试和集成测试）以及测试内容（按项目和按命名空间）分离测试。 此分离由单个测试项目内的文件夹构成，还是由多个测试项目构成，这取决于设计决策。 虽然一个项目最为简单，但是对于包含多个测试的大型项目，或者为了更轻松地运行不同测试集，可能需要具有一系列不同的测试项目。 许多团队基于要测试的项目组织测试项目，对于具有很多项目的应用程序而言，这可能导致出现大量测试项目，在根据每个项目中的测试类型进行细分的情况下更是如此。 一种折衷方式是让每个测试类型、每个应用程序具有一个项目，测试项目内的文件夹指示要测试的项目（和类）。

一种常见方式是在“src”文件夹下组织应用程序项目，在并行的“tests”文件夹下组织应用程序的测试项目。 如果你认为这种组织方式有用，可以在 Visual Studio 中创建匹配的解决方案文件夹。

![解决方案中的测试组织](./media/image9-2.png)

**图 9-2**。 解决方案中的测试组织

可使用你喜欢的任何测试框架。 xUnit 框架有效运行，编写所有 ASP.NET Core 和 EF Core 测试时皆使用此框架。 可使用图 9-3 中所示的模板或从使用 `dotnet new xunit` 的 CLI，在 Visual Studio 中添加 xUnit 测试项目。

![在 Visual Studio 中添加 xUnit 测试项目](./media/image9-3.png)

**图 9-3**。 在 Visual Studio 中添加 xUnit 测试项目

### <a name="test-naming"></a>测试命名

以一致方式命名测试，名称指示每个测试的功能。 我使用的一种很成功的方式是根据其测试的类和方法命名测试类。 虽然此方法会产生许多小测试类，但是可非常清楚地表明每个测试的作用。 在设置测试类名称以标识待测试的类和方法后，测试方法名称可用于指定要测试的行为。 此名称应包括预期行为以及任何应导致该行为的输入或假设。 部分示例测试名称：

- `CatalogControllerGetImage.CallsImageServiceWithId`

- `CatalogControllerGetImage.LogsWarningGivenImageMissingException`

- `CatalogControllerGetImage.ReturnsFileResultWithBytesGivenSuccess`

- `CatalogControllerGetImage.ReturnsNotFoundResultGivenImageMissingException`

此方式的一种变体是让每个测试类名称以“Should”结尾，并稍微修改其时态：

- `CatalogControllerGetImage`**Should**`.`**Call**`ImageServiceWithId`

- `CatalogControllerGetImage`**Should**`.`**Log**`WarningGivenImageMissingException`

虽然第二种命名方式略为冗长，但一些团队发现这种命名方式更加清楚。 任何情况下，请使用可提供测试行为见解的命名约定，以便一个或多个测试失败时，可通过其名称清楚了解已失败的事例。 避免使用 ControllerTests.Test1 等模糊的测试名称，因为查看测试结果时，此类名称不能提供任何价值。

如果使用类似上述会产生众多小测试类的命名约定，建议使用文件夹和命名空间进一步组织测试。 图 9-4 显示了一种在数个测试项目内按照文件夹组织测试的方式。

![基于要测试的类按照文件夹组织测试类](./media/image9-4.png)

**图 9-4**。 基于要测试的类按照文件夹组织测试类。

如果特定应用程序类具有多个待测试的方法（以及由此产生的众多测试类），最好将这些类放置于该应用程序类对应的文件夹中。 该组织方式与在其他情况下将文件组织到文件夹并无差别。 如果一个包含许多其他文件的文件夹中具有三个或四个以上相关文件，最好将其移动到它们自己的子文件夹中。

## <a name="unit-testing-aspnet-core-apps"></a>对 ASP.NET Core 应用执行单元测试

在具有出色设计的 ASP.NET Core 应用程序中，大多数复杂性和业务逻辑会封装在业务实体以及各种服务中。 ASP.NET Core MVC 应用本身及其控制器、筛选器、视图模型和视图需要的单元测试很少。 特定操作的很多功能体现在该操作方法之外。 使用单元测试无法有效地测试路由或全局错误处理是否正常工作。 同样，无法使用针对控制器的操作方法的测试对任何筛选器（包括模型验证筛选器以及身份验证和授权筛选器）执行单元测试。 如果没有这些行为源，大多数操作方法应非常小，这会将其大量工作委托至服务（可独立于使用这些服务的控制器对这些服务执行测试）。

有时为对代码执行单元测试，需要重构代码。 通常，此活动涉及到确定抽象以及使用依赖项注入来访问待测试代码中的抽象，而不是直接针对基础结构编码。 例如，请思考以下用于显示图像的简单操作方法：

```csharp
[HttpGet("[controller]/pic/{id}")]
public IActionResult GetImage(int id)
{
    var contentRoot = _env.ContentRootPath + "//Pics";
    var path = Path.Combine(contentRoot, id + ".png");
    Byte[] b = System.IO.File.ReadAllBytes(path);
    return File(b, "image/png");
}
```

通过 `System.IO.File` 上的直接依赖项难以对此方法执行单元测试。 可测试此行为以确保其按预期方式运行，但对实际文件执行此操作属于集成测试。 值得注意的是，无法对此方法的路由进行单元测试 - 我们很快将讲解如何通过功能测试来进行此测试。

如果无法直接对文件系统行为执行单元测试，且无法测试路由，还能测试什么呢？ 通过重构确保单元测试的可行性后，可能会发现一些测试用例以及缺失行为，例如错误处理。 如未找到文件，此方法会执行什么操作？ 它应执行什么操作？ 本示例中，重构方法如下：

```csharp
[HttpGet("[controller]/pic/{id}")]
public IActionResult GetImage(int id)
{
    byte[] imageBytes;
    try
    {
        imageBytes = _imageService.GetImageBytesById(id);
    }
    catch (CatalogImageMissingException ex)
    {
        _logger.LogWarning($"No image found for id: {id}");
        return NotFound();
    }
    return File(imageBytes, "image/png");
}
```

`_logger` 和 `_imageService` 都作为依赖项注入。 现在，可测试是否传递到操作方法的相同 ID 被传递到 `_imageService`，以及生成的字节是否作为 FileResult 的一部分返回。 还可测试错误日志记录是否按预期发生，以及如果映像缺失，是否返回 `NotFound` 结果（假定此行为是重要的应用程序行为，即不只是开发人员为诊断问题而添加的临时代码）。 已将实际文件逻辑移动到单独的实现服务，并已将其扩充，以在缺失文件的情况下返回特定于应用程序的异常。 可使用集成测试独立测试该实现。

在大多数情况下，需要在控制器中使用全局异常处理程序，因此它们中的逻辑量应该是最小的，并且可能不值得进行单元测试。 使用功能测试和下面介绍的 `TestServer` 类对控制器操作进行大部分测试。

## <a name="integration-testing-aspnet-core-apps"></a>对 ASP.NET Core 应用执行集成测试

ASP.NET Core 应用中的大多数集成测试应该是测试基础结构项目中定义的服务和其他实现类型。 例如，可以[测试 EF Core 是否已成功更新并检索](/ef/core/miscellaneous/testing/)希望从驻留在基础结构项目中的数据访问类中获得的数据。 测试 ASP.NET Core MVC 项目是否正常运行的最佳方法是针对在测试主机中运行的应用运行的功能测试。

## <a name="functional-testing-aspnet-core-apps"></a>对 ASP.NET Core 应用执行功能测试

对于 ASP.NET Core 应用程序，`TestServer` 类让功能测试非常易于编写。 可以直接使用 `WebHostBuilder`（或 `HostBuilder`）（针对应用程序的一般操作）或使用 `WebApplicationFactory` 类型（自 2.1 版开始提供）来配置 `TestServer`。 尝试将测试主机与生产主机进行尽可能密切的匹配，以便让测试执行与应用将在生产中进行的行为类似的行为。 `WebApplicationFactory` 类有助于配置 TestServer 的 ContentRoot，该 ContentRoot 由 ASP.NET Core 用于定位静态资源（例如视图）。

可通过创建实现 `IClassFixture\<WebApplicationFactory\<TEntry>>`（其中 `TEntry` 为 Web 应用程序的 `Startup` 类）的测试类来创建简单的功能测试。 接口创建完成后，测试固定例程可使用工厂的 `CreateClient` 方法来创建客户端：

```csharp
public class BasicWebTests : IClassFixture<WebApplicationFactory<Startup>>
{
    protected readonly HttpClient _client;

    public BasicWebTests(WebApplicationFactory<Startup> factory)
    {
        _client = factory.CreateClient();
    }

    // write tests that use _client
}
```

用户常常想要在运行每个测试之前对站点执行一些其他配置，例如将应用程序配置为使用内存中数据存储，然后将测试数据植入应用程序。 若要实现此功能，请创建你自己的 `WebApplicationFactory\<TEntry>` 的子类并替代其 `ConfigureWebHost` 方法。 以下示例来自 eShopOnWeb FunctionalTests 项目，并用作主要 Web 应用上的测试的一部分。

```csharp
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Mvc.Testing;
using Microsoft.EntityFrameworkCore;
using Microsoft.eShopWeb.Infrastructure.Data;
using Microsoft.eShopWeb.Infrastructure.Identity;
using Microsoft.eShopWeb.Web;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;
using System;

namespace Microsoft.eShopWeb.FunctionalTests.Web
{
    public class WebTestFixture : WebApplicationFactory<Startup>
    {
        protected override void ConfigureWebHost(IWebHostBuilder builder)
        {
            builder.UseEnvironment("Testing");

            builder.ConfigureServices(services =>
            {
                 services.AddEntityFrameworkInMemoryDatabase();

                // Create a new service provider.
                var provider = services
                    .AddEntityFrameworkInMemoryDatabase()
                    .BuildServiceProvider();

                // Add a database context (ApplicationDbContext) using an in-memory
                // database for testing.
                services.AddDbContext<CatalogContext>(options =>
                {
                    options.UseInMemoryDatabase("InMemoryDbForTesting");
                    options.UseInternalServiceProvider(provider);
                });

                services.AddDbContext<AppIdentityDbContext>(options =>
                {
                    options.UseInMemoryDatabase("Identity");
                    options.UseInternalServiceProvider(provider);
                });

                // Build the service provider.
                var sp = services.BuildServiceProvider();

                // Create a scope to obtain a reference to the database
                // context (ApplicationDbContext).
                using (var scope = sp.CreateScope())
                {
                    var scopedServices = scope.ServiceProvider;
                    var db = scopedServices.GetRequiredService<CatalogContext>();
                    var loggerFactory = scopedServices.GetRequiredService<ILoggerFactory>();

                    var logger = scopedServices
                        .GetRequiredService<ILogger<WebTestFixture>>();

                    // Ensure the database is created.
                    db.Database.EnsureCreated();

                    try
                    {
                        // Seed the database with test data.
                        CatalogContextSeed.SeedAsync(db, loggerFactory).Wait();

                        // seed sample user data
                        var userManager = scopedServices.GetRequiredService<UserManager<ApplicationUser>>();
                        var roleManager = scopedServices.GetRequiredService<RoleManager<IdentityRole>>();
                        AppIdentityDbContextSeed.SeedAsync(userManager, roleManager).Wait();
                    }
                    catch (Exception ex)
                    {
                        logger.LogError(ex, $"An error occurred seeding the " +
                            "database with test messages. Error: {ex.Message}");
                    }
                }
            });
        }
    }
}
```

测试可以使用此自定义 WebApplicationFactory 来创建客户端，并使用此客户端实例向应用程序作出请求。 该应用程序将具备植入的数据，这些数据可用作测试断言的一部分。 以下测试验证 eShopOnWeb 应用程序的主页是否正确加载并包括已作为种子数据的一部分添加至应用程序的产品列表。

```csharp
using Microsoft.eShopWeb.FunctionalTests.Web;
using System.Net.Http;
using System.Threading.Tasks;
using Xunit;

namespace Microsoft.eShopWeb.FunctionalTests.WebRazorPages
{
    [Collection("Sequential")]
    public class HomePageOnGet : IClassFixture<WebTestFixture>
    {
        public HomePageOnGet(WebTestFixture factory)
        {
            Client = factory.CreateClient();
        }

        public HttpClient Client { get; }

        [Fact]
        public async Task ReturnsHomePageWithProductListing()
        {
            // Arrange & Act
            var response = await Client.GetAsync("/");
            response.EnsureSuccessStatusCode();
            var stringResponse = await response.Content.ReadAsStringAsync();

            // Assert
            Assert.Contains(".NET Bot Black Sweatshirt", stringResponse);
        }
    }
}
```

此功能测试会演练完整的 ASP.NET Core MVC/Razor Pages 应用程序堆栈，包括准备就绪的所有中间件、筛选器和绑定器等。 它验证给定的路由 ("/") 是否返回预期的成功状态代码和 HTML 输出。 它无需设置实际的 Web 服务器即可实现该操作，并且可以避免使用实际 Web 服务器进行测试可能遇到的许多问题（例如防火墙设置问题）。 虽然针对 TestServer 运行的功能测试通常比集成测试和单元测试更慢，但是比测试 Web 服务器的网络上运行的测试速度更快。 使用功能测试来确保应用程序的前端堆栈按预期运行。 当在控制器或页面中发现了重复内容并通过添加筛选器找到了这些重复内容时，这些测试将尤为有用。 理想情况下，此重构不会改变应用程序的行为，并且将有一套功能测试来验证确实如此。

> ### <a name="references--test-aspnet-core-mvc-apps"></a>参考 - 测试 ASP.NET Core MVC 应用
>
> - 在 ASP.NET Core 中进行测试   \
>   <https://docs.microsoft.com/aspnet/core/testing/>
> - 单元测试命名约定   \
>   <https://ardalis.com/unit-test-naming-convention>
> - 测试 EF Core   \
>   <https://docs.microsoft.com/ef/core/miscellaneous/testing/>
> - ASP.NET Core 中的集成测试   \
>   <https://docs.microsoft.com/aspnet/core/test/integration-tests>

>[!div class="step-by-step"]
>[上一页](work-with-data-in-asp-net-core-apps.md)
>[下一页](development-process-for-azure.md)
