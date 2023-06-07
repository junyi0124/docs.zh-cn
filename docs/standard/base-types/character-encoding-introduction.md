---
title: .NET 中的 character 编码简介
description: 了解 .NET 中的 character 编码和解码。
ms.date: 03/09/2020
no-loc:
- Rune
- char
- string
dev_langs:
- csharp
helpviewer_keywords:
- encoding, understanding
ms.openlocfilehash: 572fcd289eea720873d94e7fc71f3b4a030d1d70
ms.sourcegitcommit: 74d05613d6c57106f83f82ce8ee71176874ea3f0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93282317"
---
# <a name="character-encoding-in-net"></a>.NET 中的字符编码

本文介绍 .NET 使用的 character 编码系统。 具体说明如何将 <xref:System.String>、<xref:System.Char>、<xref:System.Text.Rune>和 <xref:System.Globalization.StringInfo> 类型用于 Unicode、UTF-16 和 UTF-8。

本文中使用的术语“character”从读者的角度通常是指单个显示元素 。 常见的示例是字母“a”、“@”和表情符号 🐂。 有时，一个 character 实际上由多个独立的显示元素组成，具体可以参考介绍[字形群集](#grapheme-clusters)的部分。

## <a name="the-no-locstring-and-no-locchar-types"></a>string 和 char 类型

[string](xref:System.String) 类的实例表示一些文本。 `string` 在逻辑上是一个 16 位值的序列，其中每个值都是 [char](xref:System.Char) 结构的实例。 [string.Length](xref:System.String.Length) 属性返回 `string` 实例中 `char` 实例的数目。

下面的示例函数以十六进制表示法打印 `string` 中所有 `char` 实例的值：

:::code language="csharp" source="snippets/character-encoding-introduction/csharp/PrintStringChars.cs" id="SnippetPrintChars":::

将 string "Hello" 传递给此函数，将获得以下输出：

```csharp
PrintChars("Hello");
```

```output
"Hello".Length = 5
s[0] = 'H' ('\u0048')
s[1] = 'e' ('\u0065')
s[2] = 'l' ('\u006c')
s[3] = 'l' ('\u006c')
s[4] = 'o' ('\u006f')
```

每个 character 由一个 `char` 值表示。 这种模式适用于世界上大多数语言。 例如，下面是两个中文 character 的输出，听起来像“nǐ hǎo”，它们表示“Hello” ：

```csharp
PrintChars("你好");
```

```output
"你好".Length = 2
s[0] = '你' ('\u4f60')
s[1] = '好' ('\u597d')
```

但是，对于某些语言以及某些符号和表情符号，需要两个 `char` 实例来表示一个 character。 例如，比较奥塞治文中表示 Osage 的单词中的 character 和 `char` 实例：

```csharp
PrintChars("𐓏𐓘𐓻𐓘𐓻𐓟 𐒻𐓟");
```

```output
"𐓏𐓘𐓻𐓘𐓻𐓟 𐒻𐓟".Length = 17
s[0] = '�' ('\ud801')
s[1] = '�' ('\udccf')
s[2] = '�' ('\ud801')
s[3] = '�' ('\udcd8')
s[4] = '�' ('\ud801')
s[5] = '�' ('\udcfb')
s[6] = '�' ('\ud801')
s[7] = '�' ('\udcd8')
s[8] = '�' ('\ud801')
s[9] = '�' ('\udcfb')
s[10] = '�' ('\ud801')
s[11] = '�' ('\udcdf')
s[12] = ' ' ('\u0020')
s[13] = '�' ('\ud801')
s[14] = '�' ('\udcbb')
s[15] = '�' ('\ud801')
s[16] = '�' ('\udcdf')
```

在前面的示例中，除空格以外的每个 character 都由两个 `char` 实例表示。

单个 Unicode 表情符号也由两个 `char` 表示，如以下示例中所示的 ox 表情符号：

```output
"🐂".Length = 2
s[0] = '�' ('\ud83d')
s[1] = '�' ('\udc02')
```

这些示例表明，`string.Length` 的值表示 `char` 实例的数量，不一定表示显示的 character 数。 一个 `char` 实例本身不一定表示一个 character。

映射到单个 character 的 `char` 对称为“代理项对”。 若要了解它们的工作原理，需要了解 Unicode 和 UTF-16 编码。

## <a name="unicode-code-points"></a>Unicode 码位

Unicode 是一种国际编码标准，可用于各种平台以及各种语言和脚本。

Unicode 标准定义了超过 110 万个[码位](https://www.unicode.org/glossary/#code_point)。 码位是一个整数值，范围从 0 到 `U+10FFFF`（十进制 1,114,111）。 一些码位被分配给字母、符号或表情符号。 其他码位分配给控制文本或 character 显示方式的操作，例如换行。 很多码位尚未经分配。

下面是码位分配的一些示例，其中包含指向它们所在的 Unicode chart 的链接：

|十进制|Hex       |示例|描述|
|------:|----------|-------|-----------|
|10     | `U+000A` |不可用| [换行](https://www.unicode.org/charts/PDF/U0000.pdf) |
|97     | `U+0061` | a | [拉丁文小写字母 a](https://www.unicode.org/charts/PDF/U0000.pdf) |
|562    | `U+0232` | Ȳ | [带长音符的拉丁文大写字母 Y](https://www.unicode.org/charts/PDF/U0180.pdf) |
|68,675 | `U+10C43`| 𐱃 | [古突厥文字母鄂尔浑文 AT](https://www.unicode.org/charts/PDF/U10C00.pdf) |
|127,801| `U+1F339`| 🌹 | [玫瑰花表情符号](https://www.unicode.org/charts/PDF/U1F300.pdf) |

通常使用语法 `U+xxxx` 来表示码位，其中 `xxxx` 是十六进制编码的整数值。

整个码位范围包含两个子范围：

* `U+0000..U+FFFF` 范围内的基本多语言平面 (BMP)。 这个 16 位范围提供 65,536 个码位，足以涵盖世界上大多数编写系统。
* `U+10000..U+10FFFF` 范围内的补充码位。 这个 21 位范围提供了超过一百万个额外的码位，可用于不太知名的语言和其他用途，例如表情符号。

下图说明了 BMP 与补充码位之间的关系。

:::image type="content" source="media/character-encoding-introduction/bmp-and-supplementary.svg" alt-text="BMP and supplementary code points":::

## <a name="utf-16-code-units"></a>UTF-16 代码单位

16 位 Unicode 转换格式 ([UTF-16](https://www.unicode.org/faq/utf_bom.html#UTF16)) 是一种 character 编码系统，它使用 16 位代码单位来表示 Unicode 码位。 .NET 使用 UTF-16 对 `string` 中的文本进行编码。 `char` 实例表示一个 16 位代码单位。

单个 16 位代码单位可以表示基本多语言平面的 16 位范围内的任何码位。 但对于补充范围内的码位，需要两个 `char` 实例。

## <a name="surrogate-pairs"></a>代理项对

通过称为“代理项码位”的特殊范围（从 `U+D800` 到 `U+DFFF`，十进制 55,296 到 57,343，含限值），可以将两个 16 位值转换为一个 21 位值。

下图说明了 BMP 与代理项码位之间的关系。

:::image type="content" source="media/character-encoding-introduction/bmp-and-surrogate.svg" alt-text="BMP and surrogate code points":::

如果高代理项码位 (`U+D800..U+DBFF`) 后紧跟低代理项码位 (`U+DC00..U+DFFF`)，则通过使用以下公式，此代理项对将解释为补充码位：

```
code point = 0x10000 +
  ((high surrogate code point - 0xD800) * 0x0400) +
  (low surrogate code point - 0xDC00)
```

下面是使用十进制表示法的相同公式：

```
code point = 65,536 +
  ((high surrogate code point - 55,296) * 1,024) +
  (low surrogate code point - 56,320)
```

高代理项码位的数字值不高于低代理项码位的数字值。 高代理项码位之所以称为“高”，是因为它用于计算完整 21 位码位范围的高阶 11 位。 低代理项码位用于计算低阶 10 位。

例如，与代理项对对应的实际码位 `0xD83C` 和 `0xDF39` 按如下方式计算：

```
actual = 0x10000 + ((0xD83C - 0xD800) * 0x0400) + (0xDF39 - 0xDC00)
       = 0x10000 + (          0x003C  * 0x0400) +           0x0339
       = 0x10000 +                      0xF000  +           0x0339
       = 0x1F339
```

下面是使用十进制表示法的相同计算：

```
actual =  65,536 + ((55,356 - 55,296) * 1,024) + (57,145 - 56320)
       =  65,536 + (              60  * 1,024) +             825
       =  65,536 +                     61,440  +             825
       = 127,801
```

前一个示例展示的 `"\ud83c\udf39"` 是前面提到的 `U+1F339 ROSE ('🌹')` 码位的 UTF-16 编码。

## <a name="unicode-scalar-values"></a>Unicode 标量值

术语“[Unicode 标量值](https://www.unicode.org/glossary/#unicode_scalar_value)”是指除代理项码位之外的所有码位。 换句话说，标量值是分配有 character 或将来可以为其分配 character 的任何码位。 此处的“字符”是指可以分配给码位的任何内容，其中包括控制文本或 character 显示方式的操作。

下图演示了标量值码位。

:::image type="content" source="media/character-encoding-introduction/scalar-values.svg" alt-text="Scalar values":::

### <a name="the-no-locrune-type-as-a-scalar-value"></a>作为标量值的 Rune 类型

从 .NET Core 3.0 开始，<xref:System.Text.Rune?displayProperty=fullName> 类型表示 Unicode 标量值。 `Rune` 在 .NET Core 2.x 或 .NET Framework 4.x 中不可用。

`Rune` 构造函数验证生成的实例是否为有效的 Unicode 标量值，如果无效，则引发异常。 下面的示例展示成功实例化 `Rune` 实例的代码，因为输入代表有效的标量值：

:::code language="csharp" source="snippets/character-encoding-introduction/csharp/InstantiateRunes.cs" id="SnippetValid":::

下面的示例引发异常，因为码位在代理项范围内，但不是代理项对的一部分：

:::code language="csharp" source="snippets/character-encoding-introduction/csharp/InstantiateRunes.cs" id="SnippetInvalidSurrogate":::

下面的示例引发异常，因为码位超出了补充范围：

:::code language="csharp" source="snippets/character-encoding-introduction/csharp/InstantiateRunes.cs" id="SnippetInvalidHigh":::

### <a name="no-locrune-usage-example-changing-letter-case"></a>Rune 用法示例：更改字母大小写

如果 `char` 来自代理项对，则采用 `char` 并假设正在使用作为标量值的码位的 API 将无法正常工作。 例如，来看看以下方法，此方法对字符串 (string 中的每个 char 调用 <xref:System.Char.ToUpperInvariant%2A?displayProperty=nameWithType>：

:::code language="csharp" source="snippets/character-encoding-introduction/csharp/ConvertToUpper.cs" id="SnippetBadExample":::

如果 `input` 字符串 (string) 包含小写德塞莱特文字母 `er` (`𐑉`)，则此代码不会将其转换为大写形式 (`𐐡`)。 此代码对代理项码位 `U+D801` 和 `U+DC49` 分别调用 `char.ToUpperInvariant`。 但 `U+D801` 本身没有足够的信息将其标识为小写字母，因此 `char.ToUpperInvariant` 将其保持不变。 它以相同的方式处理 `U+DC49`。 结果是 `input` 字符串 (string) 中的小写“𐑉”不会转换为大写的“𐑉”。

以下两个选项可用于将字符串 ( string) 正确地转换为大写形式：

* 对 input 字符串 (string) 调用 <xref:System.String.ToUpperInvariant%2A?displayProperty=nameWithType>，而不是循环访问（一个 `char` 接着一个 `char`）。 `string.ToUpperInvariant` 方法可以访问每个代理项对的两个部分，因此它可以正确地处理所有 Unicode 码位。
* 循环访问 Unicode 标量值作为 `Rune` 实例，而不是 `char` 实例，如以下示例中所示。 由于 `Rune` 实例是有效的 Unicode 标量值，因此可将其传递给应对标量值执行操作的 API。 例如，如以下示例中所示，调用 <xref:System.Text.Rune.ToUpperInvariant%2A?displayProperty=nameWithType> 可以得到正确的结果：

  :::code language="csharp" source="snippets/character-encoding-introduction/csharp/ConvertToUpper.cs" id="SnippetGoodExample":::

### <a name="other-no-locrune-apis"></a>其他 Rune API

`Rune` 类型公开了许多 `char` API 的类似 API。 例如，以下方法在 `char` 类型上镜像静态 API：

* <xref:System.Text.Rune.IsLetter%2A?displayProperty=nameWithType>
* <xref:System.Text.Rune.IsWhiteSpace%2A?displayProperty=nameWithType>
* <xref:System.Text.Rune.IsLetterOrDigit%2A?displayProperty=nameWithType>
* <xref:System.Text.Rune.GetUnicodeCategory%2A?displayProperty=nameWithType>

若要从 `Rune` 实例获取原始标量值，请使用 <xref:System.Text.Rune.Value%2A?displayProperty=nameWithType> 属性。

若要将 `Rune` 实例转换回一连串 `char`，请使用 <xref:System.Text.Rune.ToString%2A?displayProperty=nameWithType> 或 <xref:System.Text.Rune.EncodeToUtf16%2A?displayProperty=nameWithType> 方法。

由于任何 Unicode 标量值都可以由单个 `char` 或代理项对表示，因此任何 `Rune` 实例最多可由 2 个 `char` 实例表示。 使用 <xref:System.Text.Rune.Utf16SequenceLength%2A?displayProperty=nameWithType> 查看表示 `Rune` 实例所需的 `char` 实例数目。

有关 .NET `Rune` 类型的详细信息，请参阅 [`Rune` API 参考](xref:System.Text.Rune)。

## <a name="grapheme-clusters"></a>字形群集

看起来像一个 character 的内容可能由多个码位组合而成，因此，相比“character”，“[字形群集](https://www.unicode.org/glossary/#grapheme_cluster)”术语的表述通常更贴合。 在 .NET 中使用“[文本元素](xref:System.Globalization.StringInfo.GetTextElementEnumerator%2A)”术语表示相同的内容。

比如，`string` 实例“a”、“á”、“á”和“`👩🏽‍🚒`”。 如果你的操作系统按照 Unicode 标准指定的方式来处理，则这些 `string` 实例中的每个实例都将显示为单个文本元素或字形群集。 但最后两个实例用多个标量值码位表示。

* 字符串 (string)“a”由一个标量值表示，并包含一个 `char` 实例。

  * `U+0061 LATIN SMALL LETTER A`

* 字符串 (string)“á”由一个标量值表示，并包含一个 `char` 实例。

  * `U+00E1 LATIN SMALL LETTER A WITH ACUTE`

* 字符串 (string)“á”看起来与“á”相同，但由两个标量值表示，并包含两个 `char` 实例。

  * `U+0061 LATIN SMALL LETTER A`
  * `U+0301 COMBINING ACUTE ACCENT`

* 最后，字符串 (string)“`👩🏽‍🚒`”由四个标量值表示，并包含七个 `char` 实例。

  * `U+1F469 WOMAN`（补充范围，需要一个代理项对）
  * `U+1F3FD EMOJI MODIFIER FITZPATRICK TYPE-4`（补充范围，需要一个代理项对）
  * `U+200D ZERO WIDTH JOINER`
  * `U+1F692 FIRE ENGINE`（补充范围，需要一个代理项对）

在前面的一些示例中（例如组合的重音修饰符或肤色修饰符），码位不会在屏幕上显示为独立元素。 相反，它用于修改之前出现的文本元素的外观。 这些示例表明，可能需要采用多个标量值来构成我们认为的单个“character”或“字形群集”。

若要枚举 `string` 的字形群集，请使用 <xref:System.Globalization.StringInfo> 类，如下面的示例中所示。 如果你熟悉 Swift，那么 .NET `StringInfo` 类型在概念上类似于 [Swift `character` 类型](https://developer.apple.com/documentation/swift/character)。

### <a name="example-count-no-locchar-no-locrune-and-text-element-instances"></a>示例：char、Rune 和文本元素实例计数

在 .NET API 中，字形群集称为“文本元素”。 下面的方法演示 `string`中 `char`、`Rune` 和文本元素实例之间的差异：

:::code language="csharp" source="snippets/character-encoding-introduction/csharp/CountTextElements.cs" id="SnippetCountMethod":::

:::code language="csharp" source="snippets/character-encoding-introduction/csharp/CountTextElements.cs" id="SnippetCallCountMethod":::

如果在 .NET Framework 或 .NET Core 3.1 或更早版本中运行此代码，表情符号的文本元素计数将显示 `4`。 这是由于 `StringInfo` 类中的 bug 所致（已在 .NET 5 中修复）。

### <a name="example-splitting-no-locstring-instances"></a>示例：拆分 string 实例

拆分 `string` 实例时，请避免拆分代理项对和字形群集。 下面的示例展示不正确的代码，此代码的目的是在 string 中每隔 10 个 character 插入一个换行符：

:::code language="csharp" source="snippets/character-encoding-introduction/csharp/InsertNewlines.cs" id="SnippetBadExample":::

由于此代码枚举 `char` 实例，因此会拆分一个跨越 10 个 `char` 边界的代理项对，并在它们之间插入一个换行符。 这种插入会导致数据损坏，因为代理项码位只有在作为代理项对时才具有意义。

如果枚举 `Rune` 实例（标量值）而不是 `char` 实例，仍可能损坏数据。 一组 `Rune` 实例可能构成跨越 10 个 `char` 边界的字形群集。 如果一组字形群集被拆分开，就不再有效。

更好的方法是通过对字形群集或文本元素进行计数来中断 string，如以下示例中所示：

:::code language="csharp" source="snippets/character-encoding-introduction/csharp/InsertNewlines.cs" id="SnippetGoodExample":::

但是，如前所述，在 .NET（而非 .NET 5）的实现中，`StringInfo` 类可能会错误地处理某些字形群集。

## <a name="utf-8-and-utf-32"></a>UTF-8 和 UTF-32

前面几节着重于介绍 UTF-16，因为 .NET 要使用它对 `string` 实例进行编码。 Unicode 还有其他编码系统：即 [UTF-8](https://www.unicode.org/faq/utf_bom.html#UTF8) 和 [UTF-32](https://www.unicode.org/faq/utf_bom.html#UTF32)。 这些编码分别使用 8 位代码单位和 32 位代码单位。

与 UTF-16 类似，UTF-8 需要使用多个代码单位表示某些 Unicode 标量值。 UTF-32 可以表示单个 32 位代码单位中的任何标量值。

下面的示例展示如何分别使用这三个 Unicode 编码系统表示同一个 Unicode 码位：

```
Scalar: U+0061 LATIN SMALL LETTER A ('a')
UTF-8 : [ 61 ]           (1x  8-bit code unit  = 8 bits total)
UTF-16: [ 0061 ]         (1x 16-bit code unit  = 16 bits total)
UTF-32: [ 00000061 ]     (1x 32-bit code unit  = 32 bits total)

Scalar: U+0429 CYRILLIC CAPITAL LETTER SHCHA ('Щ')
UTF-8 : [ D0 A9 ]        (2x  8-bit code units = 16 bits total)
UTF-16: [ 0429 ]         (1x 16-bit code unit  = 16 bits total)
UTF-32: [ 00000429 ]     (1x 32-bit code unit  = 32 bits total)

Scalar: U+A992 JAVANESE LETTER GA ('ꦒ')
UTF-8 : [ EA A6 92 ]     (3x  8-bit code units = 24 bits total)
UTF-16: [ A992 ]         (1x 16-bit code unit  = 16 bits total)
UTF-32: [ 0000A992 ]     (1x 32-bit code unit  = 32 bits total)

Scalar: U+104CC OSAGE CAPITAL LETTER TSHA ('𐓌')
UTF-8 : [ F0 90 93 8C ]  (4x  8-bit code units = 32 bits total)
UTF-16: [ D801 DCCC ]    (2x 16-bit code units = 32 bits total)
UTF-32: [ 000104CC ]     (1x 32-bit code unit  = 32 bits total)
```

如前文所述，[代理项对](#surrogate-pairs)中的单个 UTF-16 代码单位本身是没有意义的。 同样，如果单个 UTF-8 代码单位处于由两个、三个或四个用于计算标量值的一连串代码单位中，那么它自身也毫无意义。

### <a name="endianness"></a>字节排序方式

在 .NET 中，string 的 UTF-16 代码单位以 16 位整数（`char` 实例）的序列形式存储在连续内存中。 各个代码单位的位数根据当前体系结构的[字节顺序](https://en.wikipedia.org/wiki/Endianness)布局。

在 little-endian 体系结构中，由 UTF-16 码位 `[ D801 DCCC ]` 组成的 string 会在内存中以 `[ 0x01, 0xD8, 0xCC, 0xDC ]` 字节进行布局。 在 big-endian 体系结构中，同一 string 将在内存中以 `[ 0xD8, 0x01, 0xDC, 0xCC ]` 字节进行布局。

相互通信的计算机系统必须就跨网络数据的表示形式达成共识。 大多数网络协议在传输文本时都使用 UTF-8 标准，部分原因是为了避免 big-endian 计算机与 little-endian 计算机通信可能导致的问题。 不管字节顺序如何，由 UTF-8 码位 `[ F0 90 93 8C ]` 组成的 string 将始终表示为字节 `[ 0xF0, 0x90, 0x93, 0x8C ]`。

若要使用 UTF-8 传输文本，.NET 应用程序通常使用类似于以下示例的代码：

```csharp
string stringToWrite = GetString();
byte[] stringAsUtf8Bytes = Encoding.UTF8.GetBytes(stringToWrite);
await outputStream.WriteAsync(stringAsUtf8Bytes, 0, stringAsUtf8Bytes.Length);
```

在前面的示例中，方法 [Encoding.UTF8.GetBytes](xref:System.Text.UTF8Encoding.GetBytes%2A) 将 UTF-16 `string` 解码回一系列 Unicode 标量值，然后将这些标量值重新编码为 UTF-8，并将生成的序列放入 `byte` 数组。 [Encoding.UTF8.GetString](xref:System.Text.UTF8Encoding.GetString%2A) 方法执行相反的转换，将 UTF-8 `byte` 数组转换为 UTF-16 `string`。

> [!WARNING]
> 由于 UTF-8 在 Internet 上很普遍，因此从网络读取原始字节并将数据视为 UTF-8 会很有吸引力。 但是，应验证它的格式是否正确。 恶意客户端可能向你的服务提交格式错误的 UTF-8。 如果你按正确的格式处理数据，可能会导致应用程序出错或存在安全漏洞。 要验证 UTF-8 数据，可以使用类似于 `Encoding.UTF8.GetString` 的方法，此方法在将传入数据转换为 `string` 时将执行验证。

### <a name="well-formed-encoding"></a>格式正确的编码

格式正确的 Unicode 编码是一串 (string) 代码单位，可以将它毫无歧义地正确解码为一系列 Unicode 标量值。 格式正确的数据可以在 UTF-8、UTF-16 和 UTF-32 之间自由地来回转码。

编码序列格式是否正确的问题与计算机体系结构的字节顺序无关。 在 big-endian 和 little-endian 计算机上，格式错误的 UTF-8 序列都具有相同的错误方式。

以下是一些格式错误的编码示例：

* 在 UTF-8 中，序列 `[ 6C C2 61 ]` 格式错误，因为 `C2` 后面不能跟有 `61`。

* 在 UTF-16 中，序列 `[ DC00 DD00 ]`（或者，在 C# 中为字符串 (string) `"\udc00\udd00"`）格式错误，因为低代理项 `DC00` 后面不能跟有另一个低代理项 `DD00`。

* 在 UTF-32 中，序列 `[ 0011ABCD ]` 格式错误，因为 `0011ABCD` 不在 Unicode 标量值的范围内。

在 .NET 中，`string` 实例几乎总是包含格式正确的 UTF-16 数据，但也不能百分之百地保证。 以下示例展示有效的 C# 代码在 `string` 实例中创建格式不正确的 UTF-16 数据。

* 格式错误的文字：

  ```csharp
  const string s = "\ud800";
  ```

* 拆分代理项对的 substring：

  ```csharp
  string x = "\ud83e\udd70"; // "🥰"
  string y = x.Substring(1, 1); // "\udd70" standalone low surrogate
  ```

像 [`Encoding.UTF8.GetString`](xref:System.Text.UTF8Encoding.GetString%2A) 这样的 API 永远不会返回格式错误的 `string` 实例。 `Encoding.GetString` 和 `Encoding.GetBytes` 方法检测输入中格式错误的序列，并在生成输出时执行 character 替换。 例如，如果 [`Encoding.ASCII.GetString(byte[])`](xref:System.Text.ASCIIEncoding.GetString%2A) 在输入中发现非 ASCII 字节（超出 U+0000..U+007F 的范围），它会在返回的 `string` 实例中插入一个“?”。 [`Encoding.UTF8.GetString(byte[])`](xref:System.Text.UTF8Encoding.GetString%2A) 在返回的 `string` 实例中将格式错误的 UTF-8 序列替换为 `U+FFFD REPLACEMENT CHARACTER ('�')`。 有关详细信息，请参阅 5.22 和 3.9 小节中的 [Unicode 标准](https://www.unicode.org/versions/latest/)。

在出现格式错误的序列时，也可以将内置 `Encoding` 类配置为引发异常，而不是执行 character 替换。 在可能无法接受 character 替换的安全敏感的应用程序中，通常可以使用这种方法。

```csharp
byte[] utf8Bytes = ReadFromNetwork();
UTF8Encoding encoding = new UTF8Encoding(encoderShouldEmitUTF8Identifier: false, throwOnInvalidBytes: true);
string asString = encoding.GetString(utf8Bytes); // will throw if 'utf8Bytes' is ill-formed
```

要了解如何使用内置 `Encoding` 类，请参阅[如何在 .NET 中使用 character 编码类](character-encoding.md)。

## <a name="see-also"></a>请参阅

- <xref:System.String>
- <xref:System.Char>
- <xref:System.Text.Rune>
- [全球化和本地化](../globalization-localization/index.md)
