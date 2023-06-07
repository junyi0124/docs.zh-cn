---
title: 从 .NET Framework 移植到 .NET Core
description: 了解移植过程以及发现在将 .NET Framework 项目移植到 .NET Core 时可能有用的工具。
author: cartermp
ms.date: 10/22/2019
ms.openlocfilehash: 764c615d250474573f873b1745b9f1469e637d29
ms.sourcegitcommit: 34968a61e9bac0f6be23ed6ffb837f52d2390c85
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "94687407"
---
# <a name="overview-of-porting-from-net-framework-to-net-core"></a>从 .NET Framework 移植到 .NET Core 的概述

你可能有些代码当前正在 .NET Framework 上运行，但你想将这些代码移植到 .NET Core。 本文提供以下内容：

* 移植过程概述。
* 在将代码移植到 .NET Core 时，可能会发现一系列有用的工具。

## <a name="overview-of-the-porting-process"></a>移植过程概述

针对多个项目从 .NET Framework 移植到 .NET Core（或 .NET Standard）的操作相对简单。 需进行大量更改，但其中很多都遵循下面所列的模式。 对于可在 .NET Core 中使用应用模式的项目（例如库、控制台应用和桌面应用程序），通常需要少量更改。 对于需使用新应用模式的项目（例如从 ASP.NET 移至 ASP.NET Core），需要的工作稍微多一点，但很多模式与可在转换过程中使用的模式类似。 本文档可帮助确定用户已经采用来成功转换其基本代码以面向 .NET Standard 或 .NET Core 的主要策略，还将处理“解决方案范围”和“项目特定”这两个级别的转换。 有关应用模式特定的转换，请查看说明底部的链接。

建议在将项目移植到 .NET Core 时使用以下过程。 其中每个步骤都可能导致行为更改，因此请确保你在继续执行后续步骤之前，对你的库或应用程序进行了充分的测试。 首先是准备好项目来切换到 .NET Standard 或 .NET Core。 如果你有单元测试，则最好先转换它们，以便你能够在你使用的产品中继续测试相关更改。 由于移植到 .NET Core 对代码库来说是很大的更改，因此强烈建议移植测试项目，以便在移植代码后运行测试。 MSTest、xUnit 和 NUnit 都适用于 .NET Core。

## <a name="getting-started"></a>入门

将在整个过程中使用以下工具：

