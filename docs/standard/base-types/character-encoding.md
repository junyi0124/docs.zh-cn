---
title: 如何在 .NET 中使用字符编码类
description: 了解如何在 .NET 中使用字符编码类。
ms.date: 12/22/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- encoding, understanding
- encoding, choosing
- encoding, fallback strategy
ms.assetid: bf6d9823-4c2d-48af-b280-919c5af66ae9
ms.openlocfilehash: 520316c0733d5c56b5ab77fbec7d4dd838af6338
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94823285"
---
# <a name="how-to-use-character-encoding-classes-in-net"></a>如何在 .NET 中使用字符编码类

本文介绍如何使用 .NET 提供的类通过各种编码方案对文本进行编码和解码。 这些说明假定你已阅读 [.NET 中的字符编码简介](character-encoding-introduction.md)。

## <a name="encoders-and-decoders"></a>编码器和解码器

.NET 提供了编码类，这些类使用各种编码系统对文本进行编码和解码。 例如，<xref:System.Text.UTF8Encoding> 类描述用于编码到 UTF-8 以及从 UTF-8 解码的规则。 .NET 对 `string` 实例使用 UTF-16 编码（由 <xref:System.Text.UnicodeEncoding> 类表示）。 编码器和解码器还适用于其他编码方案。

编码和解码还可以包括验证。 例如，<xref:System.Text.UnicodeEncoding> 类检查代理项范围内的所有 `char` 实例，确保它们是有效的代理项对。 回退策略确定编码器处理无效字符的方式或解码器处理无效字节的方式。

> [!WARNING]
> .NET 编码类可用于存储和转换字符数据。 它们不应用于存储字符串形式的二进制数据。 根据所使用的编码，用编码类将二进制数据转换为字符串格式可引起意外的行为，并生成不准确或损坏的数据。 若要将二进制数据转换为字符串形式，请使用 <xref:System.Convert.ToBase64String%2A?displayProperty=nameWithType> 方法。

.NET 中的所有字符编码类都继承自 <xref:System.Text.Encoding?displayProperty=nameWithType> 类，这是定义所有字符编码通用功能的抽象类。 若要访问在 .NET 中实现的单个编码对象，请执行以下操作：

