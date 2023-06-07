---
title: 如何：定义和使用自定义数值格式提供程序
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- numeric format strings [.NET]
- formatting [.NET], numbers
- number formatting [.NET]
- custom numeric format strings
- numbers [.NET], custom numeric format strings
- displaying date and time data
- format providers [.NET]
- custom format strings
ms.assetid: a281bfbf-6596-45ed-a2d6-3782d535ada2
ms.openlocfilehash: 9676e43582afb550152d2694f9548e9f1f0d326e
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94822966"
---
# <a name="how-to-define-and-use-custom-numeric-format-providers"></a>如何：定义和使用自定义数值格式提供程序

.NET 使你可以全面控制数值的字符串表示形式。 它支持用于自定义数值格式的以下功能：  
  
- 标准数字格式字符串，提供一组预定义格式以用于将数字转换为其字符串表示形式。 可以将它们与包含 `format` 参数的任何数字格式设置方法（如 <xref:System.Decimal.ToString%28System.String%29?displayProperty=nameWithType>）结合使用。 有关详细信息，请参阅[标准数字格式字符串](standard-numeric-format-strings.md)。  
  
- 自定义数字格式字符串，提供一组可以进行组合以定义自定义数字格式说明符的符号。 它们还可以与包含 `format` 参数的任何数字格式设置方法（如 <xref:System.Decimal.ToString%28System.String%29?displayProperty=nameWithType>）结合使用。 有关详细信息，请参阅[自定义数字格式字符串](custom-numeric-format-strings.md)。  
  
- 自定义 <xref:System.Globalization.CultureInfo> 或 <xref:System.Globalization.NumberFormatInfo> 对象，定义用于显示数值的字符串表示形式的符号和格式模式。 可以将它们与包含 `provider` 参数的任何数字格式设置方法（如 <xref:System.Int32.ToString%2A>）结合使用。 `provider` 参数通常用于指定区域性专用格式设置。  
  
 在某些情况下（例如当应用程序必须显示格式化帐号、标识号或邮政编码），这三种方法都不合适。 借助 .NET，你还可以定义既不是 <xref:System.Globalization.CultureInfo> 也不是 <xref:System.Globalization.NumberFormatInfo> 对象的格式设置对象，用于确定如何设置数值的格式。 本主题提供用于实现这类对象的分步说明，并提供对电话号码设置格式的示例。  
  
## <a name="define-a-custom-format-provider"></a>定义自定义格式提供程序  
  
1. 定义实现 <xref:System.IFormatProvider> 和 <xref:System.ICustomFormatter> 接口的类。  
  
2. 实现 <xref:System.IFormatProvider.GetFormat%2A?displayProperty=nameWithType> 方法。 <xref:System.IFormatProvider.GetFormat%2A> 是格式设置方法（如 <xref:System.String.Format%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> 方法）调用的回调方法，用于检索实际负责执行自定义格式设置的对象。 <xref:System.IFormatProvider.GetFormat%2A> 的典型实现执行以下操作：  
  
    1. 确定以方法参数形式传递的 <xref:System.Type> 对象是否表示 <xref:System.ICustomFormatter> 接口。  
  
    2. 如果此参数确实表示 <xref:System.ICustomFormatter> 接口，<xref:System.IFormatProvider.GetFormat%2A> 会返回对象，用于实现负责执行自定义格式设置的 <xref:System.ICustomFormatter> 接口。 通常，自定义格式设置对象返回其自身。  
  
    3. 如果参数不表示 <xref:System.ICustomFormatter> 接口，<xref:System.IFormatProvider.GetFormat%2A> 返回的是 `null`。  
  
3. 实现 <xref:System.ICustomFormatter.Format%2A> 方法。 此方法由 <xref:System.String.Format%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> 方法调用，负责返回数字的字符串表示形式。 实现方法通常涉及以下步骤：  
  
    1. （可选）通过检查 `provider` 参数，确保此方法旨在以合法方式提供格式设置服务。 对于实现 <xref:System.IFormatProvider> 和 <xref:System.ICustomFormatter> 的格式设置对象，这涉及测试 `provider` 参数是否与当前格式设置对象相等。  
  
    2. 确定格式设置对象是否应支持自定义格式说明符。 （例如，格式说明符“N”可能指示应以 NANP 格式输出美国电话号码，而“I”可能指示以 ITU-T 建议 E.123 格式进行输出。）如果使用格式说明符，则方法应处理特定格式说明符。 它会在 `format` 参数中传递给方法。 如果没有说明符，`format` 参数的值是 <xref:System.String.Empty?displayProperty=nameWithType>。  
  
    3. 检索作为 `arg` 参数传递给方法的数值。 执行将它转换为其字符串表示形式所需的任何操作。  
  
    4. 返回 `arg` 参数的字符串表示形式。  
  
