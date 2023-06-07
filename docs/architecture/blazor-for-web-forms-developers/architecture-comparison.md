---
title: ASP.NET Web 窗体和的体系结构比较 Blazor
description: 了解 ASP.NET Web 窗体和比较的体系结构 Blazor 。
author: danroth27
ms.author: daroth
no-loc:
- Blazor
ms.date: 11/20/2020
ms.openlocfilehash: afb5d4025b81c2ddef782c462c94d32edc872a21
ms.sourcegitcommit: 2f485e721f7f34b87856a51181b5b56624b31fd5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96509723"
---
# <a name="architecture-comparison-of-aspnet-web-forms-and-no-locblazor"></a>ASP.NET Web 窗体和的体系结构比较 Blazor

尽管 ASP.NET Web 窗体和 Blazor 具有许多相似的概念，但它们的工作方式有所不同。 本章探讨了 ASP.NET Web 窗体和的内部工作原理和体系结构 Blazor 。

## <a name="aspnet-web-forms"></a>ASP.NET Web 窗体

ASP.NET Web 窗体框架基于以页面为中心的体系结构。 应用中某个位置的每个 HTTP 请求都是一个单独的页面，其中 ASP.NET 响应。 请求页面时，浏览器的内容将替换为请求的页面的结果。

页面包含以下组件：

- HTML 标记
- C# 或 Visual Basic 代码
- 包含逻辑和事件处理功能的代码隐藏类
- 控制

控件是可重复使用的 web UI 单元，可通过编程方式在页面上放置和交互。 页面由文件以 *.aspx* 结尾，其中包含标记、控件和某些代码。 根据所使用的编程语言，代码隐藏类位于具有相同基名称和 *aspx.cs* 或 *.aspx* 扩展名的文件中。 有趣的是，web 服务器将解释 *.aspx* 文件的内容，并在这些内容更改时对其进行编译。 即使 web 服务器已经在运行，也会进行重新编译。

控件可以用标记生成并作为用户控件提供。 用户控件是从类派生的 `UserControl` ，并具有与页面相似的结构。 用户控件的标记存储在 *.ascx* 文件中。 随附的代码隐藏类驻留在 *ascx.cs* 或 *.ascx* 文件中。 还可以通过从或基类继承来完全使用代码生成控件 `WebControl` `CompositeControl` 。

页面也有大量的事件生命周期。 每个页面引发初始化、加载、预生成和卸载事件的事件，这些事件在 ASP.NET 运行时针对每个请求执行页面的代码时出现。

页面上的控件通常会回发到呈现控件的同一页面，并将其与名为的隐藏窗体字段的有效负载一起使用 `ViewState` 。 `ViewState`字段包含有关控件在页面上呈现和显示时的状态的信息，从而允许 ASP.NET 运行时比较并标识提交给服务器的内容中的更改。

## Blazor

Blazor 是一个客户端 web UI 框架，本质上类似于 JavaScript 或反应等 JavaScript 前端框架。 Blazor 处理用户交互，并呈现必要的 UI 更新。 Blazor*不* 基于请求-答复模式。 用户交互作为不在任何特定 HTTP 请求上下文中的事件进行处理。

Blazor 应用程序由一个或多个在 HTML 页上呈现的根组件组成。

![：：：无 loc (Blazor) ：：：组件](./media/architecture-comparison/blazor-components-in-html.png)

用户如何指定组件的呈现位置以及组件的连接方式，以便用户交互连接到 [托管模型](hosting-models.md) 。

Blazor[组件](components.md)是表示可重复使用的 UI 部分的 .net 类。 每个组件都维护其自己的状态，并指定其自己的呈现逻辑，这可能包括呈现其他组件。 组件为特定用户交互指定事件处理程序，以更新组件的状态。

组件处理某个事件后，将 Blazor 呈现该组件并跟踪在呈现的输出中更改的内容。 组件不会直接呈现到文档对象模型 (DOM) 。 它们改为呈现 DOM 的内存中表示形式， `RenderTree` 以便 Blazor 可以跟踪更改。 Blazor 将新呈现的输出与以前的输出进行比较以计算 UI 差异，然后将其有效地应用于 DOM。

![：： no (Blazor) ：：： DOM 交互](./media/architecture-comparison/blazor-dom-interaction.png)

组件还可以手动指示在正常 UI 事件之外的状态发生更改时，应呈现这些组件。 Blazor 使用 `SynchronizationContext` 来强制执行单个逻辑线程。 组件的生命周期方法和 Blazor 引发的任何事件回调都在此 `SynchronizationContext` 上执行。

>[!div class="step-by-step"]
>[上一页](introduction.md)
>[下一页](hosting-models.md)
