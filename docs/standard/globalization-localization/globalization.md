---
title: 全球化
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- globalization [.NET], about globalization
- global applications, globalization
- international applications [.NET], globalization
- world-ready applications, globalization
- application development [.NET], globalization
- culture, globalization
ms.assetid: 4e919934-6b19-42f2-b770-275a4fae87c9
ms.openlocfilehash: 055bdfa82ea0e1721a6125ba14d7b6340dc254b9
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94829864"
---
# <a name="globalization"></a>全球化

全球化涉及到设计和开发世界通用的应用，这些应用支持本地化界面和区域数据，供位于多个区域性的用户使用。 在设计阶段开始之前，应确定应用将支持哪些区域性。 虽然应用以单一区域性或区域作为默认目标，但可设计和编写应用，使其能够轻松地供其他区域性或区域的用户使用。

作为开发人员，我们对由区域性组成的用户界面和数据都做出过假设。 例如，对于说英语的美国开发人员来说，将日期和时间数据序列化为采用 `MM/dd/yyyy hh:mm:ss` 格式的字符串似乎是相当合理的。 但是，如果在处于不同区域性的系统上将该字符串进行反序列化，则可能会引发 <xref:System.FormatException> 异常或生成不准确的数据。 全球化使我们能够识别这些特定于区域性的假设，并确保它们不会影响到应用的设计和编码。

本文将讨论在全球化应用中处理字符串、日期和时间值以及数值时，应考虑的一些主要问题和遵从的最佳做法。

## <a name="strings"></a>字符串

处理字符和字符串的是全球化的重点，因为每个区域性或区域可能使用不同的字符和字符集，且排序方式也不同。 本节提供在全球化应用中使用字符串的一些建议。

### <a name="use-unicode-internally"></a>在内部使用 Unicode

默认情况下，.NET 使用 Unicode 字符串。 一个 Unicode 字符串由零个、一个或多个 <xref:System.Char> 对象组成，其中每个对象表示一个 UTF-16 代码单元。 对于每个字符集中的几乎每个字符来说，都有一个在全球范围内使用的 Unicode 表达式。

许多应用程序和操作系统（包括 Windows 操作系统）也可以使用代码页来表示字符集。 代码页通常包含从 0x00 到 0x7F 的标准 ASCII 值，并将其他字符映射到从 0x80 到 0xFF 的剩余值。 从 0x80 到 0xFF 的值的解释取决于具体的代码页。 因此，如有可能，应避免在全球化应用中使用代码页。

以下示例阐释了当系统上的默认代码页与保存数据的代码页不同时，解释代码页数据的危险。 （若要模拟此场景，示例应明确指定不同的代码页。）首先，示例定义了一个由希腊字母表的大写字符组成的数组。 然后使用代码页 737（也称为 MS-DOS 希腊语）将其编码成一个字节数组，并保存到文件。 如果检索该文件并使用代码页 737 对其字节数组进行解码，则会还原原始字符。 但是，如果检索该文件并使用代码页 1252（或按拉丁字母表来表示字符的 Windows-1252）对其字节数组进行解码，原始数据则会丢失。

