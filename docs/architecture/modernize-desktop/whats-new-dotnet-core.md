---
title: .NET Core for Desktop 的新功能是什么？
description: 了解 .NET Core、.NET Core 与 .NET Framework 之间的差异以及添加的新功能。
ms.date: 05/12/2020
ms.openlocfilehash: 796facaf8cd4f0d23fbcd04b90cb78fb28ebc465
ms.sourcegitcommit: 67ebdb695fd017d79d9f1f7f35d145042d5a37f7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "97866474"
---
# <a name="whats-new-with-net-core-for-desktop"></a>.NET Core for Desktop 的新功能是什么？

从 .NET Core 3.0 开始，.NET Core 支持 Windows 窗体和 WPF。 现在，您可以选择在 .NET Framework 和 .NET Core 的桌面应用程序中进行选择。 本章介绍什么是 .NET Core，以及它对于桌面应用程序有何好处。

## <a name="the-motivation-behind-net-core"></a>.NET Core 背后的动机

由于它在2002中启动，因此 .NET Framework 已发展多年，以支持多种技术，如 Windows 窗体、ASP.NET、实体框架、Windows 应用商店等。 它们在本质上是不同的。 因此，Microsoft 通过获取部分 .NET Framework 并为每个技术创建不同的应用程序堆栈，来实现这一演变。 这样一来，就可以根据特定堆栈的需要定制开发功能，从而最大程度地提高每个平台的潜能。 这将导致不同独立团队所维护的 .NET Framework 版本的碎片。 所有这些堆栈都具有一个公共结构，其中包含应用模型、框架和运行时，但它们在每个部件的实现中有所不同。

如果只针对其中一个平台，则可以使用此模型。 但是，在许多情况下，你可能需要在同一个解决方案中需要多个目标平台。 例如，你的应用程序可能有一个桌面管理员部件、一个面向客户的网站，该网站共享在服务器上运行的后端逻辑，甚至是一个移动客户端。 在这种情况下，你需要一个可跨所有这些 .NET 纵向的统一编码体验。

