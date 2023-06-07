---
title: 如何将字符串转换为数字 - C# 编程指南
description: 了解如何通过调用 Parse、TryParse 或 Convert 类方法，在 C# 中将字符串转换为数字。
ms.date: 11/20/2020
helpviewer_keywords:
- conversions [C#]
- conversions [C#], string to int
- converting strings to int [C#]
- strings [C#], converting to int
ms.topic: how-to
ms.custom: contperf-fy21q2
ms.assetid: 467b9979-86ee-4afd-b734-30299cda91e3
ms.openlocfilehash: 430887ebb16570439a89f3625ac12a1fbb368227
ms.sourcegitcommit: d0990c1c1ab2f81908360f47eafa8db9aa165137
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97513154"
---
# <a name="how-to-convert-a-string-to-a-number-c-programming-guide"></a>如何将字符串转换为数字（C# 编程指南）

可以调用各种数值类型（`int`、`long`、`double` 等）中找到的 `Parse` 或 `TryParse` 方法或使用 <xref:System.Convert?displayProperty=nameWithType> 类中的方法将[字符串](../../language-reference/builtin-types/reference-types.md)转换为数字。  
  
 调用 `TryParse` 方法（例如，[`int.TryParse("11", out number)`](xref:System.Int32.TryParse%2A)）或 `Parse` 方法（例如，[`var number = int.Parse("11")`](xref:System.Int32.Parse%2A)）会稍微高效和简单一些。  使用 <xref:System.Convert> 方法对于实现 <xref:System.IConvertible> 的常规对象更有用。  
  
 对预期字符串会包含的数值类型（如 <xref:System.Int32?displayProperty=nameWithType> 类型）使用 `Parse` 或 `TryParse` 方法。  <xref:System.Convert.ToInt32%2A?displayProperty=nameWithType> 方法在内部使用 <xref:System.Int32.Parse%2A>。  `Parse` 方法返回转换后的数字；`TryParse` 方法返回 <xref:System.Boolean> 值，该值指示转换是否成功，并以 [`out` 参数](../../language-reference/keywords/out.md)形式返回转换后的数字。 如果字符串的格式无效，则 `Parse` 会引发异常，但 `TryParse` 会返回 `false`。 调用 `Parse` 方法时，应始终使用异常处理来捕获分析操作失败时的 <xref:System.FormatException>。  
  
## <a name="calling-the-parse-and-tryparse-methods"></a>调用 Parse 和 TryParse 方法

`Parse` 和 `TryParse` 方法会忽略字符串开头和末尾的空格，但所有其他字符都必须是组成合适数值类型（`int`、`long`、`ulong`、`float`、`decimal` 等）的字符。  如果组成数字的字符串中有任何空格，都会导致错误。  例如，可以使用 `decimal.TryParse` 分析“10”、“10.3”或“  10  ”，但不能使用此方法分析从“10X”、“1 0”（注意嵌入的空格）、“10 .3”（注意嵌入的空格）、“10e1”（`float.TryParse` 在此处适用）等中分析出 10。 无法成功分析值为 `null` 或 <xref:System.String.Empty?displayProperty=nameWithType> 的字符串。 在尝试通过调用 <xref:System.String.IsNullOrEmpty%2A?displayProperty=nameWithType> 方法分析字符串之前，可以检查字符串是否为 Null 或为空。

下面的示例演示了对 `Parse` 和 `TryParse` 的成功调用和不成功的调用。  
  
[!code-csharp[Parse and TryParse](~/samples/snippets/csharp/programming-guide/string-to-number/parse-tryparse/program.cs)]  

下面的示例演示了一种分析字符串的方法，该字符串应包含前导数字字符（包括十六进制字符）和尾随的非数字字符。 在调用 <xref:System.Int32.TryParse%2A> 方法之前，它从字符串的开头向新字符串分配有效字符。 因为要分析的字符串包含少量字符，所以本示例调用 <xref:System.String.Concat%2A?displayProperty=nameWithType> 方法将有效字符分配给新字符串。 对于较大的字符串，可以改用 <xref:System.Text.StringBuilder> 类。
  
[!code-csharp[Removing invalid characters](~/samples/snippets/csharp/programming-guide/string-to-number/parse-tryparse2/program.cs)]  

## <a name="calling-the-convert-methods"></a>调用转换方法

下表列出了 <xref:System.Convert> 类中可用于将字符串转换为数字的一些方法。  
  
|数值类型|方法|  
|------------------|------------|  
|`decimal`|<xref:System.Convert.ToDecimal%28System.String%29>|  
|`float`|<xref:System.Convert.ToSingle%28System.String%29>|  
|`double`|<xref:System.Convert.ToDouble%28System.String%29>|  
|`short`|<xref:System.Convert.ToInt16%28System.String%29>|  
|`int`|<xref:System.Convert.ToInt32%28System.String%29>|  
|`long`|<xref:System.Convert.ToInt64%28System.String%29>|  
|`ushort`|<xref:System.Convert.ToUInt16%28System.String%29>|  
|`uint`|<xref:System.Convert.ToUInt32%28System.String%29>|  
|`ulong`|<xref:System.Convert.ToUInt64%28System.String%29>|  
  
 下面的示例调用 <xref:System.Convert.ToInt32%28System.String%29?displayProperty=nameWithType> 方法将输入字符串转换为 [int](../../language-reference/builtin-types/integral-numeric-types.md)。该示例将捕获此方法可能引发的最常见的两个异常：<xref:System.FormatException> 和 <xref:System.OverflowException>。 如果生成的数字可以在不超过 <xref:System.Int32.MaxValue?displayProperty=nameWithType> 的情况下递增，则示例将向结果添加 1 并显示输出。  
  
[!code-csharp[Parsing with Convert methods](~/samples/snippets/csharp/programming-guide/string-to-number/convert/program.cs)]  
  
## <a name="see-also"></a>请参阅

- [类型](./index.md)
- [如何确定字符串是否表示数值](../strings/how-to-determine-whether-a-string-represents-a-numeric-value.md)
- [示例：.NET Core WinForms 格式设置实用工具 (C#)](/samples/dotnet/samples/windowsforms-formatting-utility-cs)
