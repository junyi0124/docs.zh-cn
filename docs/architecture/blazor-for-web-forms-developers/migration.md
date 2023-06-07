---
title: 从 ASP.NET Web 窗体迁移到 Blazor
description: 了解如何将现有的 ASP.NET Web 窗体应用程序迁移到 Blazor 。
author: twsouthwick
ms.author: tasou
no-loc:
- Blazor
- WebAssembly
ms.date: 11/20/2020
ms.openlocfilehash: 893b6f851681ec540629fe160749b2622b6d5440
ms.sourcegitcommit: 2f485e721f7f34b87856a51181b5b56624b31fd5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96509827"
---
# <a name="migrate-from-aspnet-web-forms-to-no-locblazor"></a>从 ASP.NET Web 窗体迁移到 Blazor

将基本代码从 ASP.NET Web 窗体迁移到 Blazor 是一项需要规划的耗时的任务。 本章概述了该过程。 可以减轻转换的问题是确保应用程序符合 *N 层* 体系结构，在这种情况下，应用模型 (在这种情况下，Web 窗体) 独立于业务逻辑。 这种逻辑分离可以清楚地确定需要移动到 .NET Core 和的内容 Blazor 。

在此示例中，将使用 [GitHub](https://github.com/dotnet-architecture/eShopOnBlazor) 上提供的 eShop 应用。 eShop 是一种目录服务，它通过表单条目和验证提供 CRUD 功能。

为什么应将正在运行的应用程序迁移到 Blazor ？ 很多时候都不需要。 多年来，ASP.NET Web 窗体将继续受到支持。 但是， Blazor 只支持对已迁移的应用程序提供的许多功能。 此类功能包括：

- 框架中的性能改进，例如 `Span<T>`
- 能够运行方式 WebAssembly
- Linux 和 macOS 的跨平台支持
- 应用本地部署或共享框架部署，不影响其他应用

如果这些或其他新功能非常引人注目，则在迁移应用程序时可能有价值。 迁移可以采用不同的形状;它可以是整个应用，或者只是需要更改的某些终结点。 迁移决策最终基于开发人员解决的业务问题。

## <a name="server-side-versus-client-side-hosting"></a>服务器端与客户端宿主

如 " [托管模型](hosting-models.md) " 一章中所述， Blazor 可通过两种不同的方式承载应用：服务器端和客户端。 服务器端模型使用 ASP.NET Core SignalR 连接来管理 DOM 更新，同时在服务器上运行任何实际代码。 客户端模型 WebAssembly 在浏览器中运行，并且不需要服务器连接。 有很多差异可能会影响最适合特定应用的应用：

- 运行方式 WebAssembly 不支持所有功能 (如当前时间线程) 
- 客户端和服务器之间的对话通信可能会导致服务器端模式发生延迟问题
- 对数据库和内部或受保护的服务的访问需要单独的服务和客户端托管

在撰写本文时，服务器端模型更加类似于 Web 窗体。 本章节中的大部分重点介绍服务器端托管模型，因为它是生产就绪的模型。

## <a name="create-a-new-project"></a>创建新项目

此初始迁移步骤是创建一个新项目。 此项目类型基于 .NET 的 SDK 样式项目，并简化了以前项目格式中使用的很多样板。 有关更多详细信息，请参阅 " [项目结构](project-structure.md)" 一章。

创建该项目后，安装上一个项目中使用的库。 在较旧的 Web 窗体项目中，可能已使用 *packages.config* 文件列出了所需的 NuGet 包。 在新的 SDK 样式项目中， *packages.config* 已替换为 `<PackageReference>` 项目文件中的元素。 此方法的优点是所有依赖项都以可传递的方式安装。 你只列出你关注的顶级依赖关系。

你使用的许多依赖项都适用于 .NET，包括实体框架6和 log4net。 如果没有可用的 .NET 或 .NET Standard 版本，则通常可以使用 .NET Framework 版本。 你的里程可能会有所不同。 使用的任何无法在 .NET 中使用的 API 都会导致运行时错误。 Visual Studio 会通知你此类包。 **解决方案资源管理器** 中项目的 "**引用**" 节点上将显示一个黄色图标。

在 Blazor 基于的 eShop 项目中，可以看到已安装的包。 以前， *packages.config* 文件列出了项目中使用的每个包，导致文件的长度几乎为50行。 *packages.config* 的代码段如下：

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
  ...
  <package id="Microsoft.ApplicationInsights.Agent.Intercept" version="2.4.0" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.DependencyCollector" version="2.9.1" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.PerfCounterCollector" version="2.9.1" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.Web" version="2.9.1" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.WindowsServer" version="2.9.1" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel" version="2.9.1" targetFramework="net472" />
  <package id="Microsoft.AspNet.FriendlyUrls" version="1.0.2" targetFramework="net472" />
  <package id="Microsoft.AspNet.FriendlyUrls.Core" version="1.0.2" targetFramework="net472" />
  <package id="Microsoft.AspNet.ScriptManager.MSAjax" version="5.0.0" targetFramework="net472" />
  <package id="Microsoft.AspNet.ScriptManager.WebForms" version="5.0.0" targetFramework="net472" />
  ...
  <package id="System.Memory" version="4.5.1" targetFramework="net472" />
  <package id="System.Numerics.Vectors" version="4.4.0" targetFramework="net472" />
  <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.0" targetFramework="net472" />
  <package id="System.Threading.Channels" version="4.5.0" targetFramework="net472" />
  <package id="System.Threading.Tasks.Extensions" version="4.5.1" targetFramework="net472" />
  <package id="WebGrease" version="1.6.0" targetFramework="net472" />
</packages>
```

`<packages>`元素包含所有必要的依赖项。 很难确定包含了哪些包，因为你需要它们。 某些 `<package>` 元素只是为了满足所需的依赖项需求而列出。

Blazor项目在项目文件的元素中列出所需的依赖项 `<ItemGroup>` ：

```xml
<ItemGroup>
    <PackageReference Include="Autofac" Version="4.9.3" />
    <PackageReference Include="EntityFramework" Version="6.4.4" />
    <PackageReference Include="log4net" Version="2.0.12" />
    <PackageReference Include="Microsoft.Extensions.Logging.Log4Net.AspNetCore" Version="2.2.12" />
</ItemGroup>
```

一个 NuGet 包可简化 Web 窗体开发人员的生活，这是 [Windows 兼容性包](../../core/porting/windows-compat-pack.md)。 尽管 .NET 是跨平台的，但某些功能只能在 Windows 上使用。 Windows 特定功能通过安装兼容包提供。 此类功能的示例包括注册表、WMI 和目录服务。 该包增加了 20000 Api，并激活了许多你可能已经熟悉的服务。 EShop 项目不需要兼容包;但是，如果你的项目使用特定于 Windows 的功能，则包会使迁移工作更加轻松。

## <a name="enable-startup-process"></a>启用启动过程

对于 Blazor 其他 ASP.NET Core 服务，的启动过程已从 Web 窗体更改并遵循类似的设置。 托管服务器端时， Blazor 组件作为正常 ASP.NET Core 应用的一部分运行。 当使用在浏览器中承载时 WebAssembly ， Blazor 组件将使用类似的承载模型。 不同之处在于，组件是作为独立的服务从任何后端进程运行的。 无论采用哪种方式，启动都是类似的。

*Global.asax.cs* 文件是 Web 窗体项目的默认启动页。 在 eShop 项目中，此文件 (IoC) 容器配置控制反转，并处理应用或请求的各种生命周期事件。 其中一些事件通过中间件 (（如) ）进行处理 `Application_BeginRequest` 。 其他事件需要通过依赖关系注入 (DI) 来覆盖特定服务。

例如，eShop 的 *Global.asax.cs* 文件包含以下代码：

```csharp
public class Global : HttpApplication, IContainerProviderAccessor
{
    private static readonly ILog _log = LogManager.GetLogger(System.Reflection.MethodBase.GetCurrentMethod().DeclaringType);

    static IContainerProvider _containerProvider;
    IContainer container;

    public IContainerProvider ContainerProvider
    {
        get { return _containerProvider; }
    }

    protected void Application_Start(object sender, EventArgs e)
    {
        // Code that runs on app startup
        RouteConfig.RegisterRoutes(RouteTable.Routes);
        BundleConfig.RegisterBundles(BundleTable.Bundles);
        ConfigureContainer();
        ConfigDataBase();
    }

    /// <summary>
    /// Track the machine name and the start time for the session inside the current session
    /// </summary>
    protected void Session_Start(Object sender, EventArgs e)
    {
        HttpContext.Current.Session["MachineName"] = Environment.MachineName;
        HttpContext.Current.Session["SessionStartTime"] = DateTime.Now;
    }

    /// <summary>
    /// https://autofaccn.readthedocs.io/en/latest/integration/webforms.html
    /// </summary>
    private void ConfigureContainer()
    {
        var builder = new ContainerBuilder();
        var mockData = bool.Parse(ConfigurationManager.AppSettings["UseMockData"]);
        builder.RegisterModule(new ApplicationModule(mockData));
        container = builder.Build();
        _containerProvider = new ContainerProvider(container);
    }

    private void ConfigDataBase()
    {
        var mockData = bool.Parse(ConfigurationManager.AppSettings["UseMockData"]);

        if (!mockData)
        {
            Database.SetInitializer<CatalogDBContext>(container.Resolve<CatalogDBInitializer>());
        }
    }

    protected void Application_BeginRequest(object sender, EventArgs e)
    {
        //set the property to our new object
        LogicalThreadContext.Properties["activityid"] = new ActivityIdHelper();

        LogicalThreadContext.Properties["requestinfo"] = new WebRequestInfo();

        _log.Debug("Application_BeginRequest");
    }
}
```

前面的文件成为 `Startup` 服务器端的类 Blazor ：

```csharp
public class Startup
{
    public Startup(IConfiguration configuration, IWebHostEnvironment env)
    {
        Configuration = configuration;
        Env = env;
    }

    public IConfiguration Configuration { get; }

    public IWebHostEnvironment Env { get; }

    // This method gets called by the runtime. Use this method to add services to the container.
    // For more information on how to configure your application, visit https://go.microsoft.com/fwlink/?LinkID=398940
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddRazorPages();
        services.AddServerSideBlazor();

        if (Configuration.GetValue<bool>("UseMockData"))
        {
            services.AddSingleton<ICatalogService, CatalogServiceMock>();
        }
        else
        {
            services.AddScoped<ICatalogService, CatalogService>();
            services.AddScoped<IDatabaseInitializer<CatalogDBContext>, CatalogDBInitializer>();
            services.AddSingleton<CatalogItemHiLoGenerator>();
            services.AddScoped(_ => new CatalogDBContext(Configuration.GetConnectionString("CatalogDBContext")));
        }
    }

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app, ILoggerFactory loggerFactory)
    {
        loggerFactory.AddLog4Net("log4Net.xml");

        if (Env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }
        else
        {
            app.UseExceptionHandler("/Home/Error");
        }

        // Middleware for Application_BeginRequest
        app.Use((ctx, next) =>
        {
            LogicalThreadContext.Properties["activityid"] = new ActivityIdHelper(ctx);
            LogicalThreadContext.Properties["requestinfo"] = new WebRequestInfo(ctx);
            return next();
        });

        app.UseStaticFiles();

        app.UseRouting();

        app.UseEndpoints(endpoints =>
        {
            endpoints.MapBlazorHub();
            endpoints.MapFallbackToPage("/_Host");
        });

        ConfigDataBase(app);
    }

    private void ConfigDataBase(IApplicationBuilder app)
    {
        using (var scope = app.ApplicationServices.CreateScope())
        {
            var initializer = scope.ServiceProvider.GetService<IDatabaseInitializer<CatalogDBContext>>();

            if (initializer != null)
            {
                Database.SetInitializer(initializer);
            }
        }
    }
}
```

你可能会注意到，Web 窗体中的一项重大更改是 DI 的突出。 DI 是 ASP.NET Core 设计中的一个指导原则。 它支持 ASP.NET Core 框架的几乎所有方面的自定义。 甚至可以在许多方案中使用内置服务提供程序。 如果需要进行更多的自定义，则很多社区项目都支持此功能。 例如，你可以继续执行第三方 DI 库投资。

在原始 eShop 应用程序中，有一些配置用于会话管理。 由于服务器端 Blazor 使用 ASP.NET Core SignalR 进行通信，因此不支持会话状态，因为连接可能独立于 HTTP 上下文。 使用会话状态的应用需要重新架构，才能作为 Blazor 应用运行。

有关应用程序启动的详细信息，请参阅 [应用程序启动](app-startup.md)。

## <a name="migrate-http-modules-and-handlers-to-middleware"></a>将 HTTP 模块和处理程序迁移到中间件

HTTP 模块和处理程序是 Web 窗体中的常见模式，用来控制 HTTP 请求管道。 实现 `IHttpModule` 或 `IHttpHandler` 可以注册并处理传入请求的类。 Web 窗体配置 *web.config* 文件中的模块和处理程序。 Web 窗体在很大程度上取决于应用生命周期事件处理。 ASP.NET Core 改为使用中间件。 中间件是在类的方法中注册的 `Configure` `Startup` 。 中间件执行顺序由注册顺序决定。

在 " [启用启动过程](#enable-startup-process) " 部分，Web 窗体将引发生命周期事件作为 `Application_BeginRequest` 方法。 此事件在 ASP.NET Core 中不可用。 实现此行为的一种方法是实现中间件，如 *Startup.cs* 文件示例中所示。 此中间件执行相同的逻辑，然后将控制转移到中间件管道中的下一个处理程序。

有关迁移模块和处理程序的详细信息，请参阅 [将 HTTP 处理程序和模块迁移到 ASP.NET Core 中间件](/aspnet/core/migration/http-modules)。

## <a name="migrate-static-files"></a>迁移静态文件

为了提供静态文件 (例如 HTML、CSS、图像和 JavaScript) ，文件必须由中间件公开。 通过调用 `UseStaticFiles` 方法，可以从 web 根路径提供静态文件。 默认 web 根目录为 *wwwroot*，但可以对其进行自定义。 在 `Configure` eShop 的类的方法中包括 `Startup` ：

```csharp
public void Configure(IApplicationBuilder app)
{
    ...

    app.UseStaticFiles();

    ...
}
```

EShop 项目可实现基本的静态文件访问。 有很多自定义可用于静态文件访问。 有关启用默认文件或文件浏览器的信息，请参阅 [ASP.NET Core 中的静态文件](/aspnet/core/fundamentals/static-files)。

## <a name="migrate-runtime-bundling-and-minification-setup"></a>迁移运行时绑定和缩减安装程序

绑定和缩减是一种性能优化技术，用于减少服务器请求检索特定文件类型的数量和大小。 在将 JavaScript 和 CSS 发送到客户端之前，通常会采用某种形式的绑定或缩减。 在 ASP.NET Web 窗体中，这些优化在运行时进行处理。 优化约定定义为 *App_Start/bundleconfig.cs* 文件。 在 ASP.NET Core 中采用了一种更具声明性的方法。 文件将列出要缩小的文件以及特定的缩减设置。

有关捆绑和缩减的详细信息，请参阅 [ASP.NET Core 中的捆绑和缩小静态资产](/aspnet/core/client-side/bundling-and-minification)。

## <a name="migrate-aspx-pages"></a>迁移 ASPX 页

Web 窗体应用中的页面是扩展名为 *.aspx* 的文件。 Web 窗体页通常可以映射到中的组件 Blazor 。 Blazor组件是在扩展名为 *razor* 的文件中创作的。 对于 eShop 项目，五个页面会转换为 Razor 页面。

例如，详细信息视图包含 Web 窗体项目中的三个文件： " *详细信息*"、" *Details.aspx.cs*" 和 " *Details.aspx.designer.cs*"。 转换为时 Blazor ，代码隐藏和标记将合并为 *详细信息。* Razor 编译 (等效于 *designer.cs* 文件中) 存储在 *obj* 目录中，默认情况下，在 **解决方案资源管理器** 中可查看。 Web 窗体页由以下标记组成：

```aspx-csharp
<%@ Page Title="Details" Language="C#" MasterPageFile="~/Site.Master" AutoEventWireup="true" CodeBehind="Details.aspx.cs" Inherits="eShopLegacyWebForms.Catalog.Details" %>

