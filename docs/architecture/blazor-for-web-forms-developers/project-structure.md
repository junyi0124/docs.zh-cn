---
title: 应用的项目结构 Blazor
description: 了解 ASP.NET Web 窗体和项目的项目结构 Blazor 比较。
author: danroth27
ms.author: daroth
no-loc:
- Blazor
- WebAssembly
ms.date: 11/20/2020
ms.openlocfilehash: d91430eb654ee16934408bf064803b34ca700640
ms.sourcegitcommit: 2f485e721f7f34b87856a51181b5b56624b31fd5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96509801"
---
# <a name="project-structure-for-no-locblazor-apps"></a>应用的项目结构 Blazor

尽管它们有重要的项目结构差异，ASP.NET Web 窗体和 Blazor 共享许多类似的概念。 在这里，我们将查看项目的结构 Blazor ，并将其与 ASP.NET Web 窗体项目进行比较。

若要创建第一个 Blazor 应用，请按照[ Blazor 入门步骤](/aspnet/core/blazor/get-started)中的说明进行操作。 您可以按照说明创建 Blazor 服务器应用程序或 Blazor WebAssembly 在 ASP.NET Core 中托管的应用程序。 除了托管模型特定的逻辑外，这两个项目中的大多数代码都是相同的。

## <a name="project-file"></a>项目文件

Blazor 服务器应用是 .NET 项目。 服务器应用程序的项目文件与 Blazor 它一样简单：

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>net5.0</TargetFramework>
  </PropertyGroup>

</Project>
```

应用的项目文件 Blazor WebAssembly 看起来稍微多一些， (完全版本号可能因) 而异：

```xml
<Project Sdk="Microsoft.NET.Sdk.BlazorWebAssembly">

  <PropertyGroup>
    <TargetFramework>net5.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.Components.WebAssembly" Version="5.0.0" />
    <PackageReference Include="Microsoft.AspNetCore.Components.WebAssembly.DevServer" Version="5.0.0" PrivateAssets="all" />
    <PackageReference Include="System.Net.Http.Json" Version="5.0.0" />
  </ItemGroup>

</Project>
```

BlazorWebAssembly项目目标 `Microsoft.NET.Sdk.BlazorWebAssembly` 而不是 `Microsoft.NET.Sdk.Web` sdk，因为它们在 WebAssembly 基于的 .net 运行时上的浏览器中运行。 不能像在服务器或开发人员计算机上那样将 .NET 安装到 web 浏览器中。 因此，项目 Blazor 使用单独的包引用来引用框架。

相比之下，默认的 ASP.NET Web 窗体项目在其 *.csproj* 文件中包含将近300行的 XML，其中的大多数行都显式列出了项目中的各种代码和内容文件。 随着 `.NET 5` `Blazor Server` 和应用程序的发布， `Blazor WebAssembly` 可以轻松共享一个统一的运行时。

尽管它们是受支持的，但在 .NET 项目中各个程序集引用不太常见。 大多数项目依赖项都作为 NuGet 包引用处理。 只需引用 .NET 项目中的顶级包依赖项。 自动包含可传递的依赖项。 包引用使用元素添加到项目文件，而不是使用 ASP.NET Web 窗体项目中常见的 *packages.config* 文件来引用包 `<PackageReference>` 。

```xml
<ItemGroup>
  <PackageReference Include="Newtonsoft.Json" Version="12.0.2" />
</ItemGroup>
```

## <a name="entry-point"></a>入口点

Blazor服务器应用的入口点是在 *Program.cs* 文件中定义的，如控制台应用中所示。 当应用程序执行时，它将使用特定于 web 应用的默认值创建并运行 web 主机实例。 Web 主机管理 Blazor 服务器应用程序的生命周期，并设置主机级服务。 此类服务的示例包括配置、日志记录、依赖关系注入和 HTTP 服务器。 此代码主要是样板化的，通常保持不变。

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStartup<Startup>();
            });
}
```

BlazorWebAssembly应用还会在 *Program.cs* 中定义入口点。 代码看起来略有不同。 此代码类似于设置应用程序主机以向应用程序提供相同的主机级服务。 WebAssembly不过，应用主机不会设置 HTTP 服务器，因为它是直接在浏览器中执行的。

