---
description: 了解 C# 中的内置字符类型
title: char 类型 - C# 引用
ms.date: 05/11/2020
f1_keywords:
- char
- char_CSharpKeyword
helpviewer_keywords:
- char data type [C#]
ms.assetid: b51cf4fb-124c-4067-af48-afbac122b228
ms.openlocfilehash: 1cb40759b81a1fcedcf5962b57d79cf3a64df561
ms.sourcegitcommit: 870bc4b4087510f6fba3c7b1c0d391f02bcc1f3e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2020
ms.locfileid: "92471871"
---
# <a name="char-c-reference"></a>char（C# 参考）

`char` 类型关键字是 .NET <xref:System.Char?displayProperty=nameWithType> 结构类型的别名，它表示 Unicode UTF-16 字符。

|类型|范围|大小|.NET 类型|
|----------|-----------|----------|-------------------------|
|`char`|U+0000 到 U+FFFF|16 位|<xref:System.Char?displayProperty=nameWithType>|

`char` 类型的默认值为 `\0`，即 U+0000。

`char` 类型支持[比较](../operators/comparison-operators.md)、[相等](../operators/equality-operators.md)、[增量](../operators/arithmetic-operators.md#increment-operator-)和[减量](../operators/arithmetic-operators.md#decrement-operator---)运算符。 此外，对于 `char` 操作数，[算数](../operators/arithmetic-operators.md)和[逻辑位](../operators/bitwise-and-shift-operators.md)运算符对相应的字符代码执行操作，并得出 `int` 类型的结果。

[字符串](reference-types.md#the-string-type)类型将文本表示为 `char` 值的序列。

## <a name="literals"></a>文本

可以使用以下命令指定 `char` 值：

- 字符文本。
- Unicode 转义序列，它是 `\u` 后跟字符代码的十六进制表示形式（四个符号）。
- 十六进制转义序列，它是 `\x` 后跟字符代码的十六进制表示形式。

[!code-csharp-interactive[char literals](snippets/shared/CharType.cs#Literals)]

如前面的示例所示，你还可以将字符代码的值转换为相应的 `char` 值。

> [!NOTE]
> 对于 Unicode 转义序列，必须指定全部四位十六进制值。 也就是说，`\u006A` 是一个有效的转义序列，而 `\u06A` 和 `\u6A` 是无效的。
>
> 对于十六进制转义序列，可以省略前导零。 也就是说，`\x006A`、`\x06A` 和 `\x6A` 转义序列是有效的，并且对应于同一个字符。

## <a name="conversions"></a>转换

`char` 类型可隐式转换为以下[整型](integral-numeric-types.md)类型：`ushort`、`int`、`uint`、`long` 和 `ulong`。 它也可以隐式转换为内置[浮点](floating-point-numeric-types.md)数值类型：`float`、`double` 和 `decimal`。 它可以显式转换为 `sbyte`、`byte` 和 `short` 整型类型。

无法将其他类型隐式转换为 `char` 类型。 但是，任何[整型](integral-numeric-types.md)或[浮点](floating-point-numeric-types.md)数值类型都可显式转换为 `char`。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的[整型类型](~/_csharplang/spec/types.md#integral-types)部分。

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [值类型](value-types.md)
- [字符串](../../programming-guide/strings/index.md)
- <xref:System.Text.Rune?displayProperty=nameWithType>
- [.NET 中的字符编码](../../../standard/base-types/character-encoding-introduction.md)
