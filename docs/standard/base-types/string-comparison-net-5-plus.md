---
title: 在 .NET 5 及更高版本中比较字符串时的行为更改
description: 了解在 Windows 上的 .NET 5 及更高版本中比较字符串时的行为更改。
ms.date: 12/07/2020
ms.openlocfilehash: a53c36b31785fb43c0aa5f5040042abb6d40031a
ms.sourcegitcommit: 45c7148f2483db2501c1aa696ab6ed2ed8cb71b2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/08/2020
ms.locfileid: "96851746"
---
# <a name="behavior-changes-when-comparing-strings-on-net-5"></a>在 .NET 5 及更高版本中比较字符串时的行为更改

.NET 5.0 引入了一项运行时行为更改，其中，全球化 API 目前在所有支持的平台上[默认使用 ICU](../../core/compatibility/globalization/5.0/icu-globalization-api.md)。 这明显有别于较早的 .NET Core 和 .NET Framework 版本，在 Windows 上运行这些版本时，它们利用操作系统的区域语言支持 (NLS) 功能。 有关这些更改的详细信息，包括还原该行为更改的兼容性开关，请参阅 [.NET 全球化和 ICU](../globalization-localization/globalization-icu.md)。

## <a name="reason-for-change"></a>更改原因

引入此更改是为了统一所有支持的操作系统上的 .NET 的全球化行为。 它还能让应用程序捆绑自己的全球化库，而不是依赖于操作系统的内置库。 有关详细信息，请参阅[中断性变更](../../core/compatibility/globalization/5.0/icu-globalization-api.md)。

## <a name="behavioral-differences"></a>行为差异

如果在使用 `string.IndexOf(string)` 这样的函数时不调用使用 <xref:System.StringComparison> 参数的重载，则可能在计划执行序号搜索时无意中依赖于特定于区域性的行为。 由于 NLS 和 ICU 在其语言比较器中实现的逻辑有所不同，因此 `string.IndexOf(string)` 等方法的结果可能会返回意外的值。

即使在全球化功能并非总是处于活动状态的地方，也会出现这类问题。 例如，根据当前运行时，下面的代码可能会生成不同的答案。

```cs
string s = "Hello\r\nworld!";
int idx = s.IndexOf("\n");
Console.WriteLine(idx);

// The snippet prints:
//
// '6' when running on .NET Framework (Windows)
// '6' when running on .NET Core 2.x - 3.x (Windows)
// '-1' when running on .NET 5 (Windows)
// '-1' when running on .NET Core 2.x - 3.x or .NET 5 (non-Windows)
// '6' when running on .NET Core 2.x or .NET 5 (in invariant mode)
```

## <a name="guard-against-unexpected-behavior"></a>防范意外行为

本节提供了处理 .NET 5.0 中的意外行为更改的两种方法。

### <a name="enable-code-analyzers"></a>启用代码分析器

[代码分析器](../../fundamentals/code-analysis/overview.md)可以检测可能存在错误的调用站点。 为了帮助防范任何意外行为，建议在项目中启用 .NET Compiler Platform (Roslyn) 分析器。 该分析器有助于标记在计划使用序号比较器时可能无意中使用语言比较器的代码。 以下规则应有助于标记这些问题：

- [CA1307:为了清晰起见，请指定 StringComparison](../../fundamentals/code-analysis/quality-rules/ca1307.md)
- [CA1309:使用按顺序的 StringComparison](../../fundamentals/code-analysis/quality-rules/ca1309.md)
- [CA1310：为了确保正确，请指定 StringComparison](../../fundamentals/code-analysis/quality-rules/ca1310.md)

默认不启用这些特定规则。 若要启用它们并将任何冲突显示为生成错误，请在项目文件中设置以下属性：

```xml
<PropertyGroup>
  <AnalysisMode>AllEnabledByDefault</AnalysisMode>
  <WarningsAsErrors>$(WarningsAsErrors);CA1307;CA1309;CA1310</WarningsAsErrors>
</PropertyGroup>
```

