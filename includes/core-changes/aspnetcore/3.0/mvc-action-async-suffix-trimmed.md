---
ms.openlocfilehash: 58b1190e3e6a3168d35700eed655f6756e076a29
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032292"
---
### <a name="mvc-async-suffix-trimmed-from-controller-action-names"></a>MVC：从控制器操作名称中剪裁的异步后缀

作为寻址 [dotnet/aspnetcore#4849](https://github.com/dotnet/aspnetcore/issues/4849) 的一部分，ASP.NET Core MVC 默认从操作名称中剪裁后缀 `Async`。 从 ASP.NET Core 3.0 开始，这一更改会影响路由和链接生成。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="old-behavior"></a>旧行为

考虑以下 ASP.NET Core MVC 控制器：

```csharp
public class ProductController : Controller
{
    public async IActionResult ListAsync()
    {
        var model = await DbContext.Products.ToListAsync();
        return View(model);
    }
}
```

此操作可通过 `Product/ListAsync` 进行路由。 链接生成需要指定 `Async` 后缀。 例如：

```cshtml
<a asp-controller="Product" asp-action="ListAsync">List</a>
```

#### <a name="new-behavior"></a>新行为

在 ASP.NET Core 3.0 中，操作可通过 `Product/List` 进行路由。 链接生成代码应省略 `Async` 后缀。 例如：

```cshtml
<a asp-controller="Product" asp-action="List">List</a>
```

此更改不会影响使用 `[ActionName]` 属性指定的名称。 可以通过在 `Startup.ConfigureServices` 中将 `MvcOptions.SuppressAsyncSuffixInActionNames` 设置为 `false` 来禁用新行为：

```csharp
services.AddMvc(options =>
{
   options.SuppressAsyncSuffixInActionNames = false;
});
```

#### <a name="reason-for-change"></a>更改原因

按照约定，异步 .NET 方法以 `Async` 为后缀。 但是，当方法定义 MVC 操作时，不需要使用 `Async` 后缀。

#### <a name="recommended-action"></a>建议操作

如果应用依赖于保留名称的 `Async` 后缀的 MVC 操作，请选择下列缓解措施之一：

- 使用 `[ActionName]` 属性以保留原始名称。
- 通过在 `Startup.ConfigureServices` 中将 `MvcOptions.SuppressAsyncSuffixInActionNames` 设置为 `false` 来完全禁用重命名：

```csharp
services.AddMvc(options =>
{
   options.SuppressAsyncSuffixInActionNames = false;
});
```

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

None

<!-- 

#### Affected APIs

Not detectable via API analysis

-->
