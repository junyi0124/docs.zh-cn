---
title: 状态管理
description: 了解用于在 ASP.NET Web 窗体和 Blazor 中管理状态的不同方法。
author: csharpfritz
ms.author: jefritz
ms.date: 05/15/2020
ms.openlocfilehash: 2ca811f886c6a245ac16c2bd66a68ff16e5bfc44
ms.sourcegitcommit: 0100be20fcf23f61dab672deced70059ed71bb2e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/17/2020
ms.locfileid: "88267719"
---
# <a name="state-management"></a>状态管理

状态管理是 Web 窗体应用程序的主要概念，通过视图状态、会话状态、应用程序状态和回发功能得以加速。 框架的这些有状态功能有助于隐藏应用程序所需的状态管理，并使应用程序开发人员能够专注于提供其功能。 使用 ASP.NET Core 和 Blazor，其中的某些功能已被重新定位，一些功能已完全删除。 本章介绍如何维护状态，并提供与 Blazor 中的新功能相同的功能。

## <a name="request-state-management-with-viewstate"></a>通过 ViewState 请求状态管理

当讨论 Web 窗体应用程序中的状态管理时，许多开发人员会立即想到视图状态。 在 Web 窗体中，视图状态通过在浏览器中来回发送大量编码的文本，来管理 HTTP 请求之间的内容状态。 使用包含多个元素的页面中的内容可能会占用视图状态字段，这可能会使大小增加到几兆字节。

使用 Blazor Server，应用维护与服务器的持续连接。 应用程序的状态称为 *线路*，连接被视为处于活动状态时，会保留在服务器内存中。 仅当用户离开应用或应用中的特定页面时才会释放状态。 活动组件的所有成员在与服务器的交互之间可用。

此功能有几个优点：

- 组件状态随时可用且不在交互之间重建。
- 状态不会传输到浏览器。

但是，内存中组件状态持久性存在一些缺点需要注意：

- 如果服务器在请求之间重启，则状态将丢失。
- 应用程序 web 服务器负载平衡解决方案必须包含粘滞会话，以确保来自同一浏览器的所有请求都返回到相同的服务器。 如果请求发送到其他服务器，则状态将丢失。
- 服务器上组件状态的持久性可能导致 web 服务器上的内存压力。

出于上述原因，请不要仅依赖于组件在服务器内存中的状态。 应用程序还应包括请求之间的数据的一些支持数据存储。 此策略的一些简单示例：

- 在购物车应用程序中，将添加到购物车中的新项的内容保存到数据库记录中。 如果服务器上的状态丢失，可以从数据库记录中重建该状态。
- 在多部分 web 窗体中，用户希望应用程序记住每个请求之间的值。 将用户的每个发布的数据写入数据存储区，以便在多部分窗体完成时，可以将这些数据提取并组合到最终的窗体响应结构。

有关在 Blazor 应用中管理状态的更多详细信息，请参阅 [ASP.NET Core Blazor 状态管理](/aspnet/core/blazor/state-management)。

## <a name="maintain-state-with-session"></a>通过会话维护状态

Web 窗体开发人员可以通过字典对象维护有关当前正在进行的用户的信息 <xref:Microsoft.AspNetCore.Http.ISession?displayProperty=nameWithType> 。 向添加具有字符串键的对象非常简单 `Session` ，并且该对象稍后会在用户与应用程序交互的过程中可用。 在尝试消除与 HTTP 的管理交互的过程中， `Session` 对象使其易于维护。

.NET Framework 对象的签名与 `Session` ASP.NET Core `Session` 对象不同。 在决定迁移和使用新的会话状态功能之前，请考虑 [新 ASP.NET Core 会话的文档](/dotnet/api/microsoft.aspnetcore.http.isession) 。

会话在 ASP.NET Core 和 Blazor 服务器中可用，但不建议将其用于适当地将数据存储在数据存储库中。 如果由于隐私问题，访问者拒绝在应用程序中使用 HTTP cookie，则会话状态也不起作用。

[ASP.NET Core 一文中的会话和状态管理](/aspnet/core/fundamentals/app-state#session-state)中提供了 ASP.NET Core 和会话状态的配置。

## <a name="application-state"></a>应用程序状态

`Application`Web 窗体框架中的对象为与应用程序范围的配置和状态交互提供了大规模的跨请求存储库。 应用程序状态是存储各种应用程序配置属性的理想位置，这些属性将由所有请求引用，而不考虑发出请求的用户。 对象的问题在于 `Application` ，数据不会在多个服务器之间保持不变。 重新启动之间丢失了应用程序对象的状态。

对于 `Session` ，建议将数据移动到可能由多个服务器实例访问的持久性后备存储。 如果有要跨请求和用户访问的易失性数据，可以轻松地将其存储在可注入到需要此信息或交互的组件中的单一服务。

用于维护应用程序状态及其消耗的对象结构可能与以下实现类似：

```csharp
public class MyApplicationState
{
    public int VisitorCounter { get; private set; } = 0;

    public void IncrementCounter() => VisitorCounter += 1;
}
```

```csharp
app.AddSingleton<MyApplicationState>();
```

```razor
@inject MyApplicationState AppState

<label>Total Visitors: @AppState.VisitorCounter</label>
```

`MyApplicationState`对象只在服务器上创建一次，并获取值并将其 `VisitorCounter` 输出到组件的标签中。 `VisitorCounter`应保留该值，并从后备数据存储中检索该值以实现持久性和可伸缩性。

## <a name="in-the-browser"></a>在浏览器中

还可以在用户的设备上将应用程序数据存储在客户端，以便以后使用。 可以通过两种浏览器功能在用户浏览器的不同范围内保留数据：

- `localStorage` -范围限定为用户的整个浏览器。 如果重新加载页面，浏览器将关闭并重新打开，或者使用相同的 URL 打开其他选项卡，则浏览器会提供相同的选项卡 `localStorage`
- `sessionStorage` -范围是用户的当前浏览器选项卡。如果重新加载该选项卡，则状态将保持不变。 但是，如果用户为应用程序打开了另一个选项卡或关闭并重新打开浏览器，则状态将丢失。

您可以编写一些自定义 JavaScript 代码来与这些功能进行交互，也可以使用多个 NuGet 包来提供此功能。 此类包之一为 [AspNetCore. ProtectedBrowserStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage)。

有关利用此包与和交互的说明 `localStorage` `sessionStorage` ，请参阅 [Blazor 状态管理](/aspnet/core/blazor/state-management#protected-browser-storage-experimental-package) 一文。

>[!div class="step-by-step"]
>[上一页](pages-routing-layouts.md)
>[下一页](forms-validation.md)