以下代码片段显示生成相关代码分析器警告或错误的代码示例。

```cs
//
// Potentially incorrect code - answer might vary based on locale.
//
string s = GetString();
// Produces analyzer warning CA1310 for string; CA1307 matches on char ','
int idx = s.IndexOf(",");
Console.WriteLine(idx);

//
// Corrected code - matches the literal substring ",".
//
string s = GetString();
int idx = s.IndexOf(",", StringComparison.Ordinal);
Console.WriteLine(idx);

//
// Corrected code (alternative) - searches for the literal ',' character.
//
string s = GetString();
int idx = s.IndexOf(',');
Console.WriteLine(idx);
```

同样，在实例化已排序的字符串集合或对现有基于字符串的集合进行排序时，请指定显式比较器。

```cs
//
// Potentially incorrect code - behavior might vary based on locale.
//
SortedSet<string> mySet = new SortedSet<string>();
List<string> list = GetListOfStrings();
list.Sort();

//
// Corrected code - uses ordinal sorting; doesn't vary by locale.
//
SortedSet<string> mySet = new SortedSet<string>(StringComparer.Ordinal);
List<string> list = GetListOfStrings();
list.Sort(StringComparer.Ordinal);
```

### <a name="revert-back-to-nls-behaviors"></a>还原到 NLS 行为

在 Windows 上运行 .NET 5 应用程序时，若要将其还原到早前的 NLS 行为，请按照 [.NET 全球化和 ICU](../globalization-localization/globalization-icu.md) 中的步骤操作。 必须在应用程序级别设置此应用程序范围的兼容性开关。 单个库不能选择加入或选择退出此行为。

