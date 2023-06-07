---
title: 如何：用前导零填充数字
description: 了解如何用前导零填充数字。 将前导零添加到整数或将数值添加到特定的总长度或特定数量的前导零。
ms.date: 02/25/2019
dev_langs:
- csharp
- vb
helpviewer_keywords:
- numeric format strings [.NET]
- formatting [.NET], numbers
- number formatting [.NET]
- numbers [.NET], format strings
ms.assetid: 0b2c2cb5-c580-4891-8d81-cb632f5ec384
ms.openlocfilehash: 911d6059d3594ca35eb1c1c3e2d29a5684bce738
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94822043"
---
# <a name="how-to-pad-a-number-with-leading-zeros"></a>如何：用前导零填充数字

通过结合使用“D”[标准数字格式字符串](standard-numeric-format-strings.md)和精度说明符，将前导零添加到整数。 你可以通过使用[自定义数字格式字符串](custom-numeric-format-strings.md)，将前导零添加到整数和浮点数。 本文介绍如何通过这两种方法用前导零填充数字。

## <a name="to-pad-an-integer-with-leading-zeros-to-a-specific-length"></a>使用前导零将整数填充到特定的长度

1. 确定整数值要显示的最小位数。 在此数字中包括任何前导位。

1. 确定是要将整数显示为十进制值还是十六进制值。

    - 若要将整数显示为十进制值，则调用其 `ToString(String)` 方法，并传递字符串“Dn”作为 `format` 参数的值，其中 n 表示字符串的最小长度 。

    - 若要将整数显示为十六进制值，则调用其 `ToString(String)` 方法，并传递字符串“Xn”作为 format 参数的值，其中 n 表示字符串的最小长度。

此外，你也可以在 [C#](../../csharp/language-reference/tokens/interpolated.md) 和 [Visual Basic](../../visual-basic/programming-guide/language-features/strings/interpolated-strings.md) 的内插字符串中使用格式字符串，或者可以调用一个 <xref:System.String.Format%2A?displayProperty=nameWithType> 或 <xref:System.Console.WriteLine%2A?displayProperty=nameWithType> 等使用[复合格式设置](composite-formatting.md)的方法。

以下示例使用前导零设置若干整数值的格式，以便格式化数字的总长度至少为八个字符。

[!code-csharp[Formatting.HowTo.PadNumber#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.PadNumber/cs/Pad1.cs#1)]
[!code-vb[Formatting.HowTo.PadNumber#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.PadNumber/vb/Pad1.vb#1)]

## <a name="to-pad-an-integer-with-a-specific-number-of-leading-zeros"></a>使用特定数目的前导零填充整数

1. 确定希望整数值显示多少个前导零。

1. 确定是要将整数显示为十进制值还是十六进制值。

    - 将其格式设置为十进制值需要使用“D”标准格式说明符。

    - 将其格式设置为十六进制值需要使用“X”标准格式说明符。

1. 通过调用整数值的 `ToString("D").Length` 或 `ToString("X").Length` 方法，确定未填充的数字字符串的长度。

1. 将你要在格式化字符串中包括的前导零的数目添加到未填充的数字字符串的长度上。 添加前导零的个数将定义填充字符串的总长度。

1. 调用整数值的 `ToString(String)` 方法，并且对于十进制字符串传递字符串“Dn”，对于十六进制字符串传递“Xn”；其中，n 表示填充的字符串的总长度。 也可以在支持复合格式设置的方法中使用“Dn”或“Xn”格式字符串。

下面的示例使用五个前导零来填充整数值。

[!code-csharp[Formatting.HowTo.PadNumber#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.PadNumber/cs/Pad1.cs#2)]
[!code-vb[Formatting.HowTo.PadNumber#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.PadNumber/vb/Pad1.vb#2)]

## <a name="to-pad-a-numeric-value-with-leading-zeros-to-a-specific-length"></a>使用前导零将数值填充到特定的长度

1. 确定数字的字符串表示形式要在小数点的左侧保留的位数。 在此总位数中包括任何前导零。

1. 定义一个使用零占位符“0”来表示零的最小数目的自定义数字格式字符串。

1. 调用数字的 `ToString(String)` 方法并向其传递自定义格式字符串。 此外，你也可以将自定义格式字符串与字符串内插或与支持复合格式设置的方法一起使用。

下面的示例使用前导零设置几个数值的格式。 因此，格式化后的数字总长度是小数点左侧至少八位数字。

[!code-csharp[Formatting.HowTo.PadNumber#3](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.PadNumber/cs/Pad1.cs#3)]
[!code-vb[Formatting.HowTo.PadNumber#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.PadNumber/vb/Pad1.vb#3)]

## <a name="to-pad-a-numeric-value-with-a-specific-number-of-leading-zeros"></a>使用特定数目的前导零填充数值

1. 确定希望数值具有多少个前导零。

1. 确定未填充数字字符串中小数点左侧的位数：

    1. 确定数字的字符串表示形式是否包括小数点符号。

    1. 如果它包括小数点符号，则确定小数点左侧的字符数。

         \- 或 -

         如果它不包括小数点符号，则确定字符串的长度。

1. 创建使用以下格式的自定义格式字符串：

    - 零占位符“0”表示在字符串中出现的每个前导零。

    - 零占位符或数字占位符“#”表示默认字符串中的每个数字。

1. 将自定义格式字符串作为对数字的 `ToString(String)` 方法或支持复合格式设置的方法的参数提供。

下面的示例使用五个前导零来填充两个 <xref:System.Double> 值。

[!code-csharp[Formatting.HowTo.PadNumber#4](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.PadNumber/cs/Pad1.cs#4)]
[!code-vb[Formatting.HowTo.PadNumber#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.PadNumber/vb/Pad1.vb#4)]

## <a name="see-also"></a>请参阅

- [自定义数字格式字符串](custom-numeric-format-strings.md)
- [标准数字格式字符串](standard-numeric-format-strings.md)
- [复合格式设置](composite-formatting.md)