<asp:Content ID="Details" ContentPlaceHolderID="MainContent" runat="server">
    <h2 class="esh-body-title">Details</h2>

    <div class="container">
        <div class="row">
            <asp:Image runat="server" CssClass="col-md-6 esh-picture" ImageUrl='<%#"/Pics/" + product.PictureFileName%>' />
            <dl class="col-md-6 dl-horizontal">
                <dt>Name
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.Name%>' />
                </dd>

                <dt>Description
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.Description%>' />
                </dd>

                <dt>Brand
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.CatalogBrand.Brand%>' />
                </dd>

                <dt>Type
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.CatalogType.Type%>' />
                </dd>
                <dt>Price
                </dt>

                <dd>
                    <asp:Label CssClass="esh-price" runat="server" Text='<%#product.Price%>' />
                </dd>

                <dt>Picture name
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.PictureFileName%>' />
                </dd>

                <dt>Stock
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.AvailableStock%>' />
                </dd>

                <dt>Restock
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.RestockThreshold%>' />
                </dd>

                <dt>Max stock
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.MaxStockThreshold%>' />
                </dd>

            </dl>
        </div>

        <div class="form-actions no-color esh-link-list">
            <a runat="server" href='<%# GetRouteUrl("EditProductRoute", new {id =product.Id}) %>' class="esh-link-item">Edit
            </a>
            |
            <a runat="server" href="~" class="esh-link-item">Back to list
            </a>
        </div>

    </div>
