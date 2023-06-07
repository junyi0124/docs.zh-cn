---
title: 面向 Web Forms ASP.NET Web Forms 开发人员的 Blazor
description: 了解如何使用 Blazor 和 .NET Core 以简单而熟悉的方式使用 .NET 构建全栈式 Web 应用。
author: danroth27
ms.author: daroth
no-loc:
- Blazor
- WebAssembly
ms.date: 12/01/2020
ms.openlocfilehash: 47f684e1b48ca95b8d999e6f1429840eb5f541de
ms.sourcegitcommit: 2f485e721f7f34b87856a51181b5b56624b31fd5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96509762"
---
# <a name="no-locblazor-for-aspnet-web-forms-developers"></a>面向 Web Forms ASP.NET Web Forms 开发人员的 Blazor

![显示无服务器应用电子书封面的屏幕截图。](./media/index/blazor-for-aspnet-web-forms-developers.png)

> 下载地址：<https://aka.ms/blazor-ebook>

**版本 v1.0**

请参阅[更改记录](https://aka.ms/blazor-ebook-changelog)了解书籍更新和社区贡献。

发布者

Microsoft 开发人员部门、.NET 和 Visual Studio 产品团队

Microsoft Corporation 的一个部门

One Microsoft Way

Redmond, Washington 98052-6399

版权所有 © 2020 Microsoft Corporation

保留所有权利。 未经发布者书面许可，不得以任何形式或任何方式复制或传播本书中的任何内容。

本书“按原样”提供，表达作者的观点和看法。 本书中表达的观点、看法和信息（包括 URL 和其他 Internet 网站引用）如有更改，恕不另行通知。

本书中提及的一些示例仅用于说明，纯属虚构。 不存在任何实际关联或联系，请勿妄加推断。

Microsoft 和 <https://www.microsoft.com> 上“商标”网页列出的商标是 Microsoft 集团公司的商标。

Mac 和 macOS 是 Apple Inc. 的商标

所有其他标记和徽标均为其各自所有者的财产。

作者：

> [Daniel Roth](https://github.com/danroth27)，Microsoft Corp 首席项目经理。

> [Jeff Fritz](https://github.com/csharpfritz)，Microsoft Corp 资深项目经理。

> [Taylor Southwick](https://github.com/twsouthwick)，Microsoft Corp 资深软件工程师。

> [Scott Addie](https://github.com/scottaddie)，Microsoft Corp 资深内容开发人员。

> [Steve "ardalis" Smith](https://ardalis.com)，Ardalis Services LLC 软件设计师及培训师

## <a name="introduction"></a>介绍

长期以来，.NET 通过 ASP.NET 支持 Web 应用开发，ASP.NET 是用于构建任何类型的 Web 应用的一组全面的框架和工具。 ASP.NET 拥有自己的 Web 框架和技术谱系，始于经典的 Active Server Pages (ASP)。 ASP.NET Web Forms、ASP.NET MVC、ASP.NET 网页以及最新的 ASP.NET Core 等框架，提供了一种高效而强大的方法来构建服务器呈现的 Web 应用，在这类应用中，会响应 HTTP 请求在服务器上动态生成 UI 内容。 每个 ASP.NET 框架都迎合不同的受众和应用构建理念。 ASP.NET Web Forms 随 .NET Framework 的原始版本一起提供，允许使用桌面应用程序开发人员熟悉的多种模式（如具有简单事件处理功能的可重用 UI 控件）实现 Web 开发。 但是，没有 ASP.NET 框架提供运行在用户浏览器中执行的代码的方法。 要执行此操作，需要编写 JavaScript 并使用这些年来经历了流行和退流行的 JavaScript 框架和工具：jQuery、Knockout、Angular、React 等。

[Blazor](https://blazor.net) 是一个新的 Web 框架，它为使用 .NET 构建 Web 应用带来了新的可能。 Blazor 是基于 C# 而不是 JavaScript 的客户端 Web UI 框架。 借助 Blazor，可用 C# 编写客户端逻辑和 UI 组件，将其编译成普通的 .NET 程序集，然后使用称为 WebAssembly 的新开放式 Web 标准在浏览器中直接运行它们。 或者，Blazor 可在服务器上运行 .NET UI 组件，并通过与浏览器的实时连接流畅地处理所有 UI 交互。 与服务器上运行的 .NET 配对时，Blazor 支持使用 .NET 进行全栈式 Web 开发。 虽然 Blazor 与 ASP.NET Web Forms 具有许多共同点，例如具有可重用的组件模型和处理用户事件的简单方法，但它还在 .NET 的基础之上提供了现代化高性能 Web 开发体验。

本书以 ASP.NET Web Forms 开发人员熟悉和方便的方式向其介绍 Blazor。 它并排介绍 Blazor 概念与 ASP.NET Web Forms 中的类似概念，还阐释了读者可能不太熟悉的新概念。 它涵盖了广泛的主题和关注点，包括组件创作、路由、布局、配置和安全性。 尽管本书的内容主要是介绍新的开发方法，但其中也介绍了将现有 ASP.NET Web Forms 迁移到 Blazor 的指导原则和策略，当你想要对现有应用进行现代化时，可以使用这些原则和策略。

## <a name="who-should-use-the-book"></a>本书的目标读者

本书适用于正在寻找可联系到现有知识和技能的 Blazor 介绍的 ASP.NET Web Forms 开发人员。 本书可帮助你快速开始构建基于 Blazor 的新项目，或帮助你为现代化 ASP.NET Web Forms 应用程序制定路线图。

## <a name="how-to-use-the-book"></a>如何使用本书

本书的第一部分介绍了什么是 Blazor，并将其与使用 ASP.NET Web Forms 进行 Web 应用开发进行了比较。 然后，本书逐章介绍各个 Blazor 主题，并将 Blazor 的每个概念与 ASP.NET Web Forms 中的相应概念相关联，或全面阐释任何全新概念。 本书还有规律地引用在 ASP.NET Web Forms 和 Blazor 中实现的完整示例应用，以演示 Blazor 的功能并提供从 ASP.NET Web Forms 迁移到 Blazor 的案例研究。 可在 [GitHub](https://github.com/dotnet-architecture/eshoponblazor) 上找到示例应用的两种实现（ASP.NET Web Forms 和 Blazor 版本）。

## <a name="what-this-book-doesnt-cover"></a>本书未涵盖的内容

本书是 Blazor 的简介，而不是全面的迁移指南。 尽管它确实有指南介绍如何将项目从 ASP.NET Web Forms 迁移到 Blazor，但它并未尝试涵盖所有细微差别和细节。 有关从 ASP.NET 迁移到 ASP.NET Core 的更多常规指南，请参阅 ASP.NET Core 文档中的[迁移指南](/aspnet/core/migration/proper-to-2x/)。

### <a name="additional-resources"></a>其他资源

可在 <https://blazor.net> 上找到 Blazor 的官方主页和文档。

## <a name="send-your-feedback"></a>提供你的反馈

我们正在不断完善本书和相关示例，欢迎你提供反馈！ 如果对如何改进本书有任何建议，请使用 [GitHub 问题](https://github.com/dotnet/docs/issues)上任何页面底部的反馈部分进行反馈。

>[!div class="step-by-step"]
>[下一页](introduction.md)
