---
title: 数据访问和管理
description: 了解如何访问和处理 ASP.NET Web 窗体和中的数据 Blazor 。
author: csharpfritz
ms.author: jefritz
no-loc:
- Blazor
ms.date: 11/20/2020
ms.openlocfilehash: 66e6001cbcac612cb556e90fb86fd694ca7d1459
ms.sourcegitcommit: 2f485e721f7f34b87856a51181b5b56624b31fd5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96509749"
---
# <a name="work-with-data"></a>处理数据

数据访问是 ASP.NET Web 窗体应用的主干。 如果要为 web 构建窗体，该数据会发生什么情况呢？ 使用 Web 窗体，可以使用多个数据访问技术来与数据库进行交互：

- “数据源”
- ADO.NET
- Entity Framework

数据源是您可以放置在 Web 窗体页上的控件，并配置为与其他控件一样。 Visual Studio 提供了一组友好对话框，可用于配置控件并将控件绑定到 Web 窗体页。 在首次发布 Web 窗体时，喜欢 "低代码" 或 "无代码" 方法首选此方法的开发人员。

![“数据源”](media/data/datasources.png)

ADO.NET 是与数据库交互的低级别方法。 您的应用程序可以使用命令、记录集和用于交互的数据集创建与数据库的连接。 然后，可以将结果绑定到屏幕上的字段，而无需太多代码。 此方法的缺点是， (、和) 的每一组 ADO.NET 对象都 `Connection` `Command` `Recordset` 绑定到数据库供应商提供的库。 使用这些组件将使代码变得非常复杂且难以迁移到其他数据库。

## <a name="entity-framework"></a>Entity Framework

实体框架 (EF) 是由 .NET Foundation 维护的开源对象关系映射框架。 最初通过 .NET Framework 发布，EF 允许为数据库连接、存储架构和交互生成代码。 通过此抽象，你可以专注于应用的业务规则，并允许受信任的数据库管理员管理数据库。 在 .NET 中，可以使用名为 EF Core 的 EF 的更新版本。 EF Core 有助于使用可供你使用命令行工具的一系列命令，生成和维护你的代码和数据库之间的交互 `dotnet ef` 。 让我们看几个示例来帮助你使用数据库。

### <a name="ef-code-first"></a>EF Code First

开始构建数据库交互的快速方法是从要使用的类对象开始。 EF 提供了一个工具，可帮助为你的类生成相应的数据库代码。 此方法称为 "Code First" 开发。 `Product`对于要存储在关系数据库（如 Microsoft SQL Server）中的示例店面应用，请考虑以下类。

```csharp
public class Product
{
    public int Id { get; set; }

    [Required]
    public string Name { get; set; }

    [MaxLength(4000)]
    public string Description { get; set; }

    [Range(0, 99999,99)]
    [DataType(DataType.Currency)]
    public decimal Price { get; set; }
}
```

Product 有一个主键，另外还会在数据库中创建三个字段：  

- EF `Id` 按约定将属性标识为主键。
- `Name` 将存储在为文本存储配置的列中。 `[Required]`修饰此属性的属性将添加 `not null` 约束，以帮助强制实施该属性的声明行为。
- `Description` 将存储在为文本存储配置的列中，并且最大长度为4000个字符，由属性所指示 `[MaxLength]` 。 将使用数据类型为的列配置数据库架构 `MaxLength` `varchar(4000)` 。
- `Price`属性将存储为货币。 `[Range]`属性将生成相应的约束，以防止在声明的最小值和最大值之外进行数据存储。

需要将此类添加 `Product` 到定义了数据库的连接和转换操作的数据库上下文类中。

```csharp
public class MyDbContext : DbContext
{
    public DbSet<Product> Products { get; set; }
}
```

`MyDbContext`类提供了一个属性，该属性定义类的访问和转换 `Product` 。  应用程序使用类的方法中的以下项，将此类配置为与数据库进行交互 `Startup` `ConfigureServices` ：

```csharp
services.AddDbContext<MyDbContext>(options =>
    options.UseSqlServer("MY DATABASE CONNECTION STRING"));
```

