---
title: 将库移植到 .NET Core
description: 了解如何将 .NET Framework 中的库项目移植到 .NET Core。
author: cartermp
ms.date: 12/07/2018
ms.openlocfilehash: 25c53cdc71f4269b058953ef1903dc72d222f65f
ms.sourcegitcommit: 635a0ff775d2447a81ef7233a599b8f88b162e5d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97633905"
---
# <a name="port-net-framework-libraries-to-net-core"></a>将 .NET Framework 库移植到 .NET Core

了解如何将 .NET Framework 库代码移植到 .NET Core，它在其中跨平台运行并扩展使用它的应用的范围。

## <a name="prerequisites"></a>先决条件

本文假定你：

- 正在使用 Visual Studio 2017 或更高版本。 （Visual Studio 的早期版本不支持 .NET Core。）
- 了解[推荐的移植过程](index.md)。
- 已解决与[第三方依赖项](third-party-deps.md)有关的任何问题。

还应熟悉下列文章的内容：

[.NET Standard](../../standard/net-standard.md)\
本文介绍了适用于所有 .NET 实现代码的 .NET API 正式规范。

[使用跨平台工具开发库](../tutorials/libraries.md)\
本文介绍如何使用 .NET Core CLI 编写库。

[.NET Core 的 csproj 格式的新增内容](../tools/csproj.md)\
本文概述了作为从移动到 csproj 和 MSBuild 的一部分，添加到项目文件的更改。

[移植到 .NET Core - 分析第三方依赖项](third-party-deps.md)\
本文介绍了第三方依赖项的可移植性及 NuGet 包依赖项无法在 .NET Core 上运行时要执行的操作。

## <a name="retarget-to-net-framework-472"></a>重定向到 .NET Framework 4.7.2

如果代码不面向 .NET Framework 4.7.2，建议重定向到 .NET Framework 4.7.2。 在 .NET Standard 不支持现有 API 情况下，这可确保最新备用 API 的可用性。

对于每个想要移植的项目，请在 Visual Studio 中执行以下操作：

1. 右键单击该项目，然后选择“属性”。
1. 在“目标框架”下拉列表中，选择“.NET Framework 4.7.2”。
1. 重新编译该项目。

因为项目现在面向 .NET Framework 4.7.2，因此可使用该版本的 .NET Framework 作为移植代码的基准。

## <a name="determine-portability"></a>确定可移植性

下一步是运行 API 可移植性分析器 (ApiPort) 生成可供分析的可移植性报表。

确保了解 [API 可移植性分析器 (ApiPort)](../../standard/analyzers/portability-analyzer.md) 及如何生成用于面向 .NET Core 的可移植性报表。 执行此操作的方式可能取决于需求和个人偏好。 以下部分详细介绍了几种不同的方法。 用户可能会发现自己根据生成代码的方式混合使用了这些方法中的步骤。

### <a name="deal-primarily-with-the-compiler"></a>主要处理编译器

此方法可能较适合小项目或不会用很多 .NET Framework API 的项目。 此方法很简单：

1. 可选择在项目上运行 ApiPort。 若运行 ApiPort，则从报告获取有关需要解决的问题的信息。
1. 将所有代码复制到新的 .NET Core 项目。
1. 查看可移植性报表（如果已生成）时，解决编译器错误，直至项目完全得到编译。

尽管它是非结构化的，但这种以代码为中心的方法通常可以快速解决问题。 只包含数据模型的项目可能是此方法的理想选择。

### <a name="stay-on-the-net-framework-until-portability-issues-are-resolved"></a>可移植性问题得到解决前停留在 .NET Framework 上

如果更希望拥有在整个过程期间编译的代码，此方法可能是最佳选择。 该方法如下所示：

1. 在项目上运行 ApiPort。
1. 通过使用可移植的不同 API 解决问题。
1. 记录阻止你使用直接替代方案的所有区域。
1. 对所有要移植的项目重复前面的步骤，直到确信每个项目都做好被复制到新的 .NET Core 项目中的准备。
1. 将代码复制到新的 .NET Core 项目。
1. 解决所有已记录的不存在直接替代方案的问题。

这种谨慎的方法比单纯解决编译器错误更有条理，但相对而言，它仍以代码为中心，且优点是始终拥有编译的代码。 解决不能通过只使用另一个 API 解决的某些问题的方法大不相同。 你可能会发现对于某些项目，需要制定更全面的计划，这将在下一种方法中介绍。

### <a name="develop-a-comprehensive-plan-of-attack"></a>制定全面的攻击计划

