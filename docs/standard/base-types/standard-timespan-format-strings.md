---
title: 标准 TimeSpan 格式字符串
description: 查看标准 TimeSpan 格式字符串，此类字符串使用单个格式说明符来定义 .NET 中 TimeSpan 值的文本表示形式。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- format specifiers, standard time interval
- format strings
- standard time interval format strings
- standard format strings, time intervals
- format specifiers, time intervals
- time intervals [.NET], formatting
- time [.NET], formatting
- formatting [.NET], time
- standard TimeSpan format strings
- formatting [.NET], time intervals
ms.assetid: 9f6c95eb-63ae-4dcc-9c32-f81985c75794
ms.openlocfilehash: 251f90e85d037d8cf4f3fd58bc27659c98d04b5e
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734239"
---
# <a name="standard-timespan-format-strings"></a>标准 TimeSpan 格式字符串

标准 <xref:System.TimeSpan> 格式字符串使用一个格式说明符，定义格式设置操作生成的 <xref:System.TimeSpan> 值的文本表示形式。 任何包含一个以上字符（包括空格）的格式字符串都被解释为自定义 <xref:System.TimeSpan> 格式字符串。 有关详细信息，请参阅[自定义 TimeSpan 格式字符串](custom-timespan-format-strings.md)。  
  
 通过调用 <xref:System.TimeSpan> 方法的重载以及通过支持复合格式设置的方法（如 <xref:System.TimeSpan.ToString%2A?displayProperty=nameWithType>）产生 <xref:System.String.Format%2A?displayProperty=nameWithType> 值的字符串表示形式。 有关更多信息，请参见[格式设置类型](formatting-types.md)和[复合格式设置](composite-formatting.md)。 以下示例演示了标准格式字符串在格式设置操作中的用法。  
  
 [!code-csharp[Conceptual.TimeSpan.Standard#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.standard/cs/formatexample1.cs#2)]
 [!code-vb[Conceptual.TimeSpan.Standard#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.standard/vb/formatexample1.vb#2)]  
  
 标准 <xref:System.TimeSpan> 格式字符串也被 <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> 和 <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType> 方法用于定义分析操作所需的输入字符串的格式。 （分析将值的字符串表示形式转换成该值。）以下示例演示了标准格式字符串在分析操作中的用法。  
  
 [!code-csharp[Conceptual.TimeSpan.Standard#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.standard/cs/parseexample1.cs#3)]
 [!code-vb[Conceptual.TimeSpan.Standard#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.standard/vb/parseexample1.vb#3)]  
  
下表列出了标准时间间隔格式说明符。  
  
|格式说明符|“属性”|描述|示例|  
|----------------------|----------|-----------------|--------------|  
|“c”|常量（固定）格式|此说明符不区分区域性。 它的形式是 `[-][d'.']hh':'mm':'ss['.'fffffff]`。<br /><br /> （“t”格式与“T”格式字符串产生的结果相同。）<br /><br /> 更多信息：[常量（“c”）格式说明符](#the-constant-c-format-specifier)。|`TimeSpan.Zero` -> 00:00:00<br /><br /> `New TimeSpan(0, 0, 30, 0)` -> 00:30:00<br /><br /> `New TimeSpan(3, 17, 25, 30, 500)` -> 3.17:25:30.5000000|  
|“g”|常规短格式|该说明符仅输出需要的内容。 它区分区域性并采用 `[-][d':']h':'mm':'ss[.FFFFFFF]` 形式。<br /><br /> 更多信息：[常规短（“g”）格式说明符](#the-general-short-g-format-specifier)。|`New TimeSpan(1, 3, 16, 50, 500)` -> 1:3:16:50.5 (en-US)<br /><br /> `New TimeSpan(1, 3, 16, 50, 500)` -> 1:3:16:50,5 (fr-FR)<br /><br /> `New TimeSpan(1, 3, 16, 50, 599)` -> 1:3:16:50.599 (en-US)<br /><br /> `New TimeSpan(1, 3, 16, 50, 599)` -> 1:3:16:50,599 (fr-FR)|  
|“G”|常规长格式|此说明符始终输出天数和七个小数位。 它区分区域性并采用 `[-]d':'hh':'mm':'ss.fffffff` 形式。<br /><br /> 更多信息：[常规长（“G”）格式说明符](#the-general-long-g-format-specifier)。|`New TimeSpan(18, 30, 0)` -> 0:18:30:00.0000000 (en-US)<br /><br /> `New TimeSpan(18, 30, 0)` -> 0:18:30:00,0000000 (fr-FR)|  

## <a name="the-constant-c-format-specifier"></a>常量（“c”）格式说明符。  

 “c”格式说明符返回的 <xref:System.TimeSpan> 值的字符串表示形式具有以下形式：  
  
 [-][*d*.]*hh*:*mm*:*ss*[.*fffffff*]  
  
 方括号 ([ and ]) 中的元素是可选的。 句点 (.) 和冒号 (:) 是文字符号。 下表介绍了剩余的元素。  
  
|元素|描述|  
|-------------|-----------------|  
|*-*|可选负号，指示负时间间隔。|  
|*d*|不带前导零的可选天数。|  
|*hh*|小时数，范围为“00”到“23”。|  
|*mm*|分钟数，范围为“00”到“59”。|  
|*ss*|秒数，范围为“0”到“59”。|  
|*fffffff*|秒的可选小数部分。  其值的范围为“0000001”（一刻度或一秒的一千万分之一）到“9999999”（一秒的一千万分之九百九十九万九千九百九或一秒少一刻度）。|  
  
 与“g”和“G”格式说明符不同，“c”格式说明符不区分区域性。 它产生了 <xref:System.TimeSpan> 值的字符串表示形式，该值不变且在 .NET Framework 4 之前的版本中通用。 “c”是默认的 <xref:System.TimeSpan> 格式字符串；<xref:System.TimeSpan.ToString?displayProperty=nameWithType> 方法使用“c”格式字符串设置时间间隔值的格式。  
  
> [!NOTE]
> <xref:System.TimeSpan> 也支持“t”和“T”标准格式字符串，其行为与“c”标准格式字符串相同。  
  
 以下示例对两个 <xref:System.TimeSpan> 对象进行了实例化，使用它们来执行算术运算并显示结果。 在每种情况下，它都通过“c”格式说明符使用复合格式设置来显示 <xref:System.TimeSpan> 值。  
  
 [!code-csharp[Conceptual.TimeSpan.Standard#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.standard/cs/standardc1.cs#1)]
 [!code-vb[Conceptual.TimeSpan.Standard#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.standard/vb/standardc1.vb#1)]  

## <a name="the-general-short-g-format-specifier"></a>常规短（“g”）格式说明符  

 “g”<xref:System.TimeSpan> 格式说明符通过只包含所需元素来返回简洁形式的 <xref:System.TimeSpan> 值的字符串表示形式。 它具有以下形式：  
  
 [-][*d*:]*h*:*mm*:*ss*[.*FFFFFFF*]  
  
 方括号 ([ and ]) 中的元素是可选的。 冒号 (:) 是一种文字符号。 下表介绍了剩余的元素。  
  
|元素|描述|  
|-------------|-----------------|  
|*-*|可选负号，指示负时间间隔。|  
|*d*|不带前导零的可选天数。|  
|*h*|范围为“0”到“23”的小时数，无前导零。|  
|*mm*|分钟数，范围为“00”到“59”。|  
|*ss*|秒数，范围为“00”到“59”。|  
|*.*|秒的小数部分的分隔符。 它相当于指定区域中无需用户重写的 <xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A> 属性。|  
|*FFFFFFF*|秒的小数部分。 显示尽可能少的数位。|  
  
 和“G”格式一样，对“g”格式说明符进行了本地化。 其秒的小数部分的分隔符基于当前区域或指定区域的 <xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A> 属性。  
  
 以下示例对两个 <xref:System.TimeSpan> 对象进行了实例化，使用它们来执行算术运算并显示结果。 在每种情况下，它都通过“g”格式说明符使用复合格式设置来显示 <xref:System.TimeSpan> 值。 此外，它通过使用当前系统区域（在此情况下为“英语 - 美国”或“en-US”）和“法语 - 法国 (fr-FR)”区域的格式设置约定来设置 <xref:System.TimeSpan> 值的格式。  
  
 [!code-csharp[Conceptual.TimeSpan.Standard#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.standard/cs/standardshort1.cs#4)]
 [!code-vb[Conceptual.TimeSpan.Standard#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.standard/vb/standardshort1.vb#4)]  

## <a name="the-general-long-g-format-specifier"></a>常规长（“G”）格式说明符  

 （“G”）<xref:System.TimeSpan> 格式说明符用始终包含日期和秒的小数部分的长格式返回 <xref:System.TimeSpan> 值的字符串表示形式。 “G”标准格式说明符生成的字符串具有以下形式：  
  
 [-]*d*:*hh*:*mm*:*ss*.*fffffff*  
  
 方括号 ([ and ]) 中的元素是可选的。 冒号 (:) 是一种文字符号。 下表介绍了剩余的元素。  
  
|元素|描述|  
|-------------|-----------------|  
|*-*|可选负号，指示负时间间隔。|  
|*d*|不带前导零的天数。|  
|*hh*|小时数，范围为“00”到“23”。|  
|*mm*|分钟数，范围为“00”到“59”。|  
|*ss*|秒数，范围为“00”到“59”。|  
|*.*|秒的小数部分的分隔符。 它相当于指定区域中无需用户重写的 <xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A> 属性。|  
|*fffffff*|秒的小数部分。|  
  
 和“G”格式一样，对“g”格式说明符进行了本地化。 其秒的小数部分的分隔符基于当前区域或指定区域的 <xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A> 属性。  
  
 以下示例对两个 <xref:System.TimeSpan> 对象进行了实例化，使用它们来执行算术运算并显示结果。 在每种情况下，它都通过“G”格式说明符使用复合格式设置来显示 <xref:System.TimeSpan> 值。 此外，它通过使用当前系统区域（在此情况下为“英语 - 美国”或“en-US”）和“法语 - 法国 (fr-FR)”区域的格式设置约定来设置 <xref:System.TimeSpan> 值的格式。  
  
 [!code-csharp[Conceptual.TimeSpan.Standard#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.standard/cs/standardlong1.cs#5)]
 [!code-vb[Conceptual.TimeSpan.Standard#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.standard/vb/standardlong1.vb#5)]
  
## <a name="see-also"></a>请参阅

- [格式设置类型](formatting-types.md)
- [自定义 TimeSpan 格式字符串](custom-timespan-format-strings.md)
- [分析字符串](parsing-strings.md)