随着 Windows 8 的发布，可移植类库的概念 (Pcl) 。 最初，.NET Framework 是围绕假设始终部署为单个单元而设计的，因此不需要进行 [分解](https://wikipedia.org/wiki/Decomposition_(computer_science)) 。 若要面对在纵向之间共享代码的问题，推动力是如何重构框架的。 协定的理念是提供合理的 API 外围应用。 协定只是针对进行编译的程序集，设计时应考虑到它们之间的依赖关系。

这就导致了在程序集级别上的不同行业之间的 API 差异，而不是我们以前的 API 级别。 此方面启用了可面向多个纵向（也称为可移植类库）的类库体验。

![.NET Framework 的发行历史记录](./media/whats-new-dotnet-core/release-history.png)

对于 PCL，开发体验在基于 API 形状的纵向范围内是统一的。 另外，还可以解决在不同的纵向创建运行库的最紧迫需求。 但有一个很大的挑战：当实现在所有纵向之间向前移动时，Api 是可移植的。

更好的方法是，通过提供合理的实现（而不是分解良好的视图）跨纵向统一实现。 让拥有特定组件的每个团队都可以考虑其 Api 在所有行业中的工作方式，而不是尝试以追溯方式提供一致的 API 堆栈。 这就是 .NET Standard 进入的位置。 请参阅下一节中的详细信息。

另一大挑战与如何部署 .NET Framework 有关。 .NET Framework 是一种计算机范围框架。 对它所做的任何更改都会影响依赖于它的所有应用程序。 尽管此部署模型有许多优点，例如减少磁盘空间和对服务的集中访问，但这会带来一些缺陷。

首先，应用程序开发人员很难对最近发布的框架进行依赖。 它们要么需要依赖于最新的操作系统，要么提供与应用程序一起安装 .NET Framework 的应用程序安装程序。 如果你是 web 开发人员，则可能甚至没有此选项，因为 IT 部门会建立服务器支持的版本。

即使您愿意浏览 .NET Framework 安装程序中链接的安装程序，也可能会发现升级 .NET Framework 可能会中断其他应用程序。

尽管提供了向后兼容的框架版本，但也存在可中断应用程序的兼容更改。 例如，将接口添加到现有类型可以更改此类型的序列化方式，并根据现有代码引发重大问题。 由于已安装的 .NET Framework 很大，因此对这些中断方案的应对会降低 .NET Framework 内的创新速度。

为了解决所有这些问题，Microsoft 开发了 .NET Core 来实现 .NET 平台的发展。

## <a name="introduction-to-net-core"></a>.NET Core 简介

.NET Core 是将 Microsoft .NET 技术发展为模块化、跨平台、开源和云就绪平台的演变。 它在 Windows、macOS 和 Linux 上运行，并且计划也在基于 ARM 的体系结构（如 Android 和 IoT）上运行。

.NET Core 的目的是为所有类型的应用程序（包括 Windows、跨平台和移动应用程序）提供一个统一的平台。 [.NET Standard](../../standard/net-standard.md) 提供共享的基本 api，每个应用程序模型都需要，并排除任何特定于应用程序模型的 API，从而实现此功能。

此框架使应用程序在效率和性能方面的优势很多，并简化了不同受支持平台的打包和部署。

.NET Core 的优点在于这三个特征：

- **跨平台：** 它允许在不同的平台上执行应用程序 (Windows、macOS 和 Linux) 。
- **开源：** .net Core 平台是开放源代码，可通过 GitHub、促进透明度和社区贡献获得。
- **支持：** Microsoft 正式支持 .NET Core。

从 .NET Core 3.0 开始，除了对 web 和云的现有支持外，还支持桌面、IoT 和 AI 域。 此框架的目标非常不错：针对每种类型的 .NET 开发提供目标和未来。 Microsoft 计划在2020年底通过 .NET 5 完成此视觉。 删除了 "Core" 名称，以加强其在 .NET 环境中的唯一性。

![.NET 5 的所有域](./media/whats-new-dotnet-core/all-domains-of-dotnet5.png)

## <a name="net-framework-vs-net-core"></a>.NET Framework 与 .NET Core

至此，你已了解 .net Core 在 .NET 中的相关关系，你可能想知道 .NET Framework 会发生什么情况。 您可能会问一些问题，例如：您是否必须放弃它？ 即将消失了吗？ 我对 .NET Framework 上的应用程序进行现代化的选择是什么？

在2019中，已发布 **4.8 .NET Framework** 的最后一个版本。 它包括三个对桌面应用程序的重大改进：

- **新式浏览器和媒体控件**：目前，.net 桌面应用程序使用 Internet Explorer 和 Windows MEDIA PLAYER 显示 HTML 和播放媒体文件。 由于这些旧控件不显示最新的 HTML 或播放最新的媒体文件，因此添加了新的控件，这些控件利用支持最新标准的 Microsoft Edge 和更高版本的媒体播放器。
- **访问 uwp 控件**： uwp 包含了利用最新的 Windows 功能和触控显示的新控件。 无需重写应用程序即可使用这些新功能和控件，因此可以在现有 WPF 或 Windows 窗体代码中使用这些新功能。
- **高 DPI 改进**：显示的分辨率提高了4K 和8k 的分辨率。 因此，.NET Framework 4.8 添加了新的 HDPI 改进，以确保现有 Windows 窗体和 WPF 应用程序在这些新显示器上看起来很有用。

由于 .NET Framework 安装在数百万台计算机上，Microsoft 将继续支持它，但不会添加新功能。

.NET Core 是 .NET 的开源、跨平台和快速移动版本。 由于它的并行性质，它可以在不中断任何应用程序的情况下进行更改。 这意味着，.NET Core 将随着时间的推移获得新的 Api 和语言功能，.NET Framework 不会。 此外， **.Net Core** 已具有 .NET Framework 无法实现的功能，例如：

- **.Net 的并行版本支持 Windows 窗体和 WPF**：这解决了更新计算机的框架版本时的副作用问题。 可以在同一台计算机上安装多个版本的 .NET Core，每个应用程序指定它应使用的 .NET Core 版本。 更为重要的是，现在您可以在 .NET Core 顶层开发和运行 Windows 窗体和 WPF。
- 将 **.net 直接嵌入应用程序**：可将 .net Core 作为应用程序包的一部分进行部署。 这使你可以利用最新的版本、功能和 Api，而无需等待计算机上安装特定版本。
- **利用 .Net core 功能**： .net core 是 .net 的快速移动开源版本。 它的并行性质使全新的创新 Api 和基类库快速引入 (BCL) 改进，而不会带来中断兼容性的风险。 现在 Windows 窗体和 WPF 应用程序可以利用最新的 .NET 核心功能，这些功能还包括更多针对运行时性能、高 DPI 支持等的基本修补程序。

Microsoft 规划的一个重要部分是使开发人员能够轻松地将应用程序迁移到 .NET Core，并使其在将来转换为 .NET 5。 但是，如果您有现成的 .NET Framework 应用程序，则不应压力迁移到 .NET Core。 .NET Framework 将完全受支持，并且将始终是 Windows 的一部分。 但是，如果想要在将来使用最新的语言功能和 Api，则需要将应用程序移到 .NET Core。

对于全新的桌面应用程序，我们建议直接从 .NET Core 开始。 它是轻型的、跨平台的，并行运行，具有高性能，并完全适合容器和微服务体系结构。

![可以使用最新的 .NET Framework 版本更新 .NET Framework 应用程序，或将应用程序移植到 .NET Core](./media/whats-new-dotnet-core/framework-vs-core.png)

## <a name="net-standard-vs-pcl"></a>.NET Standard 与 PCL

[.NET Standard](../../standard/net-standard.md) 是一套正式的 .NET API 规范，有望在所有 .NET 实现中推出。 推出 .NET Standard 的背后动机是要提高 .NET 生态系统中的一致性。 .NET Standard 是 .NET Api 的规范，它们构成了一组统一的协定，用于对代码进行编译。 这些协定是在每个 .NET 风格上实现的，因此可以跨不同的 .NET 实现实现可移植性。

.NET Standard 可实现以下重要情境：

- 为所有要实现的 .NET 实现定义一组统一的基类库 Api，而不依赖于工作负荷。
- 使开发人员能够通过同一组 API 生成可在各种 .NET 实现中使用的可移植库。

.NET Standard 是 Pcl 的发展，下表显示了 .NET Standard 与 Pcl 之间的基本差异：

- .NET Standard 是由 Microsoft 挑选的一组特选 Api。 Pcl 不是。
- PCL 包含的 Api 依赖于创建时所选择的目标平台。 这会使你选择的特定目标只能共享 PCL。
- .NET Standard 是平台无关的，它可以在 Windows、macOS、Linux 等任意位置运行。
- Pcl 也可以跨平台运行，但它们的覆盖范围更有限。 Pcl 只能针对一组有限的平台。

## <a name="new-desktop-features-in-net-core"></a>.NET Core 中的新增桌面功能

### <a name="support-for-windows-forms-and-wpf"></a>支持 Windows 窗体和 WPF

自3.0 版起，Windows 窗体和 WPF 是 .NET Core 的一部分。 这两个表示框架仅适用于 Windows，因此它们不跨平台。 你可以将 WPF 视为基于 DirectX 的丰富层，并将其作为基于 GDI + 的瘦层 Windows 窗体。 WPF 和 Windows 窗体会很好地在 Windows 中公开和执行许多桌面应用程序功能。 因此 Windows 窗体和 WPF 适用于 .NET Core 和 .NET Framework。 现在，你可以开始以 .NET Core 为目标的新桌面应用程序，并将现有的桌面应用程序从 .NET Framework 迁移到 .NET Core。

新版本的 .NET Standard 版本2.1，与 .NET Core 3.0 同时发布。 与预期一样，.NET Core 2.x 版本支持 .NET Standard 2.1 及更早版本。

另外，请务必注意，.NET Core Windows 窗体和 WPF 实现都是开放源代码。

### <a name="xaml-islands"></a>XAML 岛

[XAML 孤岛](/windows/apps/desktop/modernize/xaml-islands) 是一组组件，供开发人员使用新的 Windows 10 控件 (UWP XAML 控件) 在其当前 WPF、Windows 窗体和本机 Win32 应用 (如 MFC) 中。 您可以在您希望在 Win32 应用程序中的任何位置使用 UWP XAML 控件的 "孤岛"。

这些 XAML 孤岛之所以可行，是因为 Windows 10 版本1903引入了一组 Api，这些 Api 允许使用 Windows 处理程序在 Win32 窗口中承载 UWP XAML 内容， (Hwnd) 。 请注意，只有在 Windows 10 1903 和更高版本上运行的应用程序才能使用 XAML 孤岛。

为了更轻松地创建 Windows 窗体和 WPF 开发人员的 XAML 孤岛，Windows 社区工具包在多个 NuGet 包中引入了一组 .NET 包装。 这些包装器是包装控件和托管控件：

- [Web 视图](/windows/communitytoolkit/controls/wpf-winforms/webview)、 [WebViewCompatible](/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible)、 [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)、 [MEDIAPLAYERELEMENT](/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement)和[MapControl](/windows/communitytoolkit/controls/wpf-winforms/mapcontrol)包装控件将某些 UWP XAML 控件包装到 Windows 窗体或 WPF 控件中，从而为这些开发人员隐藏 uwp 概念。
- Windows 窗体和 WPF 的 [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 控件允许其他不换行的 UWP xaml 控件和自定义控件可以加载到 XAML 岛中。

### <a name="access-to-all-windows-10-apis"></a>访问所有 Windows 10 Api

Windows 10 提供了大量可供开发人员使用的 API。 这些 Api 可让你访问各种功能，如身份验证、蓝牙、约会和联系人。 现在，这些 Api 通过 .NET Core 公开，并使 Windows 开发人员能够利用 Windows 10 上的功能创建功能强大的桌面应用程序。

### <a name="side-by-side-support-and-self-contained-exes"></a>并行支持和自包含 Exe

.NET Core 部署模型是 Windows 桌面开发人员通过 .NET Core 获得的最大优势之一。 全局安装 .NET Core 的功能提供了许多与 .NET Framework 相同的中心安装和服务优势，同时不需要就地更新。

发布新的 .NET Core 版本后，可以根据需要在计算机上更新每个应用，而不会影响其他应用程序。 新的 .NET Core 版本安装在其自己的目录中，并且彼此并存。

如果需要通过隔离进行部署，可以将 .NET Core 与应用程序一起部署。 .NET Core 将使用 .NET Core 运行时将应用捆绑在一个可执行文件中。

开发人员需要很长时间才能完成这些部署选项，但使用 .NET Framework 很难实现。 .NET Core 使用的模块化体系结构可实现这些灵活的部署选项。

### <a name="performance"></a>性能

自其开始，面向 web 和云工作负荷，.NET Core 已将性能引入其 DNA。 服务器端代码的性能必须足以满足高并发性方案和今天的 .NET Core 评分，作为市场上最好的性能 web 平台。

使用 .NET Core 构建下一代桌面应用程序时，可以利用这些性能改进。

## <a name="benefits-of-open-source"></a>开源的优点

有关 .NET Core 是开放源代码的几个词。 构建跨平台堆栈是复杂的，需要在每个目标平台上的专用团队交互。 此工作需要在 Microsoft 内部和外部进行大量协作。 通过使其成为开放源代码，进而公开公共协作，您可以就地获得终极敏捷开发样式，提高质量栏，因为开发人员很大的活动社区都检测到问题。

这是 .NET Core 的关键成功因素，将继续加快前面提到的路线图：成为任何开发人员将需要构建任何应用程序的单一 .NET 平台。

>[!div class="step-by-step"]
>[上一页](why-modern-applications.md)
>[下一页](migrate-modern-applications.md)
