---
title: BlazorASP.NET Web 窗体开发人员简介
description: 介绍如何 Blazor 使用 .net 编写和编写全堆栈 web 应用
author: danroth27
ms.author: daroth
no-loc:
- Blazor
- WebAssembly
ms.date: 11/20/2020
ms.openlocfilehash: f1967dac0f46ba7cfefab62c5528dd1db8029514
ms.sourcegitcommit: 2f485e721f7f34b87856a51181b5b56624b31fd5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96509710"
---
# <a name="an-introduction-to-no-locblazor-for-aspnet-web-forms-developers"></a>BlazorASP.NET Web 窗体开发人员简介

ASP.NET Web 窗体框架已成为 .NET Web 开发的装订，因为 .NET Framework 首次在2002中提供。 当 Web 仍在其 infancy 的基础上，ASP.NET 的 Web 窗体通过采用用于桌面开发的多种模式来简化和高效地构建 web 应用。 在 ASP.NET Web 窗体中，可以通过可重复使用的 UI 控件快速编写网页。 用户交互作为事件自然进行处理。 Microsoft 和控制供应商提供了丰富的 Web 窗体 UI 控件生态系统。 控件可以轻松地连接到数据源并显示丰富的数据可视化效果。 对于视觉效果，Web 窗体设计器为管理控件提供了一个简单的拖放界面。

多年来，Microsoft 引入了新的基于 ASP.NET 的 web 框架，用于解决 web 开发趋势。 某些这样的 web 框架包括 ASP.NET MVC、ASP.NET 网页和最近 ASP.NET Core。 对于每个新框架，有些都预测到了 ASP.NET Web 窗体的即将发生的拒绝，并将其 criticized 为过时的过时 web 框架。 尽管进行了这些预测，但许多 .NET web 开发人员仍然可以通过一种简单、稳定且高效的方式来完成工作。

撰写本文时，每个月几乎有一百万个 web 开发人员使用 ASP.NET Web 窗体。 ASP.NET Web 窗体框架的稳定性在于，一年前的文档、示例、书籍和博客文章仍然有用且相关。 对于许多 .NET web 开发人员来说，"ASP.NET" 仍与 .NET 初次构想时的 "ASP.NET Web Forms" 同义词。 与其他新的 .NET web 框架相比，ASP.NET Web 窗体的优缺点的参数可能会风行一时。 ASP.NET Web 窗体仍是用于创建 web 应用的常用框架。

尽管如此，软件开发的创新也不会降低。 所有软件开发人员都需要及时了解新技术和趋势。 特别需要考虑两个趋势：

1. 转向开源和跨平台
2. 应用逻辑到客户端的移动

## <a name="an-open-source-and-cross-platform-net"></a>开源和跨平台 .NET

当 .NET 和 ASP.NET Web 窗体首次发布时，平台生态系统看起来与今天的内容大不相同。 桌面和服务器市场由 Windows 所占据。 其他平台（如 macOS 和 Linux）仍在努力，以获得吸引力。 ASP.NET Web 窗体随 .NET Framework 作为仅限 Windows 的组件提供，这意味着 ASP.NET Web 窗体应用只能在 Windows Server 计算机上运行。 许多新式环境现在为服务器和开发计算机使用不同种类的平台，以便对许多用户的跨平台支持是绝对要求。

大多数新式 web 框架现在也是开源，这有很多好处。 用户不会 beheld 单个项目所有者来修复 bug 和添加功能。 开源项目为开发进度和即将发生的更改提供了改进的透明度。 开源项目充分利用了整个社区的贡献，并培养了一种可获得的开源生态系统。 尽管开源的风险，许多使用者和参与者都找到了合适的缓解措施，使他们能够以一种安全合理的方式享受开源生态系统的优势。 此类缓解的示例包括参与者许可协议、友好许可证、pedigree 扫描和支持基础。

