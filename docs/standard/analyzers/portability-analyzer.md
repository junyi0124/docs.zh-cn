---
title: .NET 可移植性分析器 - .NET
description: 了解如何使用 .NET 可移植性分析器工具，评估代码在各种 .NET 实现（包括 .NET Core、.NET Standard、UWP 和 Xamarin）间的可移植性。
ms.date: 09/13/2019
ms.assetid: 0375250f-5704-4993-a6d5-e21c499cea1e
ms.openlocfilehash: 048ff916d309f4159fe78177e093a58d731c2e11
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734278"
---
# <a name="the-net-portability-analyzer"></a>.NET 可移植性分析器

想让库支持多平台吗？ 想要了解使 .NET Framework 应用程序在 .NET Core 上运行需要花费多大的精力？ [.NET 可移植性分析器](https://github.com/microsoft/dotnet-apiport)是一种工具，可分析程序集并为应用程序或库提供有关缺失的 .NET API 的详细报告，以便在指定的目标 .NET 平台上实现可移植性。 可移植性分析器作为 [Visual Studio Extension](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer) 提供，用于分析每个项目的一个程序集；也可以作为 [ApiPort 控制台应用](https://aka.ms/apiportdownload)提供，用于按指定文件或目录分析程序集。

将项目转换为面向 .NET Core 等新平台后，可以使用基于 Roslyn 的 [API 分析器工具](api-analyzer.md)来识别引发 <xref:System.PlatformNotSupportedException> 异常以及其他兼容性问题的 API。

## <a name="common-targets"></a>常用对象

- [.NET Core](../../core/introduction.md)：采用模块化设计，支持并行安装，面向跨平台方案。 可并行安装意味着无需破坏其他应用即可采用新的 .NET Core 版本。 如果目标是将应用移植到 .NET Core 以支持多个平台，则建议使用此对象。
- .[NET Standard](../net-standard.md)：包括所有 .NET 实现上提供的 .NET Standard API。 如果目标是使自己的库能够在所有 .NET 支持的平台上运行，则建议使用此对象。
- [ASP.NET Core](/aspnet/core)：在 .NET Core 基础上构建的现代 Web 框架。 如果目标是将 Web 应用移植到 .NET Core 以支持多个平台，则建议使用此对象。
- .NET Core + [平台扩展](../../core/porting/windows-compat-pack.md)：除 Windows 兼容包之外，还包括 .NET Core API，后者提供了许多可用的 .NET Framework 技术。 这是推荐的对象，用于将 Windows 上的应用从 .NET Framework 移植到 .NET Core。
- .NET Standard + [平台扩展](../../core/porting/windows-compat-pack.md)：除 Windows 兼容包之外，还包括 .NET Standard API，后者提供了许多可用的 .NET Framework 技术。 这是推荐的对象，用于将 Windows 上的库从 .NET Framework 移植到 .NET Core。

## <a name="how-to-use-the-net-portability-analyzer"></a>如何使用 .NET 可移植性分析器

若要开始在 Visual Studio 中使用 .NET 可移植性分析器，必须先从 [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer) 下载扩展并进行安装。 它适用于 Visual Studio 2017 及更高版本。 可以通过 Visual Studio 中的“分析” > “可移植性分析器设置”对其进行配置，并选择目标平台，即选择 .NET 平台/版本，用于评估与当前程序集构建的平台/版本相比的可移植性差距 。

![可移植性分析器的屏幕截图。](./media/portability-analyzer/portability-screenshot.png)

还可以使用 ApiPort 控制台应用程序，可从 [ApiPort 存储库](https://aka.ms/apiportdownload)进行下载。 可以使用 `listTargets` 命令选项以显示可用的目标列表，然后通过指定 `-t` 或 `--target` 命令选项来选择目标平台。

### <a name="solution-wide-view"></a>解决方案范围视图

分析包含多个项目的解决方案的一个很有用的步骤是，可视化依赖项以了解程序集中各个子集的依赖关系。 一般的建议是，从依赖项关系图中的叶节点开始，以自下而上的方式应用分析结果。

要检索此项，可运行以下命令：

```console
ApiPort.exe analyze -r DGML -f [directory or file]
```

在 Visual Studio 中打开时，此结果如下所示：

![DGML 分析的屏幕截图。](./media/portability-analyzer/dgml-example.png)

### <a name="analyze-portability"></a>分析可移植性

若要在 Visual Studio 中分析整个项目，请在“解决方案资源管理器”中右键单击该项目，然后选择“分析程序集可移植性” 。 也可以转到“分析”菜单，选择“分析程序集可移植性”。  在该位置选择项目的可执行文件或 DLL。

![解决方案资源管理器中的可移植性分析器的屏幕截图。](./media/portability-analyzer/portability-solution-explorer.png)

还可以使用 [ApiPort 控制台应用](https://aka.ms/apiportdownload)。

- 键入以下命令即可分析当前目录：`ApiPort.exe analyze -f .`
- 若要分析特定的 .dll 文件列表，请键入以下命令：`ApiPort.exe analyze -f first.dll -f second.dll -f third.dll`
- 运行 `ApiPort.exe -?` 以获取更多帮助

建议包含自己拥有的且要移植的所有相关 exe 和 dll 文件，并且排除应用所依赖的，但你既不拥有又无法移植的文件。 这将为你提供最相关的可移植性报表。

### <a name="view-and-interpret-portability-result"></a>查看和解释可移植性结果

报表中仅显示目标平台不支持的 API。
在 Visual Studio 中运行分析后，你将看到弹出的 .NET 可移植性报表文件链接。 如果使用的是 [ApiPort 控制台应用](https://aka.ms/apiportdownload)，.NET 可移植性报表将以指定的格式保存为文件。 默认位于当前目录中的 Excel 文件 (.xlsx) 中。

#### <a name="portability-summary"></a>可移植性摘要

![可移植性摘要的屏幕截图。](./media/portability-analyzer/api-catalog-portablility-summary.png)

报表的“可移植性摘要”部分显示运行中包含的每个程序集的可移植性百分比。 在上述示例中，`svcutil` 应用中使用的 71.24% 的 .NET Framework API 在 .NET Core + Platform Extensions 中可用。 如果针对多个程序集运行 .NET 可移植性分析器工具，则每个程序集在“可移植性摘要”报表中都应有一行。

#### <a name="details"></a>详细信息

![可移植性详细信息的屏幕截图。](./media/portability-analyzer/api-catalog-portablility-details.png)

报表的“详细信息”部分列出了任意选定目标平台缺少的 API。

- 目标类型：该类型具有目标平台缺少的 API
- 目标成员：目标平台缺少的方法
- 程序集名称：缺少的 API 所在的 .NET Framework 程序集。
- 每个选定的目标平台都是一列，例如“.NET Core”：“不支持”值表示此目标平台不支持 API。
- 建议的更改：要进行更改的推荐 API 或技术。 对于许多 API，此字段当前为空或已过时。 由于 API 数量众多，在维护 API 最新状态方面，我们面临着巨大的挑战。 我们致力于提供备用解决方案，以便为客户提供有用的信息。

#### <a name="missing-assemblies"></a>缺少程序集

![缺少的程序集的屏幕截图。](./media/portability-analyzer/api-catalog-missing-assemblies.png)

可以在报表中找到“缺少程序集”部分。 此部分包含由你的经过分析的程序集引用的程序集列表（此列表未经过分析）。 如果它是你自己拥有的程序集，请将其包含在 API 可移植性分析器运行过程中，以便你可以获得详细的 API 级别可移植性报表。 如果它是第三方库，请检查是否存在支持目标平台的更新版本，并考虑转到较新的版本。 最终，此列表应该包含你的应用依赖的所有第三方程序集（其中具有支持目标平台的版本）。

有关 .NET 可移植性分析器的详细信息，请访问 [GitHub 文档](https://github.com/Microsoft/dotnet-apiport#documentation)和[简要了解 .NET 可移植性分析器](https://channel9.msdn.com/Blogs/Seth-Juarez/A-Brief-Look-at-the-NET-Portability-Analyzer)第 9 频道视频。
