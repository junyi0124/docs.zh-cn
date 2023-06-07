---
title: 如何搜索字符串（C# 指南）
description: 了解有关使用 C# 在字符串中搜索文本的两种策略。 字符串类方法搜索特定文本。 正则表达式搜索文本中的模式。
ms.date: 02/21/2018
helpviewer_keywords:
- searching strings [C#]
- strings [C#], searching with String methods
- strings [C#], searching with regular expressions
ms.assetid: fb1d9a6d-598d-4a35-bd5f-b86012edcb2b
ms.openlocfilehash: b6a2ab25efa64b2bbf33ea234dba07cdf9582e66
ms.sourcegitcommit: e7acba36517134238065e4d50bb4a1cfe47ebd06
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2020
ms.locfileid: "89465125"
---
# <a name="how-to-search-strings"></a>如何搜索字符串

可以使用两种主要策略搜索字符串中的文本。 <xref:System.String> 类的方法搜索特定文本。 正则表达式搜索文本中的模式。

[!INCLUDE[interactive-note](~/includes/csharp-interactive-note.md)]

[string](../language-reference/builtin-types/reference-types.md#the-string-type) 类型是 <xref:System.String?displayProperty=nameWithType> 类的别名，可提供多种有效方法用于搜索字符串的内容。 其中包括 <xref:System.String.Contains%2A>、<xref:System.String.StartsWith%2A>、<xref:System.String.EndsWith%2A>、<xref:System.String.IndexOf%2A> 以及 <xref:System.String.LastIndexOf%2A>。 <xref:System.Text.RegularExpressions.Regex?displayProperty=nameWithType> 类具备丰富的词汇来对文本中的模式进行搜索。 你将在本文中了解这些技术以及如何选择符合需求的最佳方法。

## <a name="does-a-string-contain-text"></a>字符串包含文本吗？

<xref:System.String.Contains%2A?displayProperty=nameWithType>、<xref:System.String.StartsWith%2A?displayProperty=nameWithType> 和 <xref:System.String.EndsWith%2A?displayProperty=nameWithType> 方法搜索字符串中的特定文本。 下面的示例显示了每一个方法以及使用不区分大小写的搜索的差异：

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/SearchStrings.cs" id="Snippet1":::

前面的示例演示了使用这些方法的重点。 默认情况下搜索是区分大小写的。 使用 <xref:System.StringComparison.CurrentCultureIgnoreCase?displayProperty=nameWithType> 枚举值指定区分大小写的搜索。

## <a name="where-does-the-sought-text-occur-in-a-string"></a>寻找的文本出现在字符串的什么位置？

<xref:System.String.IndexOf%2A> 和 <xref:System.String.LastIndexOf%2A> 方法也搜索字符串中的文本。 这些方法返回查找到的文本的位置。 如果未找到文本，则返回 `-1`。 下面的示例显示“methods”第一次出现和最后一次出现的搜索结果，并显示了它们之间的文本。

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/SearchStrings.cs" id="Snippet2":::

## <a name="finding-specific-text-using-regular-expressions"></a>使用正则表达式查找特定文本

<xref:System.Text.RegularExpressions.Regex?displayProperty=nameWithType> 类可用于搜索字符串。 这些搜索的范围可以从简单的内容到复杂的文本模式。

下面的代码示例在一个句子中搜索了“the”或“their”（忽略大小写）。 静态方法 <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> 执行此次搜索。 你向它提供要搜索的字符串以及搜索模式。 在这种情况下，第三个参数指定不区分大小写的搜索。 有关详细信息，请参阅 <xref:System.Text.RegularExpressions.RegexOptions?displayProperty=nameWithType>。

搜索模式描述你所搜索的文本。 下表描述搜索模式的每个元素。 （下表使用单个 `\`，它在 C# 字符串中必须转义为 `\\`）。

| 模式  | 含义                          |
|----------|----------------------------------|
| `the`    | 匹配文本“the”             |
| `(eir)?` | 匹配 0 个或 1 个“eir” |
| `\s`     | 与空白符匹配    |

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/SearchStrings.cs" id="Snippet3":::

> [!TIP]
> 搜索精确的字符串时，`string` 方法通常是更好的选择。 搜索源字符串中的一些模式时，正则表达式更适用。

## <a name="does-a-string-follow-a-pattern"></a>字符串是否遵循模式？

以下代码使用正则表达式验证数组中每个字符串的格式。 验证要求每个字符串具备电话号码的形式：用短划线分隔成三组数字，前两组包含 3 个数字，而第三组包含 4 个数字。 搜索模式采用正则表达式 `^\\d{3}-\\d{3}-\\d{4}$`。 有关更多信息，请参见[正则表达式语言 - 快速参考](../../standard/base-types/regular-expression-language-quick-reference.md)。

| 模式 | 含义                             |
|---------|-------------------------------------|
| `^`     | 匹配字符串的开头部分 |
| `\d{3}` | 完全匹配 3 位字符  |
| `-`     | 匹配字符“-”           |
| `\d{3}` | 完全匹配 3 位字符  |
| `-`     | 匹配字符“-”           |
| `\d{4}` | 完全匹配 4 位字符  |
| `$`     | 匹配字符串的结尾部分       |

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/SearchStrings.cs" id="Snippet4":::

此单个搜索模式匹配很多有效字符串。 正则表达式更适用于搜索或验证模式，而不是单个文本字符串。

## <a name="see-also"></a>请参阅

- [C# 编程指南](../programming-guide/index.md)
- [字符串](../programming-guide/strings/index.md)
- [LINQ 和字符串](../programming-guide/concepts/linq/linq-and-strings.md)
- <xref:System.Text.RegularExpressions.Regex?displayProperty=nameWithType>
- [.NET 正则表达式](../../standard/base-types/regular-expressions.md)
- [正则表达式语言 - 快速参考](../../standard/base-types/regular-expression-language-quick-reference.md)
- [有关使用 .NET 中字符串的最佳做法](../../standard/base-types/best-practices-strings.md)
