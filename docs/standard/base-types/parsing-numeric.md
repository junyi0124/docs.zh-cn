---
title: 分析 .NET 中的数字字符串
description: 了解如何在 .NET 中分析数字字符串。 了解如何用格式提供程序、NumberStyles 枚举值和 Unicode 数字进行分析。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- parsing strings, numeric strings
- numeric strings
- enumerations [.NET], parsing strings
- base types, parsing strings
ms.assetid: e39324ee-72e5-42d4-a80d-bf3ee7fc6c59
ms.openlocfilehash: 1339301786ed0f7ddd41565ca3fc64c2a859b3f4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95683753"
---
# <a name="parsing-numeric-strings-in-net"></a>分析 .NET 中的数字字符串

所有数字类型都具有两个静态分析方法（`Parse` 和 `TryParse`），可以使用它们将数字的字符串表示形式转换为数字类型。 这两个方法使你可以分析使用[标准数字格式字符串](standard-numeric-format-strings.md)和[自定义数字格式字符串](custom-numeric-format-strings.md)中所述的格式字符串生成的字符串。 默认情况下，`Parse` 和 `TryParse` 方法可以成功地将仅包含整数十进制数字的字符串转化为整数值。 它们可以将包含整数和小数十进制数字、组分隔符和十进制分隔符的字符串转换为浮点值。 `Parse` 方法在操作失败时引发异常，而 `TryParse` 方法返回 `false`。  
  
## <a name="parsing-and-format-providers"></a>分析和格式提供程序  

 通常，数值的字符串表示因区域性而异。 数值字符串的元素都会因区域性而异，如货币符号、组（或千位）分隔符和十进制分隔符。 分析方法可隐式或显式使用可以识别这些特定于区域性的变体的格式提供程序。 如果在 `Parse` 或 `TryParse` 方法调用中未指定任何格式提供程序，使用的是与当前线程区域性关联的格式提供程序（<xref:System.Globalization.NumberFormatInfo.CurrentInfo%2A?displayProperty=nameWithType> 属性返回的 <xref:System.Globalization.NumberFormatInfo> 对象）。  
  
 格式提供程序由 <xref:System.IFormatProvider> 实现表示。 此接口包含一个成员，即 <xref:System.IFormatProvider.GetFormat%2A> 方法；它需要使用一个参数，即表示要设置格式的类型的 <xref:System.Type> 对象。 此方法返回提供格式设置信息的对象。 .NET 支持以下两个 <xref:System.IFormatProvider> 实现，用于分析数字字符串：  
  
- <xref:System.Globalization.CultureInfo> 对象，它的 <xref:System.Globalization.CultureInfo.GetFormat%2A?displayProperty=nameWithType> 方法返回 <xref:System.Globalization.NumberFormatInfo> 对象，提供区域性专用格式设置信息。  
  
