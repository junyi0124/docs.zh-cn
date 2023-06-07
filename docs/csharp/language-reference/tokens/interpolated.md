---
title: $ - 字符串内插 - C# 参考
description: 与传统的字符串复合格式设置相比，字符串内插提供更具可读性且更方便的语法，用于设置字符串输出的格式。
ms.date: 09/02/2019
f1_keywords:
- $_CSharpKeyword
- $
helpviewer_keywords:
- $ special character [C#]
- string interpolation [C#]
- interpolated string [C#]
author: pkulikov
ms.openlocfilehash: 2b95fa5fe5cecd4825e8c17a33f7795c6c9480c6
ms.sourcegitcommit: 465547886a1224a5435c3ac349c805e39ce77706
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/21/2020
ms.locfileid: "81738376"
---
# <a name="---string-interpolation-c-reference"></a>$ - 字符串内插（C# 参考）

`$` 特殊字符将字符串文本标识为内插字符串  。 内插字符串是可能包含内插表达式的字符串文本  。 将内插字符串解析为结果字符串时，带有内插表达式的项会替换为表达式结果的字符串表示形式。 从 C# 6 开始可以使用此功能。

与使用[字符串复合格式设置](../../../standard/base-types/composite-formatting.md)功能创建格式化字符串相比，字符串内插提供的语法更具可读性，且更加方便。 下面的示例使用了这两种功能生成同样的输出结果：

[!code-csharp-interactive[compare with composite formatting](~/samples/snippets/csharp/language-reference/tokens/string-interpolation.cs#1)]

## <a name="structure-of-an-interpolated-string"></a>内插字符串的结构

若要将字符串标识为内插字符串，可在该字符串前面加上 `$` 符号。 字符串文本开头的 `$` 和 `"` 之间不能有任何空格。

具备内插表达式的项的结构如下所示：

```csharp
{<interpolationExpression>[,<alignment>][:<formatString>]}
```

括号中的元素是可选的。 下表说明了每个元素：

|元素|描述|
|-------------|-----------------|
|`interpolationExpression`|生成需要设置格式的结果的表达式。 `null` 的字符串表示形式为 <xref:System.String.Empty?displayProperty=nameWithType>。|
|`alignment`|常数表达式，它的值定义表达式结果的字符串表示形式中的最小字符数。 如果值为正，则字符串表示形式为右对齐；如果值为负，则为左对齐。 有关详细信息，请参阅[对齐组件](../../../standard/base-types/composite-formatting.md#alignment-component)。|
|`formatString`|受表达式结果类型支持的格式字符串。 有关更多信息，请参阅[格式字符串组件](../../../standard/base-types/composite-formatting.md#format-string-component)。|

以下示例使用上述可选的格式设置组件：

[!code-csharp-interactive[specify alignment and format string](~/samples/snippets/csharp/language-reference/tokens/string-interpolation.cs#2)]

## <a name="special-characters"></a>特殊字符

要在内插字符串生成的文本中包含大括号 "{" 或 "}"，请使用两个大括号，即 "{{" 或 "}}"。 有关详细信息，请参阅[转义大括号](../../../standard/base-types/composite-formatting.md#escaping-braces)。

因为冒号（“:”）在内插表达式项中具有特殊含义，为了在内插表达式中使用[条件运算符](../operators/conditional-operator.md)，请将表达式放在括号内。

以下示例演示如何将大括号含入结果字符串中，以及如何在内插表达式中使用条件运算符：

[!code-csharp-interactive[example with ternary conditional operator](~/samples/snippets/csharp/language-reference/tokens/string-interpolation.cs#3)]

内插逐字字符串以 `$` 字符开头，后跟 `@` 字符。 有关逐字字符串的详细信息，请参阅[字符串](../builtin-types/reference-types.md)和[逐字标识符](verbatim.md)主题。

> [!NOTE]
> 从 C# 8.0 开始，可以按任意顺序使用 `$` 和 `@` 标记：`$@"..."` 和 `@$"..."` 均为有效的内插逐字字符串。 在早期 C# 版本中，`$` 标记必须出现在 `@` 标记之前。

## <a name="implicit-conversions-and-how-to-specify-iformatprovider-implementation"></a>隐式转换和指定 `IFormatProvider` 实现的方式

内插字符串有 3 种隐式转换：

1. 将内插字符串转换为 <xref:System.String> 实例，该类例是内插字符串的解析结果，其中内插表达式项被替换为结果的格式设置正确的字符串表示形式。 此转换使用 <xref:System.Globalization.CultureInfo.CurrentCulture> 设置表达式结果的格式。

1. 将内插字符串转换为表示复合格式字符串的 <xref:System.FormattableString> 实例，同时也将表达式结果格式化。 这允许通过单个 <xref:System.FormattableString> 实例创建多个包含区域性特定内容的结果字符串。 要执行此操作，请调用以下方法之一：

      - <xref:System.FormattableString.ToString> 重载，生成 <xref:System.Globalization.CultureInfo.CurrentCulture> 的结果字符串。
      - <xref:System.FormattableString.Invariant%2A> 方法，生成 <xref:System.Globalization.CultureInfo.InvariantCulture> 的结果字符串。
      - <xref:System.FormattableString.ToString(System.IFormatProvider)> 方法，生成特定区域性的结果字符串。

    还可以使用 <xref:System.FormattableString.ToString(System.IFormatProvider)> 方法，以提供支持自定义格式设置的 <xref:System.IFormatProvider> 接口的用户定义实现。 有关详细信息，请参阅[在 .NET 中设置类型格式](../../../standard/base-types/formatting-types.md)一文中的[使用 ICustomFormatter 进行自定义格式设置](../../../standard/base-types/formatting-types.md#custom-formatting-with-icustomformatter)部分。

1. 将内插字符串转换为 <xref:System.IFormattable> 实例，使用此实例也可通过单个 <xref:System.IFormattable> 实例创建多个包含区域性特定内容的结果字符串。

以下示例通过隐式转换为 <xref:System.FormattableString> 来创建特定于区域性的结果字符串：

[!code-csharp-interactive[create culture-specific result strings](~/samples/snippets/csharp/language-reference/tokens/string-interpolation.cs#4)]

## <a name="additional-resources"></a>其他资源

如果你不熟悉字符串内插，请参阅 [C# 中的字符串内插](../../tutorials/exploration/interpolated-strings.yml)交互式教程。 还可查看另一个 [C# 中的字符串内插](../../tutorials/string-interpolation.md)教程，该教程演示了如何使用内插字符串生成带格式的字符串。

## <a name="compilation-of-interpolated-strings"></a>内插字符串编译

如果内插字符串类型为 `string`，则通常将其转换为 <xref:System.String.Format%2A?displayProperty=nameWithType> 方法调用。 如果分析的行为等同于串联，则编译器可将 <xref:System.String.Format%2A?displayProperty=nameWithType> 替换为 <xref:System.String.Concat%2A?displayProperty=nameWithType>。

如果内插字符串类型为 <xref:System.IFormattable> 或 <xref:System.FormattableString>，则编译器会生成对 <xref:System.Runtime.CompilerServices.FormattableStringFactory.Create%2A?displayProperty=nameWithType> 方法的调用。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的[内插字符串](~/_csharplang/spec/expressions.md#interpolated-strings)部分。

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 特殊字符](index.md)
- [字符串](../../programming-guide/strings/index.md)
- [标准数字格式字符串](../../../standard/base-types/standard-numeric-format-strings.md)
- [复合格式设置](../../../standard/base-types/composite-formatting.md)
- <xref:System.String.Format%2A?displayProperty=nameWithType>