此方法可能最适合大型或更复杂的项目，在这种情况下，为支持 .NET Core，可能必需重构代码或将某些代码区域完全重写。 该方法如下所示：

1. 在项目上运行 ApiPort。
1. 了解每个非可移植类型使用的位置以及位置对整体可移植性的影响。
   - 了解这些类型的特性。 它们是否数量少，但使用频繁？ 它们是否数量大，但使用不频繁？ 它们是串联使用，还是在整个代码中传播？
   - 是否可以轻松隔离不可移植的代码，以便可以更有效地处理它？
   - 是否需要重构代码？
   - 对于这些不可移植的类型，是否存在可完成相同任务的备用 API？ 例如，如果使用 <xref:System.Net.WebClient> 类，也许能够改用 <xref:System.Net.Http.HttpClient> 类。
   - 是否存在其他可用于完成任务的可移植 API，即使它不是直接替代 API？ 例如，如果使用 <xref:System.Xml.Schema.XmlSchema> 来分析 XML，但是无需 XML 架构发现，则可使用 <xref:System.Xml.Linq> API 并自行实现分析，而不依赖于 API。
1. 如果具有难以移植的程序集，是否值得将其暂时留在 .NET Framework 上？ 以下是一些需要考虑的事项：
   - 库中可能具有某些与 .NET Core 不兼容的功能，因为它太依赖 .NET Framework 或 Windows 特定的功能。 是否值得暂时搁置该功能，并直到资源可用于移植这些功能，才发布具有较少功能的库的临时 .NET Core 版本？
   - 重构是否有用？
1. 编写自己对不可用 .NET Framework API 的实现是否合理？
   可以考虑复制、修改，并使用 [.NET Framework 参考源](https://github.com/Microsoft/referencesource)中的代码。 参考源代码已在 [MIT 许可证](https://github.com/Microsoft/referencesource/blob/master/LICENSE.txt)下获得许可，因此可以自由选择将此源作为自己代码的基础。 只需确保在代码中正确设置 Microsoft。
1. 根据不同项目的需要，重复此过程。

分析阶段可能需要一些时间，具体取决于代码库的大小。 在此阶段花费时间（尤其是在具有复杂的代码库时），全面了解所需的更改范围并制定计划，从长远看通常可节省时间。

计划可能包括对代码库做重大更改，同时面向 .NET Framework 4.7.2，使它成为前一种方法更有条理的版本。 着手执行计划的方式具体取决于代码库。

### <a name="mixed-approach"></a>混合方法

在每个项目的基础上，可能会将上述方法进行混合。 应该进行对你和代码库最有意义的操作。

## <a name="port-your-tests"></a>移植测试

要确保移植代码后一切正常的最佳方式是在将代码移植到 .NET Core 时进行测试。 为此，需要使用将针对 .NET Core 生成和运行测试的测试框架。 当前，有三个选择：

- [xUnit](https://xunit.net/)
  - [入门](https://xunit.net/docs/getting-started/netcore/cmdline)
  - [将 MSTest 项目转换为 xUnit 的工具](https://github.com/dotnet/codeformatter/tree/master/src/XUnitConverter)
- [NUnit](https://nunit.org/)
  - [入门](https://github.com/nunit/docs/wiki/Installation)
  - [关于从 MSTest 迁移到 NUnit 的博客文章](https://www.florian-rappl.de/News/Page/275/convert-mstest-to-nunit)
- [MSTest](/visualstudio/test/unit-test-basics)

## <a name="recommended-approach"></a>推荐的方法

从根本上讲，移植工作在很大程度上取决于生成 .NET Framework 代码的方式。 移植代码的一个好方法是从库的基项开始，这是代码的基础组件。 这可能是数据模型或某些其他内容直接或间接使用的基本类和方法。

1. 移植测试项目，该项目测试当前正在移植的库层。
1. 将库中的基项复制到新的 .NET Core 项目，然后选择想要支持的 .NET Standard 版本。
1. 进行任何所需的更改，使代码进行编译。 大部分内容可能会要求将 NuGet 包依赖项添加到 csproj 文件。
1. 运行测试并进行任何所需调整。
1. 选择下一层代码进行移植，并重复前面的步骤。

如果从库的基项开始并从基项向外移动并根据需要测试每一层，移植将是一个系统化的过程，在这种情况下，问题可以一次隔离到一层代码中。

## <a name="next-steps"></a>后续步骤

>[!div class="nextstepaction"]
>[整理 .NET Core 的项目](project-structure.md)