</asp:Content>
```

前面标记的代码隐藏包含以下代码：

```csharp
using eShopLegacyWebForms.Models;
using eShopLegacyWebForms.Services;
using log4net;
using System;
using System.Web.UI;

namespace eShopLegacyWebForms.Catalog
{
    public partial class Details : System.Web.UI.Page
    {
        private static readonly ILog _log = LogManager.GetLogger(System.Reflection.MethodBase.GetCurrentMethod().DeclaringType);

        protected CatalogItem product;

        public ICatalogService CatalogService { get; set; }

        protected void Page_Load(object sender, EventArgs e)
        {
            var productId = Convert.ToInt32(Page.RouteData.Values["id"]);
            _log.Info($"Now loading... /Catalog/Details.aspx?id={productId}");
            product = CatalogService.FindCatalogItem(productId);

            this.DataBind();
        }
    }
}
```

转换为时 Blazor ，Web 窗体页会转换为以下代码：

```razor
@page "/Catalog/Details/{id:int}"
@inject ICatalogService CatalogService
@inject ILogger<Details> Logger

<h2 class="esh-body-title">Details</h2>

<div class="container">
    <div class="row">
        <img class="col-md-6 esh-picture" src="@($"/Pics/{_item.PictureFileName}")">

        <dl class="col-md-6 dl-horizontal">
            <dt>
                Name
            </dt>

            <dd>
                @_item.Name
            </dd>

            <dt>
                Description
            </dt>

            <dd>
                @_item.Description
            </dd>

            <dt>
                Brand
            </dt>

            <dd>
                @_item.CatalogBrand.Brand
            </dd>

            <dt>
                Type
            </dt>

            <dd>
                @_item.CatalogType.Type
            </dd>
            <dt>
                Price
            </dt>

            <dd>
                @_item.Price
            </dd>

            <dt>
                Picture name
            </dt>

            <dd>
                @_item.PictureFileName
            </dd>

            <dt>
                Stock
            </dt>

            <dd>
                @_item.AvailableStock
            </dd>

            <dt>
                Restock
            </dt>

            <dd>
                @_item.RestockThreshold
            </dd>

            <dt>
                Max stock
            </dt>

            <dd>
                @_item.MaxStockThreshold
            </dd>

        </dl>
    </div>

    <div class="form-actions no-color esh-link-list">
        <a href="@($"/Catalog/Edit/{_item.Id}")" class="esh-link-item">
            Edit
        </a>
        |
        <a href="/" class="esh-link-item">
            Back to list
        </a>
    </div>

