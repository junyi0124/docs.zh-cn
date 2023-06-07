---
title: 比较 .NET 中的字符串
description: 了解用于比较 .NET 中字符串的方法。 了解 Compare、CompareOrdinal、CompareTo、StartsWith、EndsWith、Equals、IndexOf 和 LastIndexOf 方法。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- value comparisons of strings
- LastIndexOf method
- CompareTo method
- IndexOf method
- Compare method
- strings [.NET], comparing
- CompareOrdinal method
- EndsWith method
- Equals method
- StartsWith method
ms.assetid: 977dc094-fe19-4955-98ec-d2294d04a4ba
ms.openlocfilehash: 08a92e314ad0900679d46cc759c80db89b43f0f0
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94823142"
---
# <a name="compare-strings-in-net"></a>比较 .NET 中的字符串

.NET 提供几种方法来比较字符串的值。 下表列出和描述值比较方法。

|方法名称|使用|
|-----------------|---------|
|<xref:System.String.Compare%2A?displayProperty=nameWithType>|比较两个字符串的值。 返回一个整数值。|
|<xref:System.String.CompareOrdinal%2A?displayProperty=nameWithType>|比较两个字符串的值而不考虑本地区域性。 返回一个整数值。|
|<xref:System.String.CompareTo%2A?displayProperty=nameWithType>|比较当前字符串对象和另一个字符串。 返回一个整数值。|
|<xref:System.String.StartsWith%2A?displayProperty=nameWithType>|确定字符串是否以传递字的符串开头。 返回一个布尔值。|
|<xref:System.String.EndsWith%2A?displayProperty=nameWithType>|确定字符串是否以传递的字符串结尾。 返回一个布尔值。|
|<xref:System.String.Contains%2A?displayProperty=nameWithType>|确定一个字符或字符串是否出现在另一个字符串中。 返回一个布尔值。|
|<xref:System.String.Equals%2A?displayProperty=nameWithType>|确定两个字符串是否相同。 返回一个布尔值。|
|<xref:System.String.IndexOf%2A?displayProperty=nameWithType>|返回字符或字符串的索引位置，从正在检查的字符串的开头开始。 返回一个整数值。|
|<xref:System.String.LastIndexOf%2A?displayProperty=nameWithType>|返回字符或字符串的索引位置，从正在检查的字符串的结尾开始。 返回一个整数值。|

## <a name="compare-method"></a>“比较”方法

静态 <xref:System.String.Compare%2A?displayProperty=nameWithType> 方法可以全面比较两个字符串。 此方法区分区域性。 你可以使用此函数比较两个字符串或两个字符串的子字符串。 此外，还提供考虑或忽略大小写和区域性差别的重载。 下表显示此方法可能返回三个整数值。

|返回值|条件|
|------------------|---------------|
|负整数|在排序顺序中，第一个字符串在第二个字符串之前。<br /><br /> \- 或 -<br /><br /> 第一个字符串是 `null`。|
|0|第一个字符串和第二个字符串相等。<br /><br /> \- 或 -<br /><br /> 两个字符串都是 `null`。|
|正整数<br /><br /> \- 或 -<br /><br /> 1|在排序顺序中，第一个字符串在第二个字符串之后。<br /><br /> \- 或 -<br /><br /> 第二个字符串是 `null`。|

