---
ms.openlocfilehash: 4d99d0b6e99a7a9b976cf11832b33ad3bdc6d299
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032271"
---
### <a name="hosting-objectpoolprovider-removed-from-webhostbuilder-dependencies"></a>托管：ObjectPoolProvider 已从 WebHostBuilder 依赖项中删除

作为使 ASP.NET Core 更具成本效益计划的一部分，`ObjectPoolProvider` 已从主要依赖项集中删除。 依赖于 `ObjectPoolProvider` 的特定组件现在会自行添加。

有关讨论，请参阅 [dotnet/aspnetcore#5944](https://github.com/dotnet/aspnetcore/issues/5944)。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="old-behavior"></a>旧行为

`WebHostBuilder` 默认在 DI 容器中提供 `ObjectPoolProvider`。

#### <a name="new-behavior"></a>新行为

`WebHostBuilder` 默认不再在 DI 容器中提供 `ObjectPoolProvider`。

#### <a name="reason-for-change"></a>更改原因

进行此更改是为了使 ASP.NET Core 更具成本效益。

#### <a name="recommended-action"></a>建议操作

如果组件需要 `ObjectPoolProvider`，则需要通过 `IServiceCollection` 将其添加到依赖项。

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

None

<!-- 

#### Affected APIs

Not detectable via API analysis

-->
