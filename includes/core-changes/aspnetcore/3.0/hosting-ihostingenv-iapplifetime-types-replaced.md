---
ms.openlocfilehash: be1fad236dd3eed047b010e93285aec8bc607b61
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032270"
---
### <a name="hosting-ihostingenvironment-and-iapplicationlifetime-types-marked-obsolete-and-replaced"></a>托管：IHostingEnvironment 和 IApplicationLifetime 类型标记为过时并被替换

引入了新类型以替换现有的 `IHostingEnvironment` 和 `IApplicationLifetime` 类型。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="old-behavior"></a>旧行为

`Microsoft.Extensions.Hosting` 和 `Microsoft.AspNetCore.Hosting` 有两个不同的 `IHostingEnvironment` 和 `IApplicationLifetime` 类型。

#### <a name="new-behavior"></a>新行为

旧类型已标记为过时，并已替换为新类型。

#### <a name="reason-for-change"></a>更改原因

在 ASP.NET Core 2.1 中引入 `Microsoft.Extensions.Hosting` 时，从 `Microsoft.AspNetCore.Hosting` 复制了某些类型（如 `IHostingEnvironment` 和 `IApplicationLifetime`）。 某些 ASP.NET Core 3.0 更改会导致应用包括 `Microsoft.Extensions.Hosting` 和 `Microsoft.AspNetCore.Hosting` 命名空间。 如果同时引用了两个命名空间，则使用这些重复类型会导致“引用不明确”编译器错误。

#### <a name="recommended-action"></a>建议操作

将旧类型的所有使用替换为新引入的类型，如下所示：

**过时类型（警告）：**

- <xref:Microsoft.Extensions.Hosting.IHostingEnvironment?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment?displayProperty=nameWithType>
- <xref:Microsoft.Extensions.Hosting.IApplicationLifetime?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime?displayProperty=nameWithType>
- <xref:Microsoft.Extensions.Hosting.EnvironmentName?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Hosting.EnvironmentName?displayProperty=nameWithType>

**新类型：**

- <xref:Microsoft.Extensions.Hosting.IHostEnvironment?displayProperty=nameWithType>
- `Microsoft.AspNetCore.Hosting.IWebHostEnvironment : IHostEnvironment`
- <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime?displayProperty=nameWithType>
- <xref:Microsoft.Extensions.Hosting.Environments?displayProperty=nameWithType>

新的 `IHostEnvironment` `IsDevelopment` 和 `IsProduction` 扩展方法位于 `Microsoft.Extensions.Hosting` 命名空间中。 可能需要将该命名空间添加到项目中。

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

- <xref:Microsoft.AspNetCore.Hosting.EnvironmentName?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment?displayProperty=nameWithType>
- <xref:Microsoft.Extensions.Hosting.EnvironmentName?displayProperty=nameWithType>
- <xref:Microsoft.Extensions.Hosting.IApplicationLifetime?displayProperty=nameWithType>
- <xref:Microsoft.Extensions.Hosting.IHostingEnvironment?displayProperty=nameWithType>

<!-- 

#### Affected APIs

- `T:Microsoft.AspNetCore.Hosting.EnvironmentName`
- `T:Microsoft.AspNetCore.Hosting.IApplicationLifetime`
- `T:Microsoft.AspNetCore.Hosting.IHostingEnvironment`
- `T:Microsoft.Extensions.Hosting.EnvironmentName`
- `T:Microsoft.Extensions.Hosting.IApplicationLifetime`
- `T:Microsoft.Extensions.Hosting.IHostingEnvironment`

-->
