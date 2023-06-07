---
title: .NET 中的格式设置类型
description: 了解如何对 .NET 中的类型进行格式设置。 了解如何使用或替代 ToString 方法。 了解区分区域性的自定义复合格式设置。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data formatting [.NET]
- dates [.NET], formatting
- date formatting [.NET]
- number formatting [.NET]
- ToString method
- custom cultural settings [.NET]
- numbers [.NET], formatting
- formatting strings [.NET]
- time [.NET], formatting
- currency [.NET], formatting
- types [.NET], formatting
- format specifiers [.NET]
- times [.NET], formatting
- culture [.NET], formatting
- formatting [.NET], types supported
- base types [.NET], formatting
- custom formatting [.NET]
- strings [.NET], formatting
ms.assetid: 0d1364da-5b30-4d42-8e6b-03378343343f
ms.openlocfilehash: a23df3ecfd3042927636813cdb94eecbb71438f0
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94828954"
---
# <a name="format-types-in-net"></a>.NET 中的格式类型

格式设置是指将类、结构或枚举值的实例转换为其字符串表示形式的过程，通常使得最终的字符串可以显示给用户，或者进行反序列化以还原为原始数据类型。 此转换可能面临一系列挑战：

- 在内部存储值的方式不一定反映用户想要查看它们的方式。 例如，电话号码可以存储为 8009999999 格式，但此格式并非是用户友好的格式。 该电话号码应显示为 800-999-9999。 有关以这种方式设置数字格式的示例，请参见 [自定义格式字符串](#custom-format-strings) 一节。

- 有时对象到其字符串表示形式的转换不是直观的。 例如，不清楚 Temperature 对象或 Person 对象的字符串表示形式应如何显示。 有关以各种方式设置 Temperature 对象格式的示例，请参见 [标准格式字符串](#standard-format-strings) 一节。

- 值通常需要区分区域性的格式。 例如，在使用数字表示货币值的应用程序中，数字字符串应包括当前区域性的货币符号、组分隔符（在大多数区域性中，组分隔符为千位分隔符）和小数点符号。 有关示例，请参阅[使用格式提供程序进行区分区域性的格式设置](#culture-sensitive-formatting-with-format-providers)部分。

- 应用程序可能需要以不同方式显示相同的值。 例如，应用程序可能通过显示名称的字符串表示形式来表示一个枚举成员，或通过显示基础值来表示该枚举成员。 有关以不同方式设置 <xref:System.DayOfWeek> 枚举成员格式的示例，请参见 [标准格式字符串](#standard-format-strings) 一节。

> [!NOTE]
> 格式设置将类型的值转换为字符串表示形式。 分析是格式设置的反向操作。 分析操作根据数据类型的字符串表示形式创建该数据类型的实例。 有关将字符串转换成其他数据类型的信息，请参见 “[分析字符串](parsing-strings.md)”。

.NET 提供了丰富的格式设置支持，使得开发人员可以满足这些要求。

## <a name="formatting-in-net"></a>.NET 中的格式设置

格式设置的基本机制是 <xref:System.Object.ToString%2A?displayProperty=nameWithType> 方法的默认实现。有关信息，请参阅本主题稍后将介绍的[使用 ToString 方法的默认格式设置](#default-formatting-using-the-tostring-method)部分。 不过，.NET 提供了几种方法来修改和扩展其默认格式设置支持。 其中包括：

- 重写 <xref:System.Object.ToString%2A?displayProperty=nameWithType> 方法以定义对象值的自定义字符串表示形式。 有关详细信息，请参见本主题后面的[重写 ToString 方法](#override-the-tostring-method)部分。

- 定义格式说明符，格式说明符允许对象值的字符串表示形式采用多种形式。 例如，以下语句中的“X”格式说明符将整数转换为十六进制值的字符串表示形式。

     [!code-csharp[Conceptual.Formatting.Overview#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/specifier1.cs#3)]
     [!code-vb[Conceptual.Formatting.Overview#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/specifier1.vb#3)]

     有关格式说明符的更多信息，请参见 [ToString 方法和格式字符串](#the-tostring-method-and-format-strings) 部分。

- 使用格式提供程序以利用特定区域性的格式设置约定。 例如，以下语句通过使用 en-US 区域性的格式设置约定来显示货币值。

     [!code-csharp[Conceptual.Formatting.Overview#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/specifier1.cs#10)]
     [!code-vb[Conceptual.Formatting.Overview#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/specifier1.vb#10)]

     有关使用格式提供程序进行格式设置的详细信息，请参阅[格式提供程序](#culture-sensitive-formatting-with-format-providers)部分。

- 实现 <xref:System.IFormattable> 接口可以支持使用 <xref:System.Convert> 类的字符串转换以及复合格式设置。 有关更多信息，请参见 [IFormattable 接口](#the-iformattable-interface) 部分。

- 使用复合格式设置来嵌入较大字符串中值的字符串表示形式。 有关更多信息，请参见 [复合格式设置](#composite-formatting) 部分。

- 实现 <xref:System.ICustomFormatter> 和 <xref:System.IFormatProvider> 可以提供完全自定义的格式设置解决方案。 有关更多信息，请参见 [使用 ICustomFormatter 进行自定义格式设置](#custom-formatting-with-icustomformatter) 部分。

以下各部分分别使用这些方法来将对象转换为其字符串表示形式。

## <a name="default-formatting-using-the-tostring-method"></a>使用 ToString 方法的默认格式设置

每个从 <xref:System.Object?displayProperty=nameWithType> 派生的类型都自动继承无参数的 `ToString` 方法，该方法在默认情况下返回类型的名称。 下面的示例演示默认 `ToString` 方法。 它定义一个名为 `Automobile` 、不具有实现的类。 当对该类进行实例化并调用其 `ToString` 方法时，它显示其类型名称。 请注意，此示例中未显式调用 `ToString` 方法。 <xref:System.Console.WriteLine%28System.Object%29?displayProperty=nameWithType> 方法隐式调用作为参数传递给它的对象的 `ToString` 方法。

[!code-csharp[Conceptual.Formatting.Overview#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/default1.cs#1)]
[!code-vb[Conceptual.Formatting.Overview#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/default1.vb#1)]

> [!WARNING]
> 自 Windows 8.1 起，Windows 运行时包括具有单个方法 [IStringable.ToString](xref:Windows.Foundation.IStringable.ToString%2A) 的 <xref:Windows.Foundation.IStringable> 接口，用于提供默认格式支持。 但是，我们建议托管类型不实现 `IStringable` 接口。 有关详细信息，请参阅 <xref:System.Object.ToString%2A?displayProperty=nameWithType> 参考页上的“Windows 运行时和 `IStringable` 接口”部分。

由于除接口以外的所有类型都派生自 <xref:System.Object>，因此会向自定义类或结构自动提供此功能。 但是，默认的 `ToString` 方法提供的功能限于：尽管它标识类型，但无法提供有关类型实例的任何信息。 若要提供可提供该对象相关信息的对象的字符串表示形式，必须重写 `ToString` 方法。

> [!NOTE]
> 结构继承自 <xref:System.ValueType>，而后者又派生自 <xref:System.Object>。 虽然 <xref:System.ValueType> 会重写 <xref:System.Object.ToString%2A?displayProperty=nameWithType>，但是其实现是相同的。

## <a name="override-the-tostring-method"></a>重写 ToString 方法

显示类型的名称这一用法往往有限，它不允许类型使用者区分实例。 但是，你可以重写 `ToString` 方法，以提供更有用的对象值表示形式。 下面的示例定义 `Temperature` 对象并重写其 `ToString` 方法，以便以摄氏度显示温度。

[!code-csharp[Conceptual.Formatting.Overview#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/overrides1.cs#2)]
[!code-vb[Conceptual.Formatting.Overview#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/overrides1.vb#2)]

在 .NET 中，每个基元值类型的 `ToString` 方法已重写为显示对象值（而不是对象名称）。 下表显示每种基元类型的重写。 请注意，大多数重写方法调用 `ToString` 方法的另一个重载并向其传递用于定义其类型的一般格式的“G”格式说明符和表示当前区域性的 <xref:System.IFormatProvider> 对象。

|类型|ToString 重写|
|----------|-----------------------|
|<xref:System.Boolean>|返回 <xref:System.Boolean.TrueString?displayProperty=nameWithType> 或 <xref:System.Boolean.FalseString?displayProperty=nameWithType>。|
|<xref:System.Byte>|调用 `Byte.ToString("G", NumberFormatInfo.CurrentInfo)` 可以为当前区域性设置 <xref:System.Byte> 值的格式。|
|<xref:System.Char>|以字符串形式返回字符。|
|<xref:System.DateTime>|调用 `DateTime.ToString("G", DatetimeFormatInfo.CurrentInfo)` 可以为当前区域性设置日期和时间值的格式。|
|<xref:System.Decimal>|调用 `Decimal.ToString("G", NumberFormatInfo.CurrentInfo)` 可以为当前区域性设置 <xref:System.Decimal> 值的格式。|
|<xref:System.Double>|调用 `Double.ToString("G", NumberFormatInfo.CurrentInfo)` 可以为当前区域性设置 <xref:System.Double> 值的格式。|
|<xref:System.Int16>|调用 `Int16.ToString("G", NumberFormatInfo.CurrentInfo)` 可以为当前区域性设置 <xref:System.Int16> 值的格式。|
|<xref:System.Int32>|调用 `Int32.ToString("G", NumberFormatInfo.CurrentInfo)` 可以为当前区域性设置 <xref:System.Int32> 值的格式。|
|<xref:System.Int64>|调用 `Int64.ToString("G", NumberFormatInfo.CurrentInfo)` 可以为当前区域性设置 <xref:System.Int64> 值的格式。|
|<xref:System.SByte>|调用 `SByte.ToString("G", NumberFormatInfo.CurrentInfo)` 可以为当前区域性设置 <xref:System.SByte> 值的格式。|
|<xref:System.Single>|调用 `Single.ToString("G", NumberFormatInfo.CurrentInfo)` 可以为当前区域性设置 <xref:System.Single> 值的格式。|
|<xref:System.UInt16>|调用 `UInt16.ToString("G", NumberFormatInfo.CurrentInfo)` 可以为当前区域性设置 <xref:System.UInt16> 值的格式。|
|<xref:System.UInt32>|调用 `UInt32.ToString("G", NumberFormatInfo.CurrentInfo)` 可以为当前区域性设置 <xref:System.UInt32> 值的格式。|
|<xref:System.UInt64>|调用 `UInt64.ToString("G", NumberFormatInfo.CurrentInfo)` 可以为当前区域性设置 <xref:System.UInt64> 值的格式。|

## <a name="the-tostring-method-and-format-strings"></a>ToString 方法和格式字符串

对象具有单一字符串表示形式时，可以依赖于默认 `ToString` 方法或重写 `ToString` 。 但是，对象的值通常具有多种表示形式。 例如，温度可以用华氏度、摄氏度或开氏度来表示。 同样，整数值 10 可以表示为多种形式，包括 10、10.0、1.0e01 或 $10.00。

为了允许单个值具有多种字符串表示形式，.NET 使用格式字符串。 格式字符串是包含一个或多个预定义格式说明符的字符串，这些格式说明符是单一字符或字符组，用于定义 `ToString` 方法应如何设置其输出格式。 然后将格式字符串作为参数传递给对象的 `ToString` 方法，并确定应如何显示该对象值的字符串表示形式。

.NET 中的所有数字类型、日期和时间类型以及枚举类型都支持一组预定义的格式说明符。 还可以使用格式字符串定义你应用程序所定义的数据类型的多种字符串表示形式。

### <a name="standard-format-strings"></a>标准格式字符串

标准格式字符串包含单个格式说明符，该格式说明符是一个字母字符，用于定义应用该格式说明符的对象的字符串表示形式，此外，它还包含一个可选的精度说明符，该精度说明符影响在结果字符串中显示的位数。 如果省略或不支持精度说明符，则标准格式说明符等效于标准格式字符串。

.NET 为所有数字类型、所有日期和时间类型以及所有枚举类型定义一组标准格式说明符。 例如，这些类别中的每一类别都支持“G”标准格式说明符，该标准格式说明符定义该类型的值的一般字符串表示形式。

枚举类型的标准格式字符串直接控制值的字符串表示形式。 传递给枚举值的 `ToString` 方法的格式字符串决定是使用其字符串名称（“G”和“F”格式说明符）、基础整数值（“D”格式说明符）还是十六进制值（“X”格式说明符）来显示值。 下面的示例演示如何使用标准格式字符串来设置 <xref:System.DayOfWeek> 枚举值的格式。

[!code-csharp[Conceptual.Formatting.Overview#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/standard1.cs#4)]
[!code-vb[Conceptual.Formatting.Overview#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/standard1.vb#4)]

有关枚举格式字符串的信息，请参见 [Enumeration Format Strings](enumeration-format-strings.md)。

数字类型的标准格式字符串通常定义一个结果字符串，该结果字符串的确切显示由一个或多个属性值控制。 例如，“C”格式说明符会将数字的格式设置为货币值。 调用 `ToString` 方法并使用“C”格式说明符作为唯一参数时，来自当前区域性的 <xref:System.Globalization.NumberFormatInfo> 对象的以下属性值用于定义数字值的字符串表示形式：

- <xref:System.Globalization.NumberFormatInfo.CurrencySymbol%2A> 属性，指定当前区域性的货币符号。

- <xref:System.Globalization.NumberFormatInfo.CurrencyNegativePattern%2A> 或 <xref:System.Globalization.NumberFormatInfo.CurrencyPositivePattern%2A> 属性，其返回的整数决定：

  - 货币符号的位置。

  - 负值由前导负号、尾随负号还是括号来表示。

  - 在数字值和货币符号之间是否有空格。

- <xref:System.Globalization.NumberFormatInfo.CurrencyDecimalDigits%2A> 属性，定义结果字符串中的小数位数。

- <xref:System.Globalization.NumberFormatInfo.CurrencyDecimalSeparator%2A> 属性，定义结果字符串中的小数分隔符符号。

- <xref:System.Globalization.NumberFormatInfo.CurrencyGroupSeparator%2A> 属性，定义组分隔符符号。

- <xref:System.Globalization.NumberFormatInfo.CurrencyGroupSizes%2A> 属性，定义小数点左边每个组的数字位数。

- <xref:System.Globalization.NumberFormatInfo.NegativeSign%2A> 属性，确定在未使用括号表示负值时结果字符串中使用的负号。

此外，数字格式字符串可以包含一个精度说明符。 该说明符的含义取决于与其一起使用的格式字符串，但是，它通常指示应在结果字符串中显示的总位数或小数位数。 例如，下面的示例使用“X4”标准数字字符串和精度说明符来创建具有四个十六进制位的字符串值。

[!code-csharp[Conceptual.Formatting.Overview#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/precisionspecifier1.cs#6)]
[!code-vb[Conceptual.Formatting.Overview#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/precisionspecifier1.vb#6)]

有关标准数字格式字符串的更多信息，请参见 [Standard Numeric Format Strings](standard-numeric-format-strings.md)。

日期和时间值的标准格式字符串是由特定 <xref:System.Globalization.DateTimeFormatInfo> 属性存储的自定义格式字符串的别名。 例如，如果使用“D”格式说明符调用日期和时间值的 `ToString` 方法，则使用当前区域性的 <xref:System.Globalization.DateTimeFormatInfo.LongDatePattern%2A?displayProperty=nameWithType> 属性中存储的自定义格式字符串来显示日期和时间。 （若要详细了解自定义格式字符串，请参阅[下一部分](#custom-format-strings)。）下面的示例阐释了此关系。

[!code-csharp[Conceptual.Formatting.Overview#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/alias1.cs#5)]
[!code-vb[Conceptual.Formatting.Overview#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/alias1.vb#5)]

有关标准日期和时间格式字符串的更多信息，请参见 “[标准日期和时间格式字符串](standard-date-and-time-format-strings.md)”。

还可以使用标准格式字符串来定义应用程序所定义的对象的字符串表示形式，它由对象的 `ToString(String)` 方法生成。 可以定义对象支持的特定标准格式说明符，还可以决定这些格式说明符是否区分大小写。 `ToString(String)` 方法的实现应支持下列各项：

- 一个“G”格式说明符，表示对象的常用或通用格式。 对象的 `ToString` 方法的无参数重载应调用其 `ToString(String)` 重载，并向其传递“G”标准格式字符串。

- 支持等于空引用（在 Visual Basic 中为`Nothing` ）的格式说明符。 应视等于空引用的格式说明符与“G”格式说明符等效。

例如， `Temperature` 类可以用摄氏度在内部存储温度，并使用格式限定符以摄氏度、华氏度和开氏度表示 `Temperature` 对象的值。 下面的示例进行了这方面的演示。

[!code-csharp[Conceptual.Formatting.Overview#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/appstandard1.cs#7)]
[!code-vb[Conceptual.Formatting.Overview#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/appstandard1.vb#7)]

### <a name="custom-format-strings"></a>自定义格式字符串

除了标准格式字符串之外，.NET 还为数字值以及日期和时间值定义了自定义格式字符串。 自定义格式字符串由定义值的字符串表示形式的一个或多个自定义格式说明符组成。 例如，对于 en-US 区域性，自定义日期和时间格式字符串“yyyy/mm/dd hh:mm:ss.ffff t zzz”将日期转换为“2008/11/15 07:45:00.0000 P -08:00”形式的字符串表示形式。 同样，自定义格式字符串“0000”将整数值 12 转换为“0012”。 有关自定义格式字符串的完整列表，请参见 “[自定义日期和时间格式字符串](custom-date-and-time-format-strings.md)” 和 “[自定义数字格式字符串](custom-numeric-format-strings.md)”。

如果格式字符串仅包含一个自定义格式说明符，则此格式说明符前面应带有百分比 (%) 符号，以免与标准格式说明符混淆。 下面的示例使用“M”自定义格式说明符来显示特定日期的一位数或两位数的月份。

[!code-csharp[Conceptual.Formatting.Overview#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/singlecustom1.cs#8)]
[!code-vb[Conceptual.Formatting.Overview#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/singlecustom1.vb#8)]

日期和时间值的许多标准格式字符串均是由 <xref:System.Globalization.DateTimeFormatInfo> 对象的属性所定义的自定义格式字符串的别名。 自定义格式字符串还为设置数字值或日期和时间值的应用程序定义格式提供了很大的灵活性。 你可以通过将多个自定义格式说明符组合成一个自定义格式字符串来为数字值以及日期和时间值定义你自己的自定义结果字符串。 下面的示例定义一个自定义格式字符串，该字符串在月份名称、日期和年份后的括号中显示星期几。

[!code-csharp[Conceptual.Formatting.Overview#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/custom1.cs#9)]
[!code-vb[Conceptual.Formatting.Overview#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/custom1.vb#9)]

以下示例定义了自定义格式字符串，其中 <xref:System.Int64> 值显示为标准的美国七位数电话号码及其区号。

[!code-csharp[Conceptual.Formatting.Overview#21](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/telnumber1.cs#21)]
[!code-vb[Conceptual.Formatting.Overview#21](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/telnumber1.vb#21)]

尽管标准格式字符串一般可以满足应用程序定义的类型的大多数格式设置需求，但你还可以定义自定义格式说明符来设置类型的格式。

### <a name="format-strings-and-net-types"></a>格式字符串和 .NET 类型

所有数字类型（即 <xref:System.Byte>、<xref:System.Decimal>、<xref:System.Double>、<xref:System.Int16>、<xref:System.Int32>、<xref:System.Int64>、<xref:System.SByte>、<xref:System.Single>、<xref:System.UInt16>、<xref:System.UInt32>、<xref:System.UInt64> 和 <xref:System.Numerics.BigInteger>）以及 <xref:System.DateTime>、<xref:System.DateTimeOffset>、<xref:System.TimeSpan>、<xref:System.Guid> 和所有枚举类型都支持使用格式字符串设置格式。 有关各类型支持的特定格式字符串的信息，请参见下列主题：

|Title|定义|
|-----------|----------------|
|[Standard Numeric Format Strings](standard-numeric-format-strings.md)|描述用于创建数字值的常用字符串表示形式的标准格式字符串。|
|[Custom Numeric Format Strings](custom-numeric-format-strings.md)|描述用于创建数字值的应用程序特定格式的自定义格式字符串。|
|[标准日期和时间格式字符串](standard-date-and-time-format-strings.md)|介绍了创建 <xref:System.DateTime> 和 <xref:System.DateTimeOffset> 值的常用字符串表示形式的标准格式字符串。|
|[自定义日期和时间格式字符串](custom-date-and-time-format-strings.md)|介绍了创建 <xref:System.DateTime> 和 <xref:System.DateTimeOffset> 值的应用专用格式的自定义格式字符串。|
|[标准 TimeSpan 格式字符串](standard-timespan-format-strings.md)|描述用于创建时间间隔的常用字符串表示形式的标准格式字符串。|
|[自定义 TimeSpan 格式字符串](custom-timespan-format-strings.md)|描述用于创建时间间隔的应用程序特定格式的自定义格式字符串。|
|[Enumeration Format Strings](enumeration-format-strings.md)|描述用于创建枚举值的字符串表示形式的标准格式字符串。|
|<xref:System.Guid.ToString%28System.String%29?displayProperty=nameWithType>|描述 <xref:System.Guid> 值的标准格式字符串。|

## <a name="culture-sensitive-formatting-with-format-providers"></a>使用格式提供程序进行区分区域性的格式设置

尽管格式说明符允许你自定义对象的格式设置，但是生成有意义的对象字符串表示形式通常需要附加格式设置信息。 例如，通过使用“C”标准格式字符串或自定义格式字符串（如“$ #,#.00”）来将数字格式设置为货币值至少需要提供有关正确的货币符号、组分隔符和小数点分隔符的信息，以便包括在带有格式的字符串中。 在 .NET 中，此附加格式设置信息通过 <xref:System.IFormatProvider> 接口提供，具体是以参数形式提供给数字类型以及日期和时间类型的 `ToString` 方法的一个或多个重载。 <xref:System.IFormatProvider> 实现在 .NET 中用于支持区域性专用格式设置。 下面的示例演示在使用三个代表不同区域的 <xref:System.IFormatProvider> 对象设置某个对象的格式时，该对象的字符串表示形式将如何变化。

[!code-csharp[Conceptual.Formatting.Overview#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/iformatprovider1.cs#11)]
[!code-vb[Conceptual.Formatting.Overview#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/iformatprovider1.vb#11)]

<xref:System.IFormatProvider> 接口包含一个 <xref:System.IFormatProvider.GetFormat%28System.Type%29>方法，该方法只有一个参数，该参数指定提供格式设置信息的对象类型。 如果该方法可以提供该类型的对象，则返回它。 否则，它返回空引用（在 Visual Basic 中为`Nothing` ）。

<xref:System.IFormatProvider.GetFormat%2A?displayProperty=nameWithType> 为回调方法。 调用包含 `ToString` 参数的 <xref:System.IFormatProvider> 方法重载时，它调用该 <xref:System.IFormatProvider.GetFormat%2A> 对象的 <xref:System.IFormatProvider> 方法。 <xref:System.IFormatProvider.GetFormat%2A> 方法负责将提供所需格式设置信息（就像 `formatType` 参数指定的一样）的对象返回给 `ToString` 方法。

一些格式设置或字符串转换方法包含 <xref:System.IFormatProvider>类型的参数，但是很多情况下在调用该方法时将忽略该参数的值。 下表列出了使用 <xref:System.Type> 对象的参数和类型的一些格式设置方法，该对象传递给 <xref:System.IFormatProvider.GetFormat%2A?displayProperty=nameWithType> 方法。

|方法|`formatType` 参数的类型|
|------------|------------------------------------|
|数字类型的`ToString` 方法|<xref:System.Globalization.NumberFormatInfo?displayProperty=nameWithType>|
|日期和时间类型的`ToString` 方法|<xref:System.Globalization.DateTimeFormatInfo?displayProperty=nameWithType>|
|<xref:System.String.Format%2A?displayProperty=nameWithType>|<xref:System.ICustomFormatter?displayProperty=nameWithType>|
|<xref:System.Text.StringBuilder.AppendFormat%2A?displayProperty=nameWithType>|<xref:System.ICustomFormatter?displayProperty=nameWithType>|

> [!NOTE]
> 将重载数字类型以及日期和时间类型的 `ToString` 方法，并且只有某些重载包含 <xref:System.IFormatProvider> 参数。 如果方法没有 <xref:System.IFormatProvider>类型的参数，则改为传递 <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> 属性所返回的对象。 例如，对默认 <xref:System.Int32.ToString?displayProperty=nameWithType> 方法的调用最终将导致诸如以下的方法调用： `Int32.ToString("G", System.Globalization.CultureInfo.CurrentCulture)`。

.NET 提供了三个实现 <xref:System.IFormatProvider> 的类：

- <xref:System.Globalization.DateTimeFormatInfo>类，提供特定区域性的日期和时间值的格式设置信息。 其 <xref:System.IFormatProvider.GetFormat%2A?displayProperty=nameWithType> 实现返回它自身的实例。

- <xref:System.Globalization.NumberFormatInfo>类，提供特定区域性的数字格式设置信息。 其 <xref:System.IFormatProvider.GetFormat%2A?displayProperty=nameWithType> 实现返回它自身的实例。

- <xref:System.Globalization.CultureInfo>。 其 <xref:System.IFormatProvider.GetFormat%2A?displayProperty=nameWithType> 实现可以返回一个 <xref:System.Globalization.NumberFormatInfo> 对象（可提供数字格式设置信息）或一个 <xref:System.Globalization.DateTimeFormatInfo> 对象（可提供日期和时间值的格式设置信息）。

你还可以实现自己的格式提供程序来替换上述任意一个类。 但是，如果你的实现的 <xref:System.IFormatProvider.GetFormat%2A> 方法必须向 `ToString` 方法提供格式设置信息，则它必须返回上表中列出的相应类型的对象。

### <a name="culture-sensitive-formatting-of-numeric-values"></a>数值的区分区域性的格式设置

默认情况下，数值的格式设置是区分区域性的。 如果在调用格式设置方法时不指定区域性，则将使用当前线程区域性的格式设置约定。 下面的示例演示了这一点，其中对当前线程区域性进行了四次更改，随后调用了 <xref:System.Decimal.ToString%28System.String%29?displayProperty=nameWithType> 方法。 每次更改后，结果字符串均反映当前区域性的格式设置约定。 这是因为 `ToString` 和 `ToString(String)` 方法会包装对每个数值类型的 `ToString(String, IFormatProvider)` 方法的调用。

[!code-csharp[Conceptual.Formatting.Overview#19](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/culturespecific3.cs#19)]
[!code-vb[Conceptual.Formatting.Overview#19](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/culturespecific3.vb#19)]

你还可以设置特定区域性数值的格式，方法是调用具有 `ToString` 参数的 `provider` 重载，并将其作为以下对象之一的参数进行传递：

- 一个 <xref:System.Globalization.CultureInfo> 对象，此对象代表要使用其格式设置约定的区域性。 它的 <xref:System.Globalization.CultureInfo.GetFormat%2A?displayProperty=nameWithType> 方法会返回 <xref:System.Globalization.CultureInfo.NumberFormat%2A?displayProperty=nameWithType> 属性的值，即提供数字区域性特定格式设置信息的 <xref:System.Globalization.NumberFormatInfo> 对象。

- 一个 <xref:System.Globalization.NumberFormatInfo> 对象，此对象用于定义要使用的区域性特定格式设置约定。 它的 <xref:System.Globalization.NumberFormatInfo.GetFormat%2A> 方法会返回它自身的一个实例。

下面的示例使用了表示英语（美国）和英语（英国）区域性以及法语和俄罗斯语非特定区域性的 <xref:System.Globalization.NumberFormatInfo> 对象，来设置浮点数字的格式。

[!code-csharp[Conceptual.Formatting.Overview#20](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/culturespecific4.cs#20)]
[!code-vb[Conceptual.Formatting.Overview#20](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/culturespecific4.vb#20)]

### <a name="culture-sensitive-formatting-of-date-and-time-values"></a>日期和时间值的区分区域性的格式设置

默认情况下，日期和时间值的格式设置是区分区域性的。 如果在调用格式设置方法时不指定区域性，则将使用当前线程区域性的格式设置约定。 下面的示例演示了这一点，其中对当前线程区域性进行了四次更改，随后调用了 <xref:System.DateTime.ToString%28System.String%29?displayProperty=nameWithType> 方法。 每次更改后，结果字符串均反映当前区域性的格式设置约定。 这是因为 <xref:System.DateTime.ToString?displayProperty=nameWithType>、 <xref:System.DateTime.ToString%28System.String%29?displayProperty=nameWithType>、 <xref:System.DateTimeOffset.ToString?displayProperty=nameWithType>和 <xref:System.DateTimeOffset.ToString%28System.String%29?displayProperty=nameWithType> 方法会包装对 <xref:System.DateTime.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType> 及 <xref:System.DateTimeOffset.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType> 方法的调用。

[!code-csharp[Conceptual.Formatting.Overview#17](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/culturespecific1.cs#17)]
[!code-vb[Conceptual.Formatting.Overview#17](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/culturespecific1.vb#17)]

你还可以设置特定区域性日期和时间值的格式，方法是调用具有 <xref:System.DateTime.ToString%2A?displayProperty=nameWithType> 参数的 <xref:System.DateTimeOffset.ToString%2A?displayProperty=nameWithType> 或 `provider` 重载，并将其作为以下对象之一的参数进行传递：

- 一个 <xref:System.Globalization.CultureInfo> 对象，此对象代表要使用其格式设置约定的区域性。 它的 <xref:System.Globalization.CultureInfo.GetFormat%2A?displayProperty=nameWithType> 方法会返回 <xref:System.Globalization.CultureInfo.DateTimeFormat%2A?displayProperty=nameWithType> 属性的值，即提供日期和时间值区域性特定格式设置信息的 <xref:System.Globalization.DateTimeFormatInfo> 对象。

- 一个 <xref:System.Globalization.DateTimeFormatInfo> 对象，此对象用于定义要使用的区域性特定格式设置约定。 它的 <xref:System.Globalization.DateTimeFormatInfo.GetFormat%2A> 方法会返回它自身的一个实例。

下面的示例使用了表示英语（美国）和英语（英国）区域性以及法语和俄罗斯语非特定区域性的 <xref:System.Globalization.DateTimeFormatInfo> 对象，来设置日期的格式。

[!code-csharp[Conceptual.Formatting.Overview#18](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/culturespecific2.cs#18)]
[!code-vb[Conceptual.Formatting.Overview#18](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/culturespecific2.vb#18)]

## <a name="the-iformattable-interface"></a>IFormattable 接口

通常，使用格式字符串和一个 `ToString` 参数来重载 <xref:System.IFormatProvider> 方法的类型还实现 <xref:System.IFormattable> 接口。 此接口具有一个成员 <xref:System.IFormattable.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType>，该成员同时将格式字符串和格式提供程序作为参数。

对应用程序定义的类实现 <xref:System.IFormattable> 接口具有两大优势：

- 支持使用 <xref:System.Convert> 类进行字符串转换。 对 <xref:System.Convert.ToString%28System.Object%29?displayProperty=nameWithType> 和 <xref:System.Convert.ToString%28System.Object%2CSystem.IFormatProvider%29?displayProperty=nameWithType> 方法的调用会自动调用 <xref:System.IFormattable> 实现。

- 支持复合格式设置。 如果使用包含格式字符串的格式项设置自定义类型的格式，则公共语言运行时自动调用 <xref:System.IFormattable> 实现，并向其传递该格式字符串。 有关采用 <xref:System.String.Format%2A?displayProperty=nameWithType> 或 <xref:System.Console.WriteLine%2A?displayProperty=nameWithType>等方法进行复合格式设置的更多信息，请参见 [复合格式设置](#composite-formatting) 部分。

下面的示例定义一个实现 `Temperature` 接口的 <xref:System.IFormattable> 类。 它支持“C”或“G”格式说明符（用于以摄氏度显示温度）、“F”格式说明符（用于以华氏度显示温度）和“K”格式说明符（用于以开氏度显示温度）。

[!code-csharp[Conceptual.Formatting.Overview#12](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/iformattable.cs#12)]
[!code-vb[Conceptual.Formatting.Overview#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/iformattable.vb#12)]

下面的示例实例化一个 `Temperature` 对象。 然后，它调用 <xref:System.Convert.ToString%2A> 方法，并使用多个复合格式字符串获取 `Temperature` 对象的不同字符串表示形式。 其中每一个方法调用都依次调用 <xref:System.IFormattable> 类的 `Temperature` 实现。

[!code-csharp[Conceptual.Formatting.Overview#13](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/iformattable.cs#13)]
[!code-vb[Conceptual.Formatting.Overview#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/iformattable.vb#13)]

## <a name="composite-formatting"></a>复合格式设置

一些方法（如 <xref:System.String.Format%2A?displayProperty=nameWithType> 和 <xref:System.Text.StringBuilder.AppendFormat%2A?displayProperty=nameWithType>）支持复合格式设置。 复合格式字符串是一种模板，该模板返回合并了零个、一个或多个对象的字符串表示形式的单一字符串。 每个对象均由复合格式字符串中的索引格式项表示。 格式项的索引对应于格式项在方法的参数列表中所表示的对象位置。 索引是从零开始的。 例如，在以下对 <xref:System.String.Format%2A?displayProperty=nameWithType> 方法的调用中，第一个格式项 `{0:D}`被 `thatDate`的字符串表示形式；第二个格式项 `{1}`被 `item1`的字符串表示形式替换；第三个格式项 `{2:C2}`被 `item1.Value`的字符串表示形式替换。

[!code-csharp[Conceptual.Formatting.Overview#14](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/composite1.cs#14)]
[!code-vb[Conceptual.Formatting.Overview#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/composite1.vb#14)]

除了将格式项替换为其相应对象的字符串表示形式之外，格式项还可让你控制：

- 将对象表示为字符串的特定方法（如果对象实现 <xref:System.IFormattable> 接口并支持格式字符串）。 为此，可在格式项的索引后加上 `:` （冒号），后跟一个有效的格式字符串。 前面的示例执行此操作的方式是：格式化带有“d”（短日期模式）格式字符串（例如， `{0:d}`）的日期值，并格式化带有“C2”格式字符串（例如， `{2:C2}` ）的数值，将数量表示为具有两位小数位数的货币值。

- 包含对象的字符串表示形式的字段的宽度以及该字段中字符串表现形式的对齐方式。 为此，可在格式项的索引后加上 `,` （逗号），后跟字段宽度。 如果字段宽度为正值，则字段中的字符串为右对齐，如果字段宽度是负值，则为左对齐。 在下面的示例中，在由 20 个字符组成的字段中的日期值左对齐，而在由 11 个字符组成的字段中，带有一位小数的十进制值右对齐。

     [!code-csharp[Conceptual.Formatting.Overview#22](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/composite2.cs#22)]
     [!code-vb[Conceptual.Formatting.Overview#22](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/composite2.vb#22)]

     请注意，如果对齐字符串组件和格式字符串组件均存在，则前者位于后者之前（例如， `{0,-20:g}`）。

有关复合格式的更多信息，请参见 “[复合格式设置](composite-formatting.md)”。

## <a name="custom-formatting-with-icustomformatter"></a>使用 ICustomFormatter 进行自定义格式设置

两种复合格式设置方法（ <xref:System.String.Format%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> 和 <xref:System.Text.StringBuilder.AppendFormat%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType>）包括一个支持自定义格式设置的格式提供程序。 当调用其中一种格式设置方法时，该方法会将表示 <xref:System.Type> 接口的 <xref:System.ICustomFormatter> 对象传递到格式提供程序的 <xref:System.IFormatProvider.GetFormat%2A> 方法。 <xref:System.IFormatProvider.GetFormat%2A> 方法然后负责返回提供自定义格式设置功能的 <xref:System.ICustomFormatter> 实现。

<xref:System.ICustomFormatter> 接口具有一个方法 <xref:System.ICustomFormatter.Format%28System.String%2CSystem.Object%2CSystem.IFormatProvider%29>，复合格式设置方法为复合格式字符串中的每一格式项自动调用一次该方法。 <xref:System.ICustomFormatter.Format%28System.String%2CSystem.Object%2CSystem.IFormatProvider%29> 方法具有三个参数：一个格式字符串（表示格式项中的 `formatString` 参数）、一个要设置格式的对象和一个提供格式设置服务的 <xref:System.IFormatProvider> 对象。 通常，实现 <xref:System.ICustomFormatter> 的类还会实现 <xref:System.IFormatProvider>，因此上述最后一个参数是对自定义格式设置类自身的引用。 该方法返回要设置格式的对象的带格式自定义字符串表示形式。 如果该方法无法设置对象的格式，则应返回空引用（在 Visual Basic 中为`Nothing` ）。

下面的示例提供一个名为 <xref:System.ICustomFormatter> 的 `ByteByByteFormatter` 实现，该实现将整数值显示为两位的十六进制值后跟一个空格的序列。

[!code-csharp[Conceptual.Formatting.Overview#15](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/icustomformatter1.cs#15)]
[!code-vb[Conceptual.Formatting.Overview#15](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/icustomformatter1.vb#15)]

下面的示例使用 `ByteByByteFormatter` 类设置整数值的格式。 请注意，此示例中未显式调用 <xref:System.ICustomFormatter.Format%2A?displayProperty=nameWithType> 方法调用中多次调用了 <xref:System.String.Format%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> 方法，并在第三个方法调用中使用了默认 <xref:System.Globalization.NumberFormatInfo> 提供程序，这是因为 .`ByteByByteFormatter.Format` 方法无法识别“N0”格式字符串并返回空引用（在 Visual Basic 中为`Nothing` ）的格式说明符。

[!code-csharp[Conceptual.Formatting.Overview#16](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/icustomformatter1.cs#16)]
[!code-vb[Conceptual.Formatting.Overview#16](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/icustomformatter1.vb#16)]

## <a name="related-topics"></a>相关主题

|Title|定义|
|-----------|----------------|
|[Standard Numeric Format Strings](standard-numeric-format-strings.md)|描述用于创建数字值的常用字符串表示形式的标准格式字符串。|
|[Custom Numeric Format Strings](custom-numeric-format-strings.md)|描述用于创建数字值的应用程序特定格式的自定义格式字符串。|
|[标准日期和时间格式字符串](standard-date-and-time-format-strings.md)|描述用于创建 <xref:System.DateTime> 值的常用字符串表示形式的标准格式字符串。|
|[自定义日期和时间格式字符串](custom-date-and-time-format-strings.md)|描述用于创建 <xref:System.DateTime> 值的应用程序特定格式的自定义格式字符串。|
|[标准 TimeSpan 格式字符串](standard-timespan-format-strings.md)|描述用于创建时间间隔的常用字符串表示形式的标准格式字符串。|
|[自定义 TimeSpan 格式字符串](custom-timespan-format-strings.md)|描述用于创建时间间隔的应用程序特定格式的自定义格式字符串。|
|[Enumeration Format Strings](enumeration-format-strings.md)|描述用于创建枚举值的字符串表示形式的标准格式字符串。|
|[复合格式设置](composite-formatting.md)|描述如何将一个或多个设置了格式的值嵌入字符串。 然后该字符串可以显示在控制台上或被写至流。|
|[分析字符串](parsing-strings.md)|描述如何将对象初始化为这些对象的字符串表示形式所描述的值。 分析是格式化的反向操作。|

## <a name="reference"></a>参考

- <xref:System.IFormattable?displayProperty=nameWithType>
- <xref:System.IFormatProvider?displayProperty=nameWithType>
- <xref:System.ICustomFormatter?displayProperty=nameWithType>
