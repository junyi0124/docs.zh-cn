---
title: .NET Core 2.0 的新增功能
description: 了解 .NET Core 的新增功能。
ms.date: 08/13/2017
ms.openlocfilehash: bdca38d1584107727fe8e20b80ab2337f4cb05f1
ms.sourcegitcommit: 67ebdb695fd017d79d9f1f7f35d145042d5a37f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92223718"
---
# <a name="whats-new-in-net-core-20"></a>.NET Core 2.0 的新增功能

.NET Core 2.0 提供以下几个方面的增强功能和新功能：

- [工具](#tooling)
- [语言支持](#language-support)
- [平台改进](#platform-improvements)
- [API 更改](#api-changes-and-library-support)
- [Visual Studio 集成](#visual-studio-integration)
- [文档改进](#documentation-improvements)

## <a name="tooling"></a>工具

### <a name="dotnet-restore-runs-implicitly"></a>dotnet restore 隐式运行

在旧版 .NET Core 中，在使用 [dotnet new](../tools/dotnet-new.md) 命令新建项目后，以及每当向项目添加新的依赖项时，都必须立即运行 [dotnet restore](../tools/dotnet-restore.md) 命令，以便下载依赖项。

[!INCLUDE[DotNet Restore Note](~/includes/dotnet-restore-note.md)]

还可以将 `--no-restore` 开关传递到 `new`、`run`、`build`、`publish`、`pack` 和 `test` 命令，从而禁用自动调用 `dotnet restore`。

### <a name="retargeting-to-net-core-20"></a>重定目标到 .NET Core 2.0

如果已安装 .NET Core 2.0 SDK，那么定目标到 .NET Core 1.x 的项目可以重定目标到 .NET Core 2.0。

若要重定目标到 .NET Core 2.0，请将 `<TargetFramework>` 元素（或 `<TargetFrameworks>` 元素，如果项目文件中有多个目标的话）值从 1.x 更改为 2.0，从而编辑项目文件：

```xml
<PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
 </PropertyGroup>
```

还可以用同样的方式，将 .NET Standard 库重定目标到 .NET Standard 2.0：

```xml
<PropertyGroup>
    <TargetFramework>netstandard2.0</TargetFramework>
 </PropertyGroup>
```

若要详细了解如何将项目迁移到 .NET Core 2.0，请参阅[从 ASP.NET Core 1.x 迁移到 ASP.NET Core 2.0](/aspnet/core/migration/1x-to-2x/index)。

## <a name="language-support"></a>语言支持

除了支持 C# 和 F# 外，.NET Core 2.0 还支持 Visual Basic。

### <a name="visual-basic"></a>Visual Basic

在版本 2.0 发布后，.NET Core 现在支持 Visual Basic 2017。 可使用 Visual Basic 创建以下类型项目：

- .NET Core 控制台应用程序
- .NET Core 类库
- .NET Standard 类库
- .NET Core 单元测试项目
- .NET Core xUnit 测试项目

例如，若要创建 Visual Basic“Hello World”应用程序，请通过命令行按照以下步骤操作：

1. 打开控制台窗口，创建项目目录，并将它设为当前目录。

1. 输入命令 `dotnet new console -lang vb`。

   此命令创建文件扩展名为 `.vbproj` 的项目文件，以及名为 Program.vb  的 Visual Basic 源代码文件。 此文件包含用于将字符串“Hello World!”写入控制台窗口的源代码 。

1. 输入命令 `dotnet run`。 [.NET Core CLI](../tools/index.md) 自动编译并执行应用程序，在控制台窗口中 显示文本字符串“Hello World!”。

### <a name="support-for-c-71"></a>支持 C# 7.1

.NET Core 2.0 支持 C# 7.1，其中新增了大量功能，具体包括：

- 可以使用 [async](../../csharp/language-reference/keywords/async.md) 关键字标记 `Main` 方法（应用程序入口点）。
- 推断的元组名称。
- 默认表达式。

<!-- For more information see [link to C# what's new](url). -->

## <a name="platform-improvements"></a>平台改进

.NET Core 2.0 包括许多功能，可便于用户更轻松地在支持的操作系统上安装并使用 .NET Core。

### <a name="net-core-for-linux-is-a-single-implementation"></a>.NET Core for Linux 是一个实现代码

.NET Core 2.0 提供一个 Linux 实现代码，适用于多个 Linux 发行版本。 .NET Core 1.x 要求下载发行版本专属的 Linux 实现代码。

还可以开发定目标到 Linux 一个操作系统的应用程序。 .NET Core 1.x 要求分别定目标到每个 Linux 发行版本。

### <a name="support-for-the-apple-cryptographic-libraries"></a>支持 Apple 加密库

macOS 上的 .NET Core 1.x 要求使用 OpenSSL 工具包的加密库。 .NET Core 2.0 使用 Apple 加密库，不要求使用 OpenSSL，因此不再需要安装它。

## <a name="api-changes-and-library-support"></a>API 更改和库支持

### <a name="support-for-net-standard-20"></a>支持 .NET Standard 2.0

.NET Standard 定义了一组版本化 API，这些 API 必须可用于符合相应 Standard 版本要求的 .NET 实现代码。 .NET Standard 面向库开发者。 旨在保证功能对每个 .NET 实现代码中以 .NET Standard 版本为目标的库可用。 .NET Core 1.x 支持 .NET Standard 版本 1.6；.NET Core 2.0 支持最新版 .NET Core 2.0。 有关详细信息，请参阅 [.NET Standard](../../standard/net-standard.md)。

.NET Standard 2.0 比 .NET Standard 1.6 多包含 20,000 多个 API。 此扩展的外围应用的大部分来自于，将 .NET Framework 和 Xamarin 的通用 API 合并到 .NET Standard。

.NET Standard 2.0 类库还可以引用 .NET Framework 类库，但前提是它们调用 .NET Standard 2.0 中的 API。 不需要重新编译 .NET Framework 库。

有关自上一版本 (.NET Standard 1.6) 起添加到 .NET Standard 的 API 列表，请参阅 [.NET Standard 2.0 与 1.6](https://raw.githubusercontent.com/dotnet/standard/master/docs/versions/netstandard2.0_diff.md)。

### <a name="expanded-surface-area"></a>扩展的外围应用

与 .NET Core 1.1 相比，.NET Core 2.0 中的可用 API 总数增加了一倍以上。

借助 [Windows 兼容包](../porting/windows-compat-pack.md)，从 .NET Framework 移植也简单了很多。

### <a name="support-for-net-framework-libraries"></a>支持 .NET Framework 库

.NET Core 代码可以引用现有的 .NET Framework 库，包括现有的 NuGet 包。 请注意，库必须使用 .NET Standard 中的 API。

## <a name="visual-studio-integration"></a>Visual Studio 集成

Visual Studio 2017 版本 15.3 和（在某些情况下）Visual Studio for Mac 提供了大量面向 .NET Core 开发者的重大增强功能。

### <a name="retargeting-net-core-apps-and-net-standard-libraries"></a>重定目标 .NET Core 应用程序和 .NET Standard 库

如果已安装 .NET Core 2.0 SDK，可以将 .NET Core 1.x 项目重定目标到 .NET Core 2.0，并将 .NET Standard 1.x 库重定目标到 .NET Standard 2.0。

若要在 Visual Studio 中重定目标项目，可以打开项目属性对话框的“应用程序”  选项卡，再将“目标框架”  值更改为“.NET Core 2.0”  或“.NET Standard 2.0”  。 还可以通过右键单击项目并选择“编辑 \*.csproj 文件”  选项进行更改。 有关详细信息，请参阅本主题前面的[工具](#tooling)部分。

### <a name="live-unit-testing-support-for-net-core"></a>.NET Core 的 Live Unit Testing 支持

修改代码时，Live Unit Testing 在后台自动运行任何受影响的单元测试，并在 Visual Studio 环境中实时显示结果和代码覆盖率。 .NET Core 2.0 现在支持 Live Unit Testing。 以前，Live Unit Testing 仅适用于 .NET Framework 应用程序。

有关详细信息，请参阅[使用 Visual Studio 执行 Live Unit Testing](/visualstudio/test/live-unit-testing) 和 [Live Unit Testing 常见问题解答](/visualstudio/test/live-unit-testing-faq)。

### <a name="better-support-for-multiple-target-frameworks"></a>更好地支持多个目标框架

若要为多个目标框架生成项目，现在可以从顶级菜单中选择目标平台。 在下图中，名为 SCD1 的项目将目标定为 64 位 macOS X 10.11 (`osx.10.11-x64`) 和 64 位 Windows 10/Windows Server 2016 (`win10-x64`)。 可以先选择目标框架，再选择项目按钮（在此示例中是为了要运行调试版本）。

![显示生成项目时的目标框架选择的屏幕截图。](./media/dotnet-core-2-0/target-framework-selection.png)

### <a name="side-by-side-support-for-net-core-sdks"></a>对 .NET Core SDK 的并行支持

现在可以安装 .NET Core SDK，与 Visual Studio 互不影响。 这样，单版本 Visual Studio 可以生成定目标到不同 .NET Core 版本的项目。 以前，Visual Studio 和 .NET Core SDK 紧密结合在一起；特定版本的 SDK 附带了特定版本的 Visual Studio。

## <a name="documentation-improvements"></a>文档改进

### <a name="net-application-architecture"></a>.NET 应用程序体系结构

通过 [.NET 应用程序体系结构](https://dotnet.microsoft.com/learn/dotnet/architecture-guides)，可以查看一系列电子图书，其中提供了有关如何使用 .NET 生成内容的指导、最佳做法和示例应用程序：

- [微服务和 Docker 容器](../../architecture/microservices/index.md)
- [使用 ASP.NET 的 Web 应用程序](../../architecture/modern-web-apps-azure/index.md)
- [使用 Xamarin 的移动应用](/xamarin/xamarin-forms/enterprise-application-patterns/index)
- [使用 Azure 部署到云的应用程序](/azure/architecture/reference-architectures/index)

## <a name="see-also"></a>请参阅

- [ASP.NET Core 2.0 的新增功能](/aspnet/core/aspnetcore-2.0)
