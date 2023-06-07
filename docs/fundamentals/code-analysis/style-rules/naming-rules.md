---
title: 代码样式命名规则
description: 了解用于命名代码元素的 .NET 代码样式规则。
ms.date: 09/25/2020
ms.topic: reference
author: gewarren
ms.author: gewarren
dev_langs:
- CSharp
- VB
f1_keywords:
- IDE1006
- naming rules
helpviewer_keywords:
- IDE1006
- naming code style rules [EditorConfig]
- naming rules
- EditorConfig naming conventions
ms.openlocfilehash: 8ce209e64ee7f9f9028c221daedef8fc6a993ef7
ms.sourcegitcommit: 721c3e4bdbb1ea0bb420818ec944c538fe5c513a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96591096"
---
# <a name="naming-rules"></a>命名规则

命名规则涉及 .NET 编程语言代码元素的命名，如类、属性和方法。 例如，可以指定公共成员必须采用大写形式，或者私有字段必须以 `_` 开头。

命名规则有三个部分：

* 它应用于的符号组。
* 要与规则关联的命名样式。
* 强制实施约定的严重性。

在 EditorConfig 文件中定义命名规则。

## <a name="general-syntax"></a>常规语法

若要定义命名规则、符号组或命名样式，请使用以下语法设置一个或多个属性：

```ini
<prefix>.<title>.<propertyName> = <propertyValue>
```

每个属性只能设置一次，但某些设置允许以逗号分隔的多个值。

属性的顺序并不重要。

### \<prefix>

**\<prefix>** 指定正在定义的元素类型（ &mdash; 命名规则、符号组或命名样式）， &mdash; 必须为下列类型之一：

| 设置属性 | 使用前缀 | 示例 |
| --- | --- | -- |
| 命名规则 | `dotnet_naming_rule` | `dotnet_naming_rule.types_should_be_pascal_case.severity = suggestion` |
| 符号组 | `dotnet_naming_symbols` | `dotnet_naming_symbols.interface.applicable_kinds = interface` |
| 命名样式 | `dotnet_naming_style` | `dotnet_naming_style.pascal_case.capitalization = pascal_case` |

