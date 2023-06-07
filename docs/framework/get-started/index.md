---
title: .NET Framework 入门
description: 介绍 .NET Framework 入门知识，它是管理应用的运行时执行环境。 它包含公共语言运行时 (CLR) 和全面的类库。
ms.date: 10/21/2020
helpviewer_keywords:
- .NET Framework, getting started
- getting started [.NET Framework]
ms.assetid: c693fd34-88fe-4d90-b332-19eeadf3b7e7
ms.openlocfilehash: 3ab1c502508dd042940a6c5d2a73a5b71af6a9f4
ms.sourcegitcommit: 30a686fd4377fe6472aa04e215c0de711bc1c322
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2020
ms.locfileid: "94440837"
---
# <a name="get-started-with-net-framework"></a>.NET Framework 入门

.NET Framework 是管理面向 .NET Framework 的应用的运行时执行环境。 它包括公共语言运行时（提供内存管理和其他系统服务）和一个全面的类库（使程序员能利用强大可靠的代码实现所有主要领域的应用开发）。

[!INCLUDE [net-framework-future](../../../includes/net-framework-future.md)]

## <a name="what-is-net-framework"></a>什么是 .NET Framework？

.NET Framework 是 Windows 的托管执行环境，可为其运行的应用提供各种服务。 它包括两个主要组件：公共语言运行时 (CLR)，它是处理运行应用的执行引擎；.NET Framework 类库，它提供开发人员可从其自己的应用中调用的已测试、可重用代码库。 .NET Framework 提供的用于运行应用的服务包括：

- 内存管理。 在许多编程语言中，程序员负责分配和释放内存并处理对象生存期。 在 .NET Framework 应用中，CLR 代表应用提供这些服务。

- 常规类型系统。 在传统编程语言中，基本类型由编译器定义，这将使跨语言互操作性复杂化。 在 .NET Framework 中，基本类型由 .NET Framework 类型系统定义，并且是面向 .NET Framework 的所有语言所共有的。

- 一个全面的类库。 处理常见的低级编程操作时，程序员可通过 .NET Framework 类库使用类型及其成员的易访问库，而不必编写大量代码。

- 开发框架和技术。 .NET Framework 包括用于特定区域应用开发的库，例如用于 Web 应用的 ASP.NET、用于数据访问的 ADO.NET、用于面向服务的应用的 Windows Communication Foundation，以及用于 Windows 桌面应用的 Windows Presentation Foundation。

- 语言互操作性。 面向 .NET Framework 的语言编译器发出名为公共中间语言 (CIL) 的中间代码，反过来，通过公共语言运行时在运行时进行编译。 借助此功能，使用某种语言编写的例程可由另一种语言访问，程序员可以专注于使用其首选语言创建应用。

- 版本兼容性。 除少数例外，使用特定版本的 .NET Framework 开发的应用无需在更高版本中修改即可运行。

- 并行执行。 通过允许同一台计算机上存在公共语言运行时的多个版本，.NET Framework 可帮助解决版本冲突。 这意味着应用的多个版本可以共存，并且应用可在构建它的 .NET Framework 版本上运行。 并行执行适用于 .NET Framework 版本组 1.0/1.1、2.0/3.0/3.5 和 4/4.5.x/4.6.x/4.7.x/4.8。

- 多定向。 通过面向 [.NET Standard](../../standard/net-standard.md)，开发人员可创建适用于该标准版本支持的多种 .NET Framework 平台的类库。 例如，面向 .NET Framework 4.6.1、NET Core 2.0 和 UWP 10.0.16299 的应用可以使用面向 .NET Standard 2.0 的库。

<a name="ForUsers"></a>

## <a name="net-framework-for-users"></a>面向用户的 .NET Framework

如果你不开发 .NET Framework 应用，而只需要使用它们，则不需要掌握有关 .NET Framework 或其操作的特定知识。 大多数情况下，框架对用户是完全透明的。

如果你使用的是 Windows 操作系统，则你的计算机上可能已安装 .NET Framework。 此外，如果安装需要 .NET Framework 的应用，则应用的安装程序可能会在你的计算机上安装特定版本的框架。 在某些情况下，可能会显示一个要求你安装 .NET Framework 的对话框。 如果在该对话框出现时刚尝试过运行应用，并且计算机可以访问 Internet，则可以转到一个可安装缺少的 .NET Framework 版本的网页。 有关详细信息，请参阅[安装指南](../install/index.md)。