> [!IMPORTANT]
> <xref:System.String.Compare%2A?displayProperty=nameWithType> 方法主要用于对字符串进行排序。 不应使用 <xref:System.String.Compare%2A?displayProperty=nameWithType> 方法来测试相等性（即，显式查找返回值 0 而不考虑一个字符串是否小于或大于另一个）。 相反，若要确定两个字符串是否相等，请使用 <xref:System.String.Equals%28System.String%2CSystem.String%2CSystem.StringComparison%29?displayProperty=nameWithType> 方法。

 下面的示例使用 <xref:System.String.Compare%2A?displayProperty=nameWithType> 方法来确定两个字符串的相对值。

 [!code-cpp[Conceptual.String.BasicOps#6](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/compare.cpp#6)]
 [!code-csharp[Conceptual.String.BasicOps#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/compare.cs#6)]
 [!code-vb[Conceptual.String.BasicOps#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/compare.vb#6)]

 此示例向控制台显示 `-1` 。

 前面的示例默认区分区域性。 若要执行不区分区域性的字符串比较，请使用 <xref:System.String.Compare%2A?displayProperty=nameWithType> 方法的重载，这样就可以通过提供 *区域性* 参数来指定要使用的区域性。 有关展示了如何使用 <xref:System.String.Compare%2A?displayProperty=nameWithType> 方法执行非区域性敏感型比较的示例，请参阅[非区域性敏感型字符串比较](../globalization-localization/performing-culture-insensitive-string-comparisons.md)。

## <a name="compareordinal-method"></a>CompareOrdinal 方法

<xref:System.String.CompareOrdinal%2A?displayProperty=nameWithType> 方法比较两个字符串对象而不考虑本地区域性。 此方法的返回值与上表中 `Compare` 方法返回的值相同。

> [!IMPORTANT]
> <xref:System.String.CompareOrdinal%2A?displayProperty=nameWithType> 方法主要用于对字符串进行排序。 不应使用 <xref:System.String.CompareOrdinal%2A?displayProperty=nameWithType> 方法来测试相等性（即，显式查找返回值 0 而不考虑一个字符串是否小于或大于另一个）。 相反，若要确定两个字符串是否相等，请使用 <xref:System.String.Equals%28System.String%2CSystem.String%2CSystem.StringComparison%29?displayProperty=nameWithType> 方法。

 下面的示例使用 `CompareOrdinal` 方法来比较两个字符串的值。

 [!code-cpp[Conceptual.String.BasicOps#7](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/compare.cpp#7)]
 [!code-csharp[Conceptual.String.BasicOps#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/compare.cs#7)]
 [!code-vb[Conceptual.String.BasicOps#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/compare.vb#7)]

 此示例向控制台显示 `-32` 。

## <a name="compareto-method"></a>CompareTo 方法

<xref:System.String.CompareTo%2A?displayProperty=nameWithType> 方法比较当前字符串对象封装到另一个字符串或对象的字符串。 此方法的返回值与上表中 <xref:System.String.Compare%2A?displayProperty=nameWithType> 方法返回的值相同。

> [!IMPORTANT]
> <xref:System.String.CompareTo%2A?displayProperty=nameWithType> 方法主要用于对字符串进行排序。 不应使用 <xref:System.String.CompareTo%2A?displayProperty=nameWithType> 方法来测试相等性（即，显式查找返回值 0 而不考虑一个字符串是否小于或大于另一个）。 相反，若要确定两个字符串是否相等，请使用 <xref:System.String.Equals%28System.String%2CSystem.String%2CSystem.StringComparison%29?displayProperty=nameWithType> 方法。

 下面的示例使用 <xref:System.String.CompareTo%2A?displayProperty=nameWithType> 方法来比较 `string1` 对象和 `string2` 对象。

 [!code-cpp[Conceptual.String.BasicOps#8](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/compare.cpp#8)]
 [!code-csharp[Conceptual.String.BasicOps#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/compare.cs#8)]
 [!code-vb[Conceptual.String.BasicOps#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/compare.vb#8)]

 此示例向控制台显示 `-1` 。

 <xref:System.String.CompareTo%2A?displayProperty=nameWithType> 方法的所有重载均默认执行区分区域性且区分大小写的比较。 此方法不提供任何允许执行不区分区域性的比较的重载。 为了代码的清楚起见，建议改为使用 `String.Compare` 方法，指定 <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> 执行区分区域性的操作或指定 <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType> 执行不区分区域性的操作。 有关演示如何使用 `String.Compare` 方法来执行区分和不区分区域性的比较的示例，请参阅 [执行不区分区域性的字符串比较](../globalization-localization/performing-culture-insensitive-string-comparisons.md)。

## <a name="equals-method"></a>Equals 方法

<xref:System.String.Equals%2A?displayProperty=nameWithType> 方法能够轻松确定两个字符串是否相等。 这个区分大小写的方法返回 `true` 或 `false` 布尔值。 它可以在现有类中使用，如下一个示例所示。 下面的示例使用 `Equals` 方法来确定一个字符串对象是否包含短语“Hello World”。

 [!code-cpp[Conceptual.String.BasicOps#9](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/compare.cpp#9)]
 [!code-csharp[Conceptual.String.BasicOps#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/compare.cs#9)]
 [!code-vb[Conceptual.String.BasicOps#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/compare.vb#9)]

 此示例向控制台显示 `True` 。

 此方法还可作为静态方法使用。 以下示例使用静态方法比较两个字符串对象。

 [!code-cpp[Conceptual.String.BasicOps#10](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/compare.cpp#10)]
 [!code-csharp[Conceptual.String.BasicOps#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/compare.cs#10)]
 [!code-vb[Conceptual.String.BasicOps#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/compare.vb#10)]

 此示例向控制台显示 `True` 。

## <a name="startswith-and-endswith-methods"></a>StartsWith 和 EndsWith 方法

可以使用 <xref:System.String.StartsWith%2A?displayProperty=nameWithType> 方法来确定一个字符串对象是否与另一个字符串以相同字符开头。 如果当前字符串对象以传递的字符串开头，这个区分大小写的方法将返回 `true`，否则返回 `false`。 以下示例使用此方法来确定一个字符串对象是否以“Hello”开头。

 [!code-cpp[Conceptual.String.BasicOps#11](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/compare.cpp#11)]
 [!code-csharp[Conceptual.String.BasicOps#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/compare.cs#11)]
 [!code-vb[Conceptual.String.BasicOps#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/compare.vb#11)]

 此示例向控制台显示 `True` 。

 <xref:System.String.EndsWith%2A?displayProperty=nameWithType> 方法比较传递的字符串和当前字符串对象末尾的字符。 它也返回一个布尔值。 下面的示例使用 `EndsWith` 方法检查字符串的末尾。

 [!code-cpp[Conceptual.String.BasicOps#12](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/compare.cpp#12)]
 [!code-csharp[Conceptual.String.BasicOps#12](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/compare.cs#12)]
 [!code-vb[Conceptual.String.BasicOps#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/compare.vb#12)]

 此示例向控制台显示 `False` 。

## <a name="indexof-and-lastindexof-methods"></a>IndexOf 和 LastIndexOf 方法

可以使用 <xref:System.String.IndexOf%2A?displayProperty=nameWithType> 方法来确定特定字符在字符串中的第一个匹配项的位置。 这个区分大小写的方法使用从零开始的索引从字符串的开头开始计数，并返回所传递字符的位置。 如果无法找到该字符，则返回值 –1。

下面的示例使用 `IndexOf` 方法搜索字符“`l`”在字符串中的第一个匹配项。

 [!code-cpp[Conceptual.String.BasicOps#13](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/compare.cpp#13)]
 [!code-csharp[Conceptual.String.BasicOps#13](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/compare.cs#13)]
 [!code-vb[Conceptual.String.BasicOps#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/compare.vb#13)]

 此示例向控制台显示 `2` 。

 <xref:System.String.LastIndexOf%2A?displayProperty=nameWithType> 方法类似于 `String.IndexOf` 方法，但它返回特定字符在字符串中的最后一个匹配项的位置。 它不区分大小写，并且使用从零开始的索引。

 下面的示例使用 `LastIndexOf` 方法搜索字符“`l`”在字符串中的最后一个匹配项。

 [!code-cpp[Conceptual.String.BasicOps#14](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/compare.cpp#14)]
 [!code-csharp[Conceptual.String.BasicOps#14](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/compare.cs#14)]
 [!code-vb[Conceptual.String.BasicOps#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/compare.vb#14)]

 此示例向控制台显示 `9` 。

 与 <xref:System.String.Remove%2A?displayProperty=nameWithType> 方法结合使用时，这两种方法都很有用。 可以使用 `IndexOf` 或 `LastIndexOf` 方法来检索字符的位置，然后将该位置提供给 `Remove` 方法，以删除字符或以该字符开头的单词。

## <a name="see-also"></a>请参阅

- [有关使用 .NET 中字符串的最佳做法](best-practices-strings.md)
- [基本字符串操作](basic-string-operations.md)
- [执行不区分区域性的字符串操作](../globalization-localization/performing-culture-insensitive-string-operations.md)
- [排序权重表](https://www.microsoft.com/download/details.aspx?id=10921) - 适用于 Windows 上的 .NET Framework 和 .NET Core 1.0-3.1
- [默认 Unicode 排序元素表](https://www.unicode.org/Public/UCA/latest/allkeys.txt) - 适用于所有平台上的 .NET 5，以及 Linux 和 macOS 上的 .NET Core
