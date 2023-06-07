---
title: 引用程序集
description: 了解引用程序集，这是 .NET 中一种特殊类型的程序集，它只包含库的公共 API 外围应用
author: MSDN-WhiteKnight
ms.date: 09/12/2019
ms.openlocfilehash: 2f7f026c7fca4b772be85671dcc3a2a6d50a385c
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94831047"
---
# <a name="reference-assemblies"></a>引用程序集

引用程序集  是一种特殊类型的程序集，它只包含表示库的公共 API 外围应用所需的最少元数据量。 它们包括在生成工具中引用程序集时所需的所有成员的声明，但不包括所有成员实现以及对其 API 协定没有明显影响的私有成员的声明。 相比较下，常规程序集称为“实现程序集”  。

无法加载引用程序集用于执行，但可以将它们作为编译器输入进行传递，其传递方式与实现程序集相同。 引用程序集通常随特定平台或库的软件开发工具包 (SDK) 一起分发。

使用引用程序集，开发人员可以生成面向特定库版本的程序，而无需具有该版本的完整实现程序集。 假设计算机上只有某个库的最新版本，但你想要生成一个面向该库的早期版本的程序。 如果直接针对实现程序集进行编译，可能会在无意中使用在早期版本中不可用的 API 成员。 只有在目标计算机上测试程序时才会发现这种错误。 如果针对早期版本的引用程序集进行编译，则会立即出现编译时错误。

此外，引用程序集还可以表示协定，即一组不与具体实现程序集对应的 API。 此类引用程序集称为“协定程序集”  ，可用于面向支持同一组 API 的多个平台。 例如，.NET Standard 提供协定程序集 netstandard .dll  ，它表示在不同的 .NET 平台之间共享的一组公共 API。 这些 API 的实现包含在不同平台上的不同程序集中，例如 .NET Framework 上的 mscorlib.dll 或 .NET Core 上的 System.Private.CoreLib.dll   。 面向 .NET Standard 的库可以在支持 .NET Standard 的所有平台上运行。

## <a name="using-reference-assemblies"></a>使用引用程序集

若要使用项目中的某些 API，必须添加对其程序集的引用。 可以将引用添加到实现程序集，也可以将其添加到引用程序集。 建议在引用程序集可用时使用它。 这样做可确保仅使用目标版本中受支持的 API 成员，即供 API 设计人员使用。 使用引用程序集可确保不依赖于实现详细信息。

