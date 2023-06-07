---
title: 如何连接多个字符串（C# 指南）
description: 可以在 C# 中通过多种方法串联字符串。 了解多种选项和进行不同选择的原因。
ms.date: 02/20/2018
helpviewer_keywords:
- joining strings [C#]
- concatenating strings [C#]
- strings [C#], concatenation
ms.assetid: 8e16736f-4096-4f3f-be0f-9d4c3ff63520
ms.openlocfilehash: f2aae14deac967a833fb3510acdb32e0971485b5
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90537478"
---
# <a name="how-to-concatenate-multiple-strings-c-guide"></a>如何连接多个字符串（C# 指南）

串联是将一个字符串追加到另一字符串末尾的过程。  可使用 `+` 运算符连接字符串。 对于字符串文本和字符串常量，会在编译时进行串联，运行时不串联。 对于字符串变量，仅在运行时串联。

[!INCLUDE[interactive-note](~/includes/csharp-interactive-note.md)]

以下示例通过串联将长字符串文本拆分为较短的字符串，从而提高源代码的可读性。 编译时将这些部分连接到单个字符串中。 无论涉及到多少个字符串，均不产生运行时性能开销。

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/Concatenate.cs" id="Snippet1":::

若要连接字符串变量，可使用 `+` 或 `+=` 运算符、[字符串内插](../language-reference/tokens/interpolated.md)或 <xref:System.String.Format%2A?displayProperty=nameWithType>、<xref:System.String.Concat%2A?displayProperty=nameWithType>、<xref:System.String.Join%2A?displayProperty=nameWithType>、<xref:System.Text.StringBuilder.Append%2A?displayProperty=nameWithType> 方法。 `+` 运算符易于使用，有利于产生直观代码。 即使在一个语句中使用多个 `+` 运算符，字符串内容也仅会被复制一次。 以下代码演示使用 `+` 和 `+=` 运算符串联字符串的示例：

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/Concatenate.cs" id="Snippet2":::

在某些表达式中，使用字符串内插进行字符串串联更简单，如以下代码所示：

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/Concatenate.cs" id="Snippet3":::

> [!NOTE]
> 在字符串串联操作中，C# 编译器将 null 字符串视为空字符串进行处理。

另一个字符串连接方法为 <xref:System.String.Format%2A?displayProperty=nameWithType>。 此方法非常适用于从少量组件字符串生成字符串的情况。

在其他情况下，可能需要将字符串合并在循环中，此时不知道要合并的源字符串的数量，而且源字符串的实际数量可能很大。 <xref:System.Text.StringBuilder> 类专门用于此类方案。 以下代码使用 <xref:System.Text.StringBuilder> 类的 <xref:System.Text.StringBuilder.Append%2A> 方法串联字符串。

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/Concatenate.cs" id="Snippet4":::

有关详细信息，请阅读[选择字符串串联或 `StringBuilder` 类的原因](/dotnet/api/system.text.stringbuilder#the-string-and-stringbuilder-types)。

还可使用 <xref:System.String.Concat%2A?displayProperty=nameWithType> 方法联接集合中的字符串。 如果源字符串应使用分隔符分隔，请使用 <xref:System.String.Join%2A?displayProperty=nameWithType> 方法。 以下代码使用这两种方法合并单词数组：

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/Concatenate.cs" id="Snippet5":::

最后，可以使用 [LINQ](../programming-guide/concepts/linq/index.md) 和 <xref:System.Linq.Enumerable.Aggregate%2A?displayProperty=nameWithType> 方法联接集合中的字符串。 此方法利用 lambda 表达式合并源字符串。 lambda 表达式用于将所有字符串添加到现有累积。 下面的示例通过在数组中的每两个单词之间添加一个空格来合并一组单词：

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/Concatenate.cs" id="Snippet6":::

## <a name="see-also"></a>请参阅

- <xref:System.String>
- <xref:System.Text.StringBuilder>
- [C# 编程指南](../programming-guide/index.md)
- [字符串](../programming-guide/strings/index.md)