一般而言，不应卸载计算机上已安装的 .NET Framework 版本。 主要有两个原因：

- 如果使用的应用依赖于特定版本的 .NET Framework，则该版本一旦删除，应用就会暂停。

- 一些 .NET Framework 版本是早期版本的就地更新版。 例如，.NET Framework 3.5 是版本 2.0 的就地更新版，而 .NET Framework 4.8 是版本 4 到版本 4.7.2 的就地更新版。 有关详细信息，请参见 [.NET Framework 版本和依赖关系](../migration-guide/versions-and-dependencies.md)。

在 Windows 8 之前版本的 Windows 上，如果选择删除 .NET Framework，请始终通过“控制面板”的“程序及功能”进行卸载。 请勿手动删除某个版本的 .NET Framework。 在 Windows 8 和更高版本的操作系统上，.NET Framework 是一个操作系统组件，不能单独卸载。

一台计算机上可同时存在多个 .NET Framework 版本。 这意味着，你不必卸载旧版本即可安装更新版本。

## <a name="net-framework-for-developers"></a>面向开发人员的 .NET Framework

如果你是开发人员，可选择任何支持 .NET Framework 的编程语言来创建应用。 由于 .NET Framework 提供了语言独立性和互操作性，因此无论开发时使用何种语言，你都可以与其他 .NET Framework 应用和组件进行交互。

若要开发 .NET Framework 应用或组件，请执行以下操作：

1. 如果未在操作系统上预安装 .NET Framework，请安装应用所面向的 .NET Framework 版本。 最新生产版本是 .NET Framework 4.8。 此版本预安装在 Windows 10 的 2019 年 5 月更新中，并可下载到旧版 Windows 操作系统中。 有关 .NET Framework 系统要求，请参阅[系统要求](system-requirements.md)。 有关安装其他版本的 .NET Framework 的信息，请参阅[安装指南](../install/guide-for-developers.md)。 其他 .NET Framework 包为带外发布，这意味着这些包在滚动基础上发布，没有任何定期或计划的发布周期。 有关这些包的信息，请参阅 [.NET Framework 和带外版本](the-net-framework-and-out-of-band-releases.md)。

2. 选择要用于开发应用的 .NET Framework 版本所支持的语言。 大量语言可供选择，包括来自 Microsoft 的 [Visual Basic](../../visual-basic/index.yml)、[C#](../../csharp/index.yml)、[F#](../../fsharp/index.yml), 和 [C++/CLI](/cpp/dotnet/dotnet-programming-with-cpp-cli-visual-cpp)。 （一种用于开发 .NET Framework 应用的编程语言，它遵循[公共语言基础结构 (CLI) 规范](https://visualstudio.microsoft.com/license-terms/ecma-c-common-language-infrastructure-standards/)。）

3. 选择并安装将用于创建应用并支持所选程序语言的开发环境。 适用于 .NET Framework 应用的 Microsoft 集成开发环境 (IDE) 是 [Visual Studio](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link)。 它提供多种版本。

要详细了解如何开发面向 .NET Framework 的应用，请参阅[开发指南](../development-guide.md)。

## <a name="related-articles"></a>相关文章

| Title | 描述 |
| ----- |------------ |
| [概述](overview.md) | 为构建面向 .NET Framework 的应用的开发人员提供详细信息。 |
| [安装指南](../install/index.md) | 提供有关安装 .NET Framework 的信息。 |
| [.NET Framework 和带外版本](the-net-framework-and-out-of-band-releases.md) | 描述 .NET Framework 带外版本以及如何在应用程序中使用它们。 |
| [系统要求](system-requirements.md) | 列出运行 .NET Framework 的硬件和软件要求。 |
| [.NET Core 文档](../../core/introduction.md) | 提供 .NET Core 的概念和 API 参考文档。 |
| [.NET Standard](../../standard/net-standard.md) | 讨论 .NET Standard，这是 .NET 实现支持的版本管理规范，用于保证可在多个平台上使用一致的 API 集。

## <a name="see-also"></a>请参阅

- [.NET Framework 指南](../index.yml)
- [新增功能](../whats-new/index.md)
- [.NET API 浏览器](../../../api/index.md)
- [开发指南](../development-guide.md)
