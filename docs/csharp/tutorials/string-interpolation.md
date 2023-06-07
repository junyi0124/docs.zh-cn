---
title: C# 中的字符串内插
description: 了解如何在 C# 中使用字符串插值将有格式的表达式结果包括在结果字符串中。
author: pkulikov
ms.technology: csharp-fundamentals
ms.date: 09/02/2019
ms.openlocfilehash: b901ae661ebd4af625d9f3c999b0eb50dda1990d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "73039214"
---
# <a name="string-interpolation-in-c"></a>C\# 中的字符串内插

本教程演示如何使用[字符串插值](../language-reference/tokens/interpolated.md)设置表达式结果的格式并将其包含仅结果字符串中。 以下示例假设阅读者熟悉基础 C# 概念和 .NET 类型格式设置。 如果不熟悉字符串插值或 .NET 类型格式设置，请先参阅[交互式字符串内插教程](exploration/interpolated-strings.yml)。 有关 .NET 中设置类型格式的详细信息，请参阅[设置 .NET 中类型的格式](../../standard/base-types/formatting-types.md)主题。

[!INCLUDE[interactive-note](~/includes/csharp-interactive-note.md)]

## <a name="introduction"></a>介绍

[字符串插值](../language-reference/tokens/interpolated.md)功能构建在[复合格式设置](../../standard/base-types/composite-formatting.md)功能的基础之上，提供更具有可读性、更方便的语法，用于将设置了格式的表达式结果包括到结果字符串。

若要将字符串标识为内插字符串，可在该字符串前面加上 `$` 符号。 可嵌入任何会在内插字符串中返回值的有效 C# 表达式。 在以下示例中，对某个表达式执行计算后，其结果立即转换为一个字符串并包含到结果字符串中：

