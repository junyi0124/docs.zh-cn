---
title: 使用 String.Split 拆分字符串（C# 指南）
description: Split 方法返回从一组分隔符中拆分的字符串数组。 这是从字符串中提取子字符串的一种简单方法。
ms.date: 01/03/2018
helpviewer_keywords:
- splitting strings [C#]
- Split method [C#]
- strings [C#], splitting
- parse strings
ms.assetid: 729c2923-4169-41c6-9c90-ef176c1e2953
ms.custom: mvc
ms.openlocfilehash: 5361a3c60905edd19b180c5ddb14064a85f64337
ms.sourcegitcommit: 48466b8fb7332ececff5dc388f19f6b3ff503dd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2020
ms.locfileid: "93400495"
---
# <a name="how-to-separate-strings-using-stringsplit-in-c"></a>如何在 C\# 中使用 String.Split 分隔字符串

<xref:System.String.Split%2A?displayProperty=nameWithType> 方法通过基于一个或多个分隔符拆分输入字符串来创建子字符串数组。 此方法通常是分隔字边界上的字符串的最简单方法。 它也用于拆分其他特定字符或字符串上的字符串。

[!INCLUDE[interactive-note](~/includes/csharp-interactive-note.md)]

下方代码将一个常用短语拆分为一个由每个单词组成的字符串数组。

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/ParseStringsUsingSplit.cs" id="Snippet1":::

分隔符的每个实例都会在返回的数组中产生一个值。 连续的分隔符将生成空字符串作为返回的数组中的值。 下面的示例介绍如何创建空字符串，该示例使用空格字符作为分隔符。

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/ParseStringsUsingSplit.cs" id="Snippet2":::

该行为可以更容易地用逗号分隔值 (CSV) 文件之类的格式表示表格数据。 连续的逗号表示空白列。

可传递可选 <xref:System.StringSplitOptions.RemoveEmptyEntries?displayProperty=nameWithType> 参数来排除返回数组中的任何空字符串。 要对返回的集合进行更复杂的处理，可使用 [LINQ](../programming-guide/concepts/linq/index.md) 来处理结果序列。

<xref:System.String.Split%2A?displayProperty=nameWithType> 可使用多个分隔符。
下面的示例使用空格、逗号、句点、冒号和制表符作为分隔字符，这些分隔字符在数组中传递到 <xref:System.String.Split%2A>。
代码底部的循环显示返回数组中的每个单词。

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/ParseStringsUsingSplit.cs" id="Snippet3":::

任何分隔符的连续实例都会在输出数组中生成空字符串：

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/ParseStringsUsingSplit.cs" id="Snippet4":::

<xref:System.String.Split%2A?displayProperty=nameWithType> 可采用字符串数组（充当用于分析目标字符串的分隔符的字符序列，而非单个字符）。

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/ParseStringsUsingSplit.cs" id="Snippet5":::

## <a name="see-also"></a>请参阅

- [从字符串中提取元素](../../standard/base-types/divide-up-strings.md)
- [C# 编程指南](../programming-guide/index.md)
- [字符串](../programming-guide/strings/index.md)
- [.NET 正则表达式](../../standard/base-types/regular-expressions.md)
