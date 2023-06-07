---
title: 复合格式设置
description: 了解 .NET 复合格式设置，它使用对象列表和复合格式字符串作为输入，其中包含带有索引占位符的固定文本。
ms.date: 10/26/2018
dev_langs:
- csharp
- vb
helpviewer_keywords:
- parameter specifiers
- strings [.NET], alignment
- format specifiers, composite formatting
- strings [.NET], composite
- composite formatting
- objects [.NET], formatting multiple objects
ms.assetid: 87b7d528-73f6-43c6-b71a-f23043039a49
ms.openlocfilehash: a0252d013ee6cf7cba7f953fc8a1e2c66c510ca7
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95683948"
---
# <a name="composite-formatting"></a>复合格式设置

.NET 复合格式设置功能使用对象列表和复合格式字符串作为输入。 复合格式字符串由固定文本和索引占位符混和组成，其中索引占位符称为格式项，对应于列表中的对象。 格式设置操作产生的结果字符串由原始固定文本和列表中对象的字符串表示形式混和组成。  
  
> [!IMPORTANT]
> 相较使用复合格式字符串，如果正在使用的语言和语言版本支持，则可使用 *内插字符串*。 内插字符串是包含内插表达式的字符串。 每个内插表达式都使用表达式的值进行解析，并在分配字符串时包含在结果字符串中。 有关详细信息，请参阅[字符串内插（C# 参考）](../../csharp/language-reference/tokens/interpolated.md)和[内插字符串（Visual Basic 参考）](../../visual-basic/programming-guide/language-features/strings/interpolated-strings.md)。

复合格式设置功能受诸如以下方法的支持：  
  
- <xref:System.String.Format%2A?displayProperty=nameWithType>，它返回格式化的结果字符串。  
  
- <xref:System.Text.StringBuilder.AppendFormat%2A?displayProperty=nameWithType>，它将格式化的结果字符串追加到 <xref:System.Text.StringBuilder> 对象。
- <xref:System.Console.WriteLine%2A?displayProperty=nameWithType> 方法的某些重载，它将格式化的结果字符串显示到控制台上。  
  
- <xref:System.IO.TextWriter.WriteLine%2A?displayProperty=nameWithType> 方法的某些重载，它将格式化的结果字符串写入流或文件中。 派生自 <xref:System.IO.TextWriter> 的类（如 <xref:System.IO.StreamWriter> 和 <xref:System.Web.UI.HtmlTextWriter>）也共享此功能。  
  
- <xref:System.Diagnostics.Debug.WriteLine%28System.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType>，它将格式化消息输出到跟踪侦听器。  
  
- <xref:System.Diagnostics.Trace.TraceError%28System.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType>、<xref:System.Diagnostics.Trace.TraceInformation%28System.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> 和 <xref:System.Diagnostics.Trace.TraceWarning%28System.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> 方法，它们将格式化消息输出到跟踪侦听器。  
  
- <xref:System.Diagnostics.TraceSource.TraceInformation%28System.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> 方法，它将信息性方法写入跟踪侦听器中。  
  
## <a name="composite-format-string"></a>复合格式字符串  

 复合格式字符串和对象列表将用作支持复合格式设置功能的方法的参数。 复合格式字符串由零个或多个固定文本段与一个或多个格式项混和组成。 固定文本是所选择的任何字符串，并且每个格式项对应于列表中的一个对象或装箱的结构。 复合格式设置功能返回新的结果字符串，其中每个格式项都被列表中相应对象的字符串表示形式取代。  
  
 可考虑使用以下 <xref:System.String.Format%2A> 代码段。  
  
 [!code-csharp[Formatting.Composite#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Composite/cs/Composite1.cs#1)]
 [!code-vb[Formatting.Composite#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Composite/vb/Composite1.vb#1)]  
  
 固定文本为“`Name =`”和“`, hours =`”。 格式项为“`{0}`”和“`{1:hh}`”，前者的索引为 0，对应于对象 `name`，后者的索引为 1，对应于对象 `DateTime.Now`。  
  
## <a name="format-item-syntax"></a>格式项语法  

 每个格式项都采用下面的形式并包含以下组件：  
  
 `{` *index*[`,`*alignment*][`:`*formatString*]`}`  
  
 必须使用成对的大括号（“{”和“}”）。  
  
### <a name="index-component"></a>索引组件  

 必需的 *索引* 组件（也叫参数说明符）是一个从 0 开始的数字，可标识对象列表中对应的项。 也就是说，参数说明符为 0 的格式项列表中的第一个对象，参数说明符为 1 的格式项列表中的第二个对象，依次类推。 下面的示例包括四个参数说明符，编号为 0 到 3，用于表示小于 10 的质数：  
  
 [!code-csharp[Formatting.Composite#7](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Composite/cs/index1.cs#7)]
 [!code-vb[Formatting.Composite#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Composite/vb/index1.vb#7)]  
  
 通过指定相同的参数说明符，多个格式项可以引用对象列表中的同一个元素。 例如，可以将同一个数值设置为十六进制、科学记数法和数字格式，方法是通过指定复合格式字符串（例如：“0x{0:X} {0:E} {0:N}”），如以下示例所示。  
  
 [!code-csharp[Formatting.Composite#10](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Composite/cs/index1.cs#10)]
 [!code-vb[Formatting.Composite#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Composite/vb/index1.vb#10)]  
  
 每个格式项都可以引用列表中的任一对象。 例如，如果有三个对象，可以指定“{1} {0} {2}”等复合格式字符串，以设置第二个、第一个和第三个对象的格式。 格式项未引用的对象会被忽略。 如果参数说明符指定了超出对象列表范围的项，将引发运行时 <xref:System.FormatException>。  
  
### <a name="alignment-component"></a>对齐组件  

 可选的 *对齐* 组件是一个带符号的整数，指示首选的设置了格式的字段宽度。 如果 *alignment* 值小于设置了格式的字符串的长度，*alignment* 将被忽略，并使用设置了格式的字符串的长度作为字段宽度。 如果 *alignment* 为正数，字段中设置了格式的数据为右对齐；如果 *alignment* 为负数，字段中的设置了格式的数据为左对齐。 如果需要填充，则使用空白。 如果指定 *alignment*，则需要使用逗号。  
  
 下面的示例定义两个数组，一个包含雇员的姓名，另一个则包含雇员在两周内的工作小时数。 复合格式字符串使 20 字符字段中的姓名左对齐，使 5 字符字段中的工作小时数右对齐。 请注意“N1”标准格式字符串还用于设置带有小数位的小时数格式。  
  
 [!code-csharp[Formatting.Composite#8](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Composite/cs/alignment1.cs#8)]
 [!code-vb[Formatting.Composite#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Composite/vb/alignment1.vb#8)]  
  
### <a name="format-string-component"></a>格式字符串组件  

 可选的 *格式字符串* 组件是适合正在设置格式的对象类型的格式字符串。 如果相应对象是数值，指定标准或自定义数字格式字符串；如果相应对象是 <xref:System.DateTime> 对象，指定标准或自定义日期和时间格式字符串；或者，如果相应对象是枚举值，指定[枚举格式字符串](enumeration-format-strings.md)。 如果不指定 *formatString*，则对数字、日期和时间或者枚举类型使用常规（“G”）格式说明符。 如果指定 *formatString*，则需要使用冒号。  
  
 下表列出了 .NET 类库中支持预定义的格式字符串集的类型或类型的类别，并提供指向列出了支持的格式字符串的主题的链接。 请注意，字符串格式化是一个可扩展的机制，可使用该机制定义所有现有类型的新的格式字符串，并定义受应用程序定义的类型支持的格式字符串集。 有关详细信息，请参阅 <xref:System.IFormattable> 和 <xref:System.ICustomFormatter> 接口主题。  
  
|类型或类型类别|查看|  
|---------------------------|---------|  
|日期和时间类型（<xref:System.DateTime>，<xref:System.DateTimeOffset>）|[标准日期和时间格式字符串](standard-date-and-time-format-strings.md)<br /><br /> [自定义日期和时间格式字符串](custom-date-and-time-format-strings.md)|  
|枚举类型（所有派生自 <xref:System.Enum?displayProperty=nameWithType> 的类型）|[枚举格式字符串](enumeration-format-strings.md)|  
|数值类型（<xref:System.Numerics.BigInteger>、<xref:System.Byte>、<xref:System.Decimal>、<xref:System.Double>、<xref:System.Int16>、<xref:System.Int32>、<xref:System.Int64>、<xref:System.SByte>、<xref:System.Single>、<xref:System.UInt16>、 <xref:System.UInt32>、<xref:System.UInt64>）|[标准数字格式字符串](standard-numeric-format-strings.md)<br /><br /> [自定义数字格式字符串](custom-numeric-format-strings.md)|  
|<xref:System.Guid>|<xref:System.Guid.ToString%28System.String%29?displayProperty=nameWithType>|  
|<xref:System.TimeSpan>|[标准 TimeSpan 格式字符串](standard-timespan-format-strings.md)<br /><br /> [自定义 TimeSpan 格式字符串](custom-timespan-format-strings.md)|  
  
### <a name="escaping-braces"></a>转义大括号  

 左大括号和右大括号被解释为格式项的开始和结束。 因此，必须使用转义序列显示文本左大括号或右大括号。 在固定文本中指定两个左大括号 ("{{") 以显示一个左大括号 ("{")，或指定两个右大括号 ("}}") 以显示一个右大括号 ("}")。 按照在格式项中遇到大括号的顺序依次解释它们。 不支持解释嵌套的大括号。  
  
 解释转义大括号的方式会导致意外的结果。 例如，假设格式项为“{{{0:D}}}”，旨在显示左大括号、采用十进制数格式的数值和右大括号。 但是，实际是按照以下方式解释该格式项：  
  
1. 前两个左大括号 ("{{") 被转义，生成一个左大括号。  
  
2. 之后的三个字符 ("{0:") 被解释为格式项的开始。  
  
3. 下一个字符 ("D") 将被解释为 Decimal 标准数值格式说明符，但后面的两个转义大括号 ("}}") 生成单个大括号。 由于得到的字符串 ("D}") 不是标准数值格式说明符号，所以得到的字符串会被解释为用于显示字符串“D}”的自定义格式字符串。  
  
4. 最后一个大括号 ("}") 被解释为格式项的结束。  
  
5. 显示的最终结果是字符串“{D}”。 不会显示本来要设置格式的数值。  
  
 在编写代码时，避免错误解释转义大括号和格式项的一种方法是单独设置大括号和格式项的格式。 也就是说，在第一个格式设置操作中显示文本左大括号，在下一操作中显示格式项的结果，然后在最后一个操作中显示文本右大括号。 下面的示例阐释了这种方法。  
  
 [!code-csharp[Formatting.Composite#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Composite/cs/Escaping1.cs#2)]
 [!code-vb[Formatting.Composite#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Composite/vb/Escaping1.vb#2)]  
  
### <a name="processing-order"></a>处理顺序  

 如果对复合格式设置方法的调用包括其值不为 `null`的<xref:System.IFormatProvider>参数，则运行时会调用其 <xref:System.IFormatProvider.GetFormat%2A?displayProperty=nameWithType> 方法来请求 <xref:System.ICustomFormatter> 实现。 如果此方法能够返回 <xref:System.ICustomFormatter> 实现，那么它将在复合格式方法调用期间缓存。
  
 如下所示，将参数列表中与格式项对应的每个值转换为字符串：  
  
1. 如果要设置格式的值为 `null`，则将返回空字符串 <xref:System.String.Empty?displayProperty=nameWithType>。  
  
2. 如果 <xref:System.ICustomFormatter> 实现可用，则运行时将调用其 <xref:System.ICustomFormatter.Format%2A> 方法。 它向方法传递格式项的 formatString 值（若有）或 `null`（若无）以及 <xref:System.IFormatProvider> 实现。 如果对 <xref:System.ICustomFormatter.Format%2A?displayProperty=nameWithType> 方法的调用返回 `null`，则继续执行下一步骤，将返回 <xref:System.ICustomFormatter.Format%2A?displayProperty=nameWithType> 调用的结果。
  
3. 如果该值实现 <xref:System.IFormattable> 接口，则调用此接口的 <xref:System.IFormattable.ToString%28System.String%2CSystem.IFormatProvider%29> 方法。 如果格式项中存在 formatString 值，则向方法传递该值；如果不存在该值，则传递 `null`。 按如下方式确定 <xref:System.IFormatProvider> 自变量：  
  
    - 对于数值，如果调用带非 null <xref:System.IFormatProvider> 自变量的复合格式设置方法，则运行时从其 <xref:System.Globalization.NumberFormatInfo> 方法请求 <xref:System.IFormatProvider.GetFormat%2A?displayProperty=nameWithType> 对象。 在以下情况下，使用当前线程区域性的 <xref:System.Globalization.NumberFormatInfo> 对象：无法提供该值、参数值为 `null` 或复合格式设置方法没有 <xref:System.IFormatProvider> 参数。  
  
    - 对于日期和时间值，如果调用带非 null <xref:System.IFormatProvider> 自变量的复合格式设置方法，则运行时从其 <xref:System.Globalization.DateTimeFormatInfo> 方法请求 <xref:System.IFormatProvider.GetFormat%2A?displayProperty=nameWithType> 对象。 在以下情况下，使用当前线程区域性的 <xref:System.Globalization.DateTimeFormatInfo> 对象：无法提供该值、参数值为 `null` 或复合格式设置方法没有 <xref:System.IFormatProvider> 参数。  
  
    - 对于其他类型的对象，如果调用带 <xref:System.IFormatProvider> 参数的复合格式设置方法，它的值会直接传递到 <xref:System.IFormattable.ToString%2A?displayProperty=nameWithType> 实现。 否则，`null` 传递到 <xref:System.IFormattable.ToString%2A?displayProperty=nameWithType> 实现。  
  
4. 调用类型的无参数的 `ToString` 方法（该方法将重写 <xref:System.Object.ToString?displayProperty=nameWithType> 或继承其基类的行为）。 在这种情况下，如果格式项中存在 formatString 组件指定的格式字符串，则将忽略该字符串。  
  
 前面的步骤执行完毕之后应用对齐。  
  
## <a name="code-examples"></a>代码示例  

 下面的示例显示使用复合格式设置创建的一个字符串和使用对象的 `ToString` 方法创建的另一个字符串。 两种格式设置类型产生相同的结果。  
  
 [!code-csharp[Formatting.Composite#3](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Composite/cs/Composite1.cs#3)]
 [!code-vb[Formatting.Composite#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Composite/vb/Composite1.vb#3)]  
  
 假定当前日期是五月的星期四，那么在美国英语区域性中上述示例中的两个字符串的值都是 `Thursday May`英语区域性。  
  
 <xref:System.Console.WriteLine%2A?displayProperty=nameWithType> 提供与 <xref:System.String.Format%2A?displayProperty=nameWithType> 相同的功能。 这两种方法的唯一差异是 <xref:System.String.Format%2A?displayProperty=nameWithType> 将其结果作为字符串返回，而 <xref:System.Console.WriteLine%2A?displayProperty=nameWithType> 将结果写入与 <xref:System.Console> 对象关联的输出流中。 下面的示例使用 <xref:System.Console.WriteLine%2A?displayProperty=nameWithType> 方法将 `MyInt` 的值的格式设置为货币值。  
  
 [!code-csharp[Formatting.Composite#4](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Composite/cs/Composite1.cs#4)]
 [!code-vb[Formatting.Composite#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Composite/vb/Composite1.vb#4)]  
  
 下面的示例说明为多个对象设置格式，包括用两种不同的方式为一个对象设置格式。  
  
 [!code-csharp[Formatting.Composite#5](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Composite/cs/Composite1.cs#5)]
 [!code-vb[Formatting.Composite#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Composite/vb/Composite1.vb#5)]  
  
 下面的示例演示了对齐在格式设置中的使用方式。 设置了格式的参数放置在竖线字符 (|) 之间以突出显示得到的对齐。  
  
 [!code-csharp[Formatting.Composite#6](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Composite/cs/Composite1.cs#6)]
 [!code-vb[Formatting.Composite#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Composite/vb/Composite1.vb#6)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.Console.WriteLine%2A>
- <xref:System.String.Format%2A?displayProperty=nameWithType>
- [字符串内插 (C#)](../../csharp/language-reference/tokens/interpolated.md)
- [字符串内插 (Visual Basic)](../../visual-basic/programming-guide/language-features/strings/interpolated-strings.md)
- [格式设置类型](formatting-types.md)
- [标准数字格式字符串](standard-numeric-format-strings.md)
- [自定义数字格式字符串](custom-numeric-format-strings.md)
- [标准日期和时间格式字符串](standard-date-and-time-format-strings.md)
- [自定义日期和时间格式字符串](custom-date-and-time-format-strings.md)
- [标准 TimeSpan 格式字符串](standard-timespan-format-strings.md)
- [自定义 TimeSpan 格式字符串](custom-timespan-format-strings.md)
- [枚举格式字符串](enumeration-format-strings.md)
