---
ms.openlocfilehash: cd13e7560ee98e0c862c5e2293521c6aaa273455
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032404"
---
### <a name="razor-runtime-compilation-moved-to-a-package"></a>Razor：运行时编译已移动到包

支持 Razor 视图的运行时编译，已将 Razor Pages 移动到单独的包中。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="old-behavior"></a>旧行为

无需额外的包，即可使用运行时编译。

#### <a name="new-behavior"></a>新行为

此功能已移至 [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) 包中。

以下 API 以前在 `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions` 中提供，用于支持运行时编译。 现在可通过 `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation.MvcRazorRuntimeCompilationOptions` 获取 API。

- `RazorViewEngineOptions.FileProviders` 现在为 `MvcRazorRuntimeCompilationOptions.FileProviders`
- `RazorViewEngineOptions.AdditionalCompilationReferences` 现在为 `MvcRazorRuntimeCompilationOptions.AdditionalReferencePaths`

此外，已删除 `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange`。 默认情况下，通过引用 `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation` 包来启用对文件更改的重新编译。

#### <a name="reason-for-change"></a>更改原因

此更改是删除 Roslyn 上的 ASP.NET Core 共享框架依赖项所必需的。

#### <a name="recommended-action"></a>建议操作

需要对 Razor 文件进行运行时编译或重新编译的应用应执行以下步骤：

1. 添加对 `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation` 包的引用。
1. 更新项目的 `Startup.ConfigureServices` 方法以包含对 `AddRazorRuntimeCompilation` 的调用。 例如：

    ```csharp
    services.AddMvc()
        .AddRazorRuntimeCompilation();
    ```

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

<xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions?displayProperty=fullName>

<!--

#### Affected APIs

`T:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`

-->
