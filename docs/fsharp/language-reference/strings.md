---
title: 字符串
description: '了解 F # "string" 类型如何将不可变文本表示为 Unicode 字符序列。'
ms.date: 08/15/2020
ms.openlocfilehash: f6ec36feeb197bf785c702e7b626cf5cf80696ab
ms.sourcegitcommit: 9c45035b781caebc63ec8ecf912dc83fb6723b1f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2020
ms.locfileid: "88812206"
---
# <a name="strings"></a>字符串

`string`类型将不可变文本表示为 Unicode 字符序列。 `string` 是 `System.String` 在 .NET 中的别名。

## <a name="remarks"></a>注解

字符串文本由引号 ( ") 字符分隔。 反斜杠字符 ( \\ ) 用于对某些特殊字符进行编码。 反斜杠和下一个字符共同称为 *转义序列*。 下表显示了 F # 字符串文本中支持的转义序列。

|字符|转义序列|
|---------|---------------|
|警报|`\a`|
|Backspace|`\b`|
|换页|`\f`|
|换行符|`\n`|
|回车|`\r`|
|选项卡|`\t`|
|垂直制表符|`\v`|
|反斜杠|`\\`|
|引号|`\"`|
|撇号|`\'`|
|Unicode 字符|`\DDD` (，其中 `D` 指示十进制数字; 范围为 000-255; 例如， `\231` = "ç" ) |
|Unicode 字符|`\xHH` (，其中 `H` 指示十六进制数字; 范围为 00-FF; 例如， `\xE7` = "ç" ) |
|Unicode 字符|`\uHHHH` (UTF-16)  (，其中 `H` 指示十六进制数字; 范围为 0000-FFFF; 例如， `\u00E7` = "ç" ) |
|Unicode 字符|`\U00HHHHHH` (32)  (，其中 `H` 指示十六进制数字; 10FFFF 的范围; 例如， `\U0001F47D` = " 👽 " ) |

> [!IMPORTANT]
> `\DDD`转义序列是十进制符号，而不是八进制表示法，这与大多数其他语言类似。 因此，数字 `8` 和 `9` 都是有效的，序列 `\032` 表示 (U + 0020) 的空间，而八进制表示法中的码位就是 `\040` 。

> [!NOTE]
> 由于被限制为 0-255 (0xFF) 的范围， `\DDD` 且和 `\x` 转义序列实际上是 [ISO-8859-1](https://en.wikipedia.org/wiki/ISO/IEC_8859-1#Code_page_layout) 字符集，因为它匹配第一个 256 Unicode 码位。

## <a name="verbatim-strings"></a>原义字符串

如果前面带有 @ 符号，则文本为逐字字符串。 这意味着将忽略任何转义序列，只不过两个引号字符被解释为一个引号字符。

## <a name="triple-quoted-strings"></a>三个带引号的字符串

此外，字符串可以用三个引号括起来。 在这种情况下，将忽略所有转义序列，包括双引号字符。 若要指定包含嵌入的带引号的字符串的字符串，可以使用逐字字符串或带三个引号的字符串。 如果使用逐字字符串，则必须指定两个引号字符来指示单引号字符。 如果使用三个带引号的字符串，则可以使用单引号字符，而不会将它们分析为字符串的末尾。 当使用包含嵌入引号的 XML 或其他结构时，此方法会很有用。

```fsharp
// Using a verbatim string
let xmlFragment1 = @"<book author=""Milton, John"" title=""Paradise Lost"">"

// Using a triple-quoted string
let xmlFragment2 = """<book author="Milton, John" title="Paradise Lost">"""
```

在代码中，将接受带有分行符的字符串，分行符将按原义解释为换行符，除非反斜杠字符为换行符前的最后一个字符。 使用反斜杠字符时，将忽略下一行的前导空格。 下面的代码将生成一个字符串 `str1` ，该字符串具有值 `"abc\ndef"` 和 `str2` 具有值的字符串 `"abcdef"` 。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1001.fs)]

## <a name="string-indexing-and-slicing"></a>字符串索引和切片

您可以使用类似数组的语法来访问字符串中的单个字符，如下所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1002.fs)]

输出为 `b`。

或者，您可以使用数组切片语法来提取子字符串，如下面的代码所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1003.fs)]

输出如下所示。

```console
abc
def
```

可以按无符号字节的数组（类型）表示 ASCII 字符串 `byte[]` 。 将后缀添加 `B` 到字符串文字，以指示它是 ASCII 字符串。 与 byte 数组一起使用的 ASCII 字符串文本除了 Unicode 转义序列外，与 Unicode 字符串支持相同的转义序列。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1004.fs)]

## <a name="string-operators"></a>字符串运算符

`+`运算符可用于连接字符串，保持与 .NET Framework 字符串处理功能的兼容性。 下面的示例演示字符串串联。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1006.fs)]

## <a name="string-class"></a>String 类

由于 F # 中的字符串类型实际上是 .NET Framework `System.String` 类型，因此所有 `System.String` 成员都可用。 这包括 `+` 用于连接字符串、属性和属性的运算符，该运算符将 `Length` `Chars` 字符串作为 Unicode 字符数组返回。 有关字符串的详细信息，请参阅 `System.String` 。

通过使用的 `Chars` 属性 `System.String` ，可以通过指定索引来访问字符串中的各个字符，如下面的代码所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1005.fs)]

## <a name="string-module"></a>字符串模块

命名空间中的模块包含了用于字符串处理的其他功能 `String` `FSharp.Core` 。 有关详细信息，请参阅 [字符串模块](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-stringmodule.html)。

## <a name="see-also"></a>另请参阅

- [F# 语言参考](index.md)
