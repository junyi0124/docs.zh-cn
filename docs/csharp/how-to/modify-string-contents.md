---
title: 如何修改字符串内容 - C# 指南
description: 查看几种修改 C# 中现有字符串内容的方法的示例，这些方法将返回一个新的字符串对象。
ms.date: 02/26/2018
helpviewer_keywords:
- strings [C#], modifying
ms.openlocfilehash: bae54757fdb6f02cdc0dc8fc15ad3f7583c230a7
ms.sourcegitcommit: e7acba36517134238065e4d50bb4a1cfe47ebd06
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2020
ms.locfileid: "89465047"
---
# <a name="how-to-modify-string-contents-in-c"></a>如何修改以 C\# 编写的字符串内容

本文演示通过修改现有 `string` 来生成 `string` 的几种方法。 演示的所有方法均将修改的结果返回为新的 `string` 对象。 为了说明原始字符串和修改后的字符串是不同的实例，示例会将结果存储在新变量中。 运行每个示例时，可以检查原始 `string` 和修改后的新 `string`。

[!INCLUDE[interactive-note](~/includes/csharp-interactive-note.md)]

本文中演示了几种方法。 你可以替换现有文本。 可以搜索模式并将匹配的文本替换为其他文本。 可以将字符串视为字符序列。 还可以使用删除空格的简便方法。 选择与你的方案最匹配的方法。

## <a name="replace-text"></a>替换文本

下面的代码通过将现有文本替换为替代文本来创建新的字符串。

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/ModifyStrings.cs" id="Snippet1":::

上述代码演示了字符串的不可变属性。 在上述示例中可以看到，原始字符串 `source` 并未被修改。 <xref:System.String.Replace%2A?displayProperty=nameWithType> 方法创建的是包含修改的新 `string`。

<xref:System.String.Replace%2A> 可替换字符串或单个字符。 在这两种情况下，搜索文本的每个匹配项均被替换。  下面的示例将所有的“ ”替换为“\_”：

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/ModifyStrings.cs" id="Snippet2":::

源字符串并未发生更改，而是通过替换操作返回了一个新的字符串。

## <a name="trim-white-space"></a>去除空格

可使用 <xref:System.String.Trim%2A?displayProperty=nameWithType>、<xref:System.String.TrimStart%2A?displayProperty=nameWithType> 和 <xref:System.String.TrimEnd%2A?displayProperty=nameWithType> 方法删除任何前导空格或尾随空格。  下面的代码就是删除两种空格的示例。 源字符串不会发生变化；这些方法返回带修改内容的新字符串。

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/ModifyStrings.cs" id="Snippet3":::

## <a name="remove-text"></a>删除文本

可使用 <xref:System.String.Remove%2A?displayProperty=nameWithType> 方法删除字符串中的文本。 此方法移删除特定索引处开始的某些字符。 下面的示例演示如何使用 <xref:System.String.IndexOf%2A?displayProperty=nameWithType>（后接 <xref:System.String.Remove%2A>）方法，删除字符串中的文本：

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/ModifyStrings.cs" id="Snippet4":::

## <a name="replace-matching-patterns"></a>替换匹配模式

可使用[正则表达式](../../standard/base-types/regular-expressions.md)将匹配模式的文本替换为新文本，新文本可能由模式定义。 下面的示例使用 <xref:System.Text.RegularExpressions.Regex?displayProperty=nameWithType> 类从源字符串中查找模式并将其替换为正确的大写。 <xref:System.Text.RegularExpressions.Regex.Replace(System.String,System.String,System.Text.RegularExpressions.MatchEvaluator,System.Text.RegularExpressions.RegexOptions)?displayProperty=nameWithType> 方法使用将替换逻辑提供为其参数之一的函数。 在本示例中，该函数 `LocalReplaceMatchCase` 是在示例方法中声明的本地函数。 `LocalReplaceMatchCase` 使用 <xref:System.Text.StringBuilder?displayProperty=nameWithType>类，以生成具有正确大写的替换字符串。

正则表达式最适合用于搜索和替换遵循模式的文本，而不是已知的文本。 有关详细信息，请参阅[如何搜索字符串](search-strings.md)。 搜索模式“the\s”搜索“the”后接空格字符的单词。 该部分的模式可确保它不与源字符串中的“there”相匹配。 有关正则表达式语言元素的更多信息，请参阅[正则表达式语言 - 快速参考](../../standard/base-types/regular-expression-language-quick-reference.md)。

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/ModifyStrings.cs" id="Snippet5":::

<xref:System.Text.StringBuilder.ToString%2A?displayProperty=nameWithType> 方法返回一个不可变的字符串，其中包含 <xref:System.Text.StringBuilder> 对象中的内容。

## <a name="modifying-individual-characters"></a>修改单个字符

可从字符串生成字符数组，修改数组的内容，然后从数组的已修改内容创建新的字符串。

下面的示例演示如何替换字符串中的一组字符。 首先，它使用 <xref:System.String.ToCharArray?displayProperty=nameWithType> 方法来创建字符数组。 它使用 <xref:System.String.IndexOf%2A> 方法来查找单词“fox”的起始索引。 接下来的三个字符将替换为其他单词。 最终，从更新的字符串数组中构造了新的字符串。

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/ModifyStrings.cs" id="Snippet6":::

## <a name="programmatically-build-up-string-content"></a>以编程方式生成字符串内容

由于字符串是不可变的，因此前面的示例都创建了临时字符串或字符数组。 在高性能方案中，可能需要避免这些堆分配。 .NET Core 提供了一种 <xref:System.String.Create%2A?displayProperty=nameWithType> 方法，该方法使你可以通过回调以编程方式填充字符串的字符内容，同时避免中间的临时字符串分配。

:::code language="csharp" source="../../../samples/snippets/csharp/how-to/strings/ModifyStrings.cs" id="Snippet7":::

可以使用不安全的代码修改固定块中的字符串，但是强烈建议不要在创建字符串后修改字符串内容。 这样做将以不可预知的方式中断操作。 例如，如果某人暂存一个与你的内容相同的字符串，他们将获得你的副本，并且不希望你修改他们的字符串。

## <a name="see-also"></a>请参阅

- [.NET 正则表达式](../../standard/base-types/regular-expressions.md)
- [正则表达式语言 - 快速参考](../../standard/base-types/regular-expression-language-quick-reference.md)