</div>

@code {
    private CatalogItem _item;

    [Parameter]
    public int Id { get; set; }

    protected override void OnInitialized()
    {
        Logger.LogInformation("Now loading... /Catalog/Details/{Id}", Id);

        _item = CatalogService.FindCatalogItem(Id);
    }
}
```

请注意，代码和标记位于同一个文件中。 所有必需的服务都可通过属性进行访问 `@inject` 。 在 `@page` 此页上，可以在路由中访问此页 `Catalog/Details/{id}` 。 路由占位符的值已 `{id}` 被约束为一个整数。 如 " [路由](pages-routing-layouts.md) " 一节中所述，与 Web 窗体不同，Razor 组件显式声明其路由以及所包含的所有参数。 许多 Web 窗体控件在中的对应项可能不完全相同 Blazor 。 通常，具有相同用途的等效 HTML 代码段。 例如， `<asp:Label />` 可以将控件替换为 HTML `<label>` 元素。

### <a name="model-validation-in-no-locblazor"></a>模型验证 Blazor

如果 Web 窗体代码包括验证，则可以使用很少的更改传输您的大部分内容。 中运行的好处 Blazor 是，可以运行相同的验证逻辑，而无需自定义 JavaScript。 数据批注实现了简单的模型验证。

例如， *Create .aspx* 页面包含带有验证的数据输入窗体。 示例代码段如下所示：

```aspx
<div class="form-group">
    <label class="control-label col-md-2">Name</label>
    <div class="col-md-3">
        <asp:TextBox ID="Name" runat="server" CssClass="form-control"></asp:TextBox>
        <asp:RequiredFieldValidator runat="server" ControlToValidate="Name" Display="Dynamic"
            CssClass="field-validation-valid text-danger" ErrorMessage="The Name field is required." />
    </div>
