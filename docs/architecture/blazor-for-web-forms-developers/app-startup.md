---
title: 应用启动
description: 了解如何定义应用的启动逻辑。
author: csharpfritz
ms.author: jefritz
ms.date: 11/20/2020
ms.openlocfilehash: d812079f84f67409334d07c4c10c5577446503be
ms.sourcegitcommit: 2f485e721f7f34b87856a51181b5b56624b31fd5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96509697"
---
# <a name="app-startup"></a>应用启动

为 ASP.NET 编写的应用程序通常具有一个 `global.asax.cs` 定义事件的文件，该 `Application_Start` 事件控制配置哪些服务并使其可用于 HTML 呈现和 .net 处理。 本章介绍 ASP.NET Core 和 Blazor 服务器的不同之处。

## <a name="application_start-and-web-forms"></a>Application_Start 和 Web 窗体

默认 web 窗体 `Application_Start` 方法已在数年内发展，以处理许多配置任务。  使用 Visual Studio 2019 中默认模板的全新 web 窗体项目现在包含以下配置逻辑：

- `RouteConfig` -应用程序 URL 路由
- `BundleConfig` -CSS 和 JavaScript 捆绑与缩减

每个文件都驻留在该 `App_Start` 文件夹中，并在应用程序启动时只运行一次。  `RouteConfig` 在默认项目模板中，添加 `FriendlyUrlSettings` 用于 web 窗体的，以允许应用程序 url 忽略 `.ASPX` 文件扩展名。  默认模板还包含一个指令，该指令为 `.ASPX` 带有省略扩展名的文件名的友好 URL (HTTP 301) 提供永久性 http 重定向状态代码。

通过 ASP.NET Core 和 Blazor，这些方法可以简化并合并到 `Startup` 类中，也可以消除这些方法以支持常见的 web 技术。

## <a name="blazor-server-startup-structure"></a>Blazor 服务器启动结构

Blazor 服务器应用程序位于 ASP.NET Core 3.0 或更高版本的顶层。  ASP.NET Core web 应用程序通过 `Startup.cs` 应用程序根文件夹的类中的一对方法进行配置。  Startup 类的默认内容如下所示

```csharp
public class Startup
{
  public Startup(IConfiguration configuration)
  {
    Configuration = configuration;
  }

  public IConfiguration Configuration { get; }

  // This method gets called by the runtime. Use this method to add services to the container.
  // For more information on how to configure your application, visit https://go.microsoft.com/fwlink/?LinkID=398940
  public void ConfigureServices(IServiceCollection services)
  {
    services.AddRazorPages();
    services.AddServerSideBlazor();
    services.AddSingleton<WeatherForecastService>();
  }

  // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
  public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
  {
    if (env.IsDevelopment())
    {
      app.UseDeveloperExceptionPage();
    }
    else
    {
      app.UseExceptionHandler("/Error");
      // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
      app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();

    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
      endpoints.MapBlazorHub();
      endpoints.MapFallbackToPage("/_Host");
   });
  }
 }
```

与 ASP.NET Core 的其余部分一样，启动类是通过依赖关系注入原则创建的。  `IConfiguration`提供给构造函数并在公共属性中储藏更改，以便以后在配置期间进行访问。

`ConfigureServices`ASP.NET Core 中引入的方法允许为框架的内置依赖关系注入容器配置各种 ASP.NET Core 框架服务。  各种 `services.Add*` 方法添加了一些服务，这些服务可实现身份验证、razor 页面、MVC 控制器路由、SignalR 和 Blazor 服务器交互等功能。  Web 窗体中不需要此方法，因为在 web.config 配置文件中引用 ASP.NET 定义了对 ASPX、.ASCX、FOO.ASHX 和 .ASMX 文件的分析和处理。  有关 ASP.NET Core 中的依赖项注入的详细信息，请 [参阅联机文档](/aspnet/core/fundamentals/dependency-injection)。

`Configure`方法介绍 ASP.NET Core 的 HTTP 管道的概念。  在此方法中，我们将从上到下声明 [中间件](middleware.md) ，它们将处理发送到应用程序的每个请求。 默认配置中的大多数功能分散在 web 窗体配置文件中，并且现在位于一个位置以便于参考。

不会再配置放置在文件中的自定义错误页 `web.config` ，但现在将配置为在应用程序环境未标记的情况下始终显示 `Development` 。  此外，ASP.NET Core 的应用程序现已配置为在默认情况下通过方法调用为安全页面提供 TLS `UseHttpsRedirection` 。

接下来，将向中列出一个意外的配置方法 `UseStaticFiles` 。  在 ASP.NET Core 中，必须显式启用对静态文件的请求 (（如 JavaScript、CSS 和图像) 文件）的支持，并且默认情况下，只有应用的 *wwwroot* 文件夹中的文件可公开寻址。

下一行是从 web 窗体复制其中一个配置选项的第一个 `UseRouting` 。  此方法将 ASP.NET Core 路由器添加到管道，可将其配置为此处或在可考虑路由到的单个文件中。  有关路由配置的详细信息，请参阅 [路由部分](pages-routing-layouts.md)。

此方法中的最后一个语句定义 ASP.NET Core 所侦听的终结点。  这些路由是可在 web 服务器上访问的 web 可访问位置，可接收某些由 .NET 处理并返回给你的内容。  第一项 `MapBlazorHub` 配置 SignalR 集线器，以便在为服务器提供实时和持久连接的情况下处理 Blazor 组件的状态和呈现。  `MapFallbackToPage`方法调用指示启动 Blazor 应用程序的页面的 web 可访问位置，还会将应用程序配置为处理来自客户端的深层链接请求。  如果打开浏览器并直接导航到应用程序中的 Blazor 已处理路由（例如， `/counter` 在默认项目模板中），则会看到此功能在工作。 该请求由 *_Host* 的 "Blazor" 回退页面进行处理，后者随后运行 "路由器" 并呈现计数器页。

## <a name="upgrading-the-bundleconfig-process"></a>升级 BundleConfig 进程

用于捆绑 CSS 样式表和 JavaScript 文件之类的资产的技术显著发展，同时还提供了一些快速发展的工具和技术，用于管理这些资源。  为此，我们建议使用节点命令行工具（如 Grunt/Gulp/WebPack）打包静态资产。

可以将 Grunt、Gulp 和 WebPack 命令行工具及其关联的配置添加到应用程序中，ASP.NET Core 将在应用程序生成过程中不知不觉地忽略这些文件。  你可以通过在项目文件中添加一个调用来运行其任务，方法是 `Target` 在项目文件中添加，其语法类似于以下语法，它将触发 gulp 脚本和 `min` 该脚本中的目标

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp min" />
</Target>
```

有关用于管理 CSS 和 JavaScript 文件的这两种策略的更多详细信息，请参阅 [捆绑和缩小静态资产中的 ASP.NET Core](/aspnet/core/client-side/bundling-and-minification) 文档。

>[!div class="step-by-step"]
>[上一页](project-structure.md)
>[下一页](components.md)
