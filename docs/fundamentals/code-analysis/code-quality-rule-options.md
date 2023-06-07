---
title: 代码质量规则配置选项
description: 了解如何为代码质量规则指定其他配置选项。
ms.date: 09/24/2020
ms.topic: conceptual
no-loc:
- EditorConfig
ms.openlocfilehash: af2984e73c554e8a1e1b32df9460933f86cc41be
ms.sourcegitcommit: 2e4adc490c1d2a705a0592b295d606b10b9f51f1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2020
ms.locfileid: "96590227"
---
# <a name="code-quality-rule-configuration-options"></a>代码质量规则配置选项

除了 [配置其严重性](configuration-options.md#severity-level)外，*代码质量* 规则还有其他配置选项。 例如，可以将每个代码质量分析器配置为仅应用于代码库的特定部分。 您可以通过将键值对添加到 [EditorConfig](https://editorconfig.org) 指定规则严重性和一般编辑器首选项的同一文件中来指定这些选项。

## <a name="option-scopes"></a>选项作用域

每个优化选项可针对所有规则进行配置，适用于一类规则 (例如，安全或设计) 或特定规则。

### <a name="all-rules"></a>┮Τ砏玥

为 *所有* 规则配置选项的语法如下所示：

|语法|示例|
|-|-|
| dotnet_code_quality。OptionName = OptionValue | `dotnet_code_quality.api_surface = public` |

### <a name="category-of-rules"></a>规则类别

为规则 *类别* 配置选项的语法 (如命名、设计或性能) ，如下所示：

|语法|示例|
|-|-|
| dotnet_code_quality。RuleCategory. OptionName = OptionValue | `dotnet_code_quality.Naming.api_surface = public` |

### <a name="specific-rule"></a>特定规则

为 *特定* 规则配置选项的语法如下所示：

|语法|示例|
|-|-|
| dotnet_code_quality。RuleId. OptionName = OptionValue | `dotnet_code_quality.CA1040.api_surface = public` |

## <a name="options"></a>选项

本节列出了一些可用选项。 若要查看可用选项的完整列表，请参阅 [Analyzer 配置](https://github.com/dotnet/roslyn-analyzers/blob/master/docs/Analyzer%20Configuration.md)。

### <a name="api_surface"></a>api_surface

| 描述 | 允许的值 | 默认值 | 可配置规则 |
| - | - | - | - |
| 要分析的 API 面部分 | `public`<br/>`internal` 或 `friend`<br/>`private`<br/>`all`<br/><br/>使用逗号 ( 分隔多个值，)  | `public` | [CA1000](quality-rules/ca1000.md) [CA1003](quality-rules/ca1003.md) [CA1008](quality-rules/ca1008.md) [CA1010](quality-rules/ca1010.md)<br/>[CA1012](quality-rules/ca1012.md) [CA1024](quality-rules/ca1024.md) [CA1027](quality-rules/ca1027.md) [CA1028](quality-rules/ca1028.md)<br/>[CA1030](quality-rules/ca1030.md) [CA1036](quality-rules/ca1036.md) [CA1040](quality-rules/ca1040.md) [CA1041](quality-rules/ca1041.md)<br/>[CA1043](quality-rules/ca1043.md) [CA1044](quality-rules/ca1044.md) [CA1051](quality-rules/ca1051.md) [CA1052](quality-rules/ca1052.md)<br/>[CA1054](quality-rules/ca1054.md) [CA1055](quality-rules/ca1055.md) [CA1056](quality-rules/ca1056.md) [CA1058](quality-rules/ca1058.md)<br/>[CA1063](quality-rules/ca1063.md) [CA1708](quality-rules/ca1708.md) [CA1710](quality-rules/ca1710.md) [CA1711](quality-rules/ca1711.md)<br/>[CA1714](quality-rules/ca1714.md) [CA1715](quality-rules/ca1715.md) [CA1716](quality-rules/ca1716.md) [CA1717](quality-rules/ca1717.md)<br/>[CA1720](quality-rules/ca1720.md) [CA1721](quality-rules/ca1721.md) [CA1725](quality-rules/ca1725.md) [CA1801](quality-rules/ca1801.md)<br/>[CA1802](quality-rules/ca1802.md) [CA1815](quality-rules/ca1815.md) [CA1819](quality-rules/ca1819.md) [CA2217](quality-rules/ca2217.md)<br/>[CA2225](quality-rules/ca2225.md) [CA2226](quality-rules/ca2226.md) [CA2231](quality-rules/ca2231.md) [CA2234](quality-rules/ca2234.md)<br/>|

### <a name="exclude_async_void_methods"></a>exclude_async_void_methods

| 描述 | 允许的值 | 默认值 | 可配置规则 |
| - | - | - | - |
| 是否忽略未返回值的异步方法 | `true`<br/>`false` | `false` | [CA2007](quality-rules/ca2007.md) |

> [!NOTE]
> 此选项 `skip_async_void_methods` 在早期版本中命名为。

### <a name="exclude_single_letter_type_parameters"></a>exclude_single_letter_type_parameters

| 描述 | 允许的值 | 默认值 | 可配置规则 |
| - | - | - | - |
| 是否从规则中排除单字符 [类型参数](../../csharp/programming-guide/generics/generic-type-parameters.md) ， `S` 例如， `Collection<S>` | `true`<br/>`false` | `false` | [CA1715](quality-rules/ca1715.md) |

> [!NOTE]
> 此选项 `allow_single_letter_type_parameters` 在早期版本中命名为。

### <a name="output_kind"></a>output_kind

| 描述 | 允许的值 | 默认值 | 可配置规则 |
| - | - | - | - |
| 指定应分析生成此类程序集的项目中的代码 | 枚举的一个或多个字段 <xref:Microsoft.CodeAnalysis.OutputKind><br/><br/>使用逗号 ( 分隔多个值，)  | 所有输出类型 | [CA2007](quality-rules/ca2007.md) |

### <a name="required_modifiers"></a>required_modifiers

| 描述 | 允许的值 | 默认值 | 可配置规则 |
| - | - | - | - |
| 为应分析的 Api 指定必需的修饰符 | 以下允许的修饰符表中的一个或多个值<br/><br/>使用逗号 ( 分隔多个值，)  | 取决于每个规则 | [CA1802](quality-rules/ca1802.md) |

| 允许的修饰符 | 总结 |
| --- | --- |
| `none` | 无修饰符要求 |
| `static` 或 `Shared` | 必须声明为 `static` `Shared` Visual Basic 中的 ()  |
| `const` | 必须声明为 `const` |
| `readonly` | 必须声明为 `readonly` |
| `abstract` | 必须声明为 `abstract` |
| `virtual` | 必须声明为 `virtual` |
| `override` | 必须声明为 `override` |
| `sealed` | 必须声明为 `sealed` |
| `extern` | 必须声明为 `extern` |
| `async` | 必须声明为 `async` |

### <a name="exclude_extension_method_this_parameter"></a>exclude_extension_method_this_parameter

| 描述 | 允许的值 | 默认值 | 可配置规则 |
| - | - | - | - |
| 是否跳过 `this` 扩展方法的参数分析 | `true`<br/>`false` | `false` | [CA1062](quality-rules/ca1062.md) |

### <a name="null_check_validation_methods"></a>null_check_validation_methods

| 描述 | 允许的值 | 默认值 | 可配置规则 |
| - | - | - | - |
| 验证传递给方法的参数不为 null 的 null 检查验证方法的名称 | 允许的方法名称格式 (用 `|`) 分隔：<br/> -方法名称仅 (包含名称的所有方法，而不考虑包含类型或命名空间) <br/> -符号的 [文档 ID 格式](https://github.com/dotnet/csharplang/blob/master/spec/documentation-comments.md#id-string-format)的完全限定名称，带有可选 `M:` 前缀 | 无 | [CA1062](quality-rules/ca1062.md) |

### <a name="additional_string_formatting_methods"></a>additional_string_formatting_methods

| 描述 | 允许的值 | 默认值 | 可配置规则 |
| - | - | - | - |
| 其他字符串格式设置方法的名称 | 允许的方法名称格式 (用 `|`) 分隔：<br/> -方法名称仅 (包含名称的所有方法，而不考虑包含类型或命名空间) <br/> -符号的 [文档 ID 格式](/dotnet/csharp/language-reference/language-specification/documentation-comments#id-string-format)的完全限定名称，带有可选 `M:` 前缀 | 无 | [CA2241](quality-rules/ca2241.md) |

### <a name="excluded_type_names_with_derived_types"></a>excluded_type_names_with_derived_types

| 描述 | 允许的值 | 默认值 | 可配置规则 |
| - | - | - | - |
| 类型的名称，因此不包括类型及其所有派生类型进行分析 | 允许的符号名称格式 (由 `|`) 分隔：<br/> -Type name 仅 (包含名称的所有类型，而不考虑包含类型或命名空间) <br/> -符号的 [文档 ID 格式](/dotnet/csharp/language-reference/language-specification/documentation-comments#id-string-format)的完全限定名称，带有可选 `T:` 前缀 | 无 | [CA1303](quality-rules/ca1303.md) |

### <a name="excluded_symbol_names"></a>excluded_symbol_names

| 描述 | 允许的值 | 默认值 | 可配置规则 |
| - | - | - | - |
| 排除用于分析的符号的名称 | 允许的符号名称格式 (由 `|`) 分隔：<br/> -仅限符号名称 (包含名称的所有符号，而不考虑包含类型或命名空间) <br/> -符号的 [文档 ID 格式](/dotnet/csharp/language-reference/language-specification/documentation-comments#id-string-format)的完全限定名称。 每个符号名称都需要符号类型前缀，如 `M:` 方法的前缀、 `T:` 类型的前缀和 `N:` 命名空间的前缀。<br/> - `.ctor` 对于构造函数和 `.cctor` 静态构造函数 | 无 | [CA1062](quality-rules/ca1062.md) [CA1303](quality-rules/ca1303.md) [CA2000](quality-rules/ca2000.md) [CA2100](quality-rules/ca2100.md) [CA2301](quality-rules/ca2301.md) [CA2302](quality-rules/ca2302.md)<br/>[CA2311](quality-rules/ca2311.md) [CA2312](quality-rules/ca2312.md) [CA2321](quality-rules/ca2321.md) [CA2322](quality-rules/ca2322.md) [CA2327](quality-rules/ca2327.md) [CA2328](quality-rules/ca2328.md)<br/>[CA2329](quality-rules/ca2329.md) [CA2330](quality-rules/ca2330.md) [CA3001](quality-rules/ca3001.md) [CA3002](quality-rules/ca3002.md) [CA3003](quality-rules/ca3003.md) [CA3004](quality-rules/ca3004.md)<br/>[CA3005](quality-rules/ca3005.md) [CA3006](quality-rules/ca3006.md) [CA3007](quality-rules/ca3007.md) [CA3008](quality-rules/ca3008.md) [CA3009](quality-rules/ca3009.md) [CA3010](quality-rules/ca3010.md)<br/>[CA3011](quality-rules/ca3011.md) [CA3012](quality-rules/ca3012.md) [CA5361](quality-rules/ca5361.md) CA5376 CA5377 [CA5378](quality-rules/ca5378.md)<br/>[CA5380](quality-rules/ca5380.md) [CA5381](quality-rules/ca5381.md) CA5382 CA5383 CA5384 CA5387<br/>CA5388 [CA5389](quality-rules/ca5389.md) CA5390 |

### <a name="disallowed_symbol_names"></a>disallowed_symbol_names

| 描述 | 允许的值 | 默认值 | 可配置规则 |
| - | - | - | - |
| 分析上下文中不允许的符号的名称 | 允许的符号名称格式 (由 `|`) 分隔：<br/> -仅限符号名称 (包含名称的所有符号，而不考虑包含类型或命名空间) <br/> -符号的 [文档 ID 格式](/dotnet/csharp/language-reference/language-specification/documentation-comments#id-string-format)的完全限定名称。 每个符号名称都需要符号类型前缀，如 `M:` 方法的前缀、 `T:` 类型的前缀和 `N:` 命名空间的前缀。<br/> - `.ctor` 对于构造函数和 `.cctor` 静态构造函数 | 无 | [CA1031](quality-rules/ca1031.md) |