Blazor 应用有一个 `Startup` 类，而不是一个 *global.asax* 文件，用于定义应用的启动逻辑。 `Startup`类用于配置应用程序和任何特定于应用程序的服务。 在 Blazor 服务器应用中， `Startup` 类用于为 Blazor 客户端浏览器和服务器之间使用的实时连接设置终结点。 在 Blazor WebAssembly 应用程序中， `Startup` 类定义应用程序的根组件及其呈现位置。 我们将深入了解 `Startup` [应用程序启动](./app-startup.md) 部分中的类。

## <a name="static-files"></a>静态文件

与 ASP.NET Web 窗体项目不同，项目中的所有文件都不能 Blazor 被请求为静态文件。 只有 *wwwroot* 文件夹中的文件可通过 web 寻址。 此文件夹被称为应用程序的 "web 根目录"。 应用程序的 web 根目录之外的任何内容都 *无法* 进行 web 寻址。 此设置提供了额外的安全级别，可防止在 web 上意外公开项目文件。

## <a name="configuration"></a>配置

ASP.NET Web 窗体应用中的配置通常使用一个或多个 *web.config* 文件进行处理。 Blazor 应用通常不会有 *web.config* 的文件。 如果是这样，则在 IIS 上承载时，该文件仅用于配置 IIS 特定的设置。 相反， Blazor 服务器应用使用 ASP.NET Core 配置抽象 (Blazor WebAssembly 应用当前不支持相同的配置抽象，但这可能是将来) 中添加的一项功能。 例如，默认的 Blazor 服务器应用程序将 *appsettings.js上* 的设置。

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "AllowedHosts": "*"
}
```

有关配置的详细信息，请参阅 [配置](./config.md) 节 ASP.NET Core 项目。

## <a name="razor-components"></a>Razor 组件

项目中的大多数文件 Blazor 都是 *razor* 文件。 Razor 是一种基于 HTML 和 c # 的模板化语言，用于动态生成 web UI。 *Razor* 文件定义构成应用程序的 UI 的组件。 大多数情况下，对于服务器和应用而言，组件是相同的 Blazor Blazor WebAssembly 。 中的组件 Blazor 类似于 ASP.NET Web 窗体中的用户控件。

生成项目时，每个 Razor 组件文件都将编译为 .NET 类。 生成的类捕获组件的状态、呈现逻辑、生命周期方法、事件处理程序和其他逻辑。 我们将在[构建可重复使用的 UI 组件 Blazor ](./components.md)部分介绍组件。

*_Imports* 文件不是 razor 组件文件。 相反，它们定义一组 Razor 指令以导入到同一文件夹及其子文件夹中的其他 *razor* 文件中。 例如， *_Imports razor* 文件是一种为 `using` 常用命名空间添加指令的常规方法：

```razor
@using System.Net.Http
@using Microsoft.AspNetCore.Authorization
@using Microsoft.AspNetCore.Components.Authorization
@using Microsoft.AspNetCore.Components.Forms
@using Microsoft.AspNetCore.Components.Routing
@using Microsoft.AspNetCore.Components.Web
@using Microsoft.JSInterop
@using BlazorApp1
@using BlazorApp1.Shared
```

## <a name="pages"></a>页

应用中的页面位于何处 Blazor ？ Blazor 不为可寻址页面定义单独的文件扩展名，例如 ASP.NET Web Forms apps 中的 *.aspx* 文件。 而是通过将路由分配给组件来定义页面。 通常使用 `@page` Razor 指令分配路由。 例如， `Counter` 在 *Pages/Counter* 文件中编写的组件定义了以下路由：

```razor
@page "/counter"
```

中的路由 Blazor 处理客户端，而不是服务器上的。 当用户在浏览器中导航时，会 Blazor 截获导航，然后用匹配的路由呈现组件。

组件的文件位置目前不会推理组件路由，就像 *.aspx* 页面一样。 将来可能会添加此功能。 必须在组件上显式指定每个路由。 在 " *页面* " 文件夹中存储可路由的组件无特殊含义，只是一种约定。

在 Blazor " [页面"、"路由" 和 "布局](./pages-routing-layouts.md) " 部分的 "路由" 部分中，我们将更详细地介绍。

## <a name="layout"></a>布局

在 ASP.NET Web 窗体应用程序中，使用 *母版页 ()* 来处理常见的页面布局。 在 Blazor 应用程序中，将使用布局组件 (*Shared/MainLayout*) 来处理页面布局。 " [页面"、"路由" 和 "布局"](./pages-routing-layouts.md) 部分中将更详细地讨论布局组件。

## <a name="bootstrap-no-locblazor"></a>引导 Blazor

若要启动 Blazor ，应用必须：

- 指定在页面上的什么位置 (应呈现的 *app.config*) 。
- 添加相应的 Blazor 框架脚本。

在 Blazor 服务器应用中，根组件的 "主机" 页在 *_Host.* # 文件中定义。 此文件定义了一个 Razor 页面，而不是一个组件。 Razor Pages 使用 Razor 语法来定义服务器可寻址页面，与 *.aspx* 页面非常类似。 `Html.RenderComponentAsync<TComponent>(RenderMode)`方法用于定义根级别组件的呈现位置。 `RenderMode`选项指示组件的呈现方式。 下表概述了支持的 `RenderMode` 选项。

|选项                        |说明       |
|------------------------------|------------------|
|`RenderMode.Server`           |建立与浏览器的连接后交互呈现|
|`RenderMode.ServerPrerendered`|首先预呈现并以交互方式呈现|
|`RenderMode.Static`           |呈现为静态内容|

*_Framework/blazor.server.js* 的脚本引用与服务器建立实时连接，并处理所有用户交互和 UI 更新。

```razor
@page "/"
@namespace BlazorApp1.Pages
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>BlazorApp1</title>
    <base href="~/" />
    <link rel="stylesheet" href="css/bootstrap/bootstrap.min.css" />
    <link href="css/site.css" rel="stylesheet" />