每种类型的定义 &mdash; [命名规则](#naming-rule-properties)、[符号组](#symbol-group-properties)或[命名样式](#naming-style-properties) &mdash; 都具有其自己支持的属性，如以下各节所述。

### \<title>

**\<title>** 您选择的描述性名称，它将多个属性设置关联到单个定义。 例如，以下属性将生成两个符号组定义， `interface` `types` 其中每个定义都设置了两个属性。

```ini
dotnet_naming_symbols.interface.applicable_kinds = interface
dotnet_naming_symbols.interface.applicable_accessibilities = public, internal, private, protected, protected_internal, private_protected

dotnet_naming_symbols.types.applicable_kinds = class, struct, interface, enum, delegate
dotnet_naming_symbols.types.applicable_accessibilities = public, internal, private, protected, protected_internal, private_protected
```

## <a name="naming-rule-properties"></a>命名规则属性

所有命名规则属性都是规则生效的必需属性。

| Property | 描述 |
| -- | -- |
| `symbols` | 符号组的标题，定义应应用此规则的符号 |
| `style` | 应与此规则关联的命名样式的标题 |
| `severity` |  设置用于强制执行命名规则的严重性。 将关联的值设置为一个可用的 [严重性级别](https://docs.microsoft.com/dotnet/fundamentals/code-analysis/configuration-options#severity-level)。<sup>1</sup> |

**注意：**

1. 命名规则内的严重性规范仅适用于开发 Ide （如 Visual Studio）。 C # 或 VB 编译器不理解此设置，因此在生成过程中不会考虑此设置。 相反，若要在生成时强制命名样式规则，应使用 [本部分](#rule-id-ide1006-naming-rule-violation)中所述的基于规则 ID 的严重性配置来设置严重性。 有关详细信息，请参阅此 [GitHub 问题](https://github.com/dotnet/roslyn/issues/44201)。

## <a name="rule-order"></a>规则顺序

命名规则在 EditorConfig 文件中的定义顺序并不重要。 命名规则根据规则本身的定义自动排序。 [EditorConfig 语言服务扩展](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.EditorConfig)可以分析 EditorConfig 文件，如果文件中的规则顺序与编译器在运行时使用的规则不同，该扩展还会进行报告。

> [!NOTE]
>
> 如果使用早于 Visual Studio 2019 版本16.2 的 Visual Studio 版本，则应在 EditorConfig 文件中按从最特定到最小的顺序对命名规则进行排序。 遇到的第一个可应用规则是唯一应用的规则。 但是，如果有多个具有相同名称的规则属性  ，则最近找到的具有该名称的属性具有优先权。 有关详细信息，请参阅[文件层次结构和优先级](/visualstudio/ide/create-portable-custom-editor-options#file-hierarchy-and-precedence)。

## <a name="symbol-group-properties"></a>符号组属性

可以为符号组设置以下属性，以限制组中包含的符号。 若要在单个属性设置中指定多个值，请用逗号分隔它们。

| Property | 说明 | 允许的值 | 必选 |
| -- | -- | -- | -- |
| `applicable_kinds` | 组<sup>1</sup>中的符号类型 | `*`（使用此值可指定所有符号）<br/>`namespace`<br/>`class`<br/>`struct`<br/>`interface`<br/>`enum`<br/>`property`<br/>`method`<br/>`field`<br/>`event`<br/>`delegate`<br/>`parameter`<br/>`type_parameter`<br/>`local`<br/>`local_function` | 是 |
| `applicable_accessibilities` | 组中符号的可访问性级别 | `*`（使用此值可指定所有可访问性级别）<br/>`public`<br/>`internal` 或 `friend`<br/>`private`<br/>`protected`<br/>`protected_internal` 或 `protected_friend`<br/>`private_protected`<br/>`local` 方法中定义的符号 ()  | 是 |
| `required_modifiers` | 仅匹配具有 _所有_ 指定修饰符 <sup>2</sup>的符号 | `abstract` 或 `must_inherit`<br/>`async`<br/>`const`<br/>`readonly`<br/>`static` 或 `shared` <sup>3</sup> | 否 |

**注意：**

1. 当前不支持元组成员 `applicable_kinds` 。
2. 符号组与属性中的 _所有_ 修饰符匹配 `required_modifiers` 。  如果省略此属性，则匹配不需要任何特定的修饰符。 这意味着符号的修饰符不会影响是否应用此规则。
3. 如果组具有 `static` 或 `shared` 属性中的 `required_modifiers` ，则组还将包含 `const` 符号，因为它们是隐式的 `static` / `Shared` 。 但是，如果不希望 `static` 将命名规则应用于 `const` 符号，则可以使用符号组创建新的命名规则 `const` 。

## <a name="naming-style-properties"></a>命名样式属性

命名样式定义要通过规则强制执行的约定。 例如：

* 大写 `PascalCase`
* 开头为 `m_`
* 结尾为 `_g`
* 用词分隔 `__`

您可以为命名样式设置以下属性：

| Property | 说明 | 允许的值 | 必选 |
| -- | -- | -- | -- |
| `capitalization` | 符号内的单词的大小写样式 | `pascal_case`<br/>`camel_case`<br/>`first_word_upper`<br/>`all_upper`<br/>`all_lower` | 是<sup>1</sup> |
| `required_prefix` | 必须以这些字符开头 | | 否 |
| `required_suffix` | 必须以这些字符结尾 | | 否 |
| `word_separator` | 符号内的单词需要用此字符分隔 | | 否 |

**注意：**

1. 必须在命名样式中指定大写样式，否则会忽略命名样式。

## <a name="default-naming-styles"></a>默认命名样式

如果未指定任何自定义命名规则，则使用以下默认样式：

- 对于可访问性为 `public`、`private`、`internal`、`protected` 或 `protected_internal` 的类、结构、枚举、属性以及事件，默认的命名样式为帕斯卡拼写法。

- 对于可访问性为 `public`、`private`、`internal`、`protected` 或 `protected_internal` 的接口，默认的命名样式为帕斯卡拼写法并必须附加 l 前缀。 

## <a name="example"></a>示例

以下 .editorconfig 文件包含命名约定，该约定指定公共属性、方法、字段、事件和委托必须采用大写形式  。 请注意，此命名约定指定了多种应用规则的符号，以逗号分隔。

```ini
[*.{cs,vb}]

# Defining the 'public_symbols' symbol group
dotnet_naming_symbols.public_symbols.applicable_kinds           = property,method,field,event,delegate
dotnet_naming_symbols.public_symbols.applicable_accessibilities = public
dotnet_naming_symbols.public_symbols.required_modifiers         = readonly

# Defining the `first_word_upper_case_style` naming style
dotnet_naming_style.first_word_upper_case_style.capitalization = first_word_upper

# Defining the `public_members_must_be_capitalized` naming rule, by setting the symbol group to the 'public symbols' symbol group,
dotnet_naming_rule.public_members_must_be_capitalized.symbols   = public_symbols
# setting the naming style to the `first_word_upper_case_style` naming style,
dotnet_naming_rule.public_members_must_be_capitalized.style    = first_word_upper_case_style
# and setting the severity.
dotnet_naming_rule.public_members_must_be_capitalized.severity = suggestion
```

## <a name="rule-id-ide1006-naming-rule-violation"></a><a name="rule-id-ide1006-naming-rule-violation"></a>规则 ID： "IDE1006" (命名规则冲突) 

所有命名选项都有规则 ID `IDE1006` 和标题 `Naming rule violation` 。 你可以使用以下语法在 EditorConfig 文件中全局配置命名冲突的严重性：

```ini
dotnet_diagnostic.IDE1006.severity = <severity value>
```

严重性值必须是 `warning` 或 `error` 才能 [在生成时强制执行](../overview.md#code-style-analysis)。 对于所有可能的严重性值，请参阅 [严重性级别](../configuration-options.md#severity-level)。

## <a name="see-also"></a>另请参阅

- [语言规则](language-rules.md)
- [格式设置规则](formatting-rules.md)
- [Roslyn 命名规则](https://github.com/dotnet/roslyn/blob/master/.editorconfig#L63)
- [.NET 代码样式规则参考](index.md)