.NET Framework 库的引用程序集与目标包一起分发。 可以通过下载独立安装程序或在 Visual Studio 安装程序中选择组件来获取它们。 有关详细信息，请参阅[安装面向开发人员的 .NET Framework](../../framework/install/guide-for-developers.md)。 对于 .NET Core 和 .NET Standard，引用程序集将在必要时（通过 NuGet）进行自动下载和引用。 对于 .NET Core 3.0 和更高版本，核心框架的引用程序集位于 [Microsoft.NETCore.App.Ref](https://www.nuget.org/packages/Microsoft.NETCore.App.Ref) 包中（使用 [Microsoft.NETCore.App](https://www.nuget.org/packages/Microsoft.NETCore.App) 包代替 3.0 之前的版本）。

使用“添加引用”对话框在 Visual Studio 中添加对 .NET Framework 程序集的引用时，可以从列表中选择一个程序集，Visual Studio 会自动查找对应于项目中选择的目标框架版本的引用程序集。 这同样适用于使用 [Reference](/visualstudio/msbuild/common-msbuild-project-items#reference) 项目项直接在 MSBuild 项目中添加引用的情形：只需指定程序集名称，无需指定完整的文件路径。 使用 `-reference` 编译器选项（在 [C#](../../csharp/language-reference/compiler-options/reference-compiler-option.md) 和 [Visual Basic](../../visual-basic/reference/command-line-compiler/reference.md) 中），或者使用 Roslyn API 中的 <xref:Microsoft.CodeAnalysis.Compilation.AddReferences%2A?displayProperty=nameWithType> 方法在命令行中添加对这些程序集的引用时，必须为正确的目标平台版本手动指定引用程序集文件。 .NET Framework 引用程序集文件位于 %ProgramFiles(x86)%\\Reference Assemblies\\Microsoft\\Framework\\.NETFramework 目录中。 对于 .NET Core，可以通过将 `PreserveCompilationContext` 项目属性设置为 `true` 强制发布操作，以便将目标平台的引用程序集复制到输出目录的 publish/refs 子目录。 然后，可以将这些引用程序集文件传递给编译器。 使用 [Microsoft.Extensions.DependencyModel](https://www.nuget.org/packages/Microsoft.Extensions.DependencyModel/) 包中的 `DependencyContext` 有助于找到其路径。

由于它们不包含任何实现，因此无法加载引用程序集用于执行。 如果尝试这样做，则会导致 <xref:System.BadImageFormatException?displayProperty=nameWithType>。 如果要检查引用程序集的内容，你可将其加载到 .NET Framework 中的仅反射上下文中（使用 <xref:System.Reflection.Assembly.ReflectionOnlyLoad%2A?displayProperty=nameWithType> 方法），或者加载到 .NET Core 中的 <xref:System.Reflection.MetadataLoadContext>。

## <a name="generating-reference-assemblies"></a>生成引用程序集

当库使用者需要针对多个不同版本的库生成程序时，为库生成引用程序集会很有用。 对于所有这些版本，分发实现程序集可能不切实际，因为它的大小太大。 引用程序集的大小较小，将它们作为库 SDK 的一部分进行分发可减少下载大小并节省磁盘空间。

IDE 和生成工具还可以利用引用程序集来减少由多个类库组成的大型解决方案的生成时间。 通常，在增量生成方案中，当某个项目的任何输入文件发生更改时，将重新生成该项目（包括它所依赖的程序集）。 每当程序员更改任何成员的实现时，实现程序集就会发生更改。 仅当引用程序集的公共 API 受到影响时，它才会发生更改。 因此，将引用程序集用作输入文件而不是实现程序集，可以在某些情况下跳过生成依赖项目的步骤。

可以通过以下方式生成引用程序集：

- 在 MSBuild 项目中，通过使用 [`ProduceReferenceAssembly` 项目属性](/visualstudio/msbuild/common-msbuild-project-properties)生成。
- 从命令行编译程序时，方法是指定 `-refonly` ([C#](../../csharp/language-reference/compiler-options/refonly-compiler-option.md) / [Visual Basic](../../visual-basic/reference/command-line-compiler/refonly-compiler-option.md)) 和 `-refout` ([C#](../../csharp/language-reference/compiler-options/refout-compiler-option.md) / [Visual Basic](../../visual-basic/reference/command-line-compiler/refout-compiler-option.md)) 编译器选项。
- 使用 Roslyn API 时，通过在传递到 <xref:Microsoft.CodeAnalysis.Compilation.Emit%2A?displayProperty=nameWithType> 方法的对象中将 <xref:Microsoft.CodeAnalysis.Emit.EmitOptions.EmitMetadataOnly?displayProperty=nameWithType> 设置为 `true`，并将 <xref:Microsoft.CodeAnalysis.Emit.EmitOptions.IncludePrivateMembers?displayProperty=nameWithType> 设置为 `false` 来生成。

如果要将引用程序集与 NuGet 包一起分发，必须将它们包含在包目录下的 ref\\ 子目录中（而不是用于实现程序集的 lib\\ 子目录中）。

## <a name="reference-assemblies-structure"></a>引用程序集结构

引用程序集是对相关概念“仅元数据程序集”的扩展。 仅包含元数据的程序集会将方法主体替换为一个 `throw null` 主体，但包括除匿名类型以外的所有成员。 使用 `throw null` 主体（而非不使用主体）的原因在于，这样做可以运行和传递 PEVerify（从而验证元数据的完整性）。

引用程序集进一步从仅包含元数据的程序集中删除元数据（私有成员）：

- 引用程序集只包含在 API 外围应用中所需的引用。 实际程序集可能包含与特定实现相关的其他引用。 例如，`class C { private void M() { dynamic d = 1; ... } }` 的引用程序集不引用 `dynamic` 所需的任何类型。
- 删除私有函数成员（方法、属性和事件），前提是这不会对编译造成显著影响。 如果没有 [InternalsVisibleTo ](xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute) 属性，则会同时删除内部函数成员。

引用程序集中的元数据继续保留以下信息：

- 所有类型，包括专用类型和嵌套类型。
- 所有属性（甚至是内部属性）。
- 所有虚拟方法。
- 显式接口实现。
- 显式实现的属性和事件，因为它们的访问器是虚拟的。
- 结构的所有字段。

引用程序集包括程序集级 [ReferenceAssembly](xref:System.Runtime.CompilerServices.ReferenceAssemblyAttribute) 属性。 可以在源中指定此属性；之后编译器就不需要进行合成。 由于有此属性，运行时会拒绝加载用于执行的引用程序集（但可在仅限反射的模式下加载）。

具体引用程序集结构详细信息取决于编译器版本。 如果确定较新的版本不影响公共 API 外围应用，则可以选择在较新版本中排除更多元数据。

> [!NOTE]
> 本节中的信息仅适用于从 C# 7.1 版或 Visual Basic 15.3 版开始由 Roslyn 编译器生成的引用程序集。 .NET Framework 和 .NET Core 库的引用程序集结构在某些细节上可能有所不同，因为它们使用各自生成引用程序集的机制。 例如，它们可能具有完全为空的方法主体，而不是使用 `throw null` 正文。 但一般原则仍适用：它们没有可用的方法实现，并且仅包含从公共 API 角度具有明显影响的成员的元数据。

## <a name="see-also"></a>请参阅

- [.NET 中的程序集](index.md)
- [框架定位概述](/visualstudio/ide/visual-studio-multi-targeting-overview)
- [如何：使用引用管理器添加或删除引用](/visualstudio/ide/how-to-add-or-remove-references-by-using-the-reference-manager)