</head>
<body>
    <app>
        @(await Html.RenderComponentAsync<App>(RenderMode.ServerPrerendered))
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
</html>
```

在 Blazor WebAssembly 应用程序中，"主机" 页是 *wwwroot/index.html* 下的简单静态 HTML 文件。 `<div>`Id 为的元素 `app` 用于指示应呈现根组件的位置。

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
    <title>BlazorApp2</title>
    <base href="/" />
    <link href="css/bootstrap/bootstrap.min.css" rel="stylesheet" />
    <link href="css/app.css" rel="stylesheet" />
    <link href="blazor-web.styles.css" rel="stylesheet" />
</head>

<body>
    <div id="app">Loading...</div>

    <div id="blazor-error-ui">
        An unhandled error has occurred.
        <a href="" class="reload">Reload</a>
        <a class="dismiss">🗙</a>
    </div>
    <script src="_framework/blazor.webassembly.js"></script>
</body>

</html>

```

在应用程序的方法中配置要呈现的根组件， `Program.Main` 以便通过依赖关系注入注册不同服务。可以在中引用应用的 "添加服务[ Blazor WebAssembly ](https://docs.microsoft.com/aspnet/core/blazor/fundamentals/dependency-injection?view=aspnetcore-5.0#blazor-webassembly) "

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.RootComponents.Add<App>("#app");

        ....
        ....
    }
}
```

## <a name="build-output"></a>生成输出

Blazor生成项目时，所有 Razor 组件和代码文件都将编译为一个程序集。 不同于 ASP.NET Web 窗体项目， Blazor 不支持 UI 逻辑的运行时编译。

## <a name="run-the-app"></a>运行应用

若要运行 Blazor 服务器应用，请 `F5` 在 Visual Studio 中按。 Blazor 应用不支持运行时编译。 若要查看代码和组件标记更改的结果，请在附加调试器的情况下重新生成并重新启动应用。 如果在没有调试器附加 () 的情况下运行 `Ctrl+F5` ，则 Visual Studio 会监视文件更改，并在进行更改后重新启动应用。 进行更改后，手动刷新浏览器。

若要运行 Blazor WebAssembly 应用，请选择以下方法之一：

- 使用开发服务器直接运行客户端项目。
- 在将应用程序托管到 ASP.NET Core 时运行服务器项目。

BlazorWebAssembly可在浏览器和 Visual Studio 中调试应用程序。有关详细信息，请参阅[调试 ASP.NET Core Blazor WebAssembly ](/aspnet/core/blazor/debug) 。

>[!div class="step-by-step"]
>[上一页](hosting-models.md)
>[下一页](app-startup.md)
