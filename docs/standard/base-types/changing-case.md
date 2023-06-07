---
title: 更改 .NET 中的大小写
description: 了解如何更新 .NET 中字符串的大小写。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- strings [.NET], case
- case sensitivity
- ToUpper method
- ToLower method
- uppercase
- lowercase
ms.assetid: 6805f81b-e9ad-4387-9f4c-b9bdb21b87c0
ms.openlocfilehash: 157ad1e2cc626c508afbcd1b7822e7c64fdf30c2
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95686028"
---
# <a name="change-case-in-net"></a>更改 .NET 中的大小写

如果你编写可接受用户输入的应用程序，则永远无法确定用户以大写还是小写来输入数据。 通常，你希望字符串统一采用大写或小写，尤其是在用户界面显示时。 下表介绍 3 种更改大小写的方法： 前两个方法提供可接受区域性的重载。  
  
|方法名称|使用|  
|-----------------|---------|  
|<xref:System.String.ToUpper%2A?displayProperty=nameWithType>|将字符串中的所有字符均转换为大写。|  
|<xref:System.String.ToLower%2A?displayProperty=nameWithType>|将字符串中的所有字符均转换为小写。|  
|<xref:System.Globalization.TextInfo.ToTitleCase%2A?displayProperty=nameWithType>|将字符串转换为首字母大写。|  
  
