---
title: 如何：显示日期和时间值中的毫秒
description: 本文介绍如何在 .NET 中格式化日期和时间字符串中包含日期和时间的毫秒部分。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DateTime.ToString method
- displaying date and time data
- time [.NET], milliseconds
- dates [.NET], milliseconds
- milliseconds [.NET]
ms.assetid: ae1a0610-90b9-4877-8eb6-4e30bc5e00cf
ms.openlocfilehash: 722334c324f663ba46a3c861885d4221fc566b8d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95681257"
---
# <a name="how-to-display-milliseconds-in-date-and-time-values"></a>如何：显示日期和时间值中的毫秒

默认日期和时间格式设置方法（如 <xref:System.DateTime.ToString?displayProperty=nameWithType>）包含时间值的小时、分钟和秒部分，但不包含毫秒部分。 本主题说明如何在格式化日期和时间字符串中包含日期和时间的毫秒部分。  
  
### <a name="to-display-the-millisecond-component-of-a-datetime-value"></a>显示的 DateTime 值的毫秒部分  
  
1. 如果要使用日期的字符串表示形式，请使用静态 <xref:System.DateTime> 或 <xref:System.DateTimeOffset> 方法将其转换为 <xref:System.DateTime.Parse%28System.String%29?displayProperty=nameWithType> 或 <xref:System.DateTimeOffset.Parse%28System.String%29?displayProperty=nameWithType> 值。  
  
2. 若要提取时间值的毫秒部分的字符串表示形式，请调用日期和时间值的 <xref:System.DateTime.ToString%28System.String%29?displayProperty=nameWithType> 或 <xref:System.DateTimeOffset.ToString%2A> 方法，并将 `fff` 或 `FFF` 自定义格式模式单独传递，或与其他自定义格式说明符一起作为 `format` 参数传递。  
  
## <a name="example"></a>示例  

 此示例展示了如何向控制台传递 <xref:System.DateTime> 和 <xref:System.DateTimeOffset> 值的毫秒部分（单独传递以及包含在更长的日期和时间字符串中传递）。  
  
 [!code-csharp[Formatting.HowTo.Millisecond#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.Millisecond/cs/Millisecond.cs#1)]
 [!code-vb[Formatting.HowTo.Millisecond#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.Millisecond/vb/Millisecond.vb#1)]  
  
 `fff` 格式模式包含毫秒值中的任何尾随零。 `FFF` 格式模式禁止显示它们。 下面的示例阐释了差异。  
  
 [!code-csharp[Formatting.HowTo.Millisecond#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.Millisecond/cs/Millisecond.cs#2)]
 [!code-vb[Formatting.HowTo.Millisecond#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.Millisecond/vb/Millisecond.vb#2)]  
  
 与定义包含日期和时间的毫秒部分的完整自定义格式说明符有关的一个问题在于，它定义可能与应用程序当前区域性中的时间元素排列不对应的硬编码格式。 更好的替换方法是，检索当前区域性的 <xref:System.Globalization.DateTimeFormatInfo> 对象定义的日期和时间显示模式之一，并将它修改为包含毫秒部分。 该示例也阐释了这种方法。 它会从 <xref:System.Globalization.DateTimeFormatInfo.FullDateTimePattern%2A?displayProperty=nameWithType> 属性检索当前区域性的完整日期和时间模式，再在秒模式后面插入自定义模式 `.ffff`。 请注意，该示例使用正则表达式在单个方法调用中执行此操作。  
  
 还可以使用自定义格式说明符来显示毫秒之外的秒的小数部分。 例如，`f` 或 `F` 自定义格式说明符显示十分之几秒，`ff` 或 `FF` 自定义格式说明符显示百分之几秒，`ffff` 或 `FFFF` 自定义格式说明符显示万分之几秒。 毫秒的小数部分在返回的字符串中会进行截断而不是舍入。 下面的示例中使用了这些格式说明符。  
  
 [!code-csharp[Formatting.HowTo.Millisecond#3](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.Millisecond/cs/Millisecond.cs#3)]
 [!code-vb[Formatting.HowTo.Millisecond#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.Millisecond/vb/Millisecond.vb#3)]  
  
> [!NOTE]
> 可以显示秒的非常小的小数单位，如万分之几秒或十万分之几秒。 但是，这些值可能没有意义。 日期和时间值的精度取决于系统时钟的分辨率。 在 Windows NT 3.5 和更高版本以及 Windows Vista 操作系统上，时钟的分辨率大约为 10-15 毫秒。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Globalization.DateTimeFormatInfo>
- [自定义日期和时间格式字符串](custom-date-and-time-format-strings.md)
