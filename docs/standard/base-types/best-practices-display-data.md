---
title: 在 .NET 中显示和保存格式化数据的最佳做法
description: 了解如何在 .NET 应用程序中有效地显示和保存数字和日期数据。
ms.date: 05/01/2019
dev_langs:
- csharp
- vb
ms.openlocfilehash: 1748363089a80538a19e91b1955fe9257de39a4e
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94825138"
---
# <a name="best-practices-for-displaying-and-persisting-formatted-data"></a>显示和保存格式化数据的最佳做法

本文讨论如何处理数据格式（如数字数据以及日期和时间数据）以用于显示和存储。

当你使用 .NET 进行开发时，在用户界面中，使用区分区域性的格式显示非字符串数据，如数字和日期。 使用格式以[固定区域性](xref:System.Globalization.CultureInfo.InvariantCulture)使非字符串数据显示为字符串形式。 不要使用区分区域性格式以字符串形式来保存数值或日期和时间数据。

## <a name="displaying-formatted-data"></a>显示格式化数据

当给用户显示非字符串数据（如数字、日期和时间）时，使用用户的区域性设置来格式化他们。 默认情况下，以下所有内容都在格式设置操作中使用当前线程区域性：

- [C#](../../csharp/language-reference/tokens/interpolated.md) 和 [Visual Basic](../../visual-basic/programming-guide/language-features/strings/interpolated-strings.md) 编译器支持的内插字符串。
- 字符串串联操作，它使用 [C#](../../csharp/language-reference/operators/addition-operator.md#string-concatenation) 或 [Visual Basic](../../visual-basic/programming-guide/language-features/operators-and-expressions/concatenation-operators.md) 串联运算符或直接调用 <xref:System.String.Concat%2A?displayProperty=nameWithType> 方法。
- <xref:System.String.Format%2A?displayProperty=nameWithType> 方法。
- 数值类型的 `ToString` 方法以及日期和时间类型。

若要显式指定应使用指定区域性约定或[固定区域性](xref:System.Globalization.CultureInfo.InvariantCulture)设置字符串的格式，可以执行以下操作：

- 当使用 <xref:System.String.Format%2A?displayProperty=nameWithType> 和 `ToString` 方法时，调用具有 `provider` 参数（如 <xref:System.String.Format%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> 或 <xref:System.DateTime.ToString%28System.IFormatProvider%29?displayProperty=nameWithType>）的重载，并将 <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> 属性（表示所需区域性的 <xref:System.Globalization.CultureInfo> 实例）或 <xref:System.Globalization.CultureInfo.InvariantCulture?displayProperty=nameWithType> 属性传递给它。

- 对于字符串串联，不允许编译器执行任何隐式转换。 可通过调用具有 `provider` 参数的 `ToString` 重载来执行显式转换。 例如，在将 <xref:System.Double> 值转换为以下代码中的字符串时，编译器隐式使用当前区域性：

  [!code-csharp[Implicit String Conversion](./snippets/best-practices-strings/csharp/tostring/Program.cs#1)]
  [!code-vb[Implicit String Conversion](./snippets/best-practices-strings/vb/tostring/Program.vb#1)]

  可以通过调用 <xref:System.Double.ToString(System.IFormatProvider)?displayProperty=nameWithType> 方法显式指定在转换中使用格式约定的区域性，如下面的代码所示：

  [!code-csharp[Explicit String Conversion](./snippets/best-practices-strings/csharp/tostring/Program.cs#2)]
  [!code-vb[Implicit String Conversion](./snippets/best-practices-strings/vb/tostring/Program.vb#2)]

- 对于字符串内插，不是将内插字符串分配给 <xref:System.String> 实例，而是将其分配给 <xref:System.FormattableString>。 然后，可以调用其 <xref:System.FormattableString.ToString?displayProperty=nameWithType> 方法生成反映当前区域性约定的结果字符串，也可以调用 <xref:System.FormattableString.ToString(System.IFormatProvider)?displayProperty=nameWithType> 方法生成反映指定区域性约定的结果字符串。 还可以将可格式化字符串传递给静态 <xref:System.FormattableString.Invariant%2A?displayProperty=nameWithType> 方法，以生成反映固定区域性约定的结果字符串。 下面的示例阐释了这种方法。 （该示例的输出反映了当前的 zh-CN 区域性。）

  [!code-csharp[String interpolation](./snippets/best-practices-strings/csharp/formattable/Program.cs)]
  [!code-vb[String interpolation](./snippets/best-practices-strings/vb/formattable/Program.vb)]

## <a name="persisting-formatted-data"></a>保存格式化数据

您可以保留非字符串数据作为二进制数据或作为格式化数据。 如果您选择将其保存为格式化数据，您应调用包括 `provider` 参数的格式设置方法重载，并向其传递 <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType> 属性。 固定区域性为独立于区域性和计算机的格式化数据提供一致的格式。 相反，使用区域性而非固定区域性进行格式化的持久性数据具有许多限制：

- 如果在具有不同区域性的系统上检索数据，或者如果当前系统用户更改当前区域性或者尝试检索数据时，该数据可能不可用。
- 特定计算机上的区域性属性可能与标准值不同。 任何时候，用户都可以自定义区分区域性的显示设置。 因此，在系统保存的格式化数据在用户自定义区域性设置之后可能无法读取。 格式化数据在计算机之间移植可能会受到更多的限制。
- 管理数值或日期时间格式的国际、区域或国家标准会随着时间发生更改，这些更改会合并到 Windows 操作系统更新中。 在格式设置约定更改时，将无法读取使用以前的约定格式化的数据。

下面的示例演示了使用区分区域性格式设置进行持久化数据导致的有限可移植性。 该示例将日期和时间数组值保存到文件中。 这些数据通过使用英语（美国）区域性约定进行格式化。 在应用程序将当前线程区域性更改为法语（瑞士）后，它尝试使用当前区域性的格式设置约定来读取保存的值。 尝试读取两个数据条目时引发 <xref:System.FormatException> 异常，现在日期数组包含相当于 <xref:System.DateTime.MinValue>的两个错误元素。

[!code-csharp[Conceptual.Strings.BestPractices#21](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/persistence.cs#21)]
[!code-vb[Conceptual.Strings.BestPractices#21](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/persistence.vb#21)]

然而，如果你在对 <xref:System.DateTime.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType> 和 <xref:System.DateTime.Parse%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType> 的调用中将 <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> 属性替换为 <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType>，则会成功还原持久的日期和时间数据，如以下输出所示：

```console
06.05.1758 21:26
05.05.1818 07:19
22.04.1870 23:54
08.09.1890 06:47
18.02.1905 15:12
```