.NET 社区同时采用跨平台支持和开源。 .NET Core 是 .NET 的开源和跨平台实现，它在很多的平台上运行，包括 Windows、macOS 和各种 Linux 发行版。 Xamarin 提供 .NET 的开源版本。 Mono 适用于 Android、iOS 和各种其他形式因素，包括监视和智能电视。 Microsoft 发布了 [.net 5](https://devblogs.microsoft.com/dotnet/announcing-net-5-0/) ，将 .net Core 和 Mono 协调为 "单个 .net 运行时和框架，可以在任何地方使用并且具有统一的运行时行为和开发人员体验。"

ASP.NET Web Forms 是否受益于转向开源和跨平台支持？ 答案为 "否"，或者至少与平台的其余部分不相同。 最近，.NET 团队已 [明确](https://devblogs.microsoft.com/dotnet/net-core-is-the-future-of-net/) 指出，ASP.NET Web 窗体不会移植到 .net Core 或 .net 5。 为什么会这样？

.NET Core 在早期的几天内努力 ASP.NET Web 窗体。 发现所需的重大更改数量过于严重。 这里还有一种许可，甚至对于 Microsoft，都可以同时支持的 web 框架数量有限制。 社区中的某人可能会考虑到创建 ASP.NET Web 窗体的开放源和跨平台版本的原因。 [ASP.NET Web 窗体的源代码](https://github.com/microsoft/referencesource)公开了引用窗体。 但在这种情况下，似乎 ASP.NET Web 窗体仍将保持为仅限 Windows，无需开源贡献模型。 如果跨平台支持或开源对于你的方案非常重要，则需要查找新的内容。

这是否意味着 ASP.NET Web 窗体 *失效* ，不应再使用？ 当然不是！ 只要 .NET Framework 在 Windows 中提供，ASP.NET Web 窗体就会成为受支持的框架。 对于许多 Web 窗体开发人员而言，缺少跨平台和开源支持是一个不问题的问题。 如果你不要求跨平台支持、开源或 .NET Core 或 .NET 5 中的任何其他新功能，请在 Windows 上坚持使用 ASP.NET Web 窗体。 ASP.NET Web 窗体将继续成为一种高效编写 web 应用的方式。

但还有另一个需要考虑的趋势，这就是客户的转变。

## <a name="client-side-web-development"></a>客户端 web 开发

所有。基于网络的 web 框架（包括 ASP.NET Web 窗体）一直以来都有一个共同点：它们是 *服务器呈现* 的。 在服务器呈现的 web 应用中，浏览器向服务器发出请求，该请求在 ASP.NET apps) 中执行一些代码 ( .NET 代码，以生成响应。 该响应将发送回浏览器以处理。 在此模型中，浏览器用作瘦渲染引擎。 生成 UI、运行业务逻辑和管理状态的工作在服务器上发生。

但是，浏览器已成为多样的平台。 它们实现了数量不断增长的开放 web 标准，这些标准向用户计算机的功能授予访问权限。 为什么不利用计算能力、存储、内存和客户端设备的其他资源？ 尤其是在至少部分或完全完全的客户端处理时，UI 交互特别能从更丰富且更具交互性的外观中获益。 仍可以在服务器端处理在服务器上处理的逻辑和数据。 可以使用 Web API 调用，甚至可以使用类似于 Websocket 的实时协议。 如果 web 开发人员愿意编写 JavaScript，则可免费使用这些权益。 客户端 UI 框架，如角度、反应和 Vue，简化了客户端 web 开发，并在普及中得到了发展。 ASP.NET Web 窗体开发人员还可以利用客户端，甚至可以通过集成的 JavaScript 框架（如 ASP.NET AJAX）提供现成的支持。

但会将两个不同的平台和生态系统)  ( 的 .NET 和 JavaScript 结合在一起。 在两个具有不同语言、框架和工具的并行领域中需要专业知识。 无法在客户端和服务器之间轻松共享代码和逻辑，从而导致重复和工程开销。 这也很难跟上 JavaScript 生态系统，其历史记录的发展速度快。 前端框架和生成工具首选项快速更改。 该行业观察到了从 Grunt 到 Gulp 到 Webpack 等的进度。 前端框架发生相同的 restless 变动，如 jQuery、挖空、角度、反应和 Vue。 但对于 JavaScript 的浏览器 monopoly，这并不是很好的选择。 也就是说，在 web 社区共同合作并导致 *神奇* 发生之后！

## <a name="no-locwebassembly-fulfills-a-need"></a>WebAssembly 满足需求

在2015中，主要的浏览器供应商加入了一个 W3C 社区组，以创建名为的新开放 web 标准 WebAssembly 。 WebAssembly 是 Web 的字节代码。 如果你可以将代码编译为 WebAssembly ，则它可以在任何平台上的任何浏览器上以近乎本机的速度运行。 初始工作重点介绍 C/c + +。 结果就是在不使用插件的情况下直接在浏览器中运行本机3D 图形引擎的生动演示。 WebAssembly 已由所有主要浏览器标准化并实现。

上运行 .NET 的工作 WebAssembly 已于2017年发布，并在2020发布，其中包括 .net 5 支持。 直接在浏览器中运行 .NET 代码的功能可实现通过 .NET 进行的全堆栈 web 开发。

## <a name="no-locblazor-full-stack-web-development-with-net"></a>Blazor：通过 .NET 进行全堆栈 web 开发

它本身就是在浏览器中运行 .NET 代码的功能，不提供创建客户端 web 应用程序的端到端体验。 接下来就是这样 Blazor 。 Blazor 是基于 C# 而不是 JavaScript 的客户端 Web UI 框架。 Blazor 可以通过在浏览器中直接运行 WebAssembly 。 不需要浏览器插件。 或者， Blazor 应用可在 .net 上运行服务器端，并通过与浏览器进行实时连接来处理所有用户交互。

Blazor 在 Visual Studio 和 Visual Studio Code 中具有很好的工具支持。 该框架还包括一个完整的 UI 组件模型，并且具有用于的内置功能：

- 窗体和验证
- 依赖关系注入
- 客户端路由
- 布局
- 浏览器内调试
- JavaScript 互操作

Blazor 与 ASP.NET Web 窗体有很多共同之处。 这两个框架都提供基于组件、事件驱动、有状态的 UI 编程模型。 主要的体系结构差异在于，ASP.NET Web 窗体仅在服务器上运行。 Blazor 可在浏览器中的客户端上运行。 但是，如果您是从 ASP.NET Web 窗体背景开始，则很多人都非常 Blazor 熟悉。 Blazor 是一种自然解决方案，适用于 ASP.NET Web 窗体开发人员寻找一种方法来利用 .NET 的客户端开发和开源的跨平台。

本书介绍了 Blazor 专门针对 ASP.NET Web 窗体开发人员迎合的。 每个 Blazor 概念都显示在类似的 ASP.NET Web 窗体功能和实践的上下文中。 本书结束后，你将了解以下内容：

- 如何生成 Blazor 应用程序。
- Blazor工作原理。
- 如何 Blazor 与 .net 相关。
- 将现有 ASP.NET Web 窗体应用迁移到适当位置的合理策略 Blazor 。

## <a name="get-started-with-no-locblazor"></a>Blazor 入门

的入门 Blazor 非常简单。 请访问 <https://blazor.net> 并按照链接安装相应的 .NET SDK 和 Blazor 项目模板。 你还将找到有关 Blazor 在 Visual Studio 中设置工具或 Visual Studio Code 的说明。

>[!div class="step-by-step"]
>[上一页](index.md)
>[下一页](architecture-comparison.md)
