---
title: .NET Standard
description: 了解 .NET Standard 及其版本以及支持它的 .NET 实现。
ms.date: 10/05/2020
ms.prod: dotnet
ms.technology: dotnet-standard
ms.custom: updateeachrelease
ms.assetid: c044882c-af15-45f2-96d1-534557a5ee9b
ms.openlocfilehash: e0adb69d02fe427dd187c788cba34c0673e97ee6
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94822304"
---
# <a name="net-standard"></a>.NET Standard

[.NET Standard](https://github.com/dotnet/standard) 是针对多个 .NET 实现推出的一套正式的 .NET API 规范。 推出 .NET Standard 的背后动机是要提高 .NET 生态系统中的一致性。 但是，.NET 5 采用不同的方法来建立一致性，这种新方法在很多情况下都不需要 .NET Standard。 有关详细信息，请参阅本文后续部分的 [.NET 5 和 .NET Standard](#net-5-and-net-standard)。

## <a name="net-implementation-support"></a>.NET 实现支持

各种 .NET 实现以特定版本的 .NET Standard 为目标。 每个 .NET 实现版本都会公布它所支持的最高 .NET Standard 版本，这种声明意味着它也支持以前的版本。 例如，.NET Framework 4.6 实现 .NET Standard 1.3。也就是说，它会公开在 .NET Standard 版本 1.0 到 1.3 中定义的所有 API。 同样，.NET Framework 4.6.1 实现 .NET Standard 1.4，而 .NET 5.0 则实现 .NET Standard 2.1。

下表列出了支持每个 .NET Standard 版本的最低实现版本。 这意味着所列实现的更高版本也支持相应的 .NET Standard 版本。 例如，.NET Core 2.1 及更高版本支持 .NET Standard 2.0 及更早版本。

[!INCLUDE [net-standard-table](../../includes/net-standard-table.md)]

若要查找可以定位的 .NET Standard 最高版本，请按照以下步骤操作：

1. 查找要运行的 .NET 实现所在的行。
1. 在这一行中从右向左查找可以定位的 .NET Standard 版本所在的列。
1. 列标题指示目标支持的 .NET Standard 版本。 此外可以面向任何更低的 .NET Standard 版本。 更高版本的 .NET Standard 还支持实现。
1. 对要定位的每个平台重复执行此过程。 如果有多个目标平台，应选择它们都支持的最高版本。 例如，如果要在 .NET Framework 4.8 和 .NET 5.0 上运行，可以使用的最高 .NET Standard 版本是 .NET Standard 2.0。

### <a name="which-net-standard-version-to-target"></a>要定位哪个 .NET Standard 版本

选择目标 .NET Standard 版本时，请权衡以下因素：

- 版本越高，可用于库的代码的 API 就越多。
- 版本越低，可使用你的库的应用和库就越多。

建议尽可能将最低版本的 .NET Standard 作为目标。 因此，在找到可以定位的最高版本 .NET Standard 后，请按照以下步骤操作：

1. 定位前一更低版本的 .NET Standard，然后生成项目。
2. 如果成功生成项目，请重复执行第 1 步。 否则，重新定位到后一较高版本，这就是应该使用的版本。

但是，定位更低版本的 .NET Standard 会引入许多支持依赖项。 如果项目定位 .NET Standard 1.x，我们建议  还定位 .NET Standard 2.0。 这简化了在 .NET Standard 2.0 兼容实现上运行的库的用户的依赖项关系图，并减少了下载所需的包数。

### <a name="net-standard-versioning-rules"></a>.NET Standard 版本控制规则

版本控制规则主要有两个：

- 累加性：.NET Standard 版本在逻辑上形成同心圆。也就是说，较高的版本包含较低版本的所有 API。 版本之间没有重大更改。
- 不可变：一旦发布，.NET Standard 版本就会冻结起来。

在 .NET Standard 2.1 版本之后，将不会有新版本。 有关详细信息，请参阅本文后续部分的 [.NET 5 和 .NET Standard](#net-5-and-net-standard)。

## <a name="specification"></a>规范

.NET Standard 规范是一组标准化的 API。 此规范由 .NET 实现者（具体而言，由 Microsoft（包括 .NET Framework、NET Core 和 Mono）和 Unity）进行维护。

### <a name="official-artifacts"></a>正式项目

正式规范是一组用于定义标准中包含的 API 的 .cs 文件。 [dotnet/standard 存储库](https://github.com/dotnet/standard)中的 [Ref 目录](https://github.com/dotnet/standard/tree/master/src/netstandard/ref)定义了 .NET Standard API。

[NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library) 元包（[源代码](https://github.com/dotnet/standard/blob/master/src/netstandard/pkg/NETStandard.Library.dependencies.props)）描述用于部分定义一个或多个 .NET Standard 版本的库集。

给定的组件（如 `System.Runtime`）描述：

- .NET Standard 的一部分（即其范围）。
- .NET Standard 在此范围内的多个版本。

标准库提供派生项目以方便读取，并实现某些开发人员方案（例如，使用编译器）。

- [Markdown 中的 API 列表](https://github.com/dotnet/standard/tree/master/docs/versions)
- 引用程序集，以 NuGet 包的形式分发，由 [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/) 元包引用。

### <a name="package-representation"></a>包表示形式

.NET Standard 引用程序集的主要分发载体是 NuGet 包。 实现会以适用于每个 .NET 实现的各种方式提供。

NuGet 包面向一个或多个[框架](frameworks.md)。 .NET Standard 包面向“.NET Standard”框架。 可以使用 `netstandard` [精简 TFM](frameworks.md)（例如 `netstandard1.4`）来设定 .NET Standard 框架作为目标。 计划在多个 .NET 实现上运行的库应将此框架作为目标。 对于最广泛的 API 集，将 `netstandard2.0` 设定为目标，因为 .NET Standard 2.0 的可用 API 数量比 .NET Standard 1.6 的两倍还多。

[`NETStandard.Library`](https://www.nuget.org/packages/NETStandard.Library/) 元包引用定义 .NET Standard 的一整套 NuGet 包。  要指定 `netstandard` 作为目标，最常见的方法是引用此元包。 它描述并提供了对大约 40 个 .NET 库及定义 .Net Standard 的相关 API 的访问权限。 可以引用以 `netstandard` 为目标的其他包来使用其他 API。

### <a name="versioning"></a>版本管理

规范并不是单一的，而是一组以线性方式进行版本控制的 API。 该标准的第一个版本建立了一组基准 API。 后续版本将添加 API，并继承以前的版本定义的 API。 在从 Standard 中移除 API 方面，并没有成文的规定。

.NET Standard 并不特定于任何一种 .NET 实现，也不与其中任一实现的版本控制方案匹配。

如前所述，在 .NET Standard 2.1 版本之后，将不会有新版本。

## <a name="target-net-standard"></a>定位 .NET Standard

可以结合使用 `netstandard` 框架和 `NETStandard.Library` 元包来[构建 .NET Standard 库](../core/tutorials/libraries.md)。

## <a name="net-framework-compatibility-mode"></a>.NET Framework 兼容性模式

从 .NET Standard 2.0 开始，引入了 .NET Framework 兼容性模式。 此兼容性模式允许 .NET Standard 项目引用 .NET Framework 库，就像其针对 .NET Standard 编译一样。 引用 .NET Framework 库并不适用于所有项目，，例如使用 Windows Presentation Foundation (WPF) API 的库。

有关详细信息，请参阅 [.NET Framework兼容性模式](../core/porting/third-party-deps.md#net-framework-compatibility-mode)。

## <a name="net-standard-libraries-and-visual-studio"></a>.NET Standard 库和 Visual Studio

要在 Visual Studio 中生成 .NET Standard 库，请确保 Windows 上已安装 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 或 Visual Studio 2017 版本 15.3 或更高版本，或 macOS 上已安装 [Visual Studio for Mac 版本 7.1](https://visualstudio.microsoft.com/vs/mac/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link) 或更高版本。

如果项目中只需使用 .NET Standard 2.0 库，也可在 Visual Studio 2015 中执行此操作。 但是需要安装 NuGet client 3.6 或更高版本。 可从 [NuGet 下载](https://www.nuget.org/downloads)页面下载适用于 Visual Studio 2015 的 NuGet 客户端。

## <a name="net-5-and-net-standard"></a>.NET 5 和 .NET Standard

.NET 5 是 Microsoft 正在积极开发的 .NET 实现。 它是具有一组统一功能和 API 的单一产品，可用于 Windows 桌面应用和跨平台控制台应用、云服务和网站。 .NET 5.0 [TFM](frameworks.md) 反映了以下广泛的应用场景：

* `net5.0`

  此 TFM 适用于在任何位置运行的代码。 它仅包括跨平台工作的技术（少数例外情况除外）。 对于 .NET 5 代码，`net5.0` 替换了 `netcoreapp` 和 `netstandard` TFM。

* `net5.0-windows`

  这是一个[特定于 OS 的 TFM](frameworks.md#net-5-os-specific-tfms) 的示例，该 TFM 可向 `net5.0` 引用的所有内容添加特定于 OS 的功能。

### <a name="when-to-target-net50-vs-netstandard"></a>以 net5.0 为目标与以 netstandard 为目标的情形

对于以 `netstandard` 为目标的现有代码，无需将 TFM 更改为 `net5.0`。 .NET 5.0 可实现 .NET Standard 2.1 及更早版本。 将目标从 .NET Standard 更改为 .NET 5.0 的唯一原因是获取对更多运行时功能、语言功能或 API 的访问权限。 例如，为了使用 C# 9，需要以 .NET 5.0 为目标。 可同时以 .NET 5.0 和 .NET Standard 为目标来访问较新的功能，并仍然可将库供其他 .NET 实现使用。

下面是适用于 .NET 5 新代码的一些准则：

* 应用组件

  如果要使用库将应用程序分解成多个组件，建议以 `net5.x` 作为目标，其中 `5.x` 是应用程序可将其作为目标的最早的 .NET 5 版本。 为简单起见，最好在同一 .NET 版本中保留构成应用程序的所有项目。 然后，可以假设任何位置均具有相同的 BCL 功能。

* 可重用的库

  如果要构建计划在 NuGet 上发布的可重用的库，请考虑在覆盖范围和可用功能集之间进行权衡。 .NET Standard 2.0 是 .NET Framework 支持的最新版本，因此它具有相当大的功能集，可提供良好的覆盖范围。 我们建议你不要以 .NET Standard 1.x 作为目标，因为这样会限制可用的功能集，使覆盖范围的增幅降至最低。

  如果你不需要支持 .NET Framework，可以选择 .NET Standard 2.1 或 .NET 5。 我们建议你跳过 .NET Standard 2.1，而直接选择 .NET 5。 大多数广泛使用的库最终都将同时以 .NET Standard 2.0 和 .NET 5 作为目标。 支持 .NET Standard 2.0 可提供最大的覆盖范围，而支持 .NET 5 可确保你可以为已使用 .NET 5 的客户利用最新的平台功能。

### <a name="net-standard-problems"></a>.NET Standard 的问题

下面是一些关于 .NET Standard 的问题，这些问题有助于解释为什么最好使用 .NET 5 来跨平台和工作负载共享代码：

- 添加新 API 的速度缓慢

  .NET Standard 是作为所有 .NET 实现都必须支持的 API 集创建的，因此会对添加新 API 的建议进行审核。 目标是仅标准化可在所有当前和未来的 .NET 平台中实现的 API。 因此，如果某个功能错过了特定版本，则你可能需要等待几年，该功能才会被添加到 Standard 版本中。 然后，你需要等待更长的时间，新版本的 .NET Standard 才能受到广泛支持。

  .NET 5 中的解决方案：实现某项功能时，该功能便已可供所有 .NET 5 应用和库使用，因为代码基底是共享的。 由于 API 规范与其实现之间没有区别，因此相较于使用 .NET Standard，你可以更快地利用新功能。

- 复杂的版本控制

  API 规范与其实现的分离导致 API 规范版本与实现版本之间出现复杂的映射。 这种复杂性在本文前面显示的表以及其解释方式说明中显而易见。

  .NET 5 中的解决方案：.NET 5.x API 规范与其实现之间不存在任何分离。 由此实现了一个简化的 TFM 方案。 提供了一个适用于所有工作负载的 TFM 前缀：`net5.0` 用于库、控制台应用和 Web 应用。 唯一的变化是针对特定平台[指定特定于平台的 API 的后缀](frameworks.md#net-5-os-specific-tfms)，例如 `net5.0-windows`。 由于此 TFM 命名约定，你可以轻松判断给定应用是否可以使用给定的库。 不需要版本号等效表（如 .NET Standard 的版本号等效表）。

- 运行时出现不受平台支持的异常

  .NET Standard 公开了特定于平台的 API。 代码在编译时可能不会出错，并且看起来可以移植到任何平台（即该代码不可移植也是如此）。 当它在不具有给定 API 实现的平台上运行时，会出现运行时错误。

  .NET 5 中的解决方案：.NET 5 SDK 包括默认启用的代码分析器。 平台兼容性分析器会检测对你打算在其上运行的平台所不支持的 API 的意外使用情况。 有关详细信息，请参阅[平台兼容性分析器](analyzers/platform-compat-analyzer.md)。

### <a name="net-standard-not-deprecated"></a>未弃用 .NET Standard

对于可由多个 .NET 实现使用的库，仍需要 .NET Standard。 在以下情况下，建议以 .NET Standard 作为目标：

* 使用 `netstandard2.0` 在 .NET Framework 和 .NET 的所有其他实现之间共享代码。
* 使用 `netstandard2.1` 在 Mono、Xamarin 和 .NET Core 3.x 之间共享代码。

## <a name="see-also"></a>另请参阅

- [.NET Standard 版本（源）](https://github.com/dotnet/standard/blob/master/docs/versions.md)
- [.NET Standard 版本（交互式 UI）](https://dotnet.microsoft.com/platform/dotnet-standard#versions)
- [生成 .NET Standard 库](../core/tutorials/library-with-visual-studio.md)
- [跨平台定位](./library-guidance/cross-platform-targeting.md)
