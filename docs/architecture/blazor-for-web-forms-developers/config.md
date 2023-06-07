---
title: 应用配置
description: 了解如何在 Blazor 不使用 ConfigurationManager 的情况下配置应用。
author: csharpfritz
ms.author: jefritz
no-loc:
- Blazor
ms.date: 11/20/2020
ms.openlocfilehash: 360d9077bc981a2e9875bb1f86b49c0029424d6e
ms.sourcegitcommit: 2f485e721f7f34b87856a51181b5b56624b31fd5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96509788"
---
# <a name="app-configuration"></a>应用配置

在 Web 窗体中加载应用程序配置的主要方法是在服务器上使用 *web.config* 文件中的条目， &mdash; 或者 *web.config* 引用相关的配置文件。您可以使用静态 `ConfigurationManager` 对象与应用程序设置、数据存储库连接字符串以及添加到应用中的其他扩展配置提供程序进行交互。 通常，查看与应用配置的交互，如以下代码所示：

```csharp
var configurationValue = ConfigurationManager.AppSettings["ConfigurationSettingName"];
var connectionString = ConfigurationManager.ConnectionStrings["MyDatabaseConnectionName"].ConnectionString;
```

Blazor如果你的应用程序托管在 WINDOWS IIS 服务器上，则使用 ASP.NET Core 和服务器端时，可能会出现 *web.config* 文件。 但是， `ConfigurationManager` 与此配置并无交互，你可以从其他来源接收更多结构化应用配置。 让我们看看如何收集配置，以及你仍可以如何从 *web.config* 文件访问配置信息。

## <a name="configuration-sources"></a>配置源

ASP.NET Core 认识到你的应用可能需要使用很多配置源。 默认情况下，框架将尝试为您提供这些功能的最佳功能。 ASP.NET Core 从这些不同的源读取和聚合配置。 对于同一配置密钥，稍后加载的值优先于先前值。

ASP.NET Core 被设计为能够识别云，并使操作员和开发人员可以更轻松地配置应用程序。 ASP.NET Core 是环境感知的，知道它是在您的 `Production` 还是环境中运行 `Development` 。 环境指示器是在 `ASPNETCORE_ENVIRONMENT` 系统环境变量中设置的。 如果未配置任何值，应用程序默认为在环境中运行 `Production` 。

应用可根据环境名称从多个源触发并添加配置。 默认情况下，按列出的顺序从以下资源加载配置：

1. *appsettings.js* 文件（如果存在）
1. *appsettings。{ENVIRONMENT_NAME} json* 文件（如果存在）
1. 磁盘上的用户机密文件（如果存在）
1. 环境变量
1. 命令行参数

## <a name="appsettingsjson-format-and-access"></a>格式和访问 appsettings.js

文件 *上的appsettings.js* 可以通过结构化的值进行分层，如以下 JSON：

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  }
}
```

当出现前面的 JSON 时，配置系统平展子值并引用其完全限定的分层路径。 冒号 (`:`) 字符分隔层次结构中的每个属性。 例如，配置项将 `section1:key0` 访问 `section1` 对象文字的 `key0` 值。

## <a name="user-secrets"></a>用户机密

用户机密包括：

* 存储在开发人员工作站上的 JSON 文件中的配置值位于应用开发文件夹之外。
* 仅在环境中运行时加载 `Development` 。
* 与特定应用相关联。
* 通过 .NET CLI 的命令进行管理 `user-secrets` 。

通过执行以下命令为机密存储配置应用 `user-secrets` ：

```dotnetcli
dotnet user-secrets init
```

前面的命令将一个 `UserSecretsId` 元素添加到项目文件。 元素包含用于将机密与应用关联的 GUID。 然后，可以使用命令定义机密 `set` 。 例如：

```dotnetcli
dotnet user-secrets set "Parent:ApiKey" "12345"
```

上述命令使 `Parent:ApiKey` 使用值的开发人员工作站上的配置密钥可用 `12345` 。

有关创建、存储和管理用户机密的详细信息，请参阅 ASP.NET Core 文档中的开发中的 [应用机密的安全存储](/aspnet/core/security/app-secrets) 。

## <a name="environment-variables"></a>环境变量

将下一组值加载到应用配置中是系统的环境变量。 现在，你的所有系统环境变量设置都可以通过配置 API 来访问。 在应用中进行读取时，将按冒号字符分隔层次结构值。 但是，某些操作系统不允许冒号字符环境变量名称。 ASP.NET Core 通过在访问时将具有双下划线 () 的值转换为冒号来解决此限制 `__` 。 `Parent:ApiKey`可以通过环境变量重写上面的用户机密部分的值 `Parent__ApiKey` 。

## <a name="command-line-arguments"></a>命令行参数

当你的应用程序启动时，还可以将配置作为命令行参数提供。 使用双划线 (`--`) 或正斜杠 (`/`) 表示法来指示要设置的配置值的名称以及要配置的值。 语法类似于以下命令：

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run Parent:ApiKey=67890
```