> [!TIP]
> 强烈建议启用 [CA1307](../../fundamentals/code-analysis/quality-rules/ca1307.md)、[CA1309](../../fundamentals/code-analysis/quality-rules/ca1309.md) 和 [CA1310](../../fundamentals/code-analysis/quality-rules/ca1310.md) 代码分析规则，以帮助改进代码卫生和发现任何现有的潜在 bug。 有关详细信息，请参阅[启用代码分析器](#enable-code-analyzers)。

## <a name="affected-apis"></a>受影响的 API

大多数 .NET 应用程序不会遇到因 .NET 5.0 中的更改而导致的任何意外行为。 但是，由于受影响的 API 数量以及这些 API 对更广泛的 .NET 生态系统的基础性，你应知道 .NET 5.0 可能会引入不需要的行为或公开应用程序中已存在的潜在 bug。

受影响的 API 包括：

- <xref:System.String.Compare%2A?displayProperty=fullName>
- <xref:System.String.EndsWith%2A?displayProperty=fullName>
- <xref:System.String.IndexOf%2A?displayProperty=fullName>
- <xref:System.String.StartsWith%2A?displayProperty=fullName>
- <xref:System.String.ToLower%2A?displayProperty=fullName>
- <xref:System.String.ToLowerInvariant%2A?displayProperty=fullName>
- <xref:System.String.ToUpper%2A?displayProperty=fullName>
- <xref:System.String.ToUpperInvariant%2A?displayProperty=fullName>
- <xref:System.Globalization.TextInfo?displayProperty=fullName>（大多数成员）
- <xref:System.Globalization.CompareInfo?displayProperty=fullName>（大多数成员）
- <xref:System.Array.Sort%2A?displayProperty=fullName>（对字符串数组进行排序时）
- <xref:System.Collections.Generic.List%601.Sort?displayProperty=fullName>（当列表元素为字符串时）
- <xref:System.Collections.Generic.SortedDictionary%602?displayProperty=fullName>（当键为字符串时）
- <xref:System.Collections.Generic.SortedList%602?displayProperty=fullName>（当键为字符串时）
- <xref:System.Collections.Generic.SortedSet%601?displayProperty=fullName>（当集包含字符串时）

> [!NOTE]
> 这不是受影响的 API 的详尽列表。

默认情况下，上述所有 API 都使用语言字符串搜索与比较，它们使用线程的[当前区域性](xref:System.Threading.Thread.CurrentCulture)。 [序号和语言搜索与比较](#ordinal-vs-linguistic-search-and-comparison)中指明了语言和序号搜索与比较之间的区别。

由于 ICU 实现语言字符串比较的方式与 NLS 不同，从早期版本的 .NET Core 或 .NET Framework 升级到 .NET 5.0 的基于 Windows 的应用程序，在调用受影响的 API 之一时可能会注意到，这些 API 开始表现出不同的行为。

### <a name="exceptions"></a>异常

* 如果 API 接受显式 `StringComparison` 或 `CultureInfo` 参数，则该参数将覆盖 API 的默认行为。
* 第一个参数类型为 `char` 的 `System.String` 成员（例如 <xref:System.String.IndexOf(System.Char)?displayProperty=nameWithType>）使用序号搜索，除非调用方传递指定 `CurrentCulture[IgnoreCase]` 或 `InvariantCulture[IgnoreCase]` 的显式 `StringComparison` 参数。

若要详细分析每个 <xref:System.String> API 的默认行为，请参阅[默认搜索和比较类型](#default-search-and-comparison-types)部分。

## <a name="ordinal-vs-linguistic-search-and-comparison"></a>序号和语言搜索与比较

序号（也称为“非语言”）搜索与比较将字符串拆分为其单独的 `char` 元素，并执行逐字符搜索或比较。 例如，字符串 `"dog"` 和 `"dog"` 在 `Ordinal` 比较器下的比较结果为“相等”，因为这两个字符串由完全相同的字符序列组成。 但是，`"dog"` 和 `"Dog"` 在 `Ordinal` 比较器下的比较结果为“不相等”，因为它们由不完全相同的字符序列组成。 也就是说，大写 `'D'` 的码位 `U+0044` 出现在小写 `'d'` 的码位 `U+0064` 之前，因此 `"dog"` 排在 `"Dog"` 之前。

`OrdinalIgnoreCase` 比较器仍执行逐字符操作，但它在执行该操作时消除了大小写差异。 在 `OrdinalIgnoreCase` 比较器下，字符对 `'d'` 和 `'D'` 的比较结果为“相等”，字符对 `'á'` 和 `'Á'` 亦是如此。 但非重音字符 `'a'` 与重音字符 `'á'` 的比较结果为“不相等”。

下表提供了此操作的一些示例：

| 字符串 1 | 字符串 2 | `Ordinal` 比较 | `OrdinalIgnoreCase` 比较 |
|---|---|---|---|
| `"dog"` | `"dog"` | equal | equal |
| `"dog"` | `"Dog"` | 不等于 | equal |
| `"resume"` | `"Resume"` | 不等于 | equal |
| `"resume"` | `"résumé"` | 不等于 | 不等于 |

Unicode 还允许字符串具有多个不同的内存中表示形式。 例如，带锐音符的 e (é) 可以用两种方式表示：

* 单个文本 `'é'` 字符（亦可写为 `'\u00E9'`）。
* 文本无重音 `'e'` 字符，后跟一个组合用重音修饰符字符 `'\u0301'`。

这意味着下面的四个字符串的显示结果都为 `"résumé"`，即使它们的组成部分有所不同。 该字符串使用文本 `'é'` 字符或文本无重音 `'e'` 字符，以及组合用重音修饰符 `'\u0301'`。

* `"r\u00E9sum\u00E9"`
* `"r\u00E9sume\u0301"`
* `"re\u0301sum\u00E9"`
* `"re\u0301sume\u0301"`

在序号比较器下，这些字符串相互比较后的结果都不是“相等”。 这是因为它们都包含不同的基础字符序列，即使在屏幕上显示时，它们看起来都是相同的。

执行 `string.IndexOf(..., StringComparison.Ordinal)` 操作时，运行时查找完全匹配的子字符串。 结果如下所示：

```cs
Console.WriteLine("resume".IndexOf("e", StringComparison.Ordinal)); // prints '1'
Console.WriteLine("r\u00E9sum\u00E9".IndexOf("e", StringComparison.Ordinal)); // prints '-1'
Console.WriteLine("r\u00E9sume\u0301".IndexOf("e", StringComparison.Ordinal)); // prints '5'
Console.WriteLine("re\u0301sum\u00E9".IndexOf("e", StringComparison.Ordinal)); // prints '1'
Console.WriteLine("re\u0301sume\u0301".IndexOf("e", StringComparison.Ordinal)); // prints '1'
Console.WriteLine("resume".IndexOf("E", StringComparison.OrdinalIgnoreCase)); // prints '1'
Console.WriteLine("r\u00E9sum\u00E9".IndexOf("E", StringComparison.OrdinalIgnoreCase)); // prints '-1'
Console.WriteLine("r\u00E9sume\u0301".IndexOf("E", StringComparison.OrdinalIgnoreCase)); // prints '5'
Console.WriteLine("re\u0301sum\u00E9".IndexOf("E", StringComparison.OrdinalIgnoreCase)); // prints '1'
Console.WriteLine("re\u0301sume\u0301".IndexOf("E", StringComparison.OrdinalIgnoreCase)); // prints '1'
```

序号搜索与比较例程从来不受当前线程的区域性设置的影响。

语言搜索与比较例程将字符串拆分为排序元素，并对这些元素执行搜索或比较。 字符串的字符和其构成的排序元素之间不一定存在 1:1 映射。 例如，长度为 2 的字符串可能只包含单个排序元素。 使用识别语言的方式比较两个字符串时，比较器会检查两个字符串的排序元素是否具有相同的语义含义，即使这两个字符串的文本字符并不相同。

再次考虑字符串 `"résumé"` 及其四种不同的表示形式。 下表展示了拆分为其排序元素的每种表示形式。

| String | 作为排序元素 |
|---|---|
| `"r\u00E9sum\u00E9"` | `"r" + "\u00E9" + "s" + "u" + "m" + "\u00E9"` |
| `"r\u00E9sume\u0301"` | `"r" + "\u00E9" + "s" + "u" + "m" + "e\u0301"` |
| `"re\u0301sum\u00E9"` | `"r" + "e\u0301" + "s" + "u" + "m" + "\u00E9"` |
| `"re\u0301sume\u0301"` | `"r" + "e\u00E9" + "s" + "u" + "m" + "e\u0301"` |

排序元素松散地对应于读取器视为单个字符或字符群集的字符串。 它在概念上类似于[字形群集](character-encoding-introduction.md#grapheme-clusters)，但包含更大的字符串。

在语言比较器下，不需要完全匹配。 排序元素根据其语义含义进行比较。 例如，语言比较器对子字符串 `"\u00E9"` 和 `"e\u0301"` 的比较结果为“相等”，因为它们在语义上都是指“带锐音符修饰符的小写字母 e”。 这允许 `IndexOf` 方法匹配更大字符串中的子字符串 `"e\u0301"`，该更大字符串包含语义上等效的子字符串 `"\u00E9"`，如下面的代码示例中所示。

```cs
Console.WriteLine("r\u00E9sum\u00E9".IndexOf("e")); // prints '-1' (not found)
Console.WriteLine("r\u00E9sum\u00E9".IndexOf("e\u00E9")); // prints '1'
Console.WriteLine("\u00E9".IndexOf("e\u00E9")); // prints '0'
```

因此，如果使用语言比较，则两个不同长度的字符串的比较结果可能为“相等”。 调用方应注意，在这种情况下处理字符串长度时，不要使用特殊情况逻辑。

识别区域性搜索与比较例程是语言搜索与比较例程的一种特殊形式。 在识别区域性比较器下，排序元素的概念扩展为包含特定于指定区域性的信息。

例如，[在匈牙利字母中](https://en.wikipedia.org/wiki/Hungarian_alphabet)，如果两个字符 \<dz\> 连续出现，则它们被认为是与 \<d\> 或 \<z\> 不同的独特字母。 这意味着，如果在字符串中出现 \<dz\>，则匈牙利语识别区域性比较器会将其视为单个排序元素。

| String | 作为排序元素 | 注解 |
|---|---|---|
| `"endz"` | `"e" + "n" + "d" + "z"` | （使用标准语言比较器） |
| `"endz"` | `"e" + "n" + "dz"` | （使用匈牙利语识别区域性比较器） |

使用匈牙利语识别区域性比较器时，字符串 `"endz"` 不以子字符串 `"z"` 结尾，因为 <\dz\> 和 <\z\> 被视为具有不同语义含义的排序元素。

```cs
// Set thread culture to Hungarian
CultureInfo.CurrentCulture = CultureInfo.GetCultureInfo("hu-HU");
Console.WriteLine("endz".EndsWith("z")); // Prints 'False'

// Set thread culture to invariant culture
CultureInfo.CurrentCulture = CultureInfo.InvariantCulture;
Console.WriteLine("endz".EndsWith("z")); // Prints 'True'
```

> [!NOTE]
>
> - 行为：语言和识别区域性比较器可以不时地进行行为调整。 ICU 和较旧的 Windows NLS 功能都将更新，以考虑世界语言的变化。 有关详细信息，请参阅博客文章[区域设置（区域性）数据改动](/archive/blogs/shawnste/locale-culture-data-churn)。 序号比较器的行为将永远不会发生更改，因为它执行严格的按位搜索和比较。 但是，OrdinalIgnoreCase 比较器的行为可能会随着 Unicode 的增加而改变，以包含更多的字符集，并纠正现有大小写数据中的遗漏。
> - 使用情况：比较器 `StringComparison.InvariantCulture` 和 `StringComparison.InvariantCultureIgnoreCase` 是不能识别区域性的语言比较器。 也就是说，这些比较器理解一些概念，例如，重音字符 é 具有多种可能的基础表示形式，且所有这些表示形式都应视为“相等”。 但不识别区域性的语言比较器不包含对 \<dz\> 区别于 \<d\> 或 \<z\> 的特殊处理，如上所示。 它们也不能处理像德语 Eszett (ß) 这样的特殊字符。

.NET 还提供固定全球化模式。 此选择加入模式禁用处理语言搜索与比较例程的代码路径。 在此模式下，无论调用方提供何种 `CultureInfo` 或 `StringComparison` 参数，所有操作都使用序号或 OrdinalIgnoreCase 行为。 有关详细信息，请参阅[全球化运行时配置选项](../../core/run-time-config/globalization.md)和 [.NET Core 全球化固定模式](https://github.com/dotnet/runtime/blob/master/docs/design/features/globalization-invariant-mode.md)。

有关详细信息，请参阅[比较 .NET 中的字符串的最佳做法](best-practices-strings.md)。

## <a name="security-implications"></a>安全隐患

如果你的应用使用受影响的 API 进行筛选，我们建议启用 CA1307 和 CA1309 代码分析规则，以帮助查找可能无意中使用了语言搜索而不是序号搜索的位置。 下面这样的代码模式可能易受安全漏洞的攻击。

```cs
//
// THIS SAMPLE CODE IS INCORRECT.
// DO NOT USE IT IN PRODUCTION.
//
public bool ContainsHtmlSensitiveCharacters(string input)
{
    if (input.IndexOf("<") >= 0) { return true; }
    if (input.IndexOf("&") >= 0) { return true; }
    return false;
}
```

由于 `string.IndexOf(string)` 方法默认使用语言搜索，因此字符串可能包含文本 `'<'` 或 `'&'` 字符，且 `string.IndexOf(string)` 例程可能返回 `-1`，表示找不到搜索子字符串。 代码分析规则 CA1307 和 CA1309 标记此类调用站点，并警告开发人员存在潜在的问题。

## <a name="default-search-and-comparison-types"></a>默认搜索和比较类型

下表列出了各种字符串和类似于字符串的 API 的默认搜索和比较类型。 如果调用方提供显式 `CultureInfo` 或 `StringComparison` 参数，则该参数将优先于任何默认值。

| API | 默认行为 | 注解 |
|---|---|---|
| `string.Compare` | CurrentCulture | |
| `string.CompareTo` | CurrentCulture | |
| `string.Contains` | Ordinal | |
| `string.EndsWith` | Ordinal | （当第一个参数为 `char` 时） |
| `string.EndsWith` | CurrentCulture | （当第一个参数为 `string` 时） |
| `string.Equals` | Ordinal | |
| `string.GetHashCode` | Ordinal | |
| `string.IndexOf` | Ordinal | （当第一个参数为 `char` 时） |
| `string.IndexOf` | CurrentCulture | （当第一个参数为 `string` 时） |
| `string.IndexOfAny` | Ordinal | |
| `string.LastIndexOf` | Ordinal | （当第一个参数为 `char` 时） |
| `string.LastIndexOf` | CurrentCulture | （当第一个参数为 `string` 时） |
| `string.LastIndexOfAny` | Ordinal | |
| `string.Replace` | Ordinal | |
| `string.Split` | Ordinal | |
| `string.StartsWith` | Ordinal | （当第一个参数为 `char` 时） |
| `string.StartsWith` | CurrentCulture | （当第一个参数为 `string` 时） |
| `string.ToLower` | CurrentCulture | |
| `string.ToLowerInvariant` | InvariantCulture | |
| `string.ToUpper` | CurrentCulture | |
| `string.ToUpperInvariant` | InvariantCulture | |
| `string.Trim` | Ordinal | |
| `string.TrimEnd` | Ordinal | |
| `string.TrimStart` | Ordinal | |
| `string == string` | Ordinal | |
| `string != string` | Ordinal | |

与 `string` API 不同，默认情况下，所有 `MemoryExtensions` API 都执行序号搜索与比较，但以下情况例外。

| API | 默认行为 | 注解 |
|---|---|---|
| `MemoryExtensions.ToLower` | CurrentCulture | （当传递 null `CultureInfo` 参数时） |
| `MemoryExtensions.ToLowerInvariant` | InvariantCulture | |
| `MemoryExtensions.ToUpper` | CurrentCulture | （当传递 null `CultureInfo` 参数时） |
| `MemoryExtensions.ToUpperInvariant` | InvariantCulture | |

结果是，在将代码从使用 `string` 转换为使用 `ReadOnlySpan<char>`时，可能会无意中引入行为更改。 相关示例如下。

```cs
string str = GetString();
if (str.StartsWith("Hello")) { /* do something */ } // this is a CULTURE-AWARE (linguistic) comparison

ReadOnlySpan<char> span = s.AsSpan();
if (span.StartsWith("Hello")) { /* do something */ } // this is an ORDINAL (non-linguistic) comparison
```

解决此问题的建议方法是将显式 `StringComparison` 参数传递给这些 API。 代码分析规则 CA1307 和 CA1309 可帮助解决此问题。

```cs
string str = GetString();
if (str.StartsWith("Hello", StringComparison.Ordinal)) { /* do something */ } // ordinal comparison

ReadOnlySpan<char> span = s.AsSpan();
if (span.StartsWith("Hello", StringComparison.Ordinal)) { /* do something */ } // ordinal comparison
```

## <a name="see-also"></a>请参阅

- [全球化中断性变更](../../core/compatibility/globalization.md)
- [有关比较 .NET 中字符串的最佳做法](best-practices-strings.md)
- [如何比较 C# 中的字符串](../../csharp/how-to/compare-strings.md)
- [.NET 全球化和 ICU](../globalization-localization/globalization-icu.md)
- [序号与识别区域性字符串操作](/dotnet/api/system.string#ordinal-vs-culture-sensitive-operations)
- [.NET 源代码分析概述](../../fundamentals/code-analysis/overview.md)
