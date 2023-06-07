---
title: 筛选数据 (C#)
description: 筛选（也称为选择）根据条件限制结果。 了解 C# 中的 LINQ 执行筛选的标准查询运算符方法。
ms.date: 07/20/2015
ms.assetid: fbaece0d-0f23-47f7-89c5-f3ea8db692b6
ms.openlocfilehash: 51bf9f930ba67ba07c7c0f357910d5e36014138d
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91186037"
---
# <a name="filtering-data-c"></a>筛选数据 (C#)

筛选是指将结果集限制为仅包含满足指定条件的元素的操作。 它也称为选定内容。  
  
 下图演示了对字符序列进行筛选的结果。 筛选操作的谓词指定字符必须为“A”。  
  
 ![显示 LINQ 筛选操作的图表](./media/filtering-data/linq-filter-operation.png)  
  
 下面一节列出了执行所选内容的标准查询运算符方法。  
  
## <a name="methods"></a>方法  
  
|方法名|描述|C# 查询表达式语法|详细信息|  
|-----------------|-----------------|---------------------------------|----------------------|  
|OfType|根据其转换为特定类型的能力选择值。|不适用。|<xref:System.Linq.Enumerable.OfType%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.OfType%2A?displayProperty=nameWithType>|  
|Where|选择基于谓词函数的值。|`where`|<xref:System.Linq.Enumerable.Where%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Where%2A?displayProperty=nameWithType>|  
  
## <a name="query-expression-syntax-example"></a>查询表达式语法示例  

 以下示例使用 `where` 子句从数组中筛选具有特定长度的字符串。  
  
```csharp  
string[] words = { "the", "quick", "brown", "fox", "jumps" };  
  
IEnumerable<string> query = from word in words  
                            where word.Length == 3  
                            select word;  
  
foreach (string str in query)  
    Console.WriteLine(str);  
  
/* This code produces the following output:  
  
    the  
    fox  
*/  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Linq>
- [标准查询运算符概述 (C#)](./standard-query-operators-overview.md)
- [where 子句](../../../language-reference/keywords/where-clause.md)
- [在运行时动态指定谓词筛选器](../../../linq/dynamically-specify-predicate-filters-at-runtime.md)
- [如何使用反射查询程序集的元数据 (LINQ) (C#)](./how-to-query-an-assembly-s-metadata-with-reflection-linq.md)
- [如何查询具有指定特性或名称的文件 (C#)](./how-to-query-for-files-with-a-specified-attribute-or-name.md)
- [如何按任意词或字段对文本数据进行排序或筛选 (LINQ) (C#)](./how-to-sort-or-filter-text-data-by-any-word-or-field-linq.md)
