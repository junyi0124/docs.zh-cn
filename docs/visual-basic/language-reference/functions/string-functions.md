---
title: 字符串函数
ms.date: 07/20/2015
helpviewer_keywords:
- string functions
ms.assetid: f1bf9ac2-cbcf-4298-ae51-53182076bdc8
ms.openlocfilehash: 778e57eadd75baf1aabd100f9d8d41a490f79a04
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84406283"
---
# <a name="string-functions-visual-basic"></a>字符串函数 (Visual Basic)

下表列出了 Visual Basic 在类中提供的 <xref:Microsoft.VisualBasic.Strings?displayProperty=nameWithType> 用于搜索和操作字符串的函数。 它们可以视为 Visual Basic 内部函数;也就是说，您不必将它们作为类的显式成员进行调用，如示例所示。 类中提供了其他方法，在某些情况下为互补方法 <xref:System.String?displayProperty=nameWithType> 。

|.NET Framework 方法|说明|
|---------------------------|-----------------|
|<xref:Microsoft.VisualBasic.Strings.Asc%2A>, <xref:Microsoft.VisualBasic.Strings.AscW%2A>|返回一个 `Integer` 值，该值表示与某个字符相对应的字符代码。|
|<xref:Microsoft.VisualBasic.Strings.Chr%2A>, <xref:Microsoft.VisualBasic.Strings.ChrW%2A>|返回与指定字符代码相关联的字符。|
|<xref:Microsoft.VisualBasic.Strings.Filter%2A>|返回一个从零开始的数组，该数组包含基于指定筛选条件的 `String` 数组的子集。|
|<xref:Microsoft.VisualBasic.Strings.Format%2A>|返回根据格式 `String` 表达式中包含的指令设置格式的字符串。|
|<xref:Microsoft.VisualBasic.Strings.FormatCurrency%2A>|返回一个格式为货币值的表达式，该货币值使用系统控制面板中定义的货币符号。|
|<xref:Microsoft.VisualBasic.Strings.FormatDateTime%2A>|返回一个表示日期/时间值的字符串表达式。|
|<xref:Microsoft.VisualBasic.Strings.FormatNumber%2A>|返回格式化为数字的表达式。|
|<xref:Microsoft.VisualBasic.Strings.FormatPercent%2A>|返回以 % 字符结尾的百分比格式的表达式（即乘以 100）。|
|<xref:Microsoft.VisualBasic.Strings.InStr%2A>|返回一个整数，该整数指定一个字符串在另一个字符串中的第一个匹配项的起始位置。|
|<xref:Microsoft.VisualBasic.Strings.InStrRev%2A>|返回某一字符串从另一字符串的右侧开始算起第一次出现的位置。|
|<xref:Microsoft.VisualBasic.Strings.Join%2A>|返回通过连接一个数组中包含的若干子字符串创建的字符串。|
|<xref:Microsoft.VisualBasic.Strings.LCase%2A>|返回将转换为小写的字符串或字符。|
|<xref:Microsoft.VisualBasic.Strings.Left%2A>|返回一个字符串，该字符串包含从某字符串左侧算起的指定数量的字符。|
|<xref:Microsoft.VisualBasic.Strings.Len%2A>|返回一个整数，该整数包含字符串中的字符数。|
|<xref:Microsoft.VisualBasic.Strings.LSet%2A>|返回一个左对齐字符串，该字符串包含调整为指定长度的指定的字符串。|
|<xref:Microsoft.VisualBasic.Strings.LTrim%2A>|返回一个字符串，该字符串包含不带前导空格的指定字符串的副本。|
|<xref:Microsoft.VisualBasic.Strings.Mid%2A>|返回一个字符串，该字符串包含字符串中指定数目的字符。|
|<xref:Microsoft.VisualBasic.Strings.Replace%2A>|返回一个字符串，其中的指定子字符串已由另一个子字符串替换了指定的次数。|
|<xref:Microsoft.VisualBasic.Strings.Right%2A>|返回一个字符串，其中包含从某个字符串右端开始的指定数量的字符。|
|<xref:Microsoft.VisualBasic.Strings.RSet%2A>|返回包含调整为指定长度的指定字符串的右对齐字符串。|
|<xref:Microsoft.VisualBasic.Strings.RTrim%2A>|返回一个字符串，该字符串包含不带尾随空格的指定字符串的副本。|
|<xref:Microsoft.VisualBasic.Strings.Space%2A>|返回由指定数量空格组成的字符串。|
|<xref:Microsoft.VisualBasic.Strings.Split%2A>|返回一个从零开始的一维数组，其中包含指定数量的子字符串。|
|<xref:Microsoft.VisualBasic.Strings.StrComp%2A>|根据字符串的比较结果返回 -1、0 或 1。|
|<xref:Microsoft.VisualBasic.Strings.StrConv%2A>|返回按照指定方式转换的字符串。|
|<xref:Microsoft.VisualBasic.Strings.StrDup%2A>|返回由指定字符重复指定次数后形成的字符串或对象。|
|<xref:Microsoft.VisualBasic.Strings.StrReverse%2A>|返回指定字符串的字符顺序是相反的字符串。|
|<xref:Microsoft.VisualBasic.Strings.Trim%2A>|返回一个字符串，该字符串包含不带前导空格或尾随空格的指定字符串的副本。|
|<xref:Microsoft.VisualBasic.Strings.UCase%2A>|返回一个字符串或字符，其中包含转换为大写的指定字符串。|