> [!WARNING]
> 请注意，为了对 <xref:System.String.ToUpper%2A?displayProperty=nameWithType> 和 <xref:System.String.ToLower%2A?displayProperty=nameWithType> 方法进行比较或测试它们是否相等，这两种方法不应用于转换字符串。 有关详细信息，请参阅[比较混合大小写的字符串](#Comparing)部分。  
  
<a name="Comparing"></a>

## <a name="compare-strings-of-mixed-case"></a>比较混合大小写的字符串  

 若要比较混合大小写的字符串以确定它们的顺序，请调用具有 <xref:System.String.CompareTo%2A?displayProperty=nameWithType> 方法中具有 `comparisonType` 参数的其中一个重载，并向 `comparisonType` 自变量提供 <xref:System.StringComparison.CurrentCultureIgnoreCase?displayProperty=nameWithType>、<xref:System.StringComparison.InvariantCultureIgnoreCase?displayProperty=nameWithType> 或 <xref:System.StringComparison.OrdinalIgnoreCase?displayProperty=nameWithType> 的值。 对于使用特定区域性（而非当前区域性）的比较，请调用 <xref:System.String.CompareTo%2A?displayProperty=nameWithType> 方法中具有 `culture` 和 `options` 参数的重载，并提供 <xref:System.Globalization.CompareOptions.IgnoreCase?displayProperty=nameWithType> 的值作为 `options` 参数。  
  
 若要比较混合大小写的字符串以确定它们是否相等，请调用 <xref:System.String.Equals%2A?displayProperty=nameWithType> 方法中具有 `comparisonType` 参数的其中一个重载，并向 `comparisonType` 自变量提供 <xref:System.StringComparison.CurrentCultureIgnoreCase?displayProperty=nameWithType>、<xref:System.StringComparison.InvariantCultureIgnoreCase?displayProperty=nameWithType> 或 <xref:System.StringComparison.OrdinalIgnoreCase?displayProperty=nameWithType> 的值。  
  
 有关详细信息，请参阅[有关使用字符串的最佳实践](best-practices-strings.md)。  
  
## <a name="toupper"></a>ToUpper  

 <xref:System.String.ToUpper%2A?displayProperty=nameWithType> 方法将字符串中的所有字符均更改为大写。 下面的示例将字符串“Hello World!”转换为 从混合大小写转换为大写。  
  
 [!code-csharp[Strings.ChangingCase#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Strings.ChangingCase/cs/Example.cs#1)]
 [!code-vb[Strings.ChangingCase#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Strings.ChangingCase/vb/Example.vb#1)]  
  
 默认情况下，上述示例区分区域性；它应用当前区域性的大小写约定。 若要执行非区域性敏感型大小写更改或应用特定区域性的大小写约定，请使用 <xref:System.String.ToUpper%28System.Globalization.CultureInfo%29?displayProperty=nameWithType> 方法重载，并向 culture  参数提供 <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType> 值或表示指定区域性的 <xref:System.Globalization.CultureInfo?displayProperty=nameWithType> 对象。 有关展示了如何使用 <xref:System.String.ToUpper%2A> 方法执行非区域性敏感型大小写更改的示例，请参阅[执行非区域性敏感型大小写更改](../globalization-localization/performing-culture-insensitive-case-changes.md)。  
  
## <a name="tolower"></a>ToLower  

 <xref:System.String.ToLower%2A?displayProperty=nameWithType> 方法与上述方法类似，但改为将字符串中的所有字符均转换为小写。 下面的示例将字符串“Hello World!”转换为 小写。  
  
 [!code-csharp[Strings.ChangingCase#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Strings.ChangingCase/cs/Example.cs#2)]
 [!code-vb[Strings.ChangingCase#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Strings.ChangingCase/vb/Example.vb#2)]  
  
 默认情况下，上述示例区分区域性；它应用当前区域性的大小写约定。 若要执行非区域性敏感型大小写更改或应用特定区域性的大小写约定，请使用 <xref:System.String.ToLower%28System.Globalization.CultureInfo%29?displayProperty=nameWithType> 方法重载，并向 culture  参数提供 <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType> 值或表示指定区域性的 <xref:System.Globalization.CultureInfo?displayProperty=nameWithType> 对象。 有关展示了如何使用 <xref:System.String.ToLower%28System.Globalization.CultureInfo%29> 方法执行非区域性敏感型大小写更改的示例，请参阅[执行非区域性敏感型大小写更改](../globalization-localization/performing-culture-insensitive-case-changes.md)。  
  
## <a name="totitlecase"></a>ToTitleCase  

 <xref:System.Globalization.TextInfo.ToTitleCase%2A?displayProperty=nameWithType> 将每个单词的第一个字符转换为大写并将其余字符转换为小写。 但是，全部大写的单词被假定为缩写词且不会转换。  
  
 <xref:System.Globalization.TextInfo.ToTitleCase%2A?displayProperty=nameWithType> 方法区分区域性；即是，它使用特定区域性的大小写约定。 为了调用方法，首先要从特定区域性的 <xref:System.Globalization.CultureInfo.TextInfo%2A?displayProperty=nameWithType> 属性中检索表示特定区域性的大小写约定的 <xref:System.Globalization.TextInfo> 对象。  
  
 下面的示例将数组中的每个字符串传递至 <xref:System.Globalization.TextInfo.ToTitleCase%2A?displayProperty=nameWithType> 方法。  字符串包含适当的标题字符串以及首字母缩写词。 通过使用英语（美国）区域性的大小写约定将字符串转换为首字母大写。  
  
 [!code-csharp[System.Globalization.TextInfo.ToTitleCase#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.globalization.textinfo.totitlecase/cs/totitlecase2.cs#1)]
 [!code-vb[System.Globalization.TextInfo.ToTitleCase#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.globalization.textinfo.totitlecase/vb/totitlecase2.vb#1)]  
  
 请注意，<xref:System.Globalization.TextInfo.ToTitleCase%2A?displayProperty=nameWithType> 方法虽然区分区域性，但不提供语言方面的正确大小写规则。 例如，在上述示例中，方法将“a tale of two cities”转换为“A Tale Of Two Cities”。 但是，对于 en-US 区域性，语言方面的正确首字母大小写应为“A Tale of Two Cities”。  
  
## <a name="see-also"></a>请参阅

- [基本字符串操作](basic-string-operations.md)
- [执行不区分区域性的字符串操作](../globalization-localization/performing-culture-insensitive-string-operations.md)
