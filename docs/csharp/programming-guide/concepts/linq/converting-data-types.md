---
title: 转换数据类型 (C#)
description: 转换方法可更改输入对象的类型。 请参阅 C# 中的 LINQ 查询中的转换操作，如 Enumerable.AsEnumerable 和 Enumerable.OfType。
ms.date: 07/20/2015
ms.assetid: 46e5682f-77a1-4302-8f93-a2b53c408808
ms.openlocfilehash: f9e3b354fd6eeba6564067550ea3821e4946d92f
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91159132"
---
# <a name="converting-data-types-c"></a>转换数据类型 (C#)

转换方法可更改输入对象的类型。

 LINQ 查询中的转换运算可用于各种应用程序。 以下是一些示例：

- <xref:System.Linq.Enumerable.AsEnumerable%2A?displayProperty=nameWithType> 方法可用于隐藏类型的标准查询运算符自定义实现。

- <xref:System.Linq.Enumerable.OfType%2A?displayProperty=nameWithType> 方法可用于为 LINQ 查询启用非参数化集合。

- <xref:System.Linq.Enumerable.ToArray%2A?displayProperty=nameWithType>、<xref:System.Linq.Enumerable.ToDictionary%2A?displayProperty=nameWithType>、<xref:System.Linq.Enumerable.ToList%2A?displayProperty=nameWithType> 和 <xref:System.Linq.Enumerable.ToLookup%2A?displayProperty=nameWithType> 方法可用于强制执行即时的查询，而不是将其推迟到枚举该查询时。

## <a name="methods"></a>方法

 下表列出了执行数据类型转换的标准查询运算符方法。

 本表中名称以“As”开头的转换方法可更改源集合的静态类型，但不对其进行枚举。 名称以“To”开头的方法可枚举源集合，并将项放入相应的集合类型。

|方法名|描述|C# 查询表达式语法|详细信息|
|-----------------|-----------------|---------------------------------|----------------------|
|AsEnumerable|返回类型化为 <xref:System.Collections.Generic.IEnumerable%601> 的输入。|不适用。|<xref:System.Linq.Enumerable.AsEnumerable%2A?displayProperty=nameWithType>|
|AsQueryable|将（泛型）<xref:System.Collections.IEnumerable> 转换为（泛型）<xref:System.Linq.IQueryable>。|不适用。|<xref:System.Linq.Queryable.AsQueryable%2A?displayProperty=nameWithType>|
|Cast|将集合中的元素转换为指定类型。|使用显式类型化的范围变量。 例如：<br /><br /> `from string str in words`|<xref:System.Linq.Enumerable.Cast%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Cast%2A?displayProperty=nameWithType>|
|OfType|根据其转换为指定类型的能力筛选值。|不适用。|<xref:System.Linq.Enumerable.OfType%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.OfType%2A?displayProperty=nameWithType>|
|ToArray|将集合转换为数组。 此方法强制执行查询。|不适用。|<xref:System.Linq.Enumerable.ToArray%2A?displayProperty=nameWithType>|
|ToDictionary|根据键选择器函数将元素放入 <xref:System.Collections.Generic.Dictionary%602>。 此方法强制执行查询。|不适用。|<xref:System.Linq.Enumerable.ToDictionary%2A?displayProperty=nameWithType>|
|ToList|将集合转换为 <xref:System.Collections.Generic.List%601>。 此方法强制执行查询。|不适用。|<xref:System.Linq.Enumerable.ToList%2A?displayProperty=nameWithType>|
|ToLookup|根据键选择器函数将元素放入 <xref:System.Linq.Lookup%602>（一对多字典）。 此方法强制执行查询。|不适用。|<xref:System.Linq.Enumerable.ToLookup%2A?displayProperty=nameWithType>|

## <a name="query-expression-syntax-example"></a>查询表达式语法示例

下面的代码示例使用显式类型化的范围变量将类型转换为子类型，然后才访问仅在此子类型上可用的成员。

```csharp
class Plant
{
    public string Name { get; set; }
}

class CarnivorousPlant : Plant
{
    public string TrapType { get; set; }
}

static void Cast()
{
    Plant[] plants = new Plant[] {
        new CarnivorousPlant { Name = "Venus Fly Trap", TrapType = "Snap Trap" },
        new CarnivorousPlant { Name = "Pitcher Plant", TrapType = "Pitfall Trap" },
        new CarnivorousPlant { Name = "Sundew", TrapType = "Flypaper Trap" },
        new CarnivorousPlant { Name = "Waterwheel Plant", TrapType = "Snap Trap" }
    };

    var query = from CarnivorousPlant cPlant in plants
                where cPlant.TrapType == "Snap Trap"
                select cPlant;

    foreach (Plant plant in query)
        Console.WriteLine(plant.Name);

    /* This code produces the following output:

        Venus Fly Trap
        Waterwheel Plant
    */
}
```

## <a name="see-also"></a>请参阅

- <xref:System.Linq>
- [标准查询运算符概述 (C#)](./standard-query-operators-overview.md)
- [from 子句](../../../language-reference/keywords/from-clause.md)
- [LINQ 查询表达式](../../../linq/index.md)
- [如何使用 LINQ 查询 ArrayList (C#)](./how-to-query-an-arraylist-with-linq.md)
