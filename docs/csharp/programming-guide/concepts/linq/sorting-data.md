---
title: 对数据排序 (C#)
description: 了解排序操作以及使用 C# 中的 LINQ 执行排序操作的标准查询运算符方法。
ms.date: 07/20/2015
ms.assetid: d93fa055-2f19-46d2-9898-e2aed628f1c9
ms.openlocfilehash: 0665e5dec95fd2929d24d82568de66597df1c0bd
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91195501"
---
# <a name="sorting-data-c"></a>对数据排序 (C#)

排序操作基于一个或多个属性对序列的元素进行排序。 第一个排序条件对元素执行主要排序。 通过指定第二个排序条件，您可以对每个主要排序组内的元素进行排序。  
  
 下图展示了对一系列字符执行按字母顺序排序操作的结果。
  
 ![展示按字母顺序排序操作的图。](./media/sorting-data/alphabetical-sort-operation.png)  
  
 下节列出了对数据进行排序的标准查询运算符方法。  
  
## <a name="methods"></a>方法  
  
|方法名|描述|C# 查询表达式语法|更多信息|  
|-----------------|-----------------|---------------------------------|----------------------|  
|OrderBy|按升序对值排序。|`orderby`|<xref:System.Linq.Enumerable.OrderBy%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.OrderBy%2A?displayProperty=nameWithType>|  
|OrderByDescending|按降序对值排序。|`orderby … descending`|<xref:System.Linq.Enumerable.OrderByDescending%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.OrderByDescending%2A?displayProperty=nameWithType>|  
|ThenBy|按升序执行次要排序。|`orderby …, …`|<xref:System.Linq.Enumerable.ThenBy%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.ThenBy%2A?displayProperty=nameWithType>|  
|ThenByDescending|按降序执行次要排序。|`orderby …, … descending`|<xref:System.Linq.Enumerable.ThenByDescending%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.ThenByDescending%2A?displayProperty=nameWithType>|  
|Reverse|反转集合中元素的顺序。|不适用。|<xref:System.Linq.Enumerable.Reverse%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Reverse%2A?displayProperty=nameWithType>|  
  
## <a name="query-expression-syntax-examples"></a>查询表达式语法示例  
  
### <a name="primary-sort-examples"></a>主要排序示例  
  
#### <a name="primary-ascending-sort"></a>主要升序排序  

 下面的示例演示如何在 LINQ 查询中使用 `orderby` 子句按字符串长度对数组中的字符串进行升序排序。  
  
```csharp  
string[] words = { "the", "quick", "brown", "fox", "jumps" };  
  
IEnumerable<string> query = from word in words  
                            orderby word.Length  
                            select word;  
  
foreach (string str in query)  
    Console.WriteLine(str);  
  
/* This code produces the following output:  
  
    the  
    fox  
    quick  
    brown  
    jumps  
*/  
```  
  
#### <a name="primary-descending-sort"></a>主要降序排序  

 下面的示例演示如何在 LINQ 查询中使用 `orderby descending` 子句按字符串的第一个字母对字符串进行降序排序。  
  
```csharp  
string[] words = { "the", "quick", "brown", "fox", "jumps" };  
  
IEnumerable<string> query = from word in words  
                            orderby word.Substring(0, 1) descending  
                            select word;  
  
foreach (string str in query)  
    Console.WriteLine(str);  
  
/* This code produces the following output:  
  
    the  
    quick  
    jumps  
    fox  
    brown  
*/  
```  
  
### <a name="secondary-sort-examples"></a>次要排序示例  
  
#### <a name="secondary-ascending-sort"></a>次要升序排序  

 下面的示例演示如何在 LINQ 查询中使用 `orderby` 子句对数组中的字符串执行主要和次要排序。 首先按字符串长度，其次按字符串的第一个字母，对字符串进行升序排序。  
  
```csharp  
string[] words = { "the", "quick", "brown", "fox", "jumps" };  
  
IEnumerable<string> query = from word in words  
                            orderby word.Length, word.Substring(0, 1)  
                            select word;  
  
foreach (string str in query)  
    Console.WriteLine(str);  
  
/* This code produces the following output:  
  
    fox  
    the  
    brown  
    jumps  
    quick  
*/  
```  
  
#### <a name="secondary-descending-sort"></a>次要降序排序  

 下面的示例演示如何在 LINQ 查询中使用 `orderby descending` 子句按升序执行主要排序，按降序执行次要排序。 首先按字符串长度，其次按字符串的第一个字母，对字符串进行排序。  
  
```csharp  
string[] words = { "the", "quick", "brown", "fox", "jumps" };  
  
IEnumerable<string> query = from word in words  
                            orderby word.Length, word.Substring(0, 1) descending  
                            select word;  
  
foreach (string str in query)  
    Console.WriteLine(str);  
  
/* This code produces the following output:  
  
    the  
    fox  
    quick  
    jumps  
    brown  
*/  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Linq>
- [标准查询运算符概述 (C#)](./standard-query-operators-overview.md)
- [orderby 子句](../../../language-reference/keywords/orderby-clause.md)
- [对 Join 子句的结果进行排序](../../../linq/order-the-results-of-a-join-clause.md)
- [如何按任意词或字段对文本数据进行排序或筛选 (LINQ) (C#)](./how-to-sort-or-filter-text-data-by-any-word-or-field-linq.md)