</div>
```

在中 Blazor ， *创建 razor* 文件中提供了等效标记：

```razor
<EditForm Model="_item" OnValidSubmit="@...">
    <DataAnnotationsValidator />

    <div class="form-group">
        <label class="control-label col-md-2">Name</label>
        <div class="col-md-3">
            <InputText class="form-control" @bind-Value="_item.Name" />
            <ValidationMessage For="(() => _item.Name)" />
        </div>
    </div>

    ...
</EditForm>
```

`EditForm`上下文包括验证支持，可在输入周围环绕。 数据批注是添加验证的常用方法。 可以通过组件添加此类验证支持 `DataAnnotationsValidator` 。 有关此机制的详细信息，请参阅 [ASP.NET Core Blazor 窗体和验证](/aspnet/core/blazor/forms-validation)。

## <a name="migrate-configuration"></a>迁移配置

在 Web 窗体项目中，配置数据通常存储在 *web.config* 文件中。 配置数据通过访问 `ConfigurationManager` 。 通常需要服务来分析对象。 通过 .NET Framework 4.7.2，可组合性通过添加到配置 `ConfigurationBuilders` 。 这些生成器允许开发人员为配置添加各种源，然后在运行时编写这些源以检索必要的值。

ASP.NET Core 引入了一个灵活的配置系统，该系统允许你定义应用和部署所使用的配置源。 `ConfigurationBuilder`您在 Web 窗体应用程序中使用的基础结构在 ASP.NET Core 配置系统中使用的概念后建模。

以下代码片段演示了 Web 窗体 eShop 项目如何使用 *web.config* 来存储配置值：

```xml
<configuration>
  <configSections>
    <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
  </configSections>
  <connectionStrings>
    <add name="CatalogDBContext" connectionString="Data Source=(localdb)\MSSQLLocalDB; Initial Catalog=Microsoft.eShopOnContainers.Services.CatalogDb; Integrated Security=True; MultipleActiveResultSets=True;" providerName="System.Data.SqlClient" />
  </connectionStrings>
  <appSettings>
    <add key="UseMockData" value="true" />
    <add key="UseCustomizationData" value="false" />
  </appSettings>