## <a name="use-a-custom-numeric-formatting-object"></a>使用自定义数字格式设置对象  
  
1. 创建自定义格式设置类的新实例。  
  
2. 调用 <xref:System.String.Format%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> 格式设置方法，同时向它传递自定义格式设置对象、格式设置说明符（或 <xref:System.String.Empty?displayProperty=nameWithType>，如果未使用说明符的话），以及要设置格式的数值。  
  
## <a name="example"></a>示例

 下面的示例定义了一个名为 `TelephoneFormatter` 的自定义数值格式提供程序，该提供程序将代表美国电话号码的数字转化为它的 NANP 或 E.123 格式。 该方法处理两个格式说明符“N”（输出 NANP 格式）和“I”（输出国际 E.123 格式）。  
  
 [!code-csharp[Formatting.HowTo.NumericValue#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.NumericValue/cs/Telephone1.cs#1)]
 [!code-vb[Formatting.HowTo.NumericValue#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.NumericValue/vb/Telephone1.vb#1)]  
  
 自定义数字格式提供程序只能与 <xref:System.String.Format%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> 方法配合使用。 包含 <xref:System.IFormatProvider> 类型参数的数字格式设置方法（如 `ToString`）的其他重载，都会向 <xref:System.IFormatProvider.GetFormat%2A?displayProperty=nameWithType> 实现传递表示 <xref:System.Globalization.NumberFormatInfo> 类型的 <xref:System.Type> 对象。 此方法应返回 <xref:System.Globalization.NumberFormatInfo> 对象。 如果未返回，将会忽略自定义数字格式提供程序，而改用当前区域性的 <xref:System.Globalization.NumberFormatInfo> 对象。 在此示例中，`TelephoneFormatter.GetFormat` 方法检查方法参数，并在它表示除 <xref:System.ICustomFormatter> 之外的类型时返回 `null`，从而处理它可能会被不恰当地传递给数字格式设置方法的情况。  
  
 如果自定义数字格式提供程序支持一组格式说明符，请确保提供在 <xref:System.String.Format%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> 方法调用中使用的格式项没有格式说明符时的默认行为。 在示例中，“N”是默认格式说明符。 这使数字可以通过提供显式格式说明符来转换为格式化电话号码。 下面的示例演示了此类方法调用。  
  
 [!code-csharp[Formatting.HowTo.NumericValue#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.NumericValue/cs/Telephone1.cs#2)]
 [!code-vb[Formatting.HowTo.NumericValue#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.NumericValue/vb/Telephone1.vb#2)]  
  
 但是它还允许在不存在格式说明符时进行转换。 下面的示例演示了此类方法调用。  
  
 [!code-csharp[Formatting.HowTo.NumericValue#3](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.NumericValue/cs/Telephone1.cs#3)]
 [!code-vb[Formatting.HowTo.NumericValue#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.NumericValue/vb/Telephone1.vb#3)]  
  
 如果未定义默认格式说明符，<xref:System.ICustomFormatter.Format%2A?displayProperty=nameWithType> 方法实现应包含如下代码，以便 .NET 能够提供代码不支持的格式设置。  
  
 [!code-csharp[System.ICustomFormatter.Format#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.ICustomFormatter.Format/cs/format.cs#1)]
 [!code-vb[System.ICustomFormatter.Format#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.ICustomFormatter.Format/vb/Format.vb#1)]  
  
 在此示例中，实现 <xref:System.ICustomFormatter.Format%2A?displayProperty=nameWithType> 的方法旨在用作 <xref:System.String.Format%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> 方法的回调方法。 因此，它会检查 `formatProvider` 参数，以确定它是否包含对当前 `TelephoneFormatter` 对象的引用。 但是，也可以直接从代码调用该方法。 在这种情况下，可以使用 `formatProvider` 参数，提供用于提供区域性专用格式设置信息的 <xref:System.Globalization.CultureInfo> 或 <xref:System.Globalization.NumberFormatInfo> 对象。
