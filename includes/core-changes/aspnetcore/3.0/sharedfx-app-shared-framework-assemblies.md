---
ms.openlocfilehash: d598d8d3203e804e5e935c3564b0053f9fc2e9a6
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032415"
---
### <a name="shared-framework-assemblies-removed-from-microsoftaspnetcoreapp"></a>共享框架：从 Microsoft.AspNetCore.App 中删除了程序集

从 ASP.NET Core 3.0 开始，ASP.NET Core 共享框架 (`Microsoft.AspNetCore.App`) 只包含 Microsoft 完全开发、支持和可服务的第一方程序集。

#### <a name="change-description"></a>更改描述

将这一更改视为重新定义 ASP.NET Core“平台”的边界。 [任何人都可通过 GitHub 生成共享框架的源代码](https://github.com/dotnet/source-build)，共享框架将继续为应用提供 .NET Core 共享框架的现有优势。 一些优势包括部署大小更小、集中修补和启动时间更快。

作为此更改的一部分，`Microsoft.AspNetCore.App` 中引入了一些值得注意的重大更改。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="old-behavior"></a>旧行为

项目通过项目文件中的 `<PackageReference>` 元素引用了 `Microsoft.AspNetCore.App`。

此外，`Microsoft.AspNetCore.App` 包含以下子组件：

- Json.NET (`Newtonsoft.Json`)
- Entity Framework Core（以 `Microsoft.EntityFrameworkCore.` 为前缀的程序集）
- Roslyn (`Microsoft.CodeAnalysis`)

#### <a name="new-behavior"></a>新行为

对 `Microsoft.AspNetCore.App` 的引用不再需要项目文件中的 `<PackageReference>` 元素。 .NET Core SDK 支持名为 `<FrameworkReference>` 的新元素，该元素将替代对 `<PackageReference>` 的使用。

有关详细信息，请参阅 [dotnet/aspnetcore#3612](https://github.com/dotnet/aspnetcore/issues/3612)。

Entity Framework Core 作为 NuGet 包提供。 此更改将使随附模型与 .NET 上的所有其他数据访问库保持一致。 它在支持各种 .NET 平台的同时，为 Entity Framework Core 提供最简单的途径来继续创新。 将 Entity Framework Core 移出共享框架不会影响其作为 Microsoft 开发、支持和可服务的库的状态。 [.NET Core 支持策略](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)继续支持此功能。

Json.NET 和 Entity Framework Core 可继续使用 ASP.NET Core。 但是，它们不会包含在共享框架中。

有关详细信息，请参阅 [.NET Core 3.0 中 JSON 的未来](https://github.com/dotnet/announcements/issues/90)。 另请参阅[从共享框架中删除的二进制文件的完整列表](https://github.com/dotnet/aspnetcore/issues/3755)。

#### <a name="reason-for-change"></a>更改原因

此更改简化了 `Microsoft.AspNetCore.App` 的使用，并减少了 NuGet 包与共享框架之间的重复。

有关此更改动机的详细信息，请参阅[博客文章](https://devblogs.microsoft.com/aspnet/a-first-look-at-changes-coming-in-asp-net-core-3-0/)。

#### <a name="recommended-action"></a>建议操作

项目不需要使用 `Microsoft.AspNetCore.App` 中的程序集作为 NuGet 包。 为了简化 ASP.NET Core 共享框架的定位和使用，已不再生成自 ASP.NET Core 1.0 以来提供的许多 NuGet 包。 通过将 `<FrameworkReference>` 用于 `Microsoft.AspNetCore.App`，应用仍可使用这些包提供的 API。 常见的 API 示例包括 Kestrel、MVC 和 Razor。

此更改不适用于通过 ASP.NET Core 2.x 中的 `Microsoft.AspNetCore.App` 引用的所有二进制文件。 值得注意的例外包括：

- 继续以 .NET Standard 为目标的 `Microsoft.Extensions` 库将以 NuGet 包的形式提供（请参阅 <https://github.com/dotnet/extensions>）。
- 不属于 `Microsoft.AspNetCore.App` 的 ASP.NET Core 团队生成的 API。 例如，以下组件以 NuGet 包的形式提供：
  - Entity Framework Core
  - 提供第三方集成的 API
  - 实验性功能
  - 包含无法[满足共享框架中的要求](https://github.com/dotnet/aspnetcore/blob/4e44e5bcbedd961cc0d4f6b846699c7c494f5597/docs/SharedFramework.md)的依赖项的 API
- 用于保留对 Json.NET 的支持的 MVC 扩展。 API 将作为 NuGet 包提供，以支持使用 Json.NET 和 MVC。
- SignalR .NET 客户端将继续支持 .NET Standard 并作为 NuGet 包提供。 它可用于许多 .NET 运行时，例如 Xamarin 和 UWP。

有关详细信息，请参阅[停止为 3.0 中的共享框架程序集生成包](https://github.com/dotnet/aspnetcore/issues/3756)。 有关讨论，请参阅 [dotnet/aspnetcore#3757](https://github.com/dotnet/aspnetcore/issues/3757)。

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

- <xref:Microsoft.CodeAnalysis?displayProperty=nameWithType>
- <xref:Microsoft.EntityFrameworkCore?displayProperty=nameWithType>

<!--

#### Affected APIs

- `N:Microsoft.CodeAnalysis`
- `N:Microsoft.EntityFrameworkCore`

-->