您可以使用[Option Compare](../statements/option-compare-statement.md)语句来设置是使用不区分大小写的文本排序顺序（由系统的区域设置（ `Text` ）决定），还是通过字符的内部二进制表示形式来比较字符串（ `Binary` ）。 默认的文本比较方法是 `Binary`。

## <a name="example-ucase"></a>示例： UCase

本例使用 `UCase` 函数返回字符串的大写版本。
[!code-vb[VbVbalrStrings#31](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class1.vb#31)]

## <a name="example-ltrim"></a>示例： LTrim

此示例使用 `LTrim` 函数去除字符串变量的前导空格，使用 `RTrim` 函数去除尾随空格， 并使用 `Trim` 函数同时去除这两种类型的空格。

[!code-vb[VbVbalrStrings#25](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class1.vb#25)]

## <a name="example-mid"></a>示例： Mid

此示例使用 `Mid` 函数从字符串返回指定数目的字符。

[!code-vb[VbVbalrStrings#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class1.vb#17)]

## <a name="example-len"></a>示例： Len

本例使用 `Len` 返回字符串中的字符数。

[!code-vb[VbVbalrStrings#33](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class1.vb#33)]

## <a name="example-instr"></a>示例： InStr

本例使用 `InStr` 函数返回一个字符串在另一个字符串中的第一个匹配项的位置。

[!code-vb[VbVbalrStrings#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class1.vb#8)]

## <a name="example-format"></a>示例：格式

此示例演示同时使用 `Format` 格式和用户定义格式格式化值的 `String` 函数的各种用法。 对于日期分隔符 (`/`)、时间分隔符 (`:`) 和 AM/PM 指示符（`t` 和 `tt`），系统显示的实际格式化输出取决于代码使用的区域设置。 当在开发环境中显示时间和日期时，使用代码区域设置的短时间格式和短日期格式。

> [!NOTE]
> 对于使用 24 小时制的区域设置，AM/PM 指示符（`t` 和 `tt`）不显示任何内容。

[!code-vb[VbVbalrStrings#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class1.vb#27)]

## <a name="see-also"></a>另请参阅

- [关键字](../keywords/index.md)
- [Visual Basic 运行库成员](../runtime-library-members.md)
- [字符串操作摘要](../keywords/string-manipulation-summary.md)
- [System.string 类方法](xref:System.String#methods)