## <a name="the-return-of-webconfig"></a>返回 web.config

如果已将应用程序部署到 IIS 上的 Windows，则 *web.config* 文件仍会将 IIS 配置为管理你的应用程序。 默认情况下，IIS 会将对 ASP.NET Core 模块的引用添加 (ANCM) 。 ANCM 是一个本机 IIS 模块，它托管你的应用程序来代替 Kestrel web 服务器。 此 *web.config* 部分类似于下面的 XML 标记：

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath=".\MyApp.exe"
                  stdoutLogEnabled="false"
                  stdoutLogFile=".\logs\stdout"
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

应用特定的配置可以通过 `environmentVariables` 在元素中嵌套元素来定义 `aspNetCore` 。 此部分中定义的值将作为环境变量提供给 ASP.NET Core 应用。 环境变量会在应用程序启动的这段时间内正确加载。

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="inprocess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="Parent:ApiKey" value="67890" />
  </environmentVariables>
</aspNetCore>
```

## <a name="read-configuration-in-the-app"></a>在应用中读取配置

ASP.NET Core 提供通过接口的应用配置 <xref:Microsoft.Extensions.Configuration.IConfiguration> 。 此配置接口应由 Blazor 组件、 Blazor 页面以及需要访问配置的任何其他 ASP.NET Core 托管类请求。 ASP.NET Core 框架将用先前配置的解析配置自动填充此接口。 在 Blazor 页或组件的 Razor 标记上，可以将对象注入到 `IConfiguration` `@inject` *Razor* 文件顶部的指令，如下所示：

```razor
@inject IConfiguration Configuration
```

前面的语句使 `IConfiguration` 对象在 `Configuration` Razor 模板的其余部分中可用作变量。

可以通过指定作为索引器参数查找的配置设置层次结构来读取各个配置设置：

```csharp
var mySetting = Configuration["section1:key0"];
```

您可以通过使用方法检索整个配置节，方法是使用与 <xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection%2A> 类似的语法检索特定位置的键集合， `GetSection("section1")` 以便从前面的示例中检索 section1 的配置。

## <a name="strongly-typed-configuration"></a>强类型配置

使用 Web 窗体，可以创建从类型和关联的类型继承的强类型配置类型 <xref:System.Configuration.ConfigurationSection> 。 `ConfigurationSection`允许您为这些配置值配置一些业务规则和处理。

在 ASP.NET Core 中，可以指定将接收配置值的类层次结构。 这些类：

* 不需要从父类继承。
* 应包括 `public` 与你要捕获的配置结构的属性和类型引用相匹配的属性。

对于前面 *appsettings.js* 的示例，可以定义以下类来捕获值：

```csharp
public class MyConfig
{
    public MyConfigSection section0 { get; set;}

    public MyConfigSection section1 { get; set;}
}

public class MyConfigSection
{
    public string key0 { get; set; }

    public string key1 { get; set; }
}
```

可以通过将以下行添加到方法来填充此类层次结构 `Startup.ConfigureServices` ：

```csharp
services.Configure<MyConfig>(Configuration);
```

在应用程序的其余部分中，你可以将输入参数添加到 `@inject` 类型的 Razor 模板中的类或指令， `IOptions<MyConfig>` 以接收强类型配置设置。 `IOptions<MyConfig>.Value`属性将生成 `MyConfig` 从配置设置填充的值。

```razor
@inject IOptions<MyConfig> options
@code {
    var MyConfiguration = options.Value;
    var theSetting = MyConfiguration.section1.key0;
}
```

有关选项功能的详细信息，请参阅 ASP.NET Core 文档的 [选项模式](/aspnet/core/fundamentals/configuration/options#options-interfaces) 。

>[!div class="step-by-step"]
>[上一页](middleware.md)
>[下一页](security-authentication-authorization.md)
