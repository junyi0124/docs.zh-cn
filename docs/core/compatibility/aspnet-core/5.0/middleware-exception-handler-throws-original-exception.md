---
title: 中断性变更：中间件：如果找不到处理程序，则异常处理程序中间件会引发原始异常
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：中间件：如果找不到处理程序，则异常处理程序中间件会引发原始异常
author: scottaddie
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: 8801d3e6950d66fd9f24e051fd8729c50eb0b282
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95759278"
---
# <a name="middleware-exception-handler-middleware-throws-original-exception-if-handler-not-found"></a>中间件：如果找不到处理程序，则异常处理程序中间件会引发原始异常

在 ASP.NET Core 5.0 及更低版本中，[异常处理程序中间件](xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler%2A)在发生异常时执行配置的异常处理程序。 如果找不到通过 <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerOptions.ExceptionHandlingPath> 配置的异常处理程序，则会生成 HTTP 404 响应。 该响应具有误导性，因为它：

* 似乎是一个用户错误。
* 掩盖了服务器上发生异常的事实。

为了解决 ASP.NET Core 5.0 中的误导性错误，`ExceptionHandlerMiddleware` 会在找不到异常处理程序的情况下引发原始异常。 因此，服务器会生成 HTTP 500 响应。 调试发生的错误时，可以更容易地在服务器日志中检查响应。

有关讨论，请参阅 GitHub 问题 [dotnet/aspnetcore#25288](https://github.com/dotnet/aspnetcore/issues/25288)。

## <a name="version-introduced"></a>引入的版本

5.0 RC 1

## <a name="old-behavior"></a>旧行为

如果找不到配置的异常处理程序，异常处理程序中间件会生成 HTTP 404 响应。

## <a name="new-behavior"></a>新行为

如果找不到配置的异常处理程序，异常处理程序中间件会引发原始异常。

## <a name="reason-for-change"></a>更改原因

HTTP 404 错误并不意味着服务器发生了异常。 此更改会生成 HTTP 500 错误，很明显：

* 此问题不是由用户错误引起的。
* 服务器上发生了异常。

## <a name="recommended-action"></a>建议操作

无 API 更改。 所有现有的应用将继续编译并运行。 引发的异常由服务器处理。 例如，[Kestrel](/aspnet/core/fundamentals/servers/kestrel) 或 [HTTP.sys](/aspnet/core/fundamentals/servers/httpsys) 会将异常转换为 HTTP 500 错误响应。

## <a name="affected-apis"></a>受影响的 API

无

<!--

### Category

ASP.NET Core

### Affected APIs

Not detectable via API analysis

-->