</configuration>
```

机密（如数据库连接字符串）通常存储在 *web.config* 中。机密不可避免地保存在不安全的位置，如源代码管理。 对于 Blazor ASP.NET Core 上的，上面的基于 XML 的配置将替换为以下 JSON：

```json
{
  "ConnectionStrings": {
    "CatalogDBContext": "Data Source=(localdb)\\MSSQLLocalDB; Initial Catalog=Microsoft.eShopOnContainers.Services.CatalogDb; Integrated Security=True; MultipleActiveResultSets=True;"
  },
  "UseMockData": true,
  "UseCustomizationData": false
}
```

JSON 为默认配置格式;但 ASP.NET Core 支持许多其他格式，包括 XML。 还有多个社区支持的格式。

项目类中的构造函数 Blazor `Startup` `IConfiguration` 通过一种称为 "构造函数注入" 的 DI 技术接受实例：

```csharp
public class Startup
{
    public Startup(IConfiguration configuration, IWebHostEnvironment env)
    {
        Configuration = configuration;
        Env = env;
    }

    ...
}
```

默认情况下，环境变量、JSON 文件 (*appsettings.js* 和 *appsettings。 {环境} json*) 和命令行选项在配置对象中注册为有效的配置源。 可以通过访问配置源 `Configuration[key]` 。 更高级的技术是使用选项模式将配置数据绑定到对象。 有关配置和选项模式的详细信息，请分别参阅 ASP.NET Core 中 ASP.NET Core 和[options 模式](/aspnet/core/fundamentals/configuration/options)下的[配置](/aspnet/core/fundamentals/configuration/)。

## <a name="migrate-data-access"></a>迁移数据访问

数据访问是任何应用程序的一个重要方面。 EShop 项目在数据库中存储目录信息，并使用实体框架 (EF) 6 检索数据。 由于 .NET 5.0 支持 EF 6，因此项目可以继续使用它。

EShop 需要以下与 EF 相关的更改：

- 在 .NET Framework 中， `DbContext` 对象接受 *名称 = ConnectionString* 格式的字符串，并使用中的连接字符串 `ConfigurationManager.AppSettings[ConnectionString]` 进行连接。 在 .NET Core 中，不支持这种情况。 必须提供连接字符串。
- 以同步方式访问数据库。 尽管这可行，可伸缩性可能会受到影响。 此逻辑应移动到异步模式。

尽管对数据集绑定的本机支持不相同，但 Blazor 在 Razor 页中提供了其 c # 支持的灵活性和强大功能。 例如，您可以执行计算并显示结果。 有关中的数据模式的详细信息 Blazor ，请参阅 [数据访问](data.md) 章节。

## <a name="architectural-changes"></a>体系结构更改

最后，在迁移到时，需要考虑一些重要的体系结构差异 Blazor 。 其中的许多更改都适用于基于 .NET Core 或 ASP.NET Core 的任何内容。

由于 Blazor 是基于 .Net core 构建的，因此请注意确保 .Net core 支持。 一些主要更改包括删除以下功能：

- 多个 Appdomain
- 远程处理
- 代码访问安全性 (CAS)
- 安全透明度

若要详细了解如何识别在 .NET Core 上运行所需的更改，请参阅将 [代码从 .NET Framework 移植到 .Net core](../../core/porting/index.md)。

ASP.NET Core 是 ASP.NET 的重新塑造版本，并且有一些更改最初可能不明显。 主要变化如下：

- 无同步上下文，这意味着没有 `HttpContext.Current` 、 `Thread.CurrentPrincipal` 或其他静态访问器
- 无卷影复制
- 无请求队列

ASP.NET Core 中的许多操作都是异步的，因此可以更轻松地不加载 i/o 限制任务。 切勿使用或来阻止，这一点很重要 `Task.Wait()` `Task.GetResult()` ，因为这样可以快速耗尽线程池资源。

## <a name="migration-conclusion"></a>迁移结论

此时，你已了解将 Web 窗体项目移动到所需的多个示例 Blazor 。 有关完整示例，请参阅[eShopOn Blazor ](https://github.com/dotnet-architecture/eShopOnBlazor)项目。

>[!div class="step-by-step"]
>[上一页](security-authentication-authorization.md)
