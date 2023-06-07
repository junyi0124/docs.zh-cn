---
title: 配置代码分析规则
description: 了解如何在分析器配置文件中配置代码分析规则。
ms.date: 09/24/2020
ms.topic: conceptual
no-loc:
- EditorConfig
ms.openlocfilehash: 4f7b392a2b066023fec75c5295bd94651654d645
ms.sourcegitcommit: 45c7148f2483db2501c1aa696ab6ed2ed8cb71b2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/08/2020
ms.locfileid: "96851785"
---
# <a name="configuration-options-for-code-analysis"></a>代码分析的配置选项

代码分析规则具有各种配置选项。 这些选项在 [分析器配置文件](configuration-files.md) 中使用语法指定为键值对 `<option key> = <option value>` 。

您要配置的最常见的选项是 [规则的严重性](#severity-level)。 您可以为所有分析器规则（包括 [代码质量规则](quality-rules/index.md) 和 [代码样式规则](style-rules/index.md)）配置严重级别。 例如，若要启用某个规则作为警告，可以将以下键值对添加到 EditorConfig 文件中。

`dotnet_diagnostic.<rule ID>.severity = warning`

你还可以配置其他选项以自定义规则行为：

- 代码质量规则具有 [其他](code-quality-rule-options.md) 用于配置行为的选项，例如，要将规则应用到的方法的名称。
- 代码样式规则具有 [自定义代码样式选项](code-style-rule-options.md)。
- 第三方分析器规则可以定义自己的配置选项，其中包含自定义的键名和值格式。

在 [分析器配置文件](configuration-files.md) 中配置特定规则严重性的语法如下所示：

```ini
dotnet_diagnostic.<rule ID>.severity = <severity>
```

## <a name="general-options"></a>常规选项

这些选项适用于整个代码分析。 它们不能仅应用于一个或一组规则。

### <a name="exclude-generated-code"></a>排除生成的代码

你可以通过将 `generated_code = true | false` 条目添加到 [配置文件](configuration-files.md)，将其他文件和文件夹作为生成的代码进行配置。 .NET 代码分析器警告在生成的代码文件（如设计器生成的文件）上不有用，用户无法编辑这些文件来修复任何冲突。 在大多数情况下，代码分析器会跳过生成的代码文件，而不报告这些文件的冲突。

默认情况下，具有特定文件扩展名或自动生成的文件标头的文件被视为生成的代码文件。 例如，以或结尾的文件名被 `.designer.cs` `.generated.cs` 视为生成的代码。 此配置选项允许您指定其他命名模式。

例如，若要将名称以结尾的所有文件视为 `.MyGenerated.cs` 生成的代码，请添加以下条目：

```ini
[*.MyGenerated.cs]
generated_code = true
```

## <a name="rule-specific-options"></a>特定于规则的选项

特定于规则的选项可应用于单个规则、一组规则或所有规则。 特定于规则的选项包括：

- [规则严重性级别](#severity-level)
- [特定于 *代码质量规则的* 选项](code-quality-rule-options.md)

### <a name="severity-level"></a>严重性级别

下表显示了可为所有分析器规则（包括 [代码质量](quality-rules/index.md) 和 [代码样式](style-rules/index.md) 规则）配置的不同规则严重性。

| 严重性 | 生成时行为 |
|-|-|
| `error` | 冲突显示为生成 *错误* 并导致生成失败。|
| `warning` | 冲突显示为生成 *警告* ，但不会导致生成失败 (除非您将选项设置为 "将警告视为错误") 。 |
| `suggestion` | 冲突显示为生成 *消息* ，在 VISUAL Studio IDE 中显示为建议。 |
| `silent` | 冲突对用户不可见。 |
| `none` | 完全禁止显示规则。 |
| `default` | 使用规则的默认严重性。 |

> [!TIP]
> 有关 Visual Studio 中规则严重性的详细信息，请参阅 [严重性级别](/visualstudio/ide/editorconfig-language-conventions#severity-levels)。

#### <a name="scope"></a>范围

若要设置单个规则的规则严重性，请使用以下语法。

```ini
dotnet_diagnostic.<rule ID>.severity = <severity value>
```

若要为某一类别的分析器规则设置默认规则严重性，请使用以下语法。

```ini
dotnet_analyzer_diagnostic.category-<rule category>.severity = <severity value>
```

若要为所有分析器规则设置默认规则严重性，请使用以下语法。

```ini
dotnet_analyzer_diagnostic.severity = <severity value>
```

#### <a name="precedence"></a>优先级

如果有多个可应用于同一规则 ID 的严重性配置项，将按以下顺序选择优先级：

- 单个规则按 ID 列出的条目优先于类别的条目。
- 类别的条目优先于所有分析器规则条目。

请考虑以下示例，其中 [CA1822](/visualstudio/code-quality/ca1822) 的类别为 "Performance"：

```ini
[*.cs]
dotnet_diagnostic.CA1822.severity = error
dotnet_analyzer_diagnostic.category-performance.severity = warning
dotnet_analyzer_diagnostic.severity = suggestion
```

在前面的示例中，所有三个严重性条目都适用于 CA1822。 但是，使用指定的优先规则，基于第一个规则 ID 的条目将在下一条目上入选。 在此示例中，CA1822 将具有有效的严重性 `error` 。 "性能" 类别中的所有其他规则都具有严重性 `warning` 。

有关如何确定文件间优先级的信息，请参阅 [配置文件的优先级部分](configuration-files.md#precedence)。