[!code-csharp[Conceptual.Globalization#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/codepages1.cs#1)]
[!code-vb[Conceptual.Globalization#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/codepages1.vb#1)]

使用 Unicode 可确保相同的代码单元始终能映射到相同的字符，并且相同的字符始终能映射到相同的字节数组。

### <a name="use-resource-files"></a>使用资源文件

即使在开发以单一区域性或区域为目标的应用时，也应使用资源文件存储显示在用户界面中的字符串和其他资源。 切勿将它们直接添加到代码中。 使用资源文件有许多优点：

- 所有字符串都在一个位置。 不必搜索整个源代码即可识别要为特定的语言或区域性修改的字符串。

- 不需要重复字符串。 不使用资源文件的开发人员常常在多个源代码文件中定义同一字符串。 此类重复增加了在修改字符串时忽略一个或多个实例的可能性。

- 可以将非字符串资源（如图像或二进制数据）包含在资源文件中以便于检索，而不将它们存储在单独的独立文件中。

对于创建本地化应用来说，使用资源文件具有独特优势。 在附属程序集中部署资源时，公共语言运行时会基于由 <xref:System.Globalization.CultureInfo.CurrentUICulture%2A?displayProperty=nameWithType> 属性定义的用户当前 UI 区域性来自动选择适合区域性的资源。 只要提供了相应的区域性特定资源并正确示例化了 <xref:System.Resources.ResourceManager> 对象或使用了强类型的资源类，运行时就会负责检索适合的资源。

若要详细了解如何创建资源文件，请参阅[创建资源文件](../../framework/resources/creating-resource-files-for-desktop-apps.md)。 若要了解如何创建和部署附属程序集，请参阅[创建附属程序集](../../framework/resources/creating-satellite-assemblies-for-desktop-apps.md)以及[打包和部署资源](../../framework/resources/packaging-and-deploying-resources-in-desktop-apps.md)。

### <a name="search-and-compare-strings"></a>搜索和比较字符串

应尽可能地将字符串按整个字符串处理，而不是按一系列单个字符进行处理。 尤其重要的一点是在排序或搜索子字符串时，要防止出现与分析组合字符相关的问题。

> [!TIP]
> 可使用 <xref:System.Globalization.StringInfo> 类与文本元素配合使用，而不使用字符串中的单个字符。

在字符串搜索和比较中，常见的错误是将字符串作为字符的集合，其中每个字符由 <xref:System.Char> 对象表示。 实际上，单个字符串可能由一个、两个或多个 <xref:System.Char> 对象组成。 此类字符在一些字符串中出现得最频繁，这些字符串位于其字母表是由 Unicode 基本拉丁字符范围（从 U+0021 到 U+007E）以外的字符所组成的区域性中。 以下示例尝试在字符串中查找 LATIN CAPITAL LETTER A WITH GRAVE 字符 (U+00C0) 的索引。 但是，此字符有两种表示方法：单个代码单元 (U+00C0) 或复合字符（两个代码单元：U+0041 和 U+0300）。 在这种情况下，字符在字符串示例中用两个 <xref:System.Char> 对象（U+0041 和 U+0300）表示。 示例代码调用 <xref:System.String.IndexOf%28System.Char%29?displayProperty=nameWithType> 和 <xref:System.String.IndexOf%28System.String%29?displayProperty=nameWithType> 重载以查找此字符在字符串实例中的位置，但返回了不同的结果 。 第一个方法调用拥有 <xref:System.Char> 参数，它执行的是序号比较，因此无法找到匹配项。 第二个调用拥有 <xref:System.String> 参数，它执行的是区分区域性的比较，因此找到了匹配项。

[!code-csharp[Conceptual.Globalization#18](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/search1.cs#18)]
[!code-vb[Conceptual.Globalization#18](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/search1.vb#18)]

可以通过调用包含 <xref:System.StringComparison> 参数（如 <xref:System.String.IndexOf%28System.String%2CSystem.StringComparison%29?displayProperty=nameWithType> 或 <xref:System.String.LastIndexOf%28System.String%2CSystem.StringComparison%29?displayProperty=nameWithType> 方法）的重载来避免此示例中出现的一些多义性（对方法的两个相似重载的调用返回了不同的结果）。

但是，搜索并不总是区分区域性的。 如果搜索的目的在于做出安全性决策或是允许或禁止访问某些资源，应进行序号比较，此主题将在下一节中讨论。

### <a name="test-strings-for-equality"></a>测试字符串的相等性

如果要测试两个字符串是否相等，而不是确定如何按排序顺序进行比较，则使用 <xref:System.String.Equals%2A?displayProperty=nameWithType> 方法，而不是字符串比较方法，如 <xref:System.String.Compare%2A?displayProperty=nameWithType> 或 <xref:System.Globalization.CompareInfo.Compare%2A?displayProperty=nameWithType>。

通常比较相等性用于条件性地访问某些资源。 例如，可能需要比较相等性以验证密码或确认文件是否存在。 此类非语义比较应始终为序号比较，而不是区分区域性的比较。 一般情况下，应使用值为 <xref:System.StringComparison.Ordinal?displayProperty=nameWithType> 的字符串（如密码）和值为 <xref:System.StringComparison.OrdinalIgnoreCase?displayProperty=nameWithType> 的字符串（如文件名或 URI）来调用实例 <xref:System.String.Equals%28System.String%2CSystem.StringComparison%29?displayProperty=nameWithType> 方法或静态的 <xref:System.String.Equals%28System.String%2CSystem.String%2CSystem.StringComparison%29?displayProperty=nameWithType> 方法。

相等性的比较有时会涉及到搜索或子字符串比较，而不是对 <xref:System.String.Equals%2A?displayProperty=nameWithType> 方法的调用。 在某些情况下，可以使用子字符串搜索以确定子字符串是否与另一字符串相等。 如果比较的目的是非语义的，那么搜索也应该为序号搜索，而不区分区域性。

以下示例阐释了对非语义数据进行区分区域性搜索的危险。 `AccessesFileSystem` 方法旨在禁止文件系统访问以子字符串“FILE”开头的 URI。 为此，它对以字符串“FILE”开头的 URI 执行区分区域性、不区分大小写的比较。 由于访问文件系统的 URI 可以“FILE:”或“file:”开头，因此隐式假设“i”(U+0069) 始终为小写且等效于“I”(U+0049)。 但是，在土耳其语和阿塞拜疆语中，“i”的大写为“İ”(U+0130)。 由于存在此差异，因此区分区域性的比较在应禁止的情况下仍允许进行文件系统访问。

[!code-csharp[Conceptual.Globalization#12](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/equals1.cs#12)]
[!code-vb[Conceptual.Globalization#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/equals1.vb#12)]

因此，可执行忽视大小写的序号比较来避免出现此问题，如下例所示。

[!code-csharp[Conceptual.Globalization#13](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/equals2.cs#13)]
[!code-vb[Conceptual.Globalization#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/equals2.vb#13)]

### <a name="order-and-sort-strings"></a>顺序和排序字符串

通常，要在用户界面中显示的已排列字符串应根据区域性进行排序。 大多数情况下，在调用排序字符串的方法（如 <xref:System.Array.Sort%2A?displayProperty=nameWithType> 和 <xref:System.Collections.Generic.List%601.Sort%2A?displayProperty=nameWithType>）时，此类字符串比较是由 .NET 隐式处理的。 默认情况下，使用当前区域性的排序约定对字符串进行排序。 以下示例阐释了使用英语(美国)区域性和瑞典语(瑞典)区域性的约定对一组字符串进行排序时的差异。

[!code-csharp[Conceptual.Globalization#14](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/sort1.cs#14)]
[!code-vb[Conceptual.Globalization#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/sort1.vb#14)]

区分区域性的字符串比较是由 <xref:System.Globalization.CompareInfo> 对象定义的，该对象由每个区域性的 <xref:System.Globalization.CultureInfo.CompareInfo%2A?displayProperty=nameWithType> 属性返回。 使用 <xref:System.String.Compare%2A?displayProperty=nameWithType> 方法重载的区分区域性的字符串比较也使用 <xref:System.Globalization.CompareInfo> 对象。

.NET 使用表格对字符串数据进行区分区域性的排序。 这些表格的内容包含了数据的排序权重和字符串标准化，而这些内容是通过 .NET 的特定版本实现的 Unicode 标准版确定的。 下表列出了通过 .NET 指定版本所实现的 Unicode 版本。 受支持的 Unicode 版本列表仅适用于字符比较和排序；不适用于按类别分类的 Unicode 字符。 有关详细信息，请参阅 <xref:System.String> 文章中“字符串和 Unicode 标准”部分。

|.NET Framework 版本|操作系统|Unicode 版本|
|----------------------------|----------------------|---------------------|
|.NET Framework 2.0|所有操作系统|Unicode 4.1|
|.NET Framework 3.0|所有操作系统|Unicode 4.1|
|.NET Framework 3.5|所有操作系统|Unicode 4.1|
|.NET Framework 4|所有操作系统|Unicode 5.0|
|Windows 7 上的 .NET Framework 4.5 及更高版本|Unicode 5.0|
|Windows 8 和更高版本的操作系统上的 .NET Framework 4.5 及更高版本|Unicode 6.3.0|
|.NET Core 和 .NET 5+|取决于基础操作系统支持的 Unicode 标准版本。|

从 .NET Framework 4.5 起以及在 .NET Core 和 .NET 5+ 的所有版本中，字符串比较和排序取决于操作系统。 在 Windows 7 上运行的 .NET Framework 4.5 及更高版本从其自身实现 Unicode 5.0 的表中检索数据。 在 Windows 8 及更高版本上运行的 .NET Framework 4.5 及更高版本从实现 Unicode 6.3 的操作系统表中检索数据。 在 .NET Core 和 .NET 5+ 上，受支持的 Unicode 版本取决于基础操作系统。 如果对区分区域性的已排序数据进行序列化，可使用 <xref:System.Globalization.SortVersion> 类来确定何时需要对序列化数据进行排序，使其与 .NET 和操作系统的排序顺序保持一致。 有关示例，请参阅 <xref:System.Globalization.SortVersion> 类主题。

如果应用对字符串数据执行大量特定于区域性的排序，则可使用 <xref:System.Globalization.SortKey> 类来比较字符串。 排序关键字反映了特定字符串的特定于区域性的排序权重，包括字母顺序、大小写和音调符号权重。 由于使用排序关键字的比较为二进制，因此与显示或隐式使用 <xref:System.Globalization.CompareInfo> 对象的比较相比，这类比较速度更快。 可通过将字符串传递给 <xref:System.Globalization.CompareInfo.GetSortKey%2A?displayProperty=nameWithType> 方法为特定字符串创建区分区域性的排序关键字。

以下示例与前一个示例类似。 但是此示例没有调用 <xref:System.Array.Sort%28System.Array%29?displayProperty=nameWithType> 方法（其隐式调用了 <xref:System.Globalization.CompareInfo.Compare%2A?displayProperty=nameWithType> 方法），而是定义了对排序关键字进行比较的 <xref:System.Collections.Generic.IComparer%601?displayProperty=nameWithType> 实现，它会进行实例化并传递到 <xref:System.Array.Sort%60%601%28%60%600%5B%5D%2CSystem.Collections.Generic.IComparer%7B%60%600%7D%29?displayProperty=nameWithType> 方法。

[!code-csharp[Conceptual.Globalization#15](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/sortkey1.cs#15)]
[!code-vb[Conceptual.Globalization#15](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/sortkey1.vb#15)]

### <a name="avoid-string-concatenation"></a>避免字符串串联

如有可能，请避免使用在运行时从串联词组中生成的复合字符串。 复合字符串难以本地化，因为它们往往以应用的原始语言假设语法顺序，而此顺序并不适用于其他本地化语言。

## <a name="handle-dates-and-times"></a>处理日期和时间

如何处理日期和时间值取决于它们是要显示在用户界面中还是保留。 本节将讨论这两种用法。 同时也将讨论在处理日期和时间时应如何处理时区差异和算术运算。

### <a name="display-dates-and-times"></a>显示日期和时间

通常，如果日期和时间要显示在用户界面中，应使用用户区域性的格式约定，此约定是由 <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> 属性以及 `CultureInfo.CurrentCulture.DateTimeFormat` 属性返回的 <xref:System.Globalization.DateTimeFormatInfo> 对象定义的。 在使用以下 3 种方法之一设置日期的格式时会自动使用当前区域性的格式约定：

- 无参数的 <xref:System.DateTime.ToString?displayProperty=nameWithType> 方法

- <xref:System.DateTime.ToString%28System.String%29?displayProperty=nameWithType> 方法，其中包含一个格式字符串

- 无参数的 <xref:System.DateTimeOffset.ToString?displayProperty=nameWithType> 方法

- <xref:System.DateTimeOffset.ToString%28System.String%29?displayProperty=nameWithType>，其中包含一个格式字符串

- [复合格式](../base-types/composite-formatting.md)功能（与日期配合使用时）

以下示例显示了两次 2012 年 10 月 11 日的日出和日落数据。 它首先将当前区域性设置为克罗地亚语(克罗地亚)，然后是英语(英国)。 在每个用例中，日期和时间以适合当地区域性的格式显示。

[!code-csharp[Conceptual.Globalization#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/dates1.cs#2)]
[!code-vb[Conceptual.Globalization#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/dates1.vb#2)]

### <a name="persist-dates-and-times"></a>存留日期和时间

切勿将日期和时间数据保留为随区域性而异的格式。 这是常见的编程错误，会导致数据损坏或运行时异常。 以下示例使用英语(美国)区域性的格式约定将两个日期（2013 年 1 月 9 日和 2013 年 8 月 18 日）序列化为字符串。 在使用英语(美国)区域性的约定检索和分析该数据时，它会成功还原。 但在使用英语(英国)区域性的约定进行检索和分析时，第一个日期被错误地解释为 9 月 1 日，并且无法分析第二个日期，因为公历中没有第 18 个月。

[!code-csharp[Conceptual.Globalization#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/dates2.cs#3)]
[!code-vb[Conceptual.Globalization#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/dates2.vb#3)]

可通过以下 3 种方法之一来避免此问题：

- 以二进制格式对日期和时间进行序列化，而不是序列化为字符串。

- 不考虑用户的区域性，使用同一自定义格式字符串保存和分析日期和时间的字符串表示形式。

- 使用固定区域性的格式约定保存字符串。

以下示例演示了第 3 种方法。 它使用静态 <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType> 属性返回的固定区域性的格式约定。

[!code-csharp[Conceptual.Globalization#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/dates3.cs#4)]
[!code-vb[Conceptual.Globalization#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/dates3.vb#4)]

### <a name="serialization-and-time-zone-awareness"></a>序列化和时区感知

一个日期和时间值可能有多个解释，从常规时间（“商店于 2013 年 1 月 2 日上午 9 点开门。”）到某个特定时刻（“出生日期：2013 年 1 月 2 日上午 6:32:00。”）。 当时间值表示某个特定时刻并且将它从序列化的值中还原时，无论用户处于哪个地理位置或时区，都应确保它表示的是同一时刻。

以下示例阐释了此问题。 它将一个本地日期和时间值保存为字符串，采用 3 种[标准格式](../base-types/standard-date-and-time-format-strings.md)（"G" 表示常规日期长时间，"s" 表示可排序日期/时间，"o" 表示往返日期/时间）以及二进制格式。

[!code-csharp[Conceptual.Globalization#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/dates4.cs#10)]
[!code-vb[Conceptual.Globalization#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/dates4.vb#10)]

当还原数据的系统与对其进行序列化的系统位于同一时区时，反序列化的日期和时间值能准确地反映原始值，输出如下：

```console
'3/30/2013 6:00:00 PM' --> 3/30/2013 6:00:00 PM Unspecified
'2013-03-30T18:00:00' --> 3/30/2013 6:00:00 PM Unspecified
'2013-03-30T18:00:00.0000000-07:00' --> 3/30/2013 6:00:00 PM Local

3/30/2013 6:00:00 PM Local
```

但是，如果在处于其他时区的系统上还原数据，仅格式为“o”（往返）标准格式字符串的日期和时间值会保留时区信息，因此它会表示同一时刻。 当在处于罗马标准时区的系统上还原日期和时间数据时，输出如下：

```console
'3/30/2013 6:00:00 PM' --> 3/30/2013 6:00:00 PM Unspecified
'2013-03-30T18:00:00' --> 3/30/2013 6:00:00 PM Unspecified
'2013-03-30T18:00:00.0000000-07:00' --> 3/31/2013 3:00:00 AM Local

3/30/2013 6:00:00 PM Local
```

若要准确反映表示单个时刻的日期和时间值，而无需考虑对数据进行反序列化的系统所在时区，则可以执行以下任一操作：

- 使用“o”（往返）标准格式字符串将值保存为字符串。 然后在目标系统上进行反序列化。

- 将其转换为 UTC，并使用“r”(RFC1123) 标准格式字符串将其保存为一个字符串。 然后在目标系统上进行反序列化，并将其转换为本地时间。

- 将其转换为 UTC，并使用“u”（通用可排序）标准格式字符串将其保存为一个字符串。 然后在目标系统上进行反序列化，并将其转换为本地时间。

- 将其转换为 UTC，并保存为二进制格式。 然后在目标系统上进行反序列化，并将其转换为本地时间。

以下示例演示了每种方法。

[!code-csharp[Conceptual.Globalization#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/dates8.cs#11)]
[!code-vb[Conceptual.Globalization#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/dates8.vb#11)]

当在位于太平洋标准时区的系统上和在位于罗马标准时区的系统上对数据进行序列化时，该示例将显示以下输出：

```console
'2013-03-30T18:00:00.0000000-07:00' --> 3/31/2013 3:00:00 AM Local
'Sun, 31 Mar 2013 01:00:00 GMT' --> 3/31/2013 3:00:00 AM Local
'2013-03-31 01:00:00Z' --> 3/31/2013 3:00:00 AM Local

3/31/2013 3:00:00 AM Local
```

有关详细信息，请参阅[转换时区时间](../datetime/converting-between-time-zones.md)。

### <a name="perform-date-and-time-arithmetic"></a>执行日期和时间算法

<xref:System.DateTime> 和 <xref:System.DateTimeOffset> 类型都支持算术运算。 可以计算两个日期值之差，或者将日期值与特定的时间间隔相加或相减。 但是，对日期和时间值进行的算术运算时不考虑时区和时区调整规则。 因此，计算表示时刻的日期和时间值可能会返回错误结果。

例如，从太平洋标准时到太平洋夏令时的转换发生在 3 月的第二个星期日，即 2013 年 3 月 10 日。 如下面的示例所示，如果计算的日期和时间比太平洋标准时区系统上的 2013 年 3 月 9 日上午 10:30 晚 48 小时， 2013 年 3 月 11 日上午 10:30 这一结果不会考虑干预时间调整。

[!code-csharp[Conceptual.Globalization#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/dates5.cs#8)]
[!code-vb[Conceptual.Globalization#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/dates5.vb#8)]

若要确保日期和时间值的算术运算生成精准的结果，请执行以下步骤：

1. 将源时区中的时间转换为 UTC。

2. 执行算术运算。

3. 如果结果为日期和时间值，则将它从 UTC 转换成源时区中的时间。

以下示例与前一个示例类似，不同的是，它按照这 3 个步骤在 2013 年 3 月 9 日上午 10 点 30 分的基础上恰当添加了 48 个小时。

[!code-csharp[Conceptual.Globalization#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/dates6.cs#9)]
[!code-vb[Conceptual.Globalization#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/dates6.vb#9)]

有关详细信息，请参阅[执行日期和时间算术运算](../datetime/performing-arithmetic-operations.md)。

### <a name="use-culture-sensitive-names-for-date-elements"></a>对日期元素使用区分区域性的名称

应用可能需要显示月份的名称或星期几。 为此，常使用以下代码。

[!code-csharp[Conceptual.Globalization#19](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/monthname1.cs#19)]
[!code-vb[Conceptual.Globalization#19](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/monthname1.vb#19)]

但是，此代码始终以英语返回一周中某天的名称。 提取月份名称的代码通常更加固定。 它常常采用特定语言的月份名称来假设十二月历。

使用[自定义日期和时间格式字符串](../base-types/custom-date-and-time-format-strings.md)或 <xref:System.Globalization.DateTimeFormatInfo> 对象的属性，可以轻松提取字符串，以反映用户区域性中的星期几或月份名称，如下面的示例所示。 它将当前区域性更改为法语(法国)，并为 2013 年 7 月 1 日显示一周中某天的名称和月份的名称。

[!code-csharp[Conceptual.Globalization#20](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/monthname2.cs#20)]
[!code-vb[Conceptual.Globalization#20](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/monthname2.vb#20)]

## <a name="numeric-values"></a>数字值

数字的处理方式取决于它们是显示在用户界面中还是保留。 本节将讨论这两种用法。

> [!NOTE]
> 在分析和设置格式时，.NET 仅将 0 到 9（U+0030 到 U+0039）的基本拉丁字符识别为数字。

### <a name="display-numeric-values"></a>显示数字值

通常，如果数字显示在用户界面中，应使用用户区域性的格式约定，此约定由 <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> 属性以及 `CultureInfo.CurrentCulture.NumberFormat` 属性返回的 <xref:System.Globalization.NumberFormatInfo> 对象定义。 在使用以下任意方法设置日期的格式时会自动使用当前区域性的格式约定：

- 任何数值类型无参数的 `ToString` 方法

- 任何数值类型的 `ToString(String)` 方法，其中将格式字符串作为参数

- [复合格式](../base-types/composite-formatting.md)功能（与数值配合使用时）

以下示例显示法国巴黎每月的平均气温。 在显示数据之前，它首先将当前区域性设置为法语(法国)，然后再设置为英语(美国)。 在每个用例中，月份名称和气温以适合当地区域性的格式显示。 请注意，两个区域性使用不同的小数分隔符以分隔气温值。 另请注意，该示例使用“MMMM”自定义日期和时间格式字符串以显示完整的月份名称，并且它通过确定 <xref:System.Globalization.DateTimeFormatInfo.MonthNames%2A?displayProperty=nameWithType> 数组中最长月份名称的长度为结果字符串中的月份名称分配了足够的空间。

[!code-csharp[Conceptual.Globalization#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/numbers1.cs#5)]
[!code-vb[Conceptual.Globalization#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/numbers1.vb#5)]

### <a name="persist-numeric-values"></a>存留数字值

切勿将数值数据保留为特定于区域性的格式。 这是常见的编程错误，会导致数据损坏或运行时异常。 以下示例随机生成了十个浮点数，然后使用英语(美国)区域性的格式约定将它们序列化为字符串。 在使用英语(美国)区域性的约定检索和分析该数据时，它会成功还原。 但是，当使用法语(法国)区域性的约定进行检索和分析时，无法分析任何数字，因为区域性使用了不同的小数分隔符。

[!code-csharp[Conceptual.Globalization#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/numbers2.cs#6)]
[!code-vb[Conceptual.Globalization#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/numbers2.vb#6)]

若要避免此问题，可使用以下方法之一：

- 不考虑用户的区域性，使用同一自定义格式字符串保存和分析数字的字符串表示形式。

- 使用固定区域性的格式约定将数字保存为字符串，此约定是由 <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType> 属性返回的。

- 以二进制格式序列化数字，而不采用字符串格式。

以下示例演示了第 3 种方法。 它对 <xref:System.Double> 值的数组进行序列化，然后使用英语(美国)和法语(法国)区域性的格式约定进行反序列化并显示。

[!code-csharp[Conceptual.Globalization#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/numbers3.cs#7)]
[!code-vb[Conceptual.Globalization#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/numbers3.vb#7)]

货币值的序列化是一种特殊情况。 由于货币值取决于表示它的货币单位，因此将它视为独立的数值没有什么意义。 但是如果将货币值保存为包含货币符号的格式化字符串，则无法在其默认区域性使用不同货币符号的系统上对其进行反序列化，如下例所示。

[!code-csharp[Conceptual.Globalization#16](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/currency1.cs#16)]
[!code-vb[Conceptual.Globalization#16](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/currency1.vb#16)]

相反，应将数值和一些区域性信息一起序列化，如区域性的名称，这样数值和其货币符号才可在当前区域性中独立地进行反序列化。 以下示例通过定义带有两个参数（<xref:System.Decimal> 值和值所属的区域性的名称）的 `CurrencyValue` 结构来实现这一点。

[!code-csharp[Conceptual.Globalization#17](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.globalization/cs/currency2.cs#17)]
[!code-vb[Conceptual.Globalization#17](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.globalization/vb/currency2.vb#17)]

## <a name="work-with-culture-specific-settings"></a>使用特定于区域性的设置

在 .NET 中，<xref:System.Globalization.CultureInfo> 类表示特定的区域性或区域。 其中一些属性返回提供有关某些区域性方面的特定信息的对象：

- <xref:System.Globalization.CultureInfo.CompareInfo%2A?displayProperty=nameWithType> 属性返回 <xref:System.Globalization.CompareInfo> 对象，该对象包含有关如何比较区域性和排列字符串的信息。

- <xref:System.Globalization.CultureInfo.DateTimeFormat%2A?displayProperty=nameWithType> 属性返回 <xref:System.Globalization.DateTimeFormatInfo> 对象，该对象提供用于设置日期和时间数据格式的区域性特定信息。

- <xref:System.Globalization.CultureInfo.NumberFormat%2A?displayProperty=nameWithType> 属性返回 <xref:System.Globalization.NumberFormatInfo> 对象，该对象提供用于设置数值数据格式的区域性特定信息。

- <xref:System.Globalization.CultureInfo.TextInfo%2A?displayProperty=nameWithType> 属性返回 <xref:System.Globalization.TextInfo> 对象，该对象提供有关区域性写入系统的信息。

一般情况下，不要对特定的 <xref:System.Globalization.CultureInfo> 属性及其相关对象的值作出任何假设。 相反，应将区域性特定的数据视为可更改的，原因如下：

- 当数据损坏、有更好的数据可用或区域性特定的约定更改时，各个属性值是可更改且可修订。

- 各个属性值在各个 .NET 版本或操作系统版本中可能会有所不同。

- .NET 支持替换区域性。 由此可定义补充现有标准区域性或完全替换现有标准区域性的新的自定义区域性。

- 在 Windows 系统上，用户可使用“控制面板”中的“区域和语言”应用，自定义区域性专用设置。 在实例化 <xref:System.Globalization.CultureInfo> 对象时，可调用 <xref:System.Globalization.CultureInfo.%23ctor%28System.String%2CSystem.Boolean%29> 构造函数来确定它是否反射这些用户自定义。 通常，对最终用户应用而言，你应考虑用户首选项，以用户期望的格式呈现数据。

## <a name="see-also"></a>请参阅

- [全球化和本地化](index.md)
- [有关使用字符串的最佳做法](../base-types/best-practices-strings.md)
