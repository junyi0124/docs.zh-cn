---
ms.openlocfilehash: 06d5f48566c239e37355496c3f27163d952602c6
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032282"
---
### <a name="kestrel-connection-adapters-removed"></a>Kestrel：已删除连接适配器

作为将“pubternal”API 移动到 `public` 的移动的一部分，已从 Kestrel 中删除 `IConnectionAdapter` 的概念。 正在将连接适配器替换为连接中间件（这与 ASP.NET Core 管道中的 HTTP 中间件类似，但适用于较低级别的连接）。 HTTPS 和连接日志记录已从连接适配器转移到连接中间件。 这些扩展方法应能继续无缝运行，但实现详细信息发生了改变。

有关详细信息，请参阅 [dotnet/aspnetcore#11412](https://github.com/dotnet/aspnetcore/pull/11412)。 有关讨论，请参阅 [dotnet/aspnetcore#11475](https://github.com/dotnet/aspnetcore/issues/11475)。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="old-behavior"></a>旧行为

Kestrel 扩展性组件是使用 `IConnectionAdapter` 创建的。

#### <a name="new-behavior"></a>新行为

Kestrel 扩展性组件被创建为[中间件](https://github.com/dotnet/aspnetcore/pull/11412/files#diff-89acc06acf1b2e96bbdb811ce523619f)。

#### <a name="reason-for-change"></a>更改原因

此更改旨在提供更灵活的扩展性体系结构。

#### <a name="recommended-action"></a>建议操作

转换 `IConnectionAdapter` 的任何实现以使用新的中间件模式，如[此处](https://github.com/dotnet/aspnetcore/pull/11412/files#diff-89acc06acf1b2e96bbdb811ce523619f)所示。

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

`Microsoft.AspNetCore.Server.Kestrel.Core.Adapter.Internal.IConnectionAdapter`

<!-- 

#### Affected APIs

`T:Microsoft.AspNetCore.Server.Kestrel.Core.Adapter.Internal.IConnectionAdapter`

-->
