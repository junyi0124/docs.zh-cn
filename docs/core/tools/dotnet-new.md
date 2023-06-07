---
title: dotnet new 命令
description: dotnet new 命令可根据指定模板新建 .NET 项目。
no-loc:
- Blazor
- WebAssembly
ms.date: 09/04/2020
ms.openlocfilehash: 3ee644f05ea5929ffc7b11054ef1d974b811f418
ms.sourcegitcommit: b201d177e01480a139622f3bf8facd367657a472
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2020
ms.locfileid: "94634450"
---
# <a name="dotnet-new"></a>dotnet new

本文适用于： ✔️ .NET Core 2.0 SDK 及更高版本

## <a name="name"></a>“属性”

`dotnet new` - 根据指定的模板，创建新的项目、配置文件或解决方案。

## <a name="synopsis"></a>摘要

```dotnetcli
dotnet new <TEMPLATE> [--dry-run] [--force] [-i|--install {PATH|NUGET_ID}]
    [-lang|--language {"C#"|"F#"|VB}] [-n|--name <OUTPUT_NAME>]
    [--nuget-source <SOURCE>] [-o|--output <OUTPUT_DIRECTORY>]
    [-u|--uninstall] [--update-apply] [--update-check] [Template options]

dotnet new <TEMPLATE> [-l|--list] [--type <TYPE>]

dotnet new -h|--help
```

## <a name="description"></a>描述

`dotnet new` 命令基于模板创建 .NET 项目或其他项目。

