---
title: 在数组中执行不区分区域性的字符串操作
ms.date: 03/30/2017
helpviewer_keywords:
- culture-insensitive string operations, in arrays
- arrays [.NET], culture-insensitive string operations
- comparer parameter
ms.assetid: f12922e1-6234-4165-8896-63f0653ab478
ms.openlocfilehash: ac8074c5d13209b6828b7afbc720921015e11bfd
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94829798"
---
# <a name="performing-culture-insensitive-string-operations-in-arrays"></a>在数组中执行不区分区域性的字符串操作

默认情况下，<xref:System.Array.Sort%2A?displayProperty=nameWithType> 和 <xref:System.Array.BinarySearch%2A?displayProperty=nameWithType> 方法重载使用 <xref:System.Threading.Thread.CurrentCulture%2A?displayProperty=nameWithType> 属性执行区域性敏感型排序。 由于排序顺序不同，因此这些方法返回的区域性敏感型结果可能会因区域性而异。 若要消除区域性敏感型行为，请使用需要使用 `comparer` 参数的此方法重载之一。 `comparer` 参数指定要在比较数组元素时使用的 <xref:System.Collections.IComparer> 实现。 对于参数，指定使用 <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType> 的自定义固定比较器类。 [在集合中执行非区域性敏感型字符串操作](performing-culture-insensitive-string-operations-in-collections.md)主题的“使用 SortedList 类”子主题提供了自定义固定比较器类的示例。

> [!NOTE]
> 向比较方法传递 CultureInfo.InvariantCulture 确实会执行非区域性敏感型比较  。 但是，这不会导致对文件路径、注册表项、环境变量等进行非语义比较。 也不支持基于比较结果的安全决策。 若要进行非语义比较或支持基于结果的安全决策，应用应使用接受 <xref:System.StringComparison> 值的比较方法。 然后，应用应传递 <xref:System.StringComparison.Ordinal>。

## <a name="see-also"></a>另请参阅

- <xref:System.Array.Sort%2A?displayProperty=nameWithType>
- <xref:System.Array.BinarySearch%2A?displayProperty=nameWithType>
- <xref:System.Collections.IComparer>
- [执行不区分区域性的字符串操作](performing-culture-insensitive-string-operations.md)
