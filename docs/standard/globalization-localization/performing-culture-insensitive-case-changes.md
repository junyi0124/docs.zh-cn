---
title: 执行不区分区域性的大小写更改
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- String.ToLower method
- culture, case sensitivity
- Char.ToLower method
- case, changing in culture-insensitive strings
- Char.ToUpper method
- culture-insensitive string operations, case changes
- String.ToUpper method
- culture parameter
ms.assetid: 822d551c-c69a-4191-82f4-183d82c9179c
ms.openlocfilehash: d4e714e9b81ffc3e495f4ddaa8cf7eeedeb71261
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95685989"
---
# <a name="performing-culture-insensitive-case-changes"></a>执行不区分区域性的大小写更改

<xref:System.String.ToUpper%2A?displayProperty=nameWithType>、<xref:System.String.ToLower%2A?displayProperty=nameWithType>、<xref:System.Char.ToUpper%2A?displayProperty=nameWithType> 和 <xref:System.Char.ToLower%2A?displayProperty=nameWithType> 方法提供不接受任何参数的重载。 默认情况下，这些不带参数的重载根据 <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> 的值执行大小写更改。 这样生成的结果（区分大小写）可能会因区域性而异。 为了明确希望大小写更改是区域性敏感型，还是非区域性敏感型，应使用这些要求显式指定 `culture` 参数的方法重载。 对于区域性敏感型大小写更改，请为 `culture` 参数指定 `CultureInfo.CurrentCulture`。 对于非区域性敏感型大小写更改，请为 `culture` 参数指定 `CultureInfo.InvariantCulture`。  
  
 通常情况下，字符串会转换为标准大小写，以方便稍后查找。 如果按这种方式使用字符串，应为 `culture` 参数指定 `CultureInfo.InvariantCulture`，因为 <xref:System.Threading.Thread.CurrentCulture%2A?displayProperty=nameWithType> 的值可能会在大小写更改和执行查找时之间变化。  
  
 如果安全决策以大小写更改操作为依据，操作应为非区域性敏感型，以确保结果不受 `CultureInfo.CurrentCulture` 值的影响。 有关展示了区域性敏感型字符串操作如何产生不一致结果的示例，请参阅[字符串使用最佳做法](../base-types/best-practices-strings.md)的“使用当前区域性的字符串比较”部分。  
  
## <a name="using-the-stringtoupper-and-stringtolower-methods"></a>使用 String.ToUpper 和 String.ToLower 方法  

 为了代码清楚起见，建议始终使用 `String.ToUpper` 和 `String.ToLower` 方法重载，以便显式指定 `culture` 参数。 例如，下面的代码执行标识符查找。 默认情况下，`key.ToLower` 操作为区域性敏感型，但此行为并未通过读取代码明确。  
  
### <a name="example"></a>示例  
  
```vb  
Shared Function LookupKey(key As String) As Object  
   Return internalHashtable(key.ToLower())  
End Function  
```  
  
```csharp  
static object LookupKey(string key)
{  
    return internalHashtable[key.ToLower()];  
}  
```  
  
 如果希望 `key.ToLower` 操作为非区域性敏感型，应按照以下所述更改上一示例，以便在更改大小写时显式使用 `CultureInfo.InvariantCulture`。  
  
```vb  
Shared Function LookupKey(key As String) As Object  
    Return internalHashtable(key.ToLower(CultureInfo.InvariantCulture))  
End Function  
```  
  
```csharp  
static object LookupKey(string key)
{  
    return internalHashtable[key.ToLower(CultureInfo.InvariantCulture)];  
}  
```  
  
## <a name="using-the-chartoupper-and-chartolower-methods"></a>使用 Char.ToUpper 和 Char.ToLower 方法  

 尽管 `Char.ToUpper` 和 `Char.ToLower` 方法的特性与 `String.ToUpper` 和 `String.ToLower` 方法相同，但受影响的区域性只有土尔其语（土尔其）和阿塞拜疆语（拉丁，阿塞拜疆）。 这些是唯一两个存在单字符大小写差异的区域性。 有关此唯一大小写映射的详细信息，请参阅 <xref:System.String> 类主题中的“大小写”部分。 为了代码清楚起见，并确保结果一致，建议始终使用这些方法重载，以便显式指定 `culture` 参数。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.String.ToUpper%2A?displayProperty=nameWithType>
- <xref:System.String.ToLower%2A?displayProperty=nameWithType>
- <xref:System.Char.ToUpper%2A?displayProperty=nameWithType>
- <xref:System.Char.ToLower%2A?displayProperty=nameWithType>
- [执行不区分区域性的字符串操作](performing-culture-insensitive-string-operations.md)
