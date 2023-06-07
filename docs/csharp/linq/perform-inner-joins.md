---
title: 执行内联（C# 中的 LINQ）
description: 了解如何使用 C# 中的 LINQ 执行内联。
ms.date: 12/01/2016
ms.assetid: 45bceed6-f549-4114-a9b1-b44feb497742
ms.openlocfilehash: a3e8e9bd97ec630797bc48a3302b27ed45d9103e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "61659834"
---
# <a name="perform-inner-joins"></a>执行内部联接

在关系数据库术语中，内部联接  会生成一个结果集，在该结果集中，第一个集合的每个元素对于第二个集合中的每个匹配元素都会出现一次。 如果第一个集合中的元素没有匹配元素，则它不会出现在结果集中。 由 C# 中的 `join` 子句调用的 <xref:System.Linq.Enumerable.Join%2A> 方法可实现内部联接。

本文演示如何执行内联的四种变体：

- 基于简单键使两个数据源中的元素相关联的简单内部联接。

- 基于复合  键使两个数据源中的元素相关联的内部联接。 复合键是由多个值组成的键，使你可以基于多个属性使元素相关联。

- 在其中将连续联接操作相互追加的多联接  。

- 使用分组联接实现的内部联接。

## <a name="example---simple-key-join"></a>示例 - 简单键联接

下面的示例创建两个集合，其中包含两种用户定义类型 `Person` 和 `Pet` 的对象。 查询使用 C# 中的 `join` 子句将 `Person` 对象与 `Owner` 是该 `Person` 的 `Pet` 对象匹配。 C# 中的 `select` 子句定义结果对象的外观。 在此示例中，结果对象是由所有者名字和宠物姓名组成的匿名类型。

[!code-csharp[CsLINQProgJoining#1](~/samples/snippets/csharp/concepts/linq/how-to-perform-inner-joins_1.cs)]

请注意，`LastName` 是“Huff”的 `Person` 对象未出现在结果集中，因为没有 `Pet` 对象的 `Pet.Owner` 等于该 `Person`。

## <a name="example---composite-key-join"></a>示例 - 组合键联接

可以使用复合键基于多个属性来比较元素，而不是只基于一个属性使元素相关联。 为此，请为每个集合指定键选择器函数，以返回由要比较的属性组成的匿名类型。 如果对属性进行标记，则它们必须在每个键的匿名类型中具有相同标签。 属性还必须按相同顺序出现。

下面的示例使用 `Employee` 对象的列表和 `Student` 对象的列表来确定哪些雇员同时还是学生。 这两种类型都具有 <xref:System.String> 类型的 `FirstName` 和 `LastName` 属性。 通过每个列表的元素创建联接键的函数会返回由每个元素的 `FirstName` 和 `LastName` 属性组成的匿名类型。 联接运算会比较这些复合键是否相等，并从每个列表返回名字和姓氏都匹配的对象对。

[!code-csharp[CsLINQProgJoining#2](~/samples/snippets/csharp/concepts/linq/how-to-perform-inner-joins_2.cs)]

## <a name="example---multiple-join"></a>示例 - 多联接

可以将任意数量的联接操作相互追加，以执行多联接。 C# 中的每个 `join` 子句会将指定数据源与上一个联接的结果相关联。

下面的示例创建三个集合：`Person` 对象的列表、`Cat` 对象的列表和 `Dog` 对象的列表。

C# 中的第一个 `join` 子句基于与 `Cat.Owner` 匹配的 `Person` 对象来匹配人和猫。 它返回包含 `Person` 对象和 `Cat.Name` 的匿名类型的序列。

C# 中的第二个 `join` 子句基于由 `Owner` 类型的 `Person` 属性和动物姓名的第一个字母组成的复合键，将第一个联接返回的匿名类型与提供的狗列表中的 `Dog` 对象相关联。 它返回包含来自每个匹配对的 `Cat.Name` 和 `Dog.Name` 属性的匿名类型的序列。 由于这是内部联接，因此只返回第一个数据源中在第二个数据源中具有匹配项的对象。

[!code-csharp[CsLINQProgJoining#3](~/samples/snippets/csharp/concepts/linq/how-to-perform-inner-joins_3.cs)]

## <a name="example---inner-join-by-using-grouped-join"></a>示例 - 使用分组联接的内联

下面的示例演示如何使用分组联接实现内部联接。

在 `query1` 中，`Person` 对象的列表会基于与 `Pet.Owner` 属性匹配的 `Person`，分组联接到 `Pet` 对象队列中。 分组联接会创建中间组的集合，其中每个组都包含 `Person` 对象和匹配 `Pet` 对象的序列。

通过向查询添加另一个 `from` 子句，此序列的序列会合并（或平展）为一个较长的序列。 最后一个序列的元素的类型由 `select` 子句指定。 在此示例中，该类型是由每个匹配对的 `Person.FirstName` 和 `Pet.Name` 属性组成的匿名类型。

`query1` 的结果等效于通过使用 `join` 子句（不使用 `into` 子句）执行内部联接来获取的结果集。 `query2` 变量演示了此等效查询。

[!code-csharp[CsLINQProgJoining#4](~/samples/snippets/csharp/concepts/linq/how-to-perform-inner-joins_4.cs)]

## <a name="see-also"></a>另请参阅

- <xref:System.Linq.Enumerable.Join%2A>
- <xref:System.Linq.Enumerable.GroupJoin%2A>
- [执行分组联接](perform-grouped-joins.md)
- [执行左外部联接](perform-left-outer-joins.md)
- [匿名类型](../programming-guide/classes-and-structs/anonymous-types.md)