- <xref:System.Globalization.NumberFormatInfo> 对象，它的 <xref:System.Globalization.NumberFormatInfo.GetFormat%2A?displayProperty=nameWithType> 方法返回自己本身。  
  
 下面的示例尝试将数组中的每个字符串转换为 <xref:System.Double> 值。 它首先尝试使用反映“英语(美国)”区域性约定的格式提供程序来分析字符串。 如果此操作抛出 <xref:System.FormatException>，就会尝试使用反映“法语(法国)”区域性约定的格式提供程序分析字符串。  
  
 [!code-csharp[Parsing.Numbers#1](../../../samples/snippets/csharp/VS_Snippets_CLR/parsing.numbers/cs/formatproviders1.cs#1)]
 [!code-vb[Parsing.Numbers#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/parsing.numbers/vb/formatproviders1.vb#1)]  
  
## <a name="parsing-and-numberstyles-values"></a>分析和 NumberStyles 值  

 分析操作可以处理的样式元素（如空格、组分隔符和十进制分隔符）由 <xref:System.Globalization.NumberStyles> 枚举值定义。 默认情况下，表示整数值的字符串是使用 <xref:System.Globalization.NumberStyles.Integer?displayProperty=nameWithType> 值进行分析，此值仅允许数字、前导和尾随空格以及前导符号。 表示浮点值的字符串是通过结合使用 <xref:System.Globalization.NumberStyles.Float?displayProperty=nameWithType> 和 <xref:System.Globalization.NumberStyles.AllowThousands?displayProperty=nameWithType> 值进行分析；此复合样式允许数字以及前导和尾随空格、前导符号、十进制分隔符、组分隔符和指数。 通过调用包含 <xref:System.Globalization.NumberStyles> 类型参数的 `Parse` 或 `TryParse` 方法重载，并设置一个或多个 <xref:System.Globalization.NumberStyles> 标志，可以控制字符串中能够包含的样式元素，以便分析操作成功。  
  
 例如，包含组分隔符的字符串使用 <xref:System.Int32> 方法不能转换为 <xref:System.Int32.Parse%28System.String%29?displayProperty=nameWithType> 值。 但是，如果使用 <xref:System.Globalization.NumberStyles.AllowThousands?displayProperty=nameWithType> 标记，可成功转换，如下面的示例所示。  
  
 [!code-csharp[Parsing.Numbers#2](../../../samples/snippets/csharp/VS_Snippets_CLR/parsing.numbers/cs/styles1.cs#2)]
 [!code-vb[Parsing.Numbers#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/parsing.numbers/vb/styles1.vb#2)]  
  
> [!WARNING]
> 分析操作始终使用特定区域性的格式设置约定。 如果未通过传递 <xref:System.Globalization.CultureInfo> 或 <xref:System.Globalization.NumberFormatInfo> 对象来指定区域性，使用的是与当前线程关联的区域性。  
  
 下表列出了 <xref:System.Globalization.NumberStyles> 枚举成员及其对分析操作的影响。  
  
|NumberStyles 值|对要分析的字符串的影响|  
|------------------------|---------------------------------------|  
|<xref:System.Globalization.NumberStyles.None?displayProperty=nameWithType>|仅允许数字。|  
|<xref:System.Globalization.NumberStyles.AllowDecimalPoint?displayProperty=nameWithType>|允许十进制分隔符和小数数字。 对于整数值，仅允许将零作为小数数字。 有效的十进制分隔符是由 <xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A?displayProperty=nameWithType> 或 <xref:System.Globalization.NumberFormatInfo.CurrencyDecimalSeparator%2A?displayProperty=nameWithType> 属性确定。|  
|<xref:System.Globalization.NumberStyles.AllowExponent?displayProperty=nameWithType>|“e”或“E”字符可以用于指示指数记数法。 有关详细信息，请参阅 <xref:System.Globalization.NumberStyles>。|  
|<xref:System.Globalization.NumberStyles.AllowLeadingWhite?displayProperty=nameWithType>|允许前导空格。|  
|<xref:System.Globalization.NumberStyles.AllowTrailingWhite?displayProperty=nameWithType>|允许尾随空格。|  
|<xref:System.Globalization.NumberStyles.AllowLeadingSign?displayProperty=nameWithType>|正号或负号可以位于数字前面。|  
|<xref:System.Globalization.NumberStyles.AllowTrailingSign?displayProperty=nameWithType>|正号或负号可以位于数字后面。|  
|<xref:System.Globalization.NumberStyles.AllowParentheses?displayProperty=nameWithType>|括号可以用于指示负值。|  
|<xref:System.Globalization.NumberStyles.AllowThousands?displayProperty=nameWithType>|允许组分隔符。 组分隔符是由 <xref:System.Globalization.NumberFormatInfo.NumberGroupSeparator%2A?displayProperty=nameWithType> 或 <xref:System.Globalization.NumberFormatInfo.CurrencyGroupSeparator%2A?displayProperty=nameWithType> 属性确定。|  
|<xref:System.Globalization.NumberStyles.AllowCurrencySymbol?displayProperty=nameWithType>|允许货币符号。 货币符号是由 <xref:System.Globalization.NumberFormatInfo.CurrencySymbol%2A?displayProperty=nameWithType> 属性定义。|  
|<xref:System.Globalization.NumberStyles.AllowHexSpecifier?displayProperty=nameWithType>|要分析的字符串解释为十六进制数。 它可以包含十六进制数 0-9、A-F 和 a-f。 此标志只能用于分析整数值。|  
  
 此外，<xref:System.Globalization.NumberStyles> 枚举提供以下包括多个 <xref:System.Globalization.NumberStyles> 标志的复合样式。  
  
|复合 NumberStyles 值|包括成员|  
|----------------------------------|----------------------|  
|<xref:System.Globalization.NumberStyles.Integer?displayProperty=nameWithType>|包括 <xref:System.Globalization.NumberStyles.AllowLeadingWhite?displayProperty=nameWithType>、<xref:System.Globalization.NumberStyles.AllowTrailingWhite?displayProperty=nameWithType> 和 <xref:System.Globalization.NumberStyles.AllowLeadingSign?displayProperty=nameWithType> 样式。 这是用于分析整数值的默认样式。|  
|<xref:System.Globalization.NumberStyles.Number?displayProperty=nameWithType>|包括 <xref:System.Globalization.NumberStyles.AllowLeadingWhite?displayProperty=nameWithType>、<xref:System.Globalization.NumberStyles.AllowTrailingWhite?displayProperty=nameWithType>、<xref:System.Globalization.NumberStyles.AllowLeadingSign?displayProperty=nameWithType>、<xref:System.Globalization.NumberStyles.AllowTrailingSign?displayProperty=nameWithType>、<xref:System.Globalization.NumberStyles.AllowDecimalPoint?displayProperty=nameWithType> 和 <xref:System.Globalization.NumberStyles.AllowThousands?displayProperty=nameWithType> 样式。|  
|<xref:System.Globalization.NumberStyles.Float?displayProperty=nameWithType>|包括 <xref:System.Globalization.NumberStyles.AllowLeadingWhite?displayProperty=nameWithType>、<xref:System.Globalization.NumberStyles.AllowTrailingWhite?displayProperty=nameWithType>、<xref:System.Globalization.NumberStyles.AllowLeadingSign?displayProperty=nameWithType>、<xref:System.Globalization.NumberStyles.AllowDecimalPoint?displayProperty=nameWithType> 和 <xref:System.Globalization.NumberStyles.AllowExponent?displayProperty=nameWithType> 样式。|  
|<xref:System.Globalization.NumberStyles.Currency?displayProperty=nameWithType>|包括除 <xref:System.Globalization.NumberStyles.AllowExponent?displayProperty=nameWithType> 和 <xref:System.Globalization.NumberStyles.AllowHexSpecifier?displayProperty=nameWithType> 之外的所有样式。|  
|<xref:System.Globalization.NumberStyles.Any?displayProperty=nameWithType>|包括除 <xref:System.Globalization.NumberStyles.AllowHexSpecifier?displayProperty=nameWithType> 之外的所有样式。|  
|<xref:System.Globalization.NumberStyles.HexNumber?displayProperty=nameWithType>|包括 <xref:System.Globalization.NumberStyles.AllowLeadingWhite?displayProperty=nameWithType>、<xref:System.Globalization.NumberStyles.AllowTrailingWhite?displayProperty=nameWithType> 和 <xref:System.Globalization.NumberStyles.AllowHexSpecifier?displayProperty=nameWithType> 样式。|  
  
## <a name="parsing-and-unicode-digits"></a>分析和 Unicode 数字  

 Unicode 标准为各种书写系统中的数字定义码位。 例如，从 U+0030 到 U+0039 的码位表示从 0 到 9 的基本拉丁文数字，从 U+09E6 到 U+09EF 的码位表示从 0 到 9 的孟加拉文数字，而从 U+FF10 到 U+FF19 的码位表示从 0 到 9 的全角数字。 但是，分析方法唯一可识别的数字是具有 U+0030 到 U+0039 的码位的基本拉丁文数字 0-9。 如果向数字分析方法传递包含其他任何数字的字符串，此方法会抛出 <xref:System.FormatException>。  
  
 下面的示例使用 <xref:System.Int32.Parse%2A?displayProperty=nameWithType> 方法，分析包含不同书写系统中数字的字符串。 正如示例输出所示，虽然尝试分析基本拉丁文数字获得成功，但分析全角、阿拉伯 - 印度文以及孟加拉文数字的尝试将失败。  
  
 [!code-csharp[Parsing.Numbers#3](../../../samples/snippets/csharp/VS_Snippets_CLR/parsing.numbers/cs/unicode1.cs#3)]
 [!code-vb[Parsing.Numbers#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/parsing.numbers/vb/unicode1.vb#3)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.Globalization.NumberStyles>
- [分析字符串](parsing-strings.md)
- [格式设置类型](formatting-types.md)