- Visual Studio 2019
- 下载 [.NET 可移植性分析器](../../standard/analyzers/portability-analyzer.md)
- 可选：安装 [dotnet try-convert](https://github.com/dotnet/try-convert)

## <a name="porting-a-solution"></a>移植解决方案

如果解决方案中有多个项目，则移植可能看起来更复杂，因为必须按特定顺序处理项目。 应按从上到下的顺序执行转换过程，其中先转换解决方案中不依赖其他项目的项目，再继续转换，完成整个解决方案的处理。

为确定项目的迁移顺序，可使用以下工具：

- [Visual Studio 中的依赖项关系图](/visualstudio/modeling/create-layer-diagrams-from-your-code)可在解决方案中创建代码定向图。
- 运行 `msbuild _SolutionPath_ /t:GenerateRestoreGraphFile /p:RestoreGraphOutputPath=graph.dg.json` 以生成一个包含项目引用列表的 json 文档。
- 使用 `-r DGML` 开关运行 [.NET 可移植性分析器](../../standard/analyzers/portability-analyzer.md)，检索程序集的依赖项关系图。 有关详细信息，请参阅[此文](../../standard/analyzers/portability-analyzer.md#solution-wide-view)。

拥有依赖项信息后，可用它来在叶节点开始操作，一直到应用下一部分相关步骤的依赖项树。

## <a name="per-project-steps"></a>按项目步骤

建议在将项目移植到 .NET Core 时使用以下过程：

1. 通过 [Visual Studio 中的转换工具](/nuget/consume-packages/migrate-packages-config-to-package-reference)将所有 `packages.config` 依赖项转换为 [PackageReference](/nuget/consume-packages/package-references-in-project-files) 格式。

   此步骤涉及到转换旧 `packages.config` 格式的依赖项。 `packages.config` 不适用于 .NET Core，因此，如果你有包依赖项，则需要进行此转换。 此外，它仅需要你直接在项目中使用的依赖项，这通过减少必须管理的依赖项数量，使后续步骤更加简单。

1. 将项目文件转换为新的 SDK 样式文件结构。 你可为 .NET Core 创建新项目并覆盖源文件，也可尝试使用工具转换现有的项目文件。

   .NET Core 使用不同于 .NET Framework 的更简化的[项目文件格式](../tools/csproj.md)。 需要将项目文件转换为此格式才能继续操作。 借助此项目样式，还可面向 .NET Framework（你此时仍需要面向此模型）。

   你可尝试使用 [dotnet try-convert](https://github.com/dotnet/try-convert) 工具，通过一次操作将较小的解决方案或单个项目移植到 .NET Core 项目文件格式。 不能保证 `dotnet try-convert` 适用于所有项目，它可能导致所依赖的行为发生细微变化。 使用它作为起点，以自动化可自动执行的基本操作。 该解决方案不保证会迁移项目，因为与旧样式的项目文件相比，SDK 样式的项目使用的目标中存在诸多差异。

1. 将希望移植的所有项目重定向到目标 .NET Framework 4.7.2 或更高版本。

   此步骤可确保在 .NET Core 不支持特殊 API 的情况下，可以为特定于 .NET Framework 的目标使用备用 API。

1. 将所有依赖项更新到最新版本。 项目可能在使用更旧的库版本，而这些版本可能不支持 .NET Standard。 但是，之后的版本可能支持该规范，只用简单切换一下就行。 如果库中存在中断性变更，则这可能需要更改代码。

1. 使用 [.NET 可移植性分析器](../../standard/analyzers/portability-analyzer.md)来分析程序集，并查看这些程序集是否可移植到 .NET Core。

   .NET 可移植性分析器工具可分析已编译的程序集并生成报表。 此报表显示高级别可移植性摘要，以及你所使用的不适用于 .NET Core 的各个 API 细目。 使用该工具时，只提交你正在转换的单个项目，从而专注于可能需要的 API 更改。 很多 API 在 .NET Core 中具有相同的可用性，而你需要切换到该平台。

   在读取分析器生成的报表时，重要的信息是正在使用的实际 API，而不一定是对目标平台的支持百分比。 很多 API 在 .NET Standard/Core 中都有等效选项，因此了解你的库或应用程序需要 API 实现的方案将有助于确定可移植性的影响。

   在某些情况下，API 不是等效的，需要执行一些编译器预处理器指令（即 `#if NET45`）来应对平台的特殊情况。 此时，你的项目仍然面向 .NET Framework。 对于上述每种有针对性的情况，建议使用可被理解为方案的已知条件。  例如，.NET Core 中的 AppDomain 支持受到限制，但是对于加载和卸载程序集的方案，有一个新的 API 无法在 .NET Core 中使用。 要在代码中处理此情况，一种常见的方式如下所示：

   ```csharp
   #if FEATURE_APPDOMAIN_LOADING
   // Code that uses appdomains
   #elif FEATURE_ASSEMBLY_LOAD_CONTEXT
   // Code that uses assembly load context
   #else
   #error Unsupported platform
   #endif
   ```

1. 将 [.NET API 分析器](../../standard/analyzers/api-analyzer.md)安装到项目中，以识别在某些平台上会引发 <xref:System.PlatformNotSupportedException> 的 API 以及一些其他潜在的兼容性问题。

   此工具与可移植性分析器类似，但它不会分析是否可以在 .NET Core 上构建代码，而是分析你是否正在以会导致在运行时引发 <xref:System.PlatformNotSupportedException> 的某种方式使用 API。 尽管这并不常见，但如果从 .NET Framework 4.7.2 或更高版本进行移动，最好进行检查。 如需详细了解会在 .NET Core 上引发异常的 API 信息，请参阅[在 .NET Core上始终引发异常的 API](../compatibility/unsupported-apis.md)。

1. 此时，你可切换为面向 .NET Core（通常用于应用程序）或 .NET Standard（用于库）。

   选择 .NET Core 还是 .NET Standard 主要取决于将运行项目的位置。 如果它是其他应用程序将使用的库或将通过 NuGet 分发的库，通常首选的是面向 .NET Standard。 但是，出于性能原因或其他原因，可能有一些 API 只能在 .NET Core 上使用；如果不是这样，则应面向 .NET Core，但可能也有 .NET Standard 版本可供使用，其性能或功能有所降低。 通过面向 .NET Standard，项目将能够在新平台（如 WebAssembly）上运行。 如果项目对特定应用框架（如 ASP.NET Core）具有依赖项，则面向的目标将受到依赖项支持的内容限制。

   如果对 .NET Framework 或 .NET Standard 来说，没有针对条件编译代码的预处理器指令，则该操作将如同在项目文件中查找以下内容一样简单：

   ```xml
   <TargetFramework>net472</TargetFramework>
   ```

   并将它切换到所需框架。 对于 .NET Core 3.1，这为：

   ```xml
   <TargetFramework>netcoreapp3.1</TargetFramework>
   ```

   但是，如果这是一个库，而你希望它继续支持 .NET Framework 特定的版本，则可通过将其替换为以下内容来设置[多目标](../../standard/library-guidance/cross-platform-targeting.md)：

   ```xml
   <TargetFrameworks>net472;netstandard2.0</TargetFrameworks>
   ```

   如果正在使用特定于 Windows 的 API（例如注册表访问），请安装 [Windows 兼容包](./windows-compat-pack.md)。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [分析依赖项](third-party-deps.md)
> [打包 NuGet 包](../deploying/creating-nuget-packages.md)

## <a name="see-also"></a>请参阅

- [ASP.NET 到 ASP.NET Core 迁移](/aspnet/core/migration/proper-to-2x)
- [将 WPF 应用迁移到 .NET Core](/dotnet/desktop/wpf/migration/convert-project-from-net-framework)
- [将 .NET Framework Windows 窗体应用迁移到 .NET](/dotnet/desktop/winforms/migration/?view=netdesktop-5.0&preserve-view=true)
