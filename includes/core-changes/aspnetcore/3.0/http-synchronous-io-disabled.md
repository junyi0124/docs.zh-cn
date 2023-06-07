---
ms.openlocfilehash: 53d2c989120c92f4e2d18f50ce4b364bd4c9b604
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032276"
---
### <a name="http-synchronous-io-disabled-in-all-servers"></a>HTTP：所有服务器均禁用同步 IO

从 ASP.NET Core 3.0 开始，默认将禁用同步服务器操作。

#### <a name="change-description"></a>更改描述

`AllowSynchronousIO` 是每台服务器中包含的一个选项，用于启用或禁用同步 IO API，如 `HttpRequest.Body.Read`、`HttpResponse.Body.Write` 和 `Stream.Flush`。 这些 API 长期以来一直是线程不足和应用挂起的根源。 从 ASP.NET Core 3.0 预览版 3 开始，默认将禁用这些同步操作。

受影响的服务器：

- Kestrel
- HttpSys
- IIS（进程内）
- TestServer

错误应如下所示：

- `Synchronous operations are disallowed. Call ReadAsync or set AllowSynchronousIO to true instead.`
- `Synchronous operations are disallowed. Call WriteAsync or set AllowSynchronousIO to true instead.`
- `Synchronous operations are disallowed. Call FlushAsync or set AllowSynchronousIO to true instead.`

每台服务器都有一个 `AllowSynchronousIO` 选项，该选项控制此行为，且它们的默认值当前为 `false`。

作为一种临时缓解措施，还可以根据每个请求重写该行为。 例如：

```csharp
var syncIOFeature = HttpContext.Features.Get<IHttpBodyControlFeature>();
if (syncIOFeature != null)
{
    syncIOFeature.AllowSynchronousIO = true;
}
```

如果调用 `Dispose` 中同步 API 的 `TextWriter` 或另一个流出现问题，请改为调用新的 `DisposeAsync` API。

有关讨论，请参阅 [dotnet/aspnetcore#7644](https://github.com/dotnet/aspnetcore/issues/7644)。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="old-behavior"></a>旧行为

默认情况下允许 `HttpRequest.Body.Read`、`HttpResponse.Body.Write` 和 `Stream.Flush`。

#### <a name="new-behavior"></a>新行为

默认情况下，不允许使用这些同步 API：

错误应如下所示：

- `Synchronous operations are disallowed. Call ReadAsync or set AllowSynchronousIO to true instead.`
- `Synchronous operations are disallowed. Call WriteAsync or set AllowSynchronousIO to true instead.`
- `Synchronous operations are disallowed. Call FlushAsync or set AllowSynchronousIO to true instead.`

#### <a name="reason-for-change"></a>更改原因

这些同步 API 长期以来一直是线程不足和应用挂起的根源。 从 ASP.NET Core 3.0 预览版 3 开始，默认将禁用同步操作。

#### <a name="recommended-action"></a>建议操作

使用这些方法的异步版本。 作为一种临时缓解措施，还可以根据每个请求重写该行为。

```csharp
var syncIOFeature = HttpContext.Features.Get<IHttpBodyControlFeature>();
if (syncIOFeature != null)
{
    syncIOFeature.AllowSynchronousIO = true;
}
```

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.IO.Stream.Flush%2A?displayProperty=nameWithType>
- <xref:System.IO.Stream.Read%2A?displayProperty=nameWithType>
- <xref:System.IO.Stream.Write%2A?displayProperty=nameWithType>

<!--

#### Affected APIs

- `Overload:System.IO.Stream.Flush`
- `Overload:System.IO.Stream.Read`
- `Overload:System.IO.Stream.Write`

-->