命令调用[模板引擎](https://github.com/dotnet/templating)，以根据指定的模板和选项在磁盘上创建项目。

### <a name="implicit-restore"></a>隐式还原

[!INCLUDE[dotnet restore note](~/includes/dotnet-restore-note.md)]

## <a name="arguments"></a>自变量

- **`TEMPLATE`**

  调用命令时要实例化的模板。 每个模板可能具有可传递的特定选项。 有关详细信息，请参阅[模板选项](#template-options)。

  可以运行 `dotnet new --list` 或 `dotnet new -l` 以查看所有已安装模板的列表。 如果 `TEMPLATE` 值与返回表中的“模板”或“短名称”列中的文本不完全匹配，则会对这两列执行 substring 匹配。

  从 .NET Core 3.0 SDK 开始，当你在以下情况下调用 `dotnet new` 命令时，CLI 将在 NuGet.org 中搜索模板：

  - 如果在调用 `dotnet new` 时 CLI 找不到模板匹配项，即使是部分匹配也不行。
  - 如果有较新版本的模板可用。 在这种情况下，将创建项目或工件，但 CLI 会就模板的更新版本发出警告。

  下表显示随 .NET SDK 一起预安装的模板。 模板的默认语言显示在括号内。 单击短名称链接可查看特定的模板选项。

| 模板                                    | 短名称                      | 语言     | Tags                                  | 已引入 |
|----------------------------------------------|---------------------------------|--------------|---------------------------------------|------------|
| 控制台应用程序                          | [控制台](#console)             | [C#]、F#、VB | 常用/控制台                        | 1.0        |
| 类库                                | [classlib](#classlib)           | [C#]、F#、VB | 常用/库                        | 1.0        |
| WPF 应用程序                              | [wpf](#wpf)                     | [C#]、VB     | 常用/WPF                            | 3.0（对于 VB，则为 5.0）|
| WPF 类库                            | [wpflib](#wpf)                  | [C#]、VB     | 常用/WPF                            | 3.0（对于 VB，则为 5.0）|
| WPF 自定义控件库                   | [wpfcustomcontrollib](#wpf)     | [C#]、VB     | 常用/WPF                            | 3.0（对于 VB，则为 5.0）|
| WPF 用户控件库                     | [wpfusercontrollib](#wpf)       | [C#]、VB     | 常用/WPF                            | 3.0（对于 VB，则为 5.0）|
| Windows 窗体 (WinForms) 应用程序         | [winforms](#winforms)           | [C#]、VB     | 常用/WinForms                       | 3.0（对于 VB，则为 5.0）|
| Windows 窗体 (WinForms) 类库       | [winformslib](#winforms)        | [C#]、VB     | 常用/WinForms                       | 3.0（对于 VB，则为 5.0）|
| Worker Service                               | [worker](#web-others)           | [C#]         | 常用/Worker/Web                     | 3.0        |
| 单元测试项目                            | [mstest](#test)                 | [C#]、F#、VB | 测试/MSTest                           | 1.0        |
| NUnit 3 测试项目                         | [nunit](#nunit)                 | [C#]、F#、VB | 测试/NUnit                            | 2.1.400    |
| NUnit 3 测试项                            | `nunit-test`                    | [C#]、F#、VB | 测试/NUnit                            | 2.2        |
| xUnit 测试项目                           | [xunit](#test)                  | [C#]、F#、VB | 测试/xUnit                            | 1.0        |
| Razor 组件                              | `razorcomponent`                | [C#]         | Web/ASP.NET                           | 3.0        |
| Razor 页                                   | [page](#page)                   | [C#]         | Web/ASP.NET                           | 2.0        |
| MVC ViewImports                              | [viewimports](#namespace)       | [C#]         | Web/ASP.NET                           | 2.0        |
| MVC ViewStart                                | `viewstart`                     | [C#]         | Web/ASP.NET                           | 2.0        |
| Blazor 服务器应用                            | [blazorserver](#blazorserver)   | [C#]         | Web/Blazor                            | 3.0        |
| Blazor WebAssembly 应用                       | [blazorwasm](#blazorwasm)       | [C#]         | Web/Blazor/WebAssembly                | 3.1.300    |
| ASP.NET Core 空                           | [web](#web)                     | [C#]，F#     | Web/空                             | 1.0        |
| ASP.NET Core Web 应用程序 (Model-View-Controller) | [mvc](#web-options)             | [C#]，F#     | Web/MVC                               | 1.0        |
| ASP.NET Core Web 应用程序                         | [webapp、razor](#web-options)   | [C#]         | Web/MVC/Razor Pages                   | 2.2、2.0   |
| 含 Angular 的 ASP.NET Core                    | [angular](#spa)                 | [C#]         | Web/MVC/SPA                           | 2.0        |
| 含 React.js 的 ASP.NET Core                   | [react](#spa)                   | [C#]         | Web/MVC/SPA                           | 2.0        |
| 含 React.js 和 Redux 的 ASP.NET Core         | [reactredux](#reactredux)       | [C#]         | Web/MVC/SPA                           | 2.0        |
| Razor 类库                          | [razorclasslib](#razorclasslib) | [C#]         | Web/Razor/库/Razor 类库 | 2.1        |
| ASP.NET Core Web API                         | [webapi](#webapi)               | [C#]，F#     | Web/WebAPI                            | 1.0        |
| ASP.NET Core gRPC 服务                    | [grpc](#web-others)             | [C#]         | Web/gRPC                              | 3.0        |
| dotnet gitignore 文件                        | `gitignore`                     |              | 配置                                | 3.0        |
| global.json 文件                             | [globaljson](#globaljson)       |              | 配置                                | 2.0        |
| NuGet 配置                                 | `nugetconfig`                   |              | 配置                                | 1.0        |
| Dotnet 本地工具清单文件              | `tool-manifest`                 |              | 配置                                | 3.0        |
| Web 配置                                   | `webconfig`                     |              | 配置                                | 1.0        |
| 解决方案文件                                | `sln`                           |              | 解决方案                              | 1.0        |
| 协议缓冲区文件                         | [proto](#namespace)             |              | Web/gRPC                              | 3.0        |

## <a name="options"></a>选项

- **`--dry-run`**

  显示有关以下内容的摘要：给定命令运行导致模板创建时发生的情况。 自 .NET Core 2.2 SDK 起可用。

- **`--force`**

  强制生成内容，即使会更改现有文件，也不例外。 当选择的模板将覆盖输出目录中的现有文件时，需要执行此操作。

- **`-h|--help`**

  打印命令帮助。 可针对 `dotnet new` 命令本身或任何模板调用它。 例如 `dotnet new mvc --help`。

- **`-i|--install <PATH|NUGET_ID>`**

  从提供的 `PATH` 或 `NUGET_ID` 安装模板包。 若要安装模板包的预发布版本，需要以 `<package-name>::<package-version>` 格式指定该版本。 默认情况下，`dotnet new` 为该版本传递 \*，它表示最新的稳定包版本。 请在[示例](#examples)部分查看示例。
  
  如果在运行此命令时已经安装了模板的某个版本，则该模板将更新到指定版本，如果没有指定版本，则将更新到最新的稳定版本。

  若要了解如何创建自定义模板，请参阅 [dotnet new 自定义模板](custom-templates.md)。

- **`-l|--list`**

  列出包含指定名称的模板。 如果未指定名称，则列出所有模板。

- **`-lang|--language {C#|F#|VB}`**

  要创建的模板的语言。 接受的语言因模板而异（请参阅[参数](#arguments)部分中的默认值）。 对于某些模板无效。

  > [!NOTE]
  > 某些 shell 将 `#` 解释为特殊字符。 在这些情况下，请将语言参数值括在引号中。 例如 `dotnet new console -lang "F#"`。

- **`-n|--name <OUTPUT_NAME>`**

  所创建的输出的名称。 如果未指定名称，使用的是当前目录的名称。

- **`--nuget-source <SOURCE>`**

  指定在安装期间要使用的 NuGet 源。

- **`-o|--output <OUTPUT_DIRECTORY>`**

  用于放置生成的输出的位置。 默认为当前目录。

- **`--type <TYPE>`**

  根据可用类型筛选模板。 预定义的值为 `project` 和 `item`。

- **`-u|--uninstall [PATH|NUGET_ID]`**

  在提供的 `PATH` 或 `NUGET_ID` 中卸载模板包。 如果未指定 `<PATH|NUGET_ID>` 值，将显示所有当前安装的模板包及其关联的模板。 指定 `NUGET_ID` 时，请勿包含版本号。

  如果未指定此选项的参数，该命令将列出已安装的模板及其详细信息。

  > [!NOTE]
  > 若要使用 `PATH` 卸载模板，需要完全限定路径。 例如，C:/Users/\<USER>/Documents/Templates/GarciaSoftware.ConsoleTemplate.CSharp 有效，但是包含文件夹中的 ./GarciaSoftware.ConsoleTemplate.CSharp 无效 。
  > 模板路径中不要包含最后的终止目录斜杠。

- **`--update-apply`**

  检查是否有可用于当前安装的模板包的更新并安装这些更新。 自 .NET Core 3.0 SDK 起可用。

- **`--update-check`**

  检查是否有可用于当前安装的模板包的更新。 自 .NET Core 3.0 SDK 起可用。

## <a name="template-options"></a>模板选项

每个项目模板都可能有附加选项。 核心模板有以下附加选项：

### <a name="console"></a>控制台

- **`-f|--framework <FRAMEWORK>`**

  指定目标[框架](../../standard/frameworks.md)。 自 .NET Core 3.0 SDK 起可用。

  下表根据所使用的 SDK 版本号列出了默认值：

  | SDK 版本 | 默认值   |
  |-------------|-----------------|
  | 5.0         | `net5.0`        |
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |

- **`--langVersion <VERSION_NUMBER>`**

  在已创建的项目文件中设置 `LangVersion` 属性。 例如，使用 `--langVersion 7.3` 以使用 C# 7.3。 不支持 F#。 自 .NET Core 2.2 SDK 起可用。

  有关默认的 C# 版本列表，请参阅[默认](../../csharp/language-reference/configure-language-version.md#defaults)。

- **`--no-restore`**

  如已指定，则在项目创建期间不执行隐式还原。 自 .NET Core 2.2 SDK 起可用。

**_

### <a name="classlib"></a>classlib

- _ *`-f|--framework <FRAMEWORK>`**

  指定目标[框架](../../standard/frameworks.md)。 值：`net5.0` 或 `netcoreapp<version>`（若要创建 .NET 类库），或`netstandard<version>`（若要创建 .NET Standard 类库）。 .NET 5.0 SDK 的默认值是 `net5.0`。

- **`--langVersion <VERSION_NUMBER>`**

  在已创建的项目文件中设置 `LangVersion` 属性。 例如，使用 `--langVersion 7.3` 以使用 C# 7.3。 不支持 F#。 自 .NET Core 2.2 SDK 起可用。

  有关默认的 C# 版本列表，请参阅[默认](../../csharp/language-reference/configure-language-version.md#defaults)。

- **`--no-restore`**

  在项目创建期间不执行隐式还原。

**_

### <a name="wpf-wpflib-wpfcustomcontrollib-wpfusercontrollib"></a><a name="wpf"></a> wpf、wpflib、wpfcustomcontrollib、wpfusercontrollib

- _ *`-f|--framework <FRAMEWORK>`**

  指定目标[框架](../../standard/frameworks.md)。 默认值为 `net5.0`。 自 .NET Core 3.1 SDK 起可用。

- **`--langVersion <VERSION_NUMBER>`**

  在已创建的项目文件中设置 `LangVersion` 属性。 例如，使用 `--langVersion 7.3` 以使用 C# 7.3。

  有关默认的 C# 版本列表，请参阅[默认](../../csharp/language-reference/configure-language-version.md#defaults)。

- **`--no-restore`**

  在项目创建期间不执行隐式还原。

**_

### <a name="winforms-winformslib"></a><a name="winforms"></a> winforms、winformslib

- _ *`--langVersion <VERSION_NUMBER>`**

  在已创建的项目文件中设置 `LangVersion` 属性。 例如，使用 `--langVersion 7.3` 以使用 C# 7.3。

  有关默认的 C# 版本列表，请参阅[默认](../../csharp/language-reference/configure-language-version.md#defaults)。

- **`--no-restore`**

  在项目创建期间不执行隐式还原。

**_

### <a name="worker-grpc"></a><a name="web-others"></a> worker、grpc

- _ *`-f|--framework <FRAMEWORK>`**

  指定目标[框架](../../standard/frameworks.md)。 默认值为 `netcoreapp3.1`。 自 .NET Core 3.1 SDK 起可用。

- **`--exclude-launch-settings`**

  从生成的模板中排除 launchSettings.json。

- **`--no-restore`**

  在项目创建期间不执行隐式还原。

**_

### <a name="mstest-xunit"></a><a name="test"></a> mstest、xunit

- _ *`-f|--framework <FRAMEWORK>`**

  指定目标[框架](../../standard/frameworks.md)。 自 .NET Core 3.0 SDK 起可用的选项。

  下表根据所使用的 SDK 版本号列出了默认值：

  | SDK 版本 | 默认值   |
  |-------------|-----------------|
  | 5.0         | `net5.0`        |
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |

- **`-p|--enable-pack`**

  允许使用 [dotnet pack](dotnet-pack.md) 为项目打包。

- **`--no-restore`**

  在项目创建期间不执行隐式还原。

**_

### <a name="nunit"></a>nunit

- _ *`-f|--framework <FRAMEWORK>`**

  指定目标[框架](../../standard/frameworks.md)。

  下表根据所使用的 SDK 版本号列出了默认值：

  | SDK 版本 | 默认值   |
  |-------------|-----------------|
  | 5.0         | `net5.0`        |
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |
  | 2.2         | `netcoreapp2.2` |
  | 2.1         | `netcoreapp2.1` |

- **`-p|--enable-pack`**

  允许使用 [dotnet pack](dotnet-pack.md) 为项目打包。

- **`--no-restore`**

  在项目创建期间不执行隐式还原。

**_

### <a name="page"></a>页

- _ *`-na|--namespace <NAMESPACE_NAME>`**

  已生成代码的命名空间。 默认值为 `MyApp.Namespace`。

- **`-np|--no-pagemodel`**

  创建不含 PageModel 的页。

**_

### <a name="viewimports-proto"></a><a name="namespace"></a> viewimports、proto

- _ *`-na|--namespace <NAMESPACE_NAME>`**

  已生成代码的命名空间。 默认值为 `MyApp.Namespace`。

**_

### <a name="blazorserver"></a>blazorserver

- _ *`-au|--auth <AUTHENTICATION_TYPE>`**

  要使用的身份验证类型。 可能的值为：

  - `None` - 不进行身份验证（默认）。
  - `Individual` - 个人身份验证。
  - `IndividualB2C` - 使用 Azure AD B2C 进行个人身份验证。
  - `SingleOrg` - 对一个租户进行组织身份验证。
  - `MultiOrg` - 对多个租户进行组织身份验证。
  - `Windows` - Windows 身份验证。

- **`--aad-b2c-instance <INSTANCE>`**

  要连接到的 Azure Active Directory B2C 实例。 与 `IndividualB2C` 身份验证结合使用。 默认值为 `https://login.microsoftonline.com/tfp/`。

- **`-ssp|--susi-policy-id <ID>`**

  此项目的登录和注册策略 ID。 与 `IndividualB2C` 身份验证结合使用。

- **`-rp|--reset-password-policy-id <ID>`**

  此项目的重置密码策略 ID。 与 `IndividualB2C` 身份验证结合使用。

- **`-ep|--edit-profile-policy-id <ID>`**

  此项目的编辑配置文件策略 ID。 与 `IndividualB2C` 身份验证结合使用。

- **`--aad-instance <INSTANCE>`**

  要连接到的 Azure Active Directory 实例。 与 `SingleOrg` 或 `MultiOrg` 身份验证结合使用。 默认值为 `https://login.microsoftonline.com/`。

- **`--client-id <ID>`**

  此项目的客户端 ID。 与 `IndividualB2C`、`SingleOrg` 或 `MultiOrg` 身份验证结合使用。 默认值为 `11111111-1111-1111-11111111111111111`。

- **`--domain <DOMAIN>`**

  目录租户的域。 与 `SingleOrg` 或 `IndividualB2C` 身份验证结合使用。 默认值为 `qualified.domain.name`。

- **`--tenant-id <ID>`**

  要连接到的目录的 TenantId ID。 与 `SingleOrg` 身份验证结合使用。 默认值为 `22222222-2222-2222-2222-222222222222`。

- **`--callback-path <PATH>`**

  重定向 URI 的应用程序基路径中的请求路径。 与 `SingleOrg` 或 `IndividualB2C` 身份验证结合使用。 默认值为 `/signin-oidc`。

- **`-r|--org-read-access`**

  允许此应用程序对目录进行读取访问。 仅适用于 `SingleOrg` 或 `MultiOrg` 身份验证。

- **`--exclude-launch-settings`**

  从生成的模板中排除 launchSettings.json。

- **`--no-https`**

  关闭 HTTPS。 此选项仅适用于 `Individual`、`IndividualB2C`、`SingleOrg` 和 `MultiOrg` 未用于 `--auth` 的情况。

- **`-uld|--use-local-db`**

  指定应使用 LocalDB，而不使用 SQLite。 仅适用于 `Individual` 或 `IndividualB2C` 身份验证。

- **`--no-restore`**

  在项目创建期间不执行隐式还原。

**_

### <a name="blazorwasm"></a>blazorwasm

- _ *`-f|--framework <FRAMEWORK>`**

  指定目标[框架](../../standard/frameworks.md)。

  下表根据所使用的 SDK 版本号列出了默认值：

  | SDK 版本 | 默认值   |
  |-------------|-----------------|
  | 5.0         | `net5.0`        |
  | 3.1         | `netcoreapp3.1` |

- **`--no-restore`**

  在项目创建期间不执行隐式还原。

- **`-ho|--hosted`**

  包括 Blazor WebAssembly 应用的 ASP.NET Core 主机。

- **`-au|--auth <AUTHENTICATION_TYPE>`**

  要使用的身份验证类型。 可能的值为：

  - `None` - 不进行身份验证（默认）。
  - `Individual` - 个人身份验证。
  - `IndividualB2C` - 使用 Azure AD B2C 进行个人身份验证。
  - `SingleOrg` - 对一个租户进行组织身份验证。

- **`--authority <AUTHORITY>`**

  OIDC 提供程序所属的机构。 与 `Individual` 身份验证结合使用。 默认值为 `https://login.microsoftonline.com/`。

- **`--aad-b2c-instance <INSTANCE>`**

  要连接到的 Azure Active Directory B2C 实例。 与 `IndividualB2C` 身份验证结合使用。 默认值为 `https://aadB2CInstance.b2clogin.com/`。

- **`-ssp|--susi-policy-id <ID>`**

  此项目的登录和注册策略 ID。 与 `IndividualB2C` 身份验证结合使用。

- **`--aad-instance <INSTANCE>`**

  要连接到的 Azure Active Directory 实例。 与 `SingleOrg` 身份验证结合使用。 默认值为 `https://login.microsoftonline.com/`。

- **`--client-id <ID>`**

  此项目的客户端 ID。 在独立方案中与 `IndividualB2C`、`SingleOrg` 或 `Individual` 身份验证一起使用。 默认值为 `33333333-3333-3333-33333333333333333`。

- **`--domain <DOMAIN>`**

  目录租户的域。 与 `SingleOrg` 或 `IndividualB2C` 身份验证结合使用。 默认值为 `qualified.domain.name`。

- **`--app-id-uri <URI>`**

  要调用的服务器 API 的应用 ID URI。 与 `SingleOrg` 或 `IndividualB2C` 身份验证结合使用。 默认值为 `api.id.uri`。

- **`--api-client-id <ID>`**

  服务器承载的 API 的客户端 ID。 与 `SingleOrg` 或 `IndividualB2C` 身份验证结合使用。 默认值为 `11111111-1111-1111-11111111111111111`。

- **`-s|--default-scope <SCOPE>`**

  客户端为预配访问令牌所需请求的 API 作用域。 与 `SingleOrg` 或 `IndividualB2C` 身份验证结合使用。 默认值为 `user_impersonation`。

- **`--tenant-id <ID>`**

  要连接到的目录的 TenantId ID。 与 `SingleOrg` 身份验证结合使用。 默认值为 `22222222-2222-2222-2222-222222222222`。

- **`-r|--org-read-access`**

  允许此应用程序对目录进行读取访问。 仅适用于 `SingleOrg` 身份验证。

- **`--exclude-launch-settings`**

  从生成的模板中排除 launchSettings.json。

- **`-p|--pwa`**

  生成支持安装和脱机使用的渐进式 Web 应用程序 (PWA)。

- **`--no-https`**

  关闭 HTTPS。 此选项仅适用于 `Individual`、`IndividualB2C` 和 `SingleOrg` 未用于 `--auth` 的情况。

- **`-uld|--use-local-db`**

  指定应使用 LocalDB，而不使用 SQLite。 仅适用于 `Individual` 或 `IndividualB2C` 身份验证。

- **`--called-api-url <URL>`**

  要从 Web 应用调用的 API 的 URL。 仅适用于未指定 ASP.NET Core 主机的 `SingleOrg` 或 `IndividualB2C` 身份验证。 默认值为 `https://graph.microsoft.com/v1.0/me`。

- **`--calls-graph`**

  指定 Web 应用是否调用 Microsoft Graph。 仅适用于 `SingleOrg` 身份验证。

- **`--called-api-scopes <SCOPES>`**

  为从 Web 应用调用 API 而请求的作用域。 仅适用于未指定 ASP.NET Core 主机的 `SingleOrg` 或 `IndividualB2C` 身份验证。 默认值为 `user.read`。

**_

### <a name="web"></a>Web

- _ *`--exclude-launch-settings`**

  从生成的模板中排除 launchSettings.json。

- **`-f|--framework <FRAMEWORK>`**

  指定目标[框架](../../standard/frameworks.md)。 选项在 .NET Core 2.2 SDK 中不可用。

  下表根据所使用的 SDK 版本号列出了默认值：

  | SDK 版本 | 默认值   |
  |-------------|-----------------|
  | 5.0         | `net5.0`        |
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |
  | 2.1         | `netcoreapp2.1` |

- **`--no-restore`**

  在项目创建期间不执行隐式还原。

- **`--no-https`**

  关闭 HTTPS。

**_

### <a name="mvc-webapp"></a><a name="web-options"></a> mvc、webapp

- _ *`-au|--auth <AUTHENTICATION_TYPE>`**

  要使用的身份验证类型。 可能的值为：

  - `None` - 不进行身份验证（默认）。
  - `Individual` - 个人身份验证。
  - `IndividualB2C` - 使用 Azure AD B2C 进行个人身份验证。
  - `SingleOrg` - 对一个租户进行组织身份验证。
  - `MultiOrg` - 对多个租户进行组织身份验证。
  - `Windows` - Windows 身份验证。

- **`--aad-b2c-instance <INSTANCE>`**

  要连接到的 Azure Active Directory B2C 实例。 与 `IndividualB2C` 身份验证结合使用。 默认值为 `https://login.microsoftonline.com/tfp/`。

- **`-ssp|--susi-policy-id <ID>`**

  此项目的登录和注册策略 ID。 与 `IndividualB2C` 身份验证结合使用。

- **`-rp|--reset-password-policy-id <ID>`**

  此项目的重置密码策略 ID。 与 `IndividualB2C` 身份验证结合使用。

- **`-ep|--edit-profile-policy-id <ID>`**

  此项目的编辑配置文件策略 ID。 与 `IndividualB2C` 身份验证结合使用。

- **`--aad-instance <INSTANCE>`**

  要连接到的 Azure Active Directory 实例。 与 `SingleOrg` 或 `MultiOrg` 身份验证结合使用。 默认值为 `https://login.microsoftonline.com/`。

- **`--client-id <ID>`**

  此项目的客户端 ID。 与 `IndividualB2C`、`SingleOrg` 或 `MultiOrg` 身份验证结合使用。 默认值为 `11111111-1111-1111-11111111111111111`。

- **`--domain <DOMAIN>`**

  目录租户的域。 与 `SingleOrg` 或 `IndividualB2C` 身份验证结合使用。 默认值为 `qualified.domain.name`。

- **`--tenant-id <ID>`**

  要连接到的目录的 TenantId ID。 与 `SingleOrg` 身份验证结合使用。 默认值为 `22222222-2222-2222-2222-222222222222`。

- **`--callback-path <PATH>`**

  重定向 URI 的应用程序基路径中的请求路径。 与 `SingleOrg` 或 `IndividualB2C` 身份验证结合使用。 默认值为 `/signin-oidc`。

- **`-r|--org-read-access`**

  允许此应用程序对目录进行读取访问。 仅适用于 `SingleOrg` 或 `MultiOrg` 身份验证。

- **`--exclude-launch-settings`**

  从生成的模板中排除 launchSettings.json。

- **`--no-https`**

  关闭 HTTPS。 此选项仅适用于未使用 `Individual`、`IndividualB2C`、`SingleOrg` 和 `MultiOrg` 的情况。

- **`-uld|--use-local-db`**

  指定应使用 LocalDB，而不使用 SQLite。 仅适用于 `Individual` 或 `IndividualB2C` 身份验证。

- **`-f|--framework <FRAMEWORK>`**

  指定目标[框架](../../standard/frameworks.md)。 自 .NET Core 3.0 SDK 起可用的选项。

  下表根据所使用的 SDK 版本号列出了默认值：

  | SDK 版本 | 默认值   |
  |-------------|-----------------|
  | 5.0         | `net5.0`        |
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |

- **`--no-restore`**

  在项目创建期间不执行隐式还原。

- **`--use-browserlink`**

  在项目中添加 BrowserLink。 选项在 .NET Core 2.2 和 3.1 SDK 中不可用。

- **`-rrc|--razor-runtime-compilation`**

  确定项目是否配置为在调试生成中使用 [Razor 运行时编译](/aspnet/core/mvc/views/view-compilation#runtime-compilation)。 自 .NET Core 3.1.201 SDK 起可用的选项。

**_

### <a name="angular-react"></a><a name="spa"></a> angular、react

- _ *`-au|--auth <AUTHENTICATION_TYPE>`**

  要使用的身份验证类型。 自 .NET Core 3.0 SDK 起可用。
  
  可能的值为：

  - `None` - 不进行身份验证（默认）。
  - `Individual` - 个人身份验证。

- **`--exclude-launch-settings`**

  从生成的模板中排除 launchSettings.json。

- **`--no-restore`**

  在项目创建期间不执行隐式还原。

- **`--no-https`**

  关闭 HTTPS。 仅当身份验证为 `None` 时，此选项才适用。

- **`-uld|--use-local-db`**

  指定应使用 LocalDB，而不使用 SQLite。 仅适用于 `Individual` 或 `IndividualB2C` 身份验证。 自 .NET Core 3.0 SDK 起可用。

- **`-f|--framework <FRAMEWORK>`**

  指定目标[框架](../../standard/frameworks.md)。 选项在 .NET Core 2.2 SDK 中不可用。

  下表根据所使用的 SDK 版本号列出了默认值：

  | SDK 版本 | 默认值   |
  |-------------|-----------------|
  | 5.0         | `net5.0`        |
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |
  | 2.1         | `netcoreapp2.0` |

**_

### <a name="reactredux"></a>reactredux

- _ *`--exclude-launch-settings`**

  从生成的模板中排除 launchSettings.json。

- **`-f|--framework <FRAMEWORK>`**

  指定目标[框架](../../standard/frameworks.md)。 选项在 .NET Core 2.2 SDK 中不可用。

  下表根据所使用的 SDK 版本号列出了默认值：

  | SDK 版本 | 默认值   |
  |-------------|-----------------|
  | 5.0         | `net5.0`        |
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |
  | 2.1         | `netcoreapp2.0` |

- **`--no-restore`**

  在项目创建期间不执行隐式还原。

- **`--no-https`**

  关闭 HTTPS。

**_

### <a name="razorclasslib"></a>razorclasslib

- _ *`--no-restore`**

  在项目创建期间不执行隐式还原。

- **`-s|--support-pages-and-views`**

  除了将组件添加到此库以外，还支持添加传统的 Razor 页面和视图。 自 .NET Core 3.0 SDK 起可用。

**_
  
### <a name="webapi"></a>webapi

- _ *`-au|--auth <AUTHENTICATION_TYPE>`**

  要使用的身份验证类型。 可能的值为：

  - `None` - 不进行身份验证（默认）。
  - `IndividualB2C` - 使用 Azure AD B2C 进行个人身份验证。
  - `SingleOrg` - 对一个租户进行组织身份验证。
  - `Windows` - Windows 身份验证。

- **`--aad-b2c-instance <INSTANCE>`**

  要连接到的 Azure Active Directory B2C 实例。 与 `IndividualB2C` 身份验证结合使用。 默认值为 `https://login.microsoftonline.com/tfp/`。

- **`-ssp|--susi-policy-id <ID>`**

  此项目的登录和注册策略 ID。 与 `IndividualB2C` 身份验证结合使用。

- **`--aad-instance <INSTANCE>`**

  要连接到的 Azure Active Directory 实例。 与 `SingleOrg` 身份验证结合使用。 默认值为 `https://login.microsoftonline.com/`。

- **`--client-id <ID>`**

  此项目的客户端 ID。 与 `IndividualB2C` 或 `SingleOrg` 身份验证结合使用。 默认值为 `11111111-1111-1111-11111111111111111`。

- **`--domain <DOMAIN>`**

  目录租户的域。 与 `IndividualB2C` 或 `SingleOrg` 身份验证结合使用。 默认值为 `qualified.domain.name`。

- **`--tenant-id <ID>`**

  要连接到的目录的 TenantId ID。 与 `SingleOrg` 身份验证结合使用。 默认值为 `22222222-2222-2222-2222-222222222222`。

- **`-r|--org-read-access`**

  允许此应用程序对目录进行读取访问。 仅适用于 `SingleOrg` 身份验证。

- **`--exclude-launch-settings`**

  从生成的模板中排除 launchSettings.json。

- **`--no-https`**

  关闭 HTTPS。 `app.UseHsts` 和 `app.UseHttpsRedirection` 未添加到 `Startup.Configure` 中。 此选项仅适用于 `IndividualB2C` 或 `SingleOrg` 未用于身份验证的情况。

- **`-uld|--use-local-db`**

  指定应使用 LocalDB，而不使用 SQLite。 仅适用于 `IndividualB2C` 身份验证。

- **`-f|--framework <FRAMEWORK>`**

  指定目标[框架](../../standard/frameworks.md)。 选项在 .NET Core 2.2 SDK 中不可用。

  下表根据所使用的 SDK 版本号列出了默认值：

  | SDK 版本 | 默认值   |
  |-------------|-----------------|
  | 5.0         | `net5.0`        |
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |
  | 2.1         | `netcoreapp2.1` |

- **`--no-restore`**

  在项目创建期间不执行隐式还原。

**_

### <a name="globaljson"></a>globaljson

- _ *`--sdk-version <VERSION_NUMBER>`**

  指定要在 global.json 文件中使用的 .NET SDK 版本。

***

## <a name="examples"></a>示例

- 通过指定模板名称，创建 C# 控制台应用程序项目：

  ```dotnetcli
  dotnet new "Console Application"
  ```

- 在当前目录中创建 F# 控制台应用程序项目：

  ```dotnetcli
  dotnet new console -lang "F#"
  ```

- 在指定的目录中创建 .NET Standard 类库项目：

  ```dotnetcli
  dotnet new classlib -lang VB -o MyLibrary
  ```

- 在当前目录中新建没有设置身份验证的 ASP.NET Core C# MVC 项目：

  ```dotnetcli
  dotnet new mvc -au None
  ```

- 创建新的 xUnit 项目：

  ```dotnetcli
  dotnet new xunit
  ```

- 列出可用于单页应用程序 (SPA) 模板的所有模板：

  ```dotnetcli
  dotnet new spa -l
  ```

- 列出与“we”子字符串匹配的所有模板。 找不到完全匹配，因此子字符串匹配针对短名称和名称列运行。

  ```dotnetcli
  dotnet new we -l
  ```

- 尝试调用与 ng 匹配的模板。 如果无法确定单个匹配项，请列出部分匹配项的模板。

  ```dotnetcli
  dotnet new ng
  ```

- 安装 ASP.NET Core 的 SPA 模板 2.0 版：

  ```dotnetcli
  dotnet new -i Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
  ```

- 列出已安装的模板及其详细信息，包括如何卸载它们：

  ```dotnetcli
  dotnet new -u
  ```

- 在当前目录中创建 global.json，将 SDK 版本设置为 3.1.101：

  ```dotnetcli
  dotnet new globaljson --sdk-version 3.1.101
  ```

## <a name="see-also"></a>请参阅

- [dotnet new 自定义模板](custom-templates.md)
- [创建 dotnet new 自定义模板](../tutorials/cli-templates-create-item-template.md)
- [dotnet/dotnet-template-samples GitHub 存储库](https://github.com/dotnet/dotnet-template-samples)
- [dotnet new 可用模板](https://github.com/dotnet/templating/wiki/Available-templates-for-dotnet-new)