[!code-csharp-interactive[string interpolation example](~/samples/snippets/csharp/tutorials/string-interpolation/Program.cs#1)]

如示例所示，通过将表达式用大括号括起来，可将表达式包含到内插字符串中：

```csharp
{<interpolationExpression>}
```

内插字符串支持[字符串复合格式设置](../../standard/base-types/composite-formatting.md)功能的所有功能。 这使得它们成为 <xref:System.String.Format%2A?displayProperty=nameWithType> 方法的更具可读性的替代选项。

## <a name="how-to-specify-a-format-string-for-an-interpolation-expression"></a>如何为内插表达式指定格式字符串

可通过在内插表达式后接冒号（“:”）和格式字符串来指定受表达式结果类型支持的格式字符串：

```csharp
{<interpolationExpression>:<formatString>}
```

以下示例演示如何为生成日期和时间或数值结果的表达式指定标准和自定义格式字符串：

[!code-csharp-interactive[format string example](~/samples/snippets/csharp/tutorials/string-interpolation/Program.cs#2)]

有关详细信息，请参阅[复合格式设置](../../standard/base-types/composite-formatting.md)主题的[格式字符串组件](../../standard/base-types/composite-formatting.md#format-string-component)章节。 该部分提供主题链接，这些主题介绍 .NET 基类型支持的标准和自定义格式字符串。

## <a name="how-to-control-the-field-width-and-alignment-of-the-formatted-interpolation-expression"></a>如何控制设置了格式的内插表达式的字段宽度和对齐方式

通过在内插表达式后添加逗号（“,”）和常数表达式来指定设置了格式的表达式结果的最小字段宽度和对齐方式：

```csharp
{<interpolationExpression>,<alignment>}
```

如果对齐方式值为正，则设置了格式的表达式结果为右对齐，如果为负，则为左对齐  。

如果需要同时指定对齐方式和格式字符串，则先从对齐方式组件开始：

```csharp
{<interpolationExpression>,<alignment>:<formatString>}
```

以下示例演示如何指定对齐方式并使用管道字符 ("|") 分隔文本字段：

[!code-csharp-interactive[alignment example](~/samples/snippets/csharp/tutorials/string-interpolation/Program.cs#3)]

如示例输出所示，如果已设置格式的表达式结果长度超出指定字段宽度，则忽略对齐方式值  。

有关详细信息，请参阅[复合格式设置](../../standard/base-types/composite-formatting.md)主题的[对齐方式组件](../../standard/base-types/composite-formatting.md#alignment-component)部分。

## <a name="how-to-use-escape-sequences-in-an-interpolated-string"></a>如何在内插字符串中使用转义序列

内插字符串支持所有可在普通字符串文本中使用的转义序列。 有关详细信息，请参阅[字符串转义序列](../programming-guide/strings/index.md#string-escape-sequences)。

若要逐字解释转义序列，可使用[逐字](../language-reference/tokens/verbatim.md)字符串文本。 内插逐字字符串以 `$` 字符开头，后跟 `@` 字符。 从 C# 8.0 开始，可以按任意顺序使用 `$` 和 `@` 标记：`$@"..."` 和 `@$"..."` 均为有效的内插逐字字符串。

若要在结果字符串中包含大括号 "{" 或 "}"，请使用两个大括号 "{{" 或 "}}"。 有关详细信息，请参阅[复合格式设置](../../standard/base-types/composite-formatting.md)主题的[转义括号](../../standard/base-types/composite-formatting.md#escaping-braces)部分。

以下示例演示如何在结果字符串中包含大括号并构造逐字内插字符串：

[!code-csharp-interactive[escape sequence example](~/samples/snippets/csharp/tutorials/string-interpolation/Program.cs#4)]

## <a name="how-to-use-a-ternary-conditional-operator--in-an-interpolation-expression"></a>如何在内插表达式中使用三元条件运算符 `?:`

因为冒号 (:) 在具有内插表达式的项中具有特殊含义，为了在表达式中使用[条件运算符](../language-reference/operators/conditional-operator.md)，请将表达式放在括号内，如下例所示：

[!code-csharp-interactive[conditional operator example](~/samples/snippets/csharp/tutorials/string-interpolation/Program.cs#5)]

## <a name="how-to-create-a-culture-specific-result-string-with-string-interpolation"></a>如何使用字符串插值创建区域性特定的结果字符串

默认情况下，内插字符串将 <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=nameWithType> 属性定义的当前区域性用于所有格式设置操作。 使用内插字符串到 <xref:System.FormattableString?displayProperty=nameWithType> 实例的隐式转换，并调用它的 <xref:System.FormattableString.ToString(System.IFormatProvider)> 方法来创建区域性特定的结果字符串。 下面的示例演示如何执行此操作：

[!code-csharp-interactive[specify different cultures](~/samples/snippets/csharp/tutorials/string-interpolation/Program.cs#6)]

如示例所示，可使用某个 <xref:System.FormattableString> 实例为各种区域性生成多个结果字符串。

## <a name="how-to-create-a-result-string-using-the-invariant-culture"></a>如何使用固定区域性创建结果字符串

可配合 <xref:System.FormattableString.ToString(System.IFormatProvider)?displayProperty=nameWithType> 方法使用静态 <xref:System.FormattableString.Invariant%2A?displayProperty=nameWithType> 方法将内插字符串解析为 <xref:System.Globalization.CultureInfo.InvariantCulture> 的结果字符串。 下面的示例演示如何执行此操作：

[!code-csharp-interactive[format with invariant culture](~/samples/snippets/csharp/tutorials/string-interpolation/Program.cs#7)]

## <a name="conclusion"></a>结束语

本教程介绍字符串插值用法的常见方案。 有关字符串插值的详细信息，请参阅[字符串插值](../language-reference/tokens/interpolated.md)主题。 有关 .NET 中设置类型格式的详细信息，请参阅[设置 .NET 中类型的格式](../../standard/base-types/formatting-types.md)和[复合格式设置](../../standard/base-types/composite-formatting.md)主题。

## <a name="see-also"></a>另请参阅

- <xref:System.String.Format%2A?displayProperty=nameWithType>
- <xref:System.FormattableString?displayProperty=nameWithType>
- <xref:System.IFormattable?displayProperty=nameWithType>
- [字符串](../programming-guide/strings/index.md)