前面的代码将使用指定的连接字符串连接到 SQL Server 数据库。 可以将连接字符串放置在 *appsettings.js* 文件、环境变量或其他配置存储位置，并适当地替换此嵌入字符串。

然后，可以使用以下命令生成适用于此类的数据库表：

```dotnetcli
dotnet ef migrations add 'Create Product table'
dotnet ef database update
```

第一个命令定义要对数据库架构进行的更改，作为名为的新 EF 迁移 `Create Product table` 。  迁移定义了如何应用和删除新的数据库更改。

应用后，你可以在数据库中创建一个简单的 `Product` 表，并在项目中添加了一些新类，以帮助管理数据库架构。  默认情况下，你可以在名为 " *迁移*" 的新文件夹中找到这些生成的类。  当你对类进行更改 `Product` 或添加更多相关的类来与数据库进行交互时，需要使用迁移的新名称再次运行命令行命令。  此命令将生成另一组迁移类以更新数据库架构。

### <a name="ef-database-first"></a>EF Database First

对于现有数据库，可以使用 .NET 命令行工具生成 EF Core 的类。 若要基架类，请使用以下命令的变体：

```dotnetcli
dotnet ef dbcontext scaffold "CONNECTION STRING" Microsoft.EntityFrameworkCore.SqlServer -c MyDbContext -t Product -t Customer
```

上述命令使用指定的连接字符串和提供程序连接到数据库 `Microsoft.EntityFrameworkCore.SqlServer` 。 连接后，将创建一个名为的数据库上下文类 `MyDbContext` 。 此外，还会为 `Product` `Customer` 与选项一起指定的和表创建支持类 `-t` 。 此命令有很多配置选项，可生成适用于数据库的类层次结构。 有关完整参考，请参阅 [命令的文档](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)。

有关 [EF Core](/ef/core/) 的详细信息可在 Microsoft Docs 站点上找到。

## <a name="interact-with-web-services"></a>与 web 服务进行交互

首次发布 ASP.NET 时，SOAP 服务是 web 服务器和客户端交换数据的首选方式。 自那时起，这种情况已发生变化，而与服务的首选交互已移动到直接 HTTP 客户端交互。 通过 ASP.NET Core 和 Blazor ，你可以 `HttpClient` 在 `Startup` 类的方法中注册的配置 `ConfigureServices` 。 需要与 HTTP 终结点交互时，请使用该配置。 请考虑以下配置代码：

```csharp
services.AddHttpClient("github", client =>
{
    client.BaseAddress = new Uri("http://api.github.com/");
    // Github API versioning
    client.DefaultRequestHeaders.Add("Accept", "application/vnd.github.v3+json");
    // Github requires a user-agent
    client.DefaultRequestHeaders.Add("User-Agent", "BlazorWebForms-Sample");
});
```

每当需要访问 GitHub 中的数据时，请创建名称为的客户端 `github` 。 为客户端配置了基址，并正确设置了请求标头。 将 `IHttpClientFactory` Blazor `@inject` 指令或属性上的属性注入到组件 `[Inject]` 。 使用以下语法创建命名客户端并与服务交互：

```razor
@inject IHttpClientFactory factory

...

@code {
    protected override async Task OnInitializedAsync()
    {
        var client = factory.CreateClient("github");
        var response = await client.GetAsync("repos/dotnet/docs/issues");
        response.EnsureStatusCode();
        var content = await response.Content.ReadAsStringAsync();
    }
}
```

此方法返回描述 *dotnet/文档* GitHub 存储库中的问题集合的字符串。 它将返回 JSON 格式的内容，并将其反序列化为适当的 GitHub 问题对象。 可以通过多种方式配置 `HttpClientFactory` 来提供预先配置的 `HttpClient` 对象。 尝试使用 `HttpClient` 不同的名称和终结点为你使用的各种 web 服务配置多个实例。 此方法将使你与这些服务的交互更易于在每个页面上使用。 有关更多详细信息，请参阅 [IHttpClientFactory 的文档](/aspnet/core/fundamentals/http-requests)。

>[!div class="step-by-step"]
>[上一页](forms-validation.md)
>[下一页](middleware.md)
