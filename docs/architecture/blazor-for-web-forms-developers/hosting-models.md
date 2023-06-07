---
title: Blazor 应用托管模型
description: 了解托管应用的不同方法 Blazor ，包括在服务器上的浏览器中 WebAssembly 。
author: danroth27
ms.author: daroth
no-loc:
- Blazor
- WebAssembly
ms.date: 11/20/2020
ms.openlocfilehash: 5d80b28642ee1e975d334f89504a1748d13dea8f
ms.sourcegitcommit: 2f485e721f7f34b87856a51181b5b56624b31fd5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96509775"
---
# <a name="no-locblazor-app-hosting-models"></a>Blazor 应用托管模型

Blazor 应用程序可以在 IIS 中托管，就像 ASP.NET Web 窗体应用程序一样。 Blazor 还可以通过以下方式之一来托管应用：

- 在浏览器中的客户端 WebAssembly 。
- ASP.NET Core 应用中的服务器端。

## <a name="no-locblazor-no-locwebassembly-apps"></a>BlazorWebAssembly应用

BlazorWebAssembly应用在 WebAssembly 基于的 .net 运行时上的浏览器中直接执行。 BlazorWebAssembly应用的工作方式类似于前端 JavaScript 框架，如角度或反应。 不过，编写 c # 不是编写 JavaScript。 .NET 运行时随应用程序一起下载，并随应用程序集和任何所需的依赖项一起下载。 不需要浏览器插件或扩展。

下载的程序集是普通的 .NET 程序集，就像在任何其他 .NET 应用程序中使用一样。 由于运行时支持 .NET Standard，因此你可以将现有的 .NET Standard 库用于 Blazor WebAssembly 应用。 但是，这些程序集仍将在浏览器安全沙箱中执行。 某些功能可能会引发 <xref:System.PlatformNotSupportedException> ，如尝试访问文件系统或打开任意网络连接。

当应用程序加载时，.NET 运行时将启动并指向应用程序集。 应用启动逻辑运行，并呈现根组件。 Blazor 基于组件的呈现输出来计算 UI 更新。 然后应用 DOM 更新。

![：： no (Blazor) ：：：：：： no loc (WebAssembly) ：：：](media/hosting-models/blazor-webassembly.png)

BlazorWebAssembly应用纯粹运行客户端。 此类应用可部署到静态站点托管解决方案，如 GitHub 页面或 Azure 静态网站托管。 服务器上根本不需要 .NET。 深层链接到应用程序通常需要服务器上的路由解决方案。 路由解决方案将请求重定向到应用程序的根。 例如，可以在 IIS 中使用 URL 重写规则来处理此重定向。

若要获得 Blazor 和全堆栈 .net web 开发的所有优点，请在 ASP.NET Core 中托管 Blazor WebAssembly 应用程序。 通过在客户端和服务器上使用 .NET，可以轻松地共享代码，并使用一组一致的语言、框架和工具来生成应用程序。 Blazor 提供方便的模板，用于设置包含 Blazor WebAssembly 应用程序和 ASP.NET Core 主机项目的解决方案。 构建解决方案时，应用中的生成静态文件 Blazor 由已设置了回退路由的 ASP.NET Core 应用程序托管。

## <a name="no-locblazor-server-apps"></a>Blazor 服务器应用

