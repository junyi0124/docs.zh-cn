---
title: 将字符串分离为子字符串
description: 了解用于提取字符串各个部分的不同技术，其中包括 String.Split、正则表达式和 String.Substring。
ms.date: 10/30/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- strings [.NET], breaking up
ms.openlocfilehash: b753476b7d8e5808fdcacc6f28bd1de5f8b232bb
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94829644"
---
# <a name="extract-substrings-from-a-string"></a>从字符串中提取子字符串

本文介绍了一些用于提取字符串各个部分的不同技术。

- 当所需的子字符串由一个已知的分隔符（或多个分隔符）分隔时，请使用 [Split 方法](#stringsplit-method)。
- 如果字符串符合某种固定模式，则可使用[正则表达式](#regular-expressions)。
- 如果不需要提取字符串中的所有子字符串，请结合使用 [IndexOf 和 Substring 方法](#stringindexof-and-stringsubstring-methods)。

## <a name="stringsplit-method"></a>String.Split 方法

<xref:System.String.Split%2A?displayProperty=nameWithType> 可提供少量重载，来根据指定的一个或多个分隔符将字符串分解为一组子字符串。 可以选择限制最终结果中子字符串的总数、剪裁子字符串中的空白字符或排除空子字符串。

下面的示例显示了三种不同的 `String.Split()` 重载。 第一个示例调用 <xref:System.String.Split(System.Char[])> 重载，而不传递任何分隔符。 如果未指定任何分隔符，`String.Split()` 将使用默认分隔符（空白字符）来拆分字符串。

[!code-csharp-interactive[Intro#1](snippets/parse-strings/csharp/intro.cs#1)]
:::code language="vb" source="snippets/parse-strings/vb/intro.vb" id="1":::

正如你所看到的那样，两个子字符串之间包含句点字符 (`.`)。 如果要排除句点字符，可以将句点字符添加为额外的分隔符。 下面的示例演示了如何执行此操作。

[!code-csharp-interactive[Intro#1](snippets/parse-strings/csharp/intro.cs#2)]
:::code language="vb" source="snippets/parse-strings/vb/intro.vb" id="2":::

子字符串之间的句点消息，但现在包含了两个额外的空子字符串。 这些空子字符串表示单词与紧跟单词之后的句点之间的子字符串。 若要从生成的数组中删除空字符串，可以调用 <xref:System.String.Split(System.Char[],System.StringSplitOptions)> 重载，并为 `options` 参数指定 <xref:System.StringSplitOptions.RemoveEmptyEntries?displayProperty=nameWithType>。

[!code-csharp-interactive[Intro#1](snippets/parse-strings/csharp/intro.cs#3)]
:::code language="vb" source="snippets/parse-strings/vb/intro.vb" id="3":::

## <a name="regular-expressions"></a>正则表达式

如果字符串符合某种固定模式，则可以使用正则表达式提取并处理其元素。 例如，如果字符串采用“数字 操作数 数字”  格式，则可以[使用正则表达式](regular-expressions.md)提取并处理字符串的元素。 下面是一个示例：

:::code language="csharp" source="snippets/parse-strings/csharp/regex.cs" id="1" interactive="try-dotnet":::
:::code language="vb" source="snippets/parse-strings/vb/regex.vb" id="1":::

正则表达式模式 `(\d+)\s+([-+*/])\s+(\d+)` 的定义如下：

|模式|说明|
|-------------|-----------------|
|`(\d+)`|匹配一个或多个十进制数字。 这是第一个捕获组。|
|`\s+`|匹配一个或多个空白字符。|
|`([-+*/])`|匹配算术运算符（+、-、* 或 /）。 这是第二个捕获组。|
|`\s+`|匹配一个或多个空白字符。|
|`(\d+)`|匹配一个或多个十进制数字。 这是第三个捕获组。|

你也可以使用正则表达式根据某种模式而非固定字符集提取字符串中的子字符串。 在下面的任意一种情况下，常用此方案：

- 一个或多个分隔符不总是用作 <xref:System.String> 实例中的分隔符。

- 分隔符的顺序和数量多变或未知。

例如，不能使用 <xref:System.String.Split%2A> 方法拆分以下字符串，因为 `\n`（换行）符的数量是可变的，并且它们不总是用作分隔符。

```text
[This is captured\ntext.]\n\n[\n[This is more captured text.]\n]
\n[Some more captured text:\n   Option1\n   Option2][Terse text.]
```

正则表达式可以轻松拆分此字符串，如下面的示例所示。

:::code language="csharp" source="snippets/parse-strings/csharp/regex.cs" id="2" interactive="try-dotnet":::
:::code language="vb" source="snippets/parse-strings/vb/regex.vb" id="2":::

正则表达式模式 `\[([^\[\]]+)\]` 的定义如下：

|模式|说明|
|-------------|-----------------|
|`\[`|匹配左方括号。|
|`([^\[\]]+)`|一次或多次与非左或右方括号的字符匹配 这是第一个捕获组。|
|`\]`|匹配右方括号。|

<xref:System.Text.RegularExpressions.Regex.Split%2A?displayProperty=nameWithType> 方法几乎与 <xref:System.String.Split%2A?displayProperty=nameWithType> 相同，不同之处在于，它根据正则表达式模式而非固定字符集拆分字符串。 例如，下面的示例使用 <xref:System.Text.RegularExpressions.Regex.Split%2A?displayProperty=nameWithType> 方法拆分字符串，该字符串包含由连字符和其他字符的各种组合分隔的子字符串。

:::code language="csharp" source="snippets/parse-strings/csharp/regex.cs" id="3" interactive="try-dotnet":::
:::code language="vb" source="snippets/parse-strings/vb/regex.vb" id="3":::

正则表达式模式 `\s-\s?[+*]?\s?-\s` 的定义如下：

|模式|说明|
|-------------|-----------------|
|`\s-`|匹配后跟一个连字符的空白字符。|
|`\s?`|匹配零个或一个空白字符。|
|`[+*]?`|与 + 或 * 字符的零个或一个匹配项匹配。|
|`\s?`|匹配零个或一个空白字符。|
|`-\s`|匹配后跟一个空白字符的连字符。|

## <a name="stringindexof-and-stringsubstring-methods"></a>String.IndexOf 和 String.Substring 方法

如果并不需要字符串中的所有子字符串，则可能更想要使用一种字符串比较方法来返回匹配开始之处的索引。 然后，可以调用 <xref:System.String.Substring%2A> 方法来提取所需的子字符串。 字符串比较方法包括：

- <xref:System.String.IndexOf%2A>，它返回字符串实例中的某个字符或字符串的第一个匹配项的从零开始的索引。

- <xref:System.String.IndexOfAny%2A>，它返回在当前字符串实例中字符数组中任何字符的第一个匹配项的从零开始的索引。

- <xref:System.String.LastIndexOf%2A>，它返回字符串实例中的某个字符或字符串的最后一个匹配项的从零开始的索引。

- <xref:System.String.LastIndexOfAny%2A>，它返回在当前字符串实例中字符数组中任何字符的最后一个匹配项的从零开始的索引。

以下示例使用 <xref:System.String.IndexOf%2A> 方法查找字符串中的句点。 然后，它使用 <xref:System.String.Substring%2A> 方法返回完整句子。

:::code language="csharp" source="snippets/parse-strings/csharp/indexof.cs" id="1" interactive="try-dotnet":::
:::code language="vb" source="snippets/parse-strings/vb/indexof.vb" id="1":::

## <a name="see-also"></a>请参阅

- [.NET 中的基本字符串操作](basic-string-operations.md)
- [.NET 正则表达式](regular-expressions.md)
- [如何使用 C# 中的 String.Split 分析字符串](../../csharp/how-to/parse-strings-using-split.md)
