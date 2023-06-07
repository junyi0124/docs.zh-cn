---
title: 代码分析规则的配置文件
description: 了解用于配置代码分析规则的不同配置文件。
ms.date: 09/24/2020
ms.topic: conceptual
no-loc:
- EditorConfig
ms.openlocfilehash: 0d64df42ffb1763afed3e883c4f043755e158489
ms.sourcegitcommit: 635a0ff775d2447a81ef7233a599b8f88b162e5d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97633983"
---
# <a name="configuration-files-for-code-analysis-rules"></a>代码分析规则的配置文件

代码分析规则具有各种 [配置选项](configuration-options.md)。 在以下任一分析器配置文件中，将这些选项指定为键值对：

- [EditorConfig](#editorconfig) 文件：基于文件或基于文件夹的配置选项。
- [全局 AnalyzerConfig](#global-analyzerconfig) 文件：项目级别配置选项。

## EditorConfig

[EditorConfig](/visualstudio/ide/create-portable-custom-editor-options) 文件用于提供适用于 **特定源文件或文件夹的选项**。 选项位于节标头下，用于标识适用的文件和文件夹。 为要配置的每个规则添加一个条目，并将其放置在相应的文件扩展名部分下，例如 `[*.cs]` 。

```ini
[*.cs]
<option_name> = <option_value>
```

在上面的示例中， `[*.cs]` 是一个 editorconfig 节标头，用于选择当前文件夹中具有文件扩展名的所有 c # 文件 `.cs` ，包括子文件夹。 接下来的条目 `<option_name> = <option_value>` 是一个分析器选项，它将应用于所有 c # 文件。

您可以 EditorConfig 通过将文件放在相应的目录中，将文件约定应用于文件夹、项目或整个存储库。 当在 Visual Studio 中执行分析时，以及在 Visual Studio 中编辑代码时，将应用这些选项。

如果有一个现有的 *editorconfig* 文件用于编辑器设置（如缩进大小或是否剪裁尾随空格），则可以将代码分析配置选项放在同一文件中。

> [!TIP]
> Visual Studio 提供了 *editorconfig* 项模板，可以轻松地将这些文件之一添加到你的项目中。 有关详细信息，请参阅 [将 EditorConfig 文件添加到项目](/visualstudio/ide/create-portable-custom-editor-options#add-an-editorconfig-file-to-a-project)。

### <a name="example"></a>示例

下面是一个示例 EditorConfig 文件，用于配置选项和规则严重性：

```ini
# Remove the line below if you want to inherit .editorconfig settings from higher directories
root = true

# C# files
[*.cs]

#### Core EditorConfig Options ####

# Indentation and spacing
indent_size = 4
indent_style = space
tab_width = 4

#### .NET Coding Conventions ####

# this. and Me. preferences
dotnet_style_qualification_for_method = true:warning

#### Diagnostic configuration ####

# CA1000: Do not declare static members on generic types
dotnet_diagnostic.CA1000.severity = warning
```

## <a name="global-analyzerconfig"></a>全局 AnalyzerConfig

从 .NET 5 SDK 开始 (在 Visual Studio 2019 版本16.8 及更高版本) 中受支持，你还可以配置包含全局 _AnalyzerConfig_ 文件的分析器选项。 这些文件用于提供 **应用于项目中所有源文件的选项**，而不考虑它们的文件名或文件路径。

与 [EditorConfig](#editorconfig) 文件不同，全局配置文件不能用于为 ide 配置编辑器样式设置，如缩进大小或是否剪裁尾随空格。 相反，它们专用于指定项目级分析器配置选项。

### <a name="format"></a>格式

与 EditorConfig 必须包含节标头（例如）的文件不同， `[*.cs]` 全局 AnalyzerConfig 文件没有节标头。 相反，它们需要窗体的顶级条目 `is_global = true` ，以便将它们与常规文件区分开来 EditorConfig 。 这表示文件中的所有选项都适用于整个项目。 例如：

```ini
is_global = true
<option_name> = <option_value>
```

### <a name="naming"></a>命名

与 EditorConfig 必须命名的文件不同， `.editorconfig` 全局配置文件不需要具有特定的名称或扩展名。 但是，如果将这些文件命名为，则这些文件 `.globalconfig` 将隐式应用于当前文件夹中的所有 c # 和 Visual Basic 项目，包括子文件夹。 否则，你必须将该项显式添加 `GlobalAnalyzerConfigFiles` 到 MSBuild 项目文件中：

```xml
<ItemGroup>
  <GlobalAnalyzerConfigFiles Include="<path_to_global_analyzer_config>" />
</ItemGroup>
```

> [!NOTE]
> `is_global = true`即使文件命名为，也需要顶级条目 `.globalconfig` 。

### <a name="example"></a>示例

下面是一个示例全局 AnalyzerConfig 文件，用于在项目级别配置选项和规则严重性：

```ini
# Top level entry required to mark this as a global AnalyzerConfig file
is_global = true

# NOTE: No section headers for configuration entries

#### .NET Coding Conventions ####

# this. and Me. preferences
dotnet_style_qualification_for_method = true:warning

#### Diagnostic configuration ####

# CA1000: Do not declare static members on generic types
dotnet_diagnostic.CA1000.severity = warning
```

## <a name="precedence"></a>优先级

EditorConfig文件和全局 AnalyzerConfig 文件都为每个选项指定键值对。 如果有多个条目具有相同键但值不同，则会发生冲突。

### <a name="general-options"></a>常规选项

当选项之间发生冲突时，将使用以下优先规则来解决冲突：

- 相同配置文件中的条目冲突：在文件中后面显示的条目入选。 这适用于单个 EditorConfig 文件中和单个全局 AnalyzerConfig 文件中的冲突项。

- 两个文件中的冲突条目 EditorConfig ：文件中在文件系统中的条目 EditorConfig ，因此文件路径较长，wins。

- 两个全局 AnalyzerConfig 文件中的冲突条目：报告编译器警告，并忽略这两个条目。

- EditorConfig文件和全局 AnalyzerConfig 文件中的条目冲突：文件中的条目 EditorConfig 入选。

### <a name="severity-options"></a>严重性选项

上述 [常规优先规则](#general-options) 适用于在配置文件中指定的所有选项。 对于 [严重级别配置](configuration-options.md#severity-level) 选项，以下附加优先规则适用：

- 在命令行上作为编译器选项指定的严重性选项 (`/nowarn` 或 `/warnaserror`) 始终重写在和全局 AnalyzerConfig 文件中指定的 [严重级别配置](configuration-options.md#severity-level) 选项 EditorConfig 。

- 还可以使用 [规则集](/visualstudio/code-quality/using-rule-sets-to-group-code-analysis-rules) 文件指定严重性选项。 但是，规则集文件已被弃用，以支持 EditorConfig 和全局 AnalyzerConfig 文件。 建议 [将规则集文件转换为等效的 EditorConfig 文件](/visualstudio/code-quality/use-roslyn-analyzers#convert-an-existing-ruleset-file-to-editorconfig-file)。 规则集文件和或全局 AnalyzerConfig 文件中的冲突严重性条目的优先级未 EditorConfig _定义_。

- 有关具有不同键的相关严重性选项的优先规则的信息（例如，为单个规则指定了不同的严重性，并为规则所属的类别指定了不同的严重性），请参阅 [代码分析的配置选项](configuration-options.md#precedence)。

## <a name="see-also"></a>另请参阅

- [EditorConfig vs global AnalyzerConfig 设计问题](https://github.com/dotnet/roslyn/issues/47707)
