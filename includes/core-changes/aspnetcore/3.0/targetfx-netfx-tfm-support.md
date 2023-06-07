---
ms.openlocfilehash: ec6724ab378dd614c55a024ede18d997d27be3a3
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032439"
---
### <a name="target-framework-net-framework-support-dropped"></a>目标框架：删除了 .NET Framework 支持

从 ASP.NET Core 3.0 开始，.NET Framework 不再是受支持的目标框架。

#### <a name="change-description"></a>更改描述

.NET Framework 4.8 是 .NET Framework 的上一个主要版本。 新的 ASP.NET Core 应用应基于 .NET Core 构建。 从 .NET Core 3.0 版开始，可将 ASP.NET Core 3.0 视为 .NET Core 的一部分。

将 .NET Framework 和 ASP.NET Core 结合使用的客户可以使用 [2.1 LTS 版本](https://dotnet.microsoft.com/download/dotnet-core/2.1)以完全受支持的方式继续。 至少在 2021 年 8 月 21 日之前，将继续提供对 2.1 的支持和服务。 根据 [.NET 支持策略](https://dotnet.microsoft.com/platform/support-policy)，此日期为声明 LTS 版本后三年。 .NET Framework 对 ASP.NET Core 2.1 包的支持  将无限延期，类似于[其他基于包的 ASP.NET 框架的服务策略](https://dotnet.microsoft.com/platform/support/policy/aspnet)。

有关从 .NET Framework 移植到 .NET Core 的详细信息，请参阅[移植到 .NET Core](~/docs/core/porting/index.md)。

`Microsoft.Extensions` 包（如日志记录、依赖项注入和配置）和 Entity Framework Core 不受影响。 它们将继续支持 .NET Standard。

有关此更改动机的详细信息，请参阅[原始博客文章](https://devblogs.microsoft.com/aspnet/a-first-look-at-changes-coming-in-asp-net-core-3-0/)。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="old-behavior"></a>旧行为

ASP.NET Core 应用可在 .NET Core 或 .NET Framework 上运行。

#### <a name="new-behavior"></a>新行为

ASP.NET Core 应用只能在 .NET Core 上运行。

#### <a name="recommended-action"></a>建议操作

请执行以下一项操作：

- 将应用保留在 ASP.NET Core 2.1 上。
- 将应用和依赖项迁移到 .NET Core。

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

无

<!-- 

#### Affected APIs

Not detectable via API analysis

-->
