---
title: 基本 LINQ 查询操作 (C#)
description: 介绍一下 LINQ 查询表达式以及在查询中可能执行的一些操作。
ms.date: 07/20/2015
helpviewer_keywords:
- orderby clause [LINQ in C#]
- ordering data [LINQ in C#]
- selecting data [LINQ in C#]
- queries [LINQ in C#], basic operations
- grouping data [LINQ in C#]
- data sources [LINQ in C#]
- sorting data [LINQ in C#]
- projections [LINQ in C#]
- filtering data [LINQ in C#]
- joining data [LINQ in C#]
- select clause [LINQ in C#]
- LINQ [C#], basic query operations
- join clause [LINQ in C#]
- group clause [LINQ in C#]
ms.assetid: a7ea3421-1cf4-4df7-832a-aa22fe6379e9
ms.openlocfilehash: 9f5d39e396e9be3e633326d4034a89d874373d75
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91159288"
---
# <a name="basic-linq-query-operations-c"></a>基本 LINQ 查询操作 (C#)

本主题简要介绍了 LINQ 查询表达式和一些在查询中执行的典型操作。 下面各主题中提供了更具体的信息：  
  
 [LINQ 查询表达式](../../../linq/index.md)  
  
 [标准查询运算符概述 (C#)](./standard-query-operators-overview.md)  
  
 [演练：用 C# 编写查询](./walkthrough-writing-queries-linq.md)  
  
> [!NOTE]
> 如果你已熟悉查询语言（如 SQL 或 XQuery），则可以跳过本主题的大部分内容。 请参阅下一节中的“`from` 子句”部分，了解 LINQ 查询表达式中的子句顺序。  
  
## <a name="obtaining-a-data-source"></a>获取数据源  

 在 LINQ 查询中，第一步是指定数据源。 和大多数编程语言相同，在使用 C# 时也必须先声明变量，然后才能使用它。 在 LINQ 查询中，先使用 `from` 子句引入数据源 (`customers`) 和范围变量 (`cust`)。  
  
 [!code-csharp[csLINQGettingStarted#23](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#23)]  
  
 范围变量就像 `foreach` 循环中的迭代变量，但查询表达式中不会真正发生迭代。 当执行查询时，范围变量将充当对 `customers` 中每个连续的元素的引用。 由于编译器可以推断 `cust` 的类型，因此无需显式指定它。 可通过 `let` 子句引入其他范围变量。 有关详细信息，请参阅 [let 子句](../../../language-reference/keywords/let-clause.md)。  
  
> [!NOTE]
> 对于非泛型数据源（例如 <xref:System.Collections.ArrayList>），必须显式键入范围变量。 有关详细信息，请参阅[如何使用 LINQ (C#)](./how-to-query-an-arraylist-with-linq.md) 和 [From 子句](../../../language-reference/keywords/from-clause.md)查询 ArrayList。  
  
## <a name="filtering"></a>筛选  

 或许，最常见的查询操作是以布尔表达式的形式应用筛选器。 筛选器使查询仅返回表达式为 true 的元素。 将通过使用 `where` 子句生成结果。 筛选器实际指定要从源序列排除哪些元素。 在下列示例中，仅返回地址位于“London”的 `customers`。  
  
 [!code-csharp[csLINQGettingStarted#24](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#24)]  
  
 可使用熟悉的 C# 逻辑 `AND` 和 `OR` 运算符，在 `where` 子句中根据需要应用尽可能多的筛选器表达式。 例如，若要仅返回来自“London”的客户 `AND` 该客户名称为“Devon”，可编写以下代码：  
  
 [!code-csharp[csLINQGettingStarted#25](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#25)]  
  
 要返回来自 London 或 Paris 的客户，可编写以下代码：  
  
 [!code-csharp[csLINQGettingStarted#26](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#26)]  
  
 有关详细信息，请参阅 [where 子句](../../../language-reference/keywords/where-clause.md)。  
  
## <a name="ordering"></a>中间件排序  

 对返回的数据进行排序通常很方便。 `orderby` 子句根据要排序类型的默认比较器，对返回序列中的元素排序。 例如，基于 `Name` 属性，可将下列查询扩展为对结果排序。 由于 `Name` 是字符串，默认比较器将按字母顺序从 A 到 Z 进行排序。  
  
 [!code-csharp[csLINQGettingStarted#27](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#27)]  
  
 要对结果进行从 Z 到 A 的逆序排序，请使用 `orderby…descending` 子句。  
  
 有关详细信息，请参阅 [orderby 子句](../../../language-reference/keywords/orderby-clause.md)。  
  
## <a name="grouping"></a>分组  

 `group` 子句用于对根据您指定的键所获得的结果进行分组。 例如，可指定按 `City` 对结果进行分组，使来自 London 或 Paris 的所有客户位于单独的组内。 在这种情况下，`cust.City` 是键。  
  
 [!code-csharp[csLINQGettingStarted#28](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#28)]  
  
 使用 `group` 子句结束查询时，结果将以列表的形式列出。 列表中的每个元素都是具有 `Key` 成员的对象，列表中的元素根据该键被分组。 在循环访问生成组序列的查询时，必须使用嵌套 `foreach` 循环。 外层循环循环访问每个组，内层循环循环访问每个组的成员。  
  
 如果必须引用某个组操作的结果，可使用 `into` 关键字创建能被进一步查询的标识符。 下列查询仅返回包含两个以上客户的组：  
  
 [!code-csharp[csLINQGettingStarted#29](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#29)]  
  
 有关详细信息，请参阅 [group 子句](../../../language-reference/keywords/group-clause.md)。  
  
## <a name="joining"></a>联接  

 联接操作在不同序列间创建关联，这些序列在数据源中未被显式模块化。 例如，可通过执行联接来查找所有位置相同的客户和分销商。 在 LINQ 中，`join` 子句始终作用于对象集合，而非直接作用于数据库表。  
  
 [!code-csharp[csLINQGettingStarted#36](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#36)]  
  
 在 LINQ 中，不必像在 SQL 中那样频繁使用 `join`，因为 LINQ 中的外键在对象模型中表示为包含项集合的属性。 例如 `Customer` 对象包含 `Order` 对象的集合。 不必执行联接，只需使用点表示法访问订单：  
  
```csharp
from order in Customer.Orders...  
```  
  
 有关详细信息，请参阅 [join 子句](../../../language-reference/keywords/join-clause.md)。  
  
## <a name="selecting-projections"></a>选择（投影）  

 `select` 子句生成查询结果并指定每个返回的元素的“形状”或类型。 例如，可以指定结果包含的是整个 `Customer` 对象、仅一个成员、成员的子集，还是某个基于计算或新对象创建的完全不同的结果类型。 当 `select` 子句生成除源元素副本以外的内容时，该操作称为投影。 使用投影转换数据是 LINQ 查询表达式的一种强大功能。 有关详细信息，请参阅[使用 LINQ (C#)](./data-transformations-with-linq.md) 和 [select 子句](../../../language-reference/keywords/select-clause.md)进行数据转换。  
  
## <a name="see-also"></a>请参阅

- [LINQ 查询表达式](../../../linq/index.md)
- [演练：用 C# 编写查询](./walkthrough-writing-queries-linq.md)
- [查询关键字 (LINQ)](../../../language-reference/keywords/query-keywords.md)
- [匿名类型](../../classes-and-structs/anonymous-types.md)
