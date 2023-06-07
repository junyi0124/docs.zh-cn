---
title: 修整和删除 .NET 中的字符串字符
description: 了解如何在字符串的开头或结尾修整空格，或者从 .NET 的字符串中的指定位置删除任意数量的空格或字符。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- strings [.NET], removing characters
- Remove method
- TrimEnd method
- Trim method
- trimming characters
- TrimStart method
- removing characters
ms.assetid: ab248dab-70d4-4413-81c6-542d153fd195
ms.openlocfilehash: 9b0fd87bfec747f8dd09d3972167374a1a2daffa
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734175"
---
# <a name="trimming-and-removing-characters-from-strings-in-net"></a>修整和删除 .NET 中的字符串字符

如果将句子分析成单个单词，最后得到的结果可能是任意一端带有空白（也称为空格）的单词。 在这种情形下，可以使用 **System.String** 类中的其中一种剪裁方法，从字符串中的指定位置移除任何数量的空格或其他字符。 下表描述了可用的剪裁方法。  
  
|方法名称|使用|  
|-----------------|---------|  
|<xref:System.String.Trim%2A?displayProperty=nameWithType>|从字符串的开头和结尾移除空白或者指定的字符|  
|<xref:System.String.TrimEnd%2A?displayProperty=nameWithType>|从字符串的结尾移除在字符数组中指定的字符。|  
|<xref:System.String.TrimStart%2A?displayProperty=nameWithType>|从字符串的开头移除在字符数组中指定的字符。|  
|<xref:System.String.Remove%2A?displayProperty=nameWithType>|从字符串中的指定索引位置移除指定数量的字符。|  
  
## <a name="trim"></a>Trim

 通过使用 <xref:System.String.Trim%2A?displayProperty=nameWithType> 方法，可以方便地从字符串两端移除空白，如下面的示例所示。  
  
 [!code-cpp[Conceptual.String.BasicOps#17](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/trimming.cpp#17)]
 [!code-csharp[Conceptual.String.BasicOps#17](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/trimming.cs#17)]
 [!code-vb[Conceptual.String.BasicOps#17](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/trimming.vb#17)]  
  
 还可以从字符串的开始和结尾移除指定的字符。 下面的示例将移除空白字符、句点和星号。  
  
 [!code-csharp[Conceptual.String.BasicOps#22](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/trim2.cs#22)]
 [!code-vb[Conceptual.String.BasicOps#22](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/trim2.vb#22)]  
  
## <a name="trimend"></a>TrimEnd

 **String.TrimEnd** 方法从字符串的结尾移除字符，同时创建新的字符串对象。 通过向此方法传递一个字符数组来指定要移除的字符。 字符数组中的元素顺序并不影响剪裁操作。 找到未在数组中指定的字符时，剪裁停止。  
  
 下面的示例使用 TrimEnd 方法，删除字符串的最后几个字母。 在此示例中，`'r'` 字符和 `'W'` 字符的位置交换，说明数组中字符的顺序并不重要。 请注意，此代码移除 `MyString` 的最后一个单词和第一个单词的一部分。  
  
 [!code-cpp[Conceptual.String.BasicOps#18](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/trimming.cpp#18)]
 [!code-csharp[Conceptual.String.BasicOps#18](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/trimming.cs#18)]
 [!code-vb[Conceptual.String.BasicOps#18](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/trimming.vb#18)]  
  
 此代码向控制台显示 `He`。  
  
 下面的示例使用 **TrimEnd** 方法移除字符串的最后一个单词。 在此代码中，单词 `Hello` 后跟一个逗号，而由于在要剪裁的字符数组中未指定逗号，因此剪裁在逗号处结束。  
  
 [!code-cpp[Conceptual.String.BasicOps#19](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/trimming.cpp#19)]
 [!code-csharp[Conceptual.String.BasicOps#19](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/trimming.cs#19)]
 [!code-vb[Conceptual.String.BasicOps#19](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/trimming.vb#19)]  
  
 此代码向控制台显示 `Hello,`。  
  
## <a name="trimstart"></a>TrimStart

 **String.TrimStart** 方法类似于 **String.TrimEnd** 方法，不同之处在于它通过从现有字符串对象的开头移除字符来创建新的字符串。 通过向 **TrimStart** 方法传递一个字符数组来指定要移除的字符。 与 **TrimEnd** 方法一样，字符数组中元素的顺序并不影响剪裁操作。 找到未在数组中指定的字符时，剪裁停止。  
  
 下面的示例移除字符串的第一个单词。 在此示例中，`'l'` 字符和 `'H'` 字符的位置交换，说明数组中字符的顺序并不重要。  
  
 [!code-cpp[Conceptual.String.BasicOps#20](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/trimming.cpp#20)]
 [!code-csharp[Conceptual.String.BasicOps#20](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/trimming.cs#20)]
 [!code-vb[Conceptual.String.BasicOps#20](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/trimming.vb#20)]  
  
 此代码向控制台显示 `World!`。  
  
## <a name="remove"></a>删除

 <xref:System.String.Remove%2A?displayProperty=nameWithType> 方法从现有字符串的指定位置开始移除指定数量的字符。 此方法采用从零开始的索引。  
  
 下面的示例在字符串从零开始的索引中第五个位置开始移除十个字符。  
  
 [!code-cpp[Conceptual.String.BasicOps#21](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/trimming.cpp#21)]
 [!code-csharp[Conceptual.String.BasicOps#21](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/trimming.cs#21)]
 [!code-vb[Conceptual.String.BasicOps#21](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/trimming.vb#21)]  
  
## <a name="replace"></a>替换

 通过调用 <xref:System.String.Replace%28System.String%2CSystem.String%29?displayProperty=nameWithType> 方法并指定空字符串 (<xref:System.String.Empty?displayProperty=nameWithType>) 作为替代，也可以从字符串中移除指定字符或子字符串。 下面的示例从字符串中移除所有逗号。  
  
 [!code-csharp[Conceptual.String.BasicOps#23](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/replace1.cs#23)]
 [!code-vb[Conceptual.String.BasicOps#23](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/replace1.vb#23)]  
  
## <a name="see-also"></a>请参阅

- [基本字符串操作](basic-string-operations.md)
