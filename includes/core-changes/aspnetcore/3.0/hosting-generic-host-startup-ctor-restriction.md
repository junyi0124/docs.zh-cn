---
ms.openlocfilehash: d1ddba72ce25c5e01025d916d52f785b5a1a9e71
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032268"
---
### <a name="hosting-generic-host-restricts-startup-constructor-injection"></a>托管：通用主机限制 Startup 构造函数注入

通用主机支持的 `Startup` 类构造函数注入的唯一类型为 `IHostEnvironment`、`IWebHostEnvironment` 和 `IConfiguration`。 使用 `WebHost` 的应用不受影响。

#### <a name="change-description"></a>更改描述

在低于 ASP.NET Core 3.0 的版本中，构造函数注入可用于 `Startup` 类的构造函数中的任意类型。 在 ASP.NET Core 3.0 中，Web 堆栈将重新平台化到通用主机库上。 可以在模板的 Program .cs  文件中查看更改：

**ASP.NET Core 2.x：**

<https://github.com/dotnet/aspnetcore/blob/5cb615fcbe8559e49042e93394008077e30454c0/src/Templating/src/Microsoft.DotNet.Web.ProjectTemplates/content/EmptyWeb-CSharp/Program.cs#L20-L22>

**ASP.NET Core 3.0：**

<https://github.com/dotnet/aspnetcore/blob/b1ca2c1155da3920f0df5108b9fedbe82efaa11c/src/ProjectTemplates/Web.ProjectTemplates/content/EmptyWeb-CSharp/Program.cs#L19-L24>

`Host` 使用一个依赖关系注入 (DI) 容器生成应用。 `WebHost` 使用两个容器：一个用于主机，另一个用于应用。 因此，`Startup` 构造函数不再支持自定义服务注入。 只能注入 `IHostEnvironment`、`IWebHostEnvironment` 和 `IConfiguration`。 此更改可防止出现 DI 问题，例如重复创建单一实例服务。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="reason-for-change"></a>更改原因

此更改是将 Web 堆栈重新平台化到通用主机库的结果。

#### <a name="recommended-action"></a>建议操作

将服务注入 `Startup.Configure` 方法签名。 例如：

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> options)
```

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

None

<!-- 

#### Affected APIs

Not detectable via API analysis

-->