从[ Blazor 体系结构](architecture-comparison.md#blazor)讨论中回忆，组件会将 Blazor 其输出呈现为名为的中间抽象 `RenderTree` 。 然后，框架会将 Blazor 呈现的内容与之前呈现的内容进行比较。 这些差异应用于 DOM。 Blazor 组件与应用呈现输出的方式是分离的。 因此，组件本身不必在更新 UI 的过程中运行。 事实上，它们甚至不需要在同一台计算机上运行。

在 Blazor 服务器应用中，组件在服务器上运行，而不是在浏览器中运行。 浏览器中发生的 UI 事件通过实时连接发送到服务器。 事件将分派给正确的组件实例。 组件将呈现，并将计算的 UI 差异序列化并发送到浏览器，并将其应用于 DOM。

![：： no (Blazor) ：：： Server](media/hosting-models/blazor-server.png)

Blazor如果你使用了 ASP.NET AJAX 和控件，则服务器托管模型可能听起来非常熟悉 <xref:System.Web.UI.UpdatePanel> 。 `UpdatePanel`控件处理应用部分页面更新以响应页面上的触发器事件。 触发时， `UpdatePanel` 请求部分更新，然后应用该更新，而无需刷新页面。 UI 的状态使用进行管理 `ViewState` 。 Blazor 服务器应用略有不同，因为应用需要与客户端建立活动连接。 此外，还会在服务器上维护所有 UI 状态。 除了这些差异以外，这两个模型在概念上类似。

## <a name="how-to-choose-the-right-no-locblazor-hosting-model"></a>如何选择正确的 Blazor 托管模型

如[ Blazor 宿主模型文档](/aspnet/core/blazor/hosting-models)中所述，不同的 Blazor 宿主模型有不同的折衷方式。

Blazor WebAssembly 宿主模型具有以下优势：

- 没有 .NET 服务器端依赖项。 应用在下载到客户端之后完全正常运行。
- 可充分利用客户端资源和功能。
- 工作可从服务器转移到客户端。
- 无需 ASP.NET Core Web 服务器即可托管应用。 无服务器部署方案可行（例如通过 CDN 为应用提供服务的方案）。

托管模型的缺点 Blazor WebAssembly 是：

- 浏览器功能限制了应用程序。
- 支持的客户端硬件和软件 (例如， WebAssembly 需要) 支持。
- 下载项大小较大，应用加载耗时较长。
- .NET 运行时和工具支持不够完善。 例如， [.NET Standard](../../standard/net-standard.md) 支持和调试存在一些限制。

相反， Blazor 服务器托管模型具有以下优势：

- 下载大小要比客户端应用小得多，并且应用的加载速度要快得多。
- 应用充分利用了服务器功能，包括使用任何 .NET 兼容的 Api。
- 服务器上的 .NET 用于运行应用程序，因此现有的 .NET 工具（如调试）可按预期方式工作。
- 支持瘦客户端。 例如，服务器端应用程序适用于 WebAssembly 在资源受限的设备上不支持和的浏览器。
- 应用的 .NET/C# 代码库（其中包括应用的组件代码）不适用于客户端。

Blazor服务器托管模型的缺点是：

- UI 延迟更高。 每次用户交互都涉及到网络跃点。
- 不支持脱机工作。 如果客户端连接失败，应用会停止工作。
- 如果具有多名用户，则应用扩缩性存在挑战。 服务器必须管理多个客户端连接并处理客户端状态。
- 需要 ASP.NET Core 服务器为应用提供服务。 无法进行无服务器部署方案。 例如，你不能从 CDN 服务应用。

前面的权衡列表可能是造成威胁，但你的托管模型可在以后更改。 无论选择哪种 Blazor 托管模型，组件模型都是 *相同* 的。 原则上，同一组件可用于任一宿主模型。 你的应用程序代码不会更改;不过，最好的做法是引入抽象，使组件保持与模型无关。 抽象使你的应用能够更轻松地采用不同的承载模型。

## <a name="deploy-your-app"></a>部署你的应用

ASP.NET Web 窗体应用通常在 Windows Server 计算机或群集上的 IIS 上托管。 Blazor 应用还可以：

- 作为静态文件或作为 ASP.NET Core 应用托管在 IIS 上。
- 利用 ASP.NET Core 在各种平台和服务器基础结构上的灵活性。 例如，可以 Blazor 在 Linux 上使用 [Nginx](/aspnet/core/host-and-deploy/linux-nginx) 或 [Apache](/aspnet/core/host-and-deploy/linux-apache) 来托管应用。 有关如何发布和部署应用的详细信息 Blazor ，请参阅 Blazor [托管和部署](/aspnet/core/host-and-deploy/blazor/) 文档。

在下一部分中，我们将介绍如何 Blazor WebAssembly Blazor 设置和服务器应用的项目。

>[!div class="step-by-step"]
>[上一页](architecture-comparison.md)
>[下一页](project-structure.md)