- 使用 <xref:System.Text.Encoding> 类的静态属性，这些属性返回表示 .NET 标准字符编码（ASCII、UTF-7、UTF-8、UTF-16 和 UTF-32）的对象。 例如， <xref:System.Text.Encoding.Unicode%2A?displayProperty=nameWithType> 属性返回 <xref:System.Text.UnicodeEncoding> 对象。 每个对象都使用替换回退处理不能进行编码的字符串和不能进行解码的字节。 有关详细信息，请参阅[替换回退](character-encoding.md#Replacement)。

- 调用编码的类构造函数。 以这种方式可以将 ASCII、utf-7、utf-8、utf-16 和 utf-32 编码对象实例化。 默认情况下，每个对象都使用替换回退处理不能进行编码的字符串和不能进行解码的字节，但你可指定应引发异常。 有关详细信息，请参阅[替换回退](character-encoding.md#Replacement)和[异常回退](character-encoding.md#Exception)。

- 调用 <xref:System.Text.Encoding.%23ctor%28System.Int32%29> 构造函数并向其传递一个表示编码的整数。 标准编码对象使用替换回退，代码页编码和双字节字符集 (DBCS) 编码对象使用最佳回退处理不能进行编码的字符串和不能进行解码的字节。 有关详细信息，请参阅[最佳回退](character-encoding.md#BestFit)。

- 调用 <xref:System.Text.Encoding.GetEncoding%2A?displayProperty=nameWithType> 方法，此方法返回 .NET 中的任何标准编码、代码页编码或 DBCS 编码。 可通过重载同时指定编码器和解码器的回退对象。

可以检索所有 .NET 编码的相关信息，具体操作是调用 <xref:System.Text.Encoding.GetEncodings%2A?displayProperty=nameWithType> 方法。 .NET 支持下表中列出的字符编码系统。

|编码类|描述|
|--------------|-----------|
|[ASCII](xref:System.Text.ASCIIEncoding)|通过使用较低的七位字节将有限范围的字符进行编码。 由于此编码仅支持从 U+0000 到 U+007F 的字符值，因此在大多数情况下不足以支持国际化的应用程序。|
|[UTF-7](xref:System.Text.UTF7Encoding)|将字符表示为 7 位 ASCII 字符的序列。 非 ASCII Unicode 字符由 ASCII 字符的转义序列表示。 UTF-7 支持电子邮件和新闻组协等协议。 但是，utf-7 不是特别安全或可靠。 在某些情况下，更改一位可以彻底更改对整个 utf-7 字符串的解释。 在其他情况下，不同的 utf-7 字符串可以对相同的文本进行编码。 对于包含非 ASCII 字符的序列，utf-7 需要比 utf-8 更多的空间，且编码/解码更慢。 因此，应尽可能使用 utf-8，而不是 utf-7。|
|[UTF-8](xref:System.Text.UTF8Encoding)|将每个 Unicode 码位表示为一至四个字节的序列。 Utf-8 支持 8 位数据大小，适用于许多现有的操作系统。 对于字符的 ASCII 范围，utf-8 等同于 ASCII 编码，并适用于范围更广的字符集。 但是，对于中日韩 (CJK) 脚本而言，针对每个字符 utf-8 可能需要三个字节，并且可能导致比 utf-16 更大的数据大小。 有时 ASCII 数据（如 HTML 标记）的量证明了 CJK 范围大小增加的合理性。|
|[UTF-16](xref:System.Text.UnicodeEncoding)|将每个 Unicode 码位表示为一至两个 16 位整数的序列。 尽管 Unicode 补充字符（U+10000 和更高版本）需要两个 utf-16 代理项码位，但最常见的 Unicode 字符仅需一个 utf-16 码位。 little-endian 和 big endian 字节顺序均受支持。 公共语言运行时使用 Utf-16 编码表示 <xref:System.Char> 和 <xref:System.String> 值，Windows 操作系统使用它表示 `WCHAR` 值。|
|[UTF-32](xref:System.Text.UTF32Encoding)|将每个 Unicode 码位表示为一个 32 位整数。 little-endian 和 big endian 字节顺序均受支持。 当应用程序想要避免操作系统上的 utf-16 编码的代理项码位行为时，则使用 utf-32 编码，编码空间对操作系统十分重要。 显示器上呈现的单个标志符号仍可使用多个 UTF-32 字符进行编码。|
|ANSI/ISO 编码|为各种代码页提供支持。 在 Windows 操作系统上，代码页用于支持特定语言或语言组。 有关列出 .NET 支持代码页的表，请参阅 <xref:System.Text.Encoding> 类。 通过调用 <xref:System.Text.Encoding.GetEncoding%28System.Int32%29?displayProperty=nameWithType> 方法可检索特定代码页的编码对象。 一个代码页包含 256 个码位，并且是从零开始。 在大多数代码页中，码位 0 到 127 表示 ASCII 字符集，而码位 128 到 255 在代码页之间存在显著差异。 例如，代码页 1252 为拉丁语书写系统（包括英语、德语和法语）提供字符。 代码页 1252 中最后 128 个码位包含重音字符。 代码页 1253 提供希腊语书写系统中所需的字符代码。 代码页 1253 中最后 128 个码位包含希腊语字符。 因此，基于 ANSI 代码页的应用程序不能将希腊语和德语存储在同一个文本流中，除非它包含一个指示所引用的代码页的标识符。|
|双字节字符集 (DBCS) 编码|支持包含超过 256 个字符的语言，例如中文、日语和朝鲜语。 在 DBCS 中，一对码位（双字节）表示一个字符。 <xref:System.Text.Encoding.IsSingleByte%2A?displayProperty=nameWithType> 属性返回 DBCS 编码的 `false` 。 通过调用 <xref:System.Text.Encoding.GetEncoding%28System.Int32%29?displayProperty=nameWithType> 方法可以为特定的 DBCS 检索编码对象。 当应用程序处理 DBCS 数据时，DBCS 字符的第一个字节（前导字节）与紧随其后的结尾字节一起处理。 因为根据代码页，一对双字节码位可以代表不同的字符，此模式仍不支持在同一数据流中进行两种语言（如日语和中文）的组合。|

这些编码使你能够使用 Unicode 字符以及旧版应用程序中最常用的编码。 此外，通过定义从 <xref:System.Text.Encoding> 派生的类并重写其成员，可创建自定义编码。

## <a name="net-core-encoding-support"></a>.NET Core 编码支持

默认情况下，.NET Core 不提供除代码页 28591 以外的其他任何代码页编码和 Unicode 编码，例如 UTF-8 和 UTF-16。 不过，可以向应用中添加定目标到 .NET 的标准 Windows 应用中的代码页编码。 有关详细信息，请参见<xref:System.Text.CodePagesEncodingProvider>主题。

<a name="Selecting"></a>

## <a name="selecting-an-encoding-class"></a>选择编码类

如果有机会选择应用程序要使用的编码，则应使用 Unicode 编码，最好是 <xref:System.Text.UTF8Encoding> 或 <xref:System.Text.UnicodeEncoding>。 （.NET 还支持第三种 Unicode 编码，即 <xref:System.Text.UTF32Encoding>。）

如果打算使用 ASCII 编码 (<xref:System.Text.ASCIIEncoding>)，请选择 <xref:System.Text.UTF8Encoding> 。 这两个编码对于 ASCII 字符集而言是相同的，但 <xref:System.Text.UTF8Encoding> 具有以下优点：

- 它可以表示每个 Unicode 字符，而 <xref:System.Text.ASCIIEncoding> 仅支持 U+0000 到 U+007F 之间的 Unicode 字符值。

- 它提供错误检测，具有更高的安全性。

- 速度已达到最优，应快于任何其他编码。 即使对于全是 ASCII 的内容，使用 <xref:System.Text.UTF8Encoding> 执行操作的速度要快于 <xref:System.Text.ASCIIEncoding>。

应考虑仅针对旧版应用程序使用 <xref:System.Text.ASCIIEncoding> 。 但是，即使对于旧版应用程序，由于以下原因， <xref:System.Text.UTF8Encoding> 可能是更好的选择（假定采用默认设置）：

- 如果应用程序具有的内容不完全是 ASCII 并使用 <xref:System.Text.ASCIIEncoding>将其编码，则每个非 ASCII 字符将编码为一个问号 (?)。 如果应用程序随后对此数据进行解码，则信息丢失。

- 如果应用程序具有的内容不完全是 ASCII 并使用 <xref:System.Text.UTF8Encoding>将其编码，则结果看起来将无法识别（如果解释为 ASCII）。 但是，如果应用程序随后使用 utf-8 解码器将此数据进行解码，则数据成功执行一次往返过程。

在 web 应用程序中，发送到响应 web 请求的客户端中的字符应反映客户端上所使用的编码。 在大多数情况下，应将 <xref:System.Web.HttpResponse.ContentEncoding%2A?displayProperty=nameWithType> 属性设置为 <xref:System.Web.HttpRequest.ContentEncoding%2A?displayProperty=nameWithType> 属性返回的值，从而以用户期望的编码显示文本。

<a name="Using"></a>

## <a name="using-an-encoding-object"></a>使用编码对象

编码器将一个字符串（最常见的为 Unicode 字符）转换为其数字（字节）等效项。 例如，你可能会使用 ASCII 编码器将 Unicode 字符转换为 ASCII，以便可在控制台中显示。 若要执行此转换，调用 <xref:System.Text.Encoding.GetBytes%2A?displayProperty=nameWithType> 方法。 如果想要在执行编码前确定需要多少个字节来存储编码字符，可调用 <xref:System.Text.Encoding.GetByteCount%2A> 方法。

下面的示例使用单个字节数组，从而在两个单独的操作中对字符串进行编码。 它为下一组 ASCII 编码字节保持指示字节数组中的起始位置的索引。 调用 <xref:System.Text.ASCIIEncoding.GetByteCount%28System.String%29?displayProperty=nameWithType> 方法，以确保字节数组足够大，从而可容纳已编码的字符串。 然后调用 <xref:System.Text.ASCIIEncoding.GetBytes%28System.String%2CSystem.Int32%2CSystem.Int32%2CSystem.Byte%5B%5D%2CSystem.Int32%29?displayProperty=nameWithType> 方法以在字符串中对字符进行编码。

[!code-csharp[Conceptual.Encoding#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.encoding/cs/getbytes1.cs#8)]
[!code-vb[Conceptual.Encoding#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.encoding/vb/getbytes1.vb#8)]

解码器将反映特定字符编码的字节数组转换为字符数组或字符串中的一组字符。 若要将字节数组解码为字符数组，则调用 <xref:System.Text.Encoding.GetChars%2A?displayProperty=nameWithType> 方法。 若要将字节数组解码为字符串，则调用 <xref:System.Text.Encoding.GetString%2A> 方法。 如果想在执行解码前确定需要多少个字符来存储已解码的字节，则可调用 <xref:System.Text.Encoding.GetCharCount%2A> 方法。

下面的示例对三个字符串进行编码，然后将它们解码为单个字符数组。 它为下一组解码的字符保持指示字符数组中的起始位置的索引。 调用 <xref:System.Text.ASCIIEncoding.GetCharCount%2A> 方法，以确保字符数组足够大，从而可容纳所有已解码的字符。 然后调用 <xref:System.Text.ASCIIEncoding.GetChars%28System.Byte%5B%5D%2CSystem.Int32%2CSystem.Int32%2CSystem.Char%5B%5D%2CSystem.Int32%29?displayProperty=nameWithType> 方法对字节数组进行解码。

[!code-csharp[Conceptual.Encoding#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.encoding/cs/getchars1.cs#9)]
[!code-vb[Conceptual.Encoding#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.encoding/vb/getchars1.vb#9)]

从 <xref:System.Text.Encoding> 派生的类的编码和解码方法旨在用于一组完整的数据；也就是说，在单个方法调用中提供要进行编码或解码的所有数据。 但是，在某些情况下，数据在流中可用，并且要编码或解码的数据可能仅可从单独的读取操作中获取。 这要求编码或解码操作记住其之前调用中保存的任何状态。 从 <xref:System.Text.Encoder> 和 <xref:System.Text.Decoder> 派生的类的方法能够处理跨多个方法调用的编码和解码操作。

特定编码的 <xref:System.Text.Encoder> 对象可从此编码的 <xref:System.Text.Encoding.GetEncoder%2A?displayProperty=nameWithType> 属性获取。 特定编码的 <xref:System.Text.Decoder> 对象可从此编码的 <xref:System.Text.Encoding.GetDecoder%2A?displayProperty=nameWithType> 属性获取。 对于解码操作，请注意，从 <xref:System.Text.Decoder> 派生的类包括 <xref:System.Text.Decoder.GetChars%2A?displayProperty=nameWithType> 方法，但它们不具有对应于 <xref:System.Text.Encoding.GetString%2A?displayProperty=nameWithType>的方法。

下面的示例演示使用 <xref:System.Text.Encoding.GetString%2A?displayProperty=nameWithType> 和 <xref:System.Text.Decoder.GetChars%2A?displayProperty=nameWithType> 方法解码 Unicode 字节数组的差异。 该示例将包含某些 Unicode 字符的字符串编码为文件，然后使用两种解码方法一次解码十个字节。 由于代理项对发生在第十个和第十一个字节，因此它在单独的方法调用中进行解码。 如输出所示， <xref:System.Text.Encoding.GetString%2A?displayProperty=nameWithType> 方法不能正确地对字节进行解码，而是将它们替换为 U + FFFD（替换字符）。 另一方面， <xref:System.Text.Decoder.GetChars%2A?displayProperty=nameWithType> 方法能够成功地对字节数组进行解码以获取原始字符串。

[!code-csharp[Conceptual.Encoding#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.encoding/cs/stream1.cs#10)]
[!code-vb[Conceptual.Encoding#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.encoding/vb/stream1.vb#10)]

<a name="FallbackStrategy"></a>

## <a name="choosing-a-fallback-strategy"></a>选择回退策略

当某个方法尝试对字符进行编码或解码，但不存在映射时，它必须实现回退策略，以确定应如何处理失败的映射。 有三种类型的回退策略：

- Best-Fit Fallback

- Replacement Fallback

- Exception Fallback

> [!IMPORTANT]
> 当某一 Unicode 字符不能映射到特定代码页编码时，在编码操作中将发生最常见的问题。 当无效的字节序列无法转换为有效的 Unicode 字符，在解码操作中将发生最常见的问题。 出于这些原因，应该了解特定的编码对象使用哪种回退策略。 只要有可能，应指定实例化对象时编码对象使用的回退策略。

<a name="BestFit"></a>

### <a name="best-fit-fallback"></a>Best-Fit Fallback

当一个字符在目标编码中不具有准确匹配时，编码器可以尝试将其映射到类似的字符。 （最佳回退主要是编码问题而非解码问题。 很少有代码页包含无法成功映射到 Unicode 的字符。）最佳回退是代码页的默认设置，以及 <xref:System.Text.Encoding.GetEncoding%28System.Int32%29?displayProperty=nameWithType> 和 <xref:System.Text.Encoding.GetEncoding%28System.String%29?displayProperty=nameWithType> 重载检索的双字节字符集编码。

> [!NOTE]
> 从理论上讲，.NET 中的 Unicode 编码类（<xref:System.Text.UTF8Encoding>、<xref:System.Text.UnicodeEncoding> 和 <xref:System.Text.UTF32Encoding>）支持所有字符集中的每个字符，因此可用于消除最佳匹配回退问题。

最佳匹配策略因代码页而异。 例如，对于某些代码页，全角拉丁字符映射到更常见的半角拉丁字符。 对于其他代码页，不进行此映射。 即使是在积极的最佳策略下，也不能完全适合某些编码中的某些字符。 例如，中文象形文字不具有到代码页 1252 的合理映射。 在这种情况下，使用替换字符串。 默认情况下，此字符串只是一个问号 (U+003F)。

> [!NOTE]
> 最佳匹配策略不会得到详细记录。 不过，[Unicode 联盟](https://www.unicode.org/Public/MAPPINGS/VENDORS/MICSFT/WindowsBestFit/)网站上记录了多个代码页。 若要了解如何解释映射文件，请查看相应文件夹中的 readme.txt 文件。

下面的示例使用代码页 1252 （适合西欧语言 Windows 代码页）演示最佳映射及其缺点。 <xref:System.Text.Encoding.GetEncoding%28System.Int32%29?displayProperty=nameWithType> 方法用于检索代码页 1252 的编码对象。 默认情况下，它使用其不支持的 Unicode 字符的最佳映射。 该示例将包含三个非 ASCII 字符的字符串实例化，这三个字符分别为带圆圈拉丁文大写字母 S (U+24C8)、上标五 (U+2075) 和无穷大 (U+221E) 且由空格分隔。 如示例输出所示，当对字符串进行编码时，三个原始的非空格字符替换为问号 (U+003F)、数字五 (U+0035) 和数字八 (U+0038)。 数字八是对不受支持的无穷大字符的不良替换，问号指示没有映射可用于原始字符。

[!code-csharp[Conceptual.Encoding#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.encoding/cs/bestfit1.cs#1)]
[!code-vb[Conceptual.Encoding#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.encoding/vb/bestfit1.vb#1)]

最佳映射是 <xref:System.Text.Encoding> 对象的默认行为，该对象将 Unicode 数据编码为代码页数据，并且存在依赖此行为的旧版应用程序。 但是，为了安全起见，大多数新应用程序应避免最佳行为。 例如，应用程序不应通过最佳编码放置域名。

> [!NOTE]
> 还可实现编码的自定义最佳回退映射。 有关详细信息，请参阅 [Implementing a Custom Fallback Strategy](character-encoding.md#Custom) 一节。

如果最佳回退是编码对象的默认设置，当通过调用 <xref:System.Text.Encoding> 或 <xref:System.Text.Encoding.GetEncoding%28System.Int32%2CSystem.Text.EncoderFallback%2CSystem.Text.DecoderFallback%29?displayProperty=nameWithType> 重载检索. <xref:System.Text.Encoding.GetEncoding%28System.String%2CSystem.Text.EncoderFallback%2CSystem.Text.DecoderFallback%29?displayProperty=nameWithType> 对象时，可选择另一个回退策略。 以下一节的内容包括一个示例，该示例用星号 (*) 替换每个不可映射到代码页 1252 的每个字符。

[!code-csharp[Conceptual.Encoding#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.encoding/cs/bestfit1a.cs#3)]
[!code-vb[Conceptual.Encoding#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.encoding/vb/bestfit1a.vb#3)]

<a name="Replacement"></a>

### <a name="replacement-fallback"></a>Replacement Fallback

当字符在目标方案中没有准确匹配，但是也没有其可映射到的相应字符，则应用程序可指定替换字符或字符串。 这是 Unicode 编码器的默认行为，此默认行为替换其无法用 REPLACEMENT_CHARACTER (U+FFFD) 进行解码的任何双字节序列。 它也是 <xref:System.Text.ASCIIEncoding> 类的默认行为，此默认行为替换其无法用问号进行编码或解码的每个字符。 下面的示例演示上一示例中 Unicode 字符串的字符替换。 如输出所示，不能解码为 ASCII 字节值的每个字符都替换为 0x3F，这是问号的 ASCII 代码。

[!code-csharp[Conceptual.Encoding#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.encoding/cs/replacementascii.cs#2)]
[!code-vb[Conceptual.Encoding#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.encoding/vb/replacementascii.vb#2)]

.NET 包括 <xref:System.Text.EncoderReplacementFallback> 和 <xref:System.Text.DecoderReplacementFallback> 类，用于替换字符没有在编码或解码操作中完全映射时的替换字符串。 默认情况下，此替换字符串是一个问号，但可以调用类构造函数重载以选择不同的字符串。 通常，替换字符串是单个字符，但这不是一项要求。 下面的示例通过将以星号 (*) 作为替换字符串的 <xref:System.Text.EncoderReplacementFallback> 对象实例化，更改代码页 1252 编码器的行为。

[!code-csharp[Conceptual.Encoding#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.encoding/cs/bestfit1a.cs#3)]
[!code-vb[Conceptual.Encoding#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.encoding/vb/bestfit1a.vb#3)]

> [!NOTE]
> 还可以实现编码的替换类。 有关详细信息，请参阅 [Implementing a Custom Fallback Strategy](character-encoding.md#Custom) 一节。

除了问号 (U+003F) 外，Unicode 替换字符 (U+FFFD) 通常用作替换字符串，特别是当解码无法成功转换为 Unicode 字符的字节序列时。 但是，你可以自由选择任何替换字符串，并且它可以包含多个字符。

<a name="Exception"></a>

### <a name="exception-fallback"></a>Exception Fallback

如果编码器不能对一组字符进行编码，它就不提供最佳回退或替换字符串，而是可能引发 <xref:System.Text.EncoderFallbackException> ，如果解码器不能对字节数组进行解码，它就可能引发 <xref:System.Text.DecoderFallbackException> 。 若要在编码和解码操作中引发异常，请分别提供一个 <xref:System.Text.EncoderExceptionFallback> 对象和一个 <xref:System.Text.DecoderExceptionFallback> 对象到 <xref:System.Text.Encoding.GetEncoding%28System.String%2CSystem.Text.EncoderFallback%2CSystem.Text.DecoderFallback%29?displayProperty=nameWithType> 方法。 下面的示例用 <xref:System.Text.ASCIIEncoding> 类演示异常回退。

[!code-csharp[Conceptual.Encoding#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.encoding/cs/exceptionascii.cs#4)]
[!code-vb[Conceptual.Encoding#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.encoding/vb/exceptionascii.vb#4)]

> [!NOTE]
> 还可以实现编码操作的自定义异常处理程序。 有关详细信息，请参阅 [Implementing a Custom Fallback Strategy](character-encoding.md#Custom) 一节。

<xref:System.Text.EncoderFallbackException> 和 <xref:System.Text.DecoderFallbackException> 对象提供以下有关导致异常的条件的信息：

- <xref:System.Text.EncoderFallbackException> 对象包括 <xref:System.Text.EncoderFallbackException.IsUnknownSurrogate%2A> 方法，该方法指示不能对其进行编码的一个字符或多个字符是代表未知的代理项对（在这种情况下，该方法返回 `true`）还是未知的单个字符（在这种情况下，该方法将返回 `false`）。 代理项对中的字符可从 <xref:System.Text.EncoderFallbackException.CharUnknownHigh%2A?displayProperty=nameWithType> 和 <xref:System.Text.EncoderFallbackException.CharUnknownLow%2A?displayProperty=nameWithType> 属性获取。 未知的单个字符可从 <xref:System.Text.EncoderFallbackException.CharUnknown%2A?displayProperty=nameWithType> 属性获取。 <xref:System.Text.EncoderFallbackException.Index%2A?displayProperty=nameWithType> 属性指示字符串中第一个无法进行编码的字符的位置。

- <xref:System.Text.DecoderFallbackException> 对象包括 <xref:System.Text.DecoderFallbackException.BytesUnknown%2A> 属性，该属性返回一个无法解码的字节数组。 <xref:System.Text.DecoderFallbackException.Index%2A?displayProperty=nameWithType> 属性指示未知字节的起始位置。

尽管 <xref:System.Text.EncoderFallbackException> 和 <xref:System.Text.DecoderFallbackException> 对象提供足够的有关异常的诊断信息，但它们不提供对编码或解码缓冲区的访问权限。 因此，它们不允许在编码或解码方法内替换或更正无效数据。

<a name="Custom"></a>

## <a name="implementing-a-custom-fallback-strategy"></a>Implementing a Custom Fallback Strategy

除了由代码页在内部实现的最佳映射，.NET 包括用于实现回退策略的以下类：

- 使用 <xref:System.Text.EncoderReplacementFallback> 和 <xref:System.Text.EncoderReplacementFallbackBuffer> 来替换编码操作中的字符。

- 使用 <xref:System.Text.DecoderReplacementFallback> 和 <xref:System.Text.DecoderReplacementFallbackBuffer> 来替换解码操作中的字符。

- 当字符不能进行编码时，使用 <xref:System.Text.EncoderExceptionFallback> 和 <xref:System.Text.EncoderExceptionFallbackBuffer> 来引发 <xref:System.Text.EncoderFallbackException> 。

- 当字符不能进行解码时，使用 <xref:System.Text.DecoderExceptionFallback> 和 <xref:System.Text.DecoderExceptionFallbackBuffer> 引发 <xref:System.Text.DecoderFallbackException> 。

此外，通过执行以下步骤，可以实现使用最佳回退、替换回退或异常回退的自定义解决方案：

1. 从 <xref:System.Text.EncoderFallback> 派生一个类用于编码操作，并从 <xref:System.Text.DecoderFallback> 派生一个类用于解码操作。

2. 从 <xref:System.Text.EncoderFallbackBuffer> 派生一个类用于编码操作，并从 <xref:System.Text.DecoderFallbackBuffer> 派生一个类用于解码操作。

3. 有关异常回退，如果预定义的 <xref:System.Text.EncoderFallbackException> 和 <xref:System.Text.DecoderFallbackException> 类不能满足你的需求，则从异常对象中派生一个类，如 <xref:System.Exception> 或 <xref:System.ArgumentException>。

### <a name="deriving-from-encoderfallback-or-decoderfallback"></a>从 EncoderFallback 或 DecoderFallback 中派生

若要实现自定义的回退解决方案，必须创建一个继承自 <xref:System.Text.EncoderFallback> 的类用于编码操作，以及一个继承自 <xref:System.Text.DecoderFallback> 的类用于解码操作。 这些类的实例传递给 <xref:System.Text.Encoding.GetEncoding%28System.String%2CSystem.Text.EncoderFallback%2CSystem.Text.DecoderFallback%29?displayProperty=nameWithType> 方法，并作为编码类和回退实现之间的媒介。

当为编码器或解码器创建自定义回退解决方案时，必须实现以下成员：

- <xref:System.Text.EncoderFallback.MaxCharCount%2A?displayProperty=nameWithType> 或 <xref:System.Text.DecoderFallback.MaxCharCount%2A?displayProperty=nameWithType> 属性，返回最佳、替换或异常回退可能返回以替换单个字符的最大数量的字符数。 对于自定义异常回退，其值为 0。

- <xref:System.Text.EncoderFallback.CreateFallbackBuffer%2A?displayProperty=nameWithType> 或 <xref:System.Text.DecoderFallback.CreateFallbackBuffer%2A?displayProperty=nameWithType> 方法，该方法返回自定义的 <xref:System.Text.EncoderFallbackBuffer> 或 <xref:System.Text.DecoderFallbackBuffer> 实现。 当遇到不能成功进行编码的第一个字节时或当遇到不能成功进行解码的第一个字节时，则解码器调用该方法。

### <a name="deriving-from-encoderfallbackbuffer-or-decoderfallbackbuffer"></a>从 EncoderFallbackBuffer 或 DecoderFallbackBuffer 中派生

若要实现自定义的回退解决方案，还必须创建一个继承自 <xref:System.Text.EncoderFallbackBuffer> 的类用于编码操作，以及一个继承自 <xref:System.Text.DecoderFallbackBuffer> 的类用于解码操作。 <xref:System.Text.EncoderFallback.CreateFallbackBuffer%2A> 和 <xref:System.Text.EncoderFallback> 类的 <xref:System.Text.DecoderFallback> 方法返回这些类的实例。 当遇到不能对其进行编码第一个字符时，编码器调用 <xref:System.Text.EncoderFallback.CreateFallbackBuffer%2A?displayProperty=nameWithType> 方法，当遇到不能对其进行解码的一个或多个字节时，解码器调用 <xref:System.Text.DecoderFallback.CreateFallbackBuffer%2A?displayProperty=nameWithType> 方法。 <xref:System.Text.EncoderFallbackBuffer> 和 <xref:System.Text.DecoderFallbackBuffer> 类提供回退实现。 每个实例表示一个包含回退字符的缓冲区，该回退字符将替换不能进行编码的字符或不能进行解码的字节序列。

当为编码器或解码器创建自定义回退解决方案时，必须实现以下成员：

- <xref:System.Text.EncoderFallbackBuffer.Fallback%2A?displayProperty=nameWithType> 或 <xref:System.Text.DecoderFallbackBuffer.Fallback%2A?displayProperty=nameWithType> 方法。 编码器调用<xref:System.Text.EncoderFallbackBuffer.Fallback%2A?displayProperty=nameWithType> 来为回退缓冲区提供其不能进行编码的字符的相关信息。 因为要进行编码的字符可能是代理项对，此方法被重载。 向重载传递了字符串中将进行编码的字符及其索引。 向第二个重载传递了字符串中高代理项和低代理项及其索引。 解码器调用 <xref:System.Text.DecoderFallbackBuffer.Fallback%2A?displayProperty=nameWithType> 方法来为回退缓冲区提供有关无法解码的字节的信息。 向此方法传递了其无法解码的字节数组以及第一个字节的索引。 如果回退缓冲区可以提供一个或多个最佳或替换字符，则回退方法应返回 `true` ，否则应返回 `false`。 对于异常回退，回退方法应引发异常。

- <xref:System.Text.EncoderFallbackBuffer.GetNextChar%2A?displayProperty=nameWithType> 或 <xref:System.Text.DecoderFallbackBuffer.GetNextChar%2A?displayProperty=nameWithType> 方法，编码器或解码器重复调用该方法以从回退缓冲区获取下一个字符。 返回所有回退字符后，该方法应返回 U+0000。

- <xref:System.Text.EncoderFallbackBuffer.Remaining%2A?displayProperty=nameWithType> 或 <xref:System.Text.DecoderFallbackBuffer.Remaining%2A?displayProperty=nameWithType> 属性，返回回退缓冲区中的剩余字符数。

- <xref:System.Text.EncoderFallbackBuffer.MovePrevious%2A?displayProperty=nameWithType> 或 <xref:System.Text.DecoderFallbackBuffer.MovePrevious%2A?displayProperty=nameWithType> 方法，将回退缓冲区中的当前位置移到前一个字符。

- <xref:System.Text.EncoderFallbackBuffer.Reset%2A?displayProperty=nameWithType> 或 <xref:System.Text.DecoderFallbackBuffer.Reset%2A?displayProperty=nameWithType> 方法，将回退缓冲区重新初始化。

如果回退实现是最佳回退或替换回退，则从 <xref:System.Text.EncoderFallbackBuffer> 和 <xref:System.Text.DecoderFallbackBuffer> 派生的类还保持两个私有实例字段：缓冲区中字符的精确数目；缓冲区中下一个要返回的字符的索引。

### <a name="an-encoderfallback-example"></a>EncoderFallback 示例

前面的一个示例使用替换回退替换与带星号 (*) 的 ASCII 字符不对应的 Unicode 字符。 下面的示例改为使用自定义的最佳回退实现提供更好的非 ASCII 字符映射。

下面的代码定义一个名为 `CustomMapper` 、派生自 <xref:System.Text.EncoderFallback> 的类，用以处理非 ASCII 字符的最佳映射。 其 `CreateFallbackBuffer` 方法返回 `CustomMapperFallbackBuffer` 对象，该对象提供 <xref:System.Text.EncoderFallbackBuffer> 实现。 `CustomMapper` 类使用 <xref:System.Collections.Generic.Dictionary%602> 对象存储不受支持的 Unicode 字符（键值）和其对应的 8 位字符（以 64 位整数存储在两个连续字节中）的映射。 若要使此映射可用于回退缓冲区，将 `CustomMapper` 实例作为参数传递给 `CustomMapperFallbackBuffer` 类构造函数。 因为最长的映射是 Unicode 字符 U+221E 的字符串“INF”，所以 `MaxCharCount` 属性返回 3。

[!code-csharp[Conceptual.Encoding#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.encoding/cs/custom1.cs#5)]
[!code-vb[Conceptual.Encoding#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.encoding/vb/custom1.vb#5)]

下面的代码定义 `CustomMapperFallbackBuffer` 类，该类派生自 <xref:System.Text.EncoderFallbackBuffer>。 包含最佳映射并在 `CustomMapper` 实例中定义的字典可从其类构造函数获取。 如果映射字典中定义了 ASCII 编码器无法对其进行编码的 Unicode 字符，则其 `Fallback` 方法返回 `true` ；否则返回 `false`。 对于每个回退，私有 `count` 变量指示仍需返回的字符数目，私有 `index` 变量指示字符串缓冲区 `charsToReturn`中下一个要返回的字符的位置。

[!code-csharp[Conceptual.Encoding#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.encoding/cs/custom1.cs#6)]
[!code-vb[Conceptual.Encoding#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.encoding/vb/custom1.vb#6)]

然后下面的代码实例化 `CustomMapper` 对象并将它的一个实例传递给 <xref:System.Text.Encoding.GetEncoding%28System.String%2CSystem.Text.EncoderFallback%2CSystem.Text.DecoderFallback%29?displayProperty=nameWithType> 方法。 输出指示最佳回退实现成功处理原始字符串中的三个非 ASCII 字符。

[!code-csharp[Conceptual.Encoding#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.encoding/cs/custom1.cs#7)]
[!code-vb[Conceptual.Encoding#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.encoding/vb/custom1.vb#7)]

## <a name="see-also"></a>请参阅

- [.NET 中的字符编码简介](character-encoding-introduction.md)
- <xref:System.Text.Encoder>
- <xref:System.Text.Decoder>
- <xref:System.Text.DecoderFallback>
- <xref:System.Text.Encoding>
- <xref:System.Text.EncoderFallback>
- [全球化和本地化](../globalization-localization/index.md)
