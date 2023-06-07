---
title: 对查询结果进行分组（C# 中的 LINQ）
description: 了解如何使用 C# 中的 LINQ 对结果进行分组。
ms.date: 12/01/2016
ms.assetid: 2e4ec27f-06fb-4de7-8973-0189906d4520
ms.openlocfilehash: 577a358c31fcf5346e7aab7a2e2b6be10fd1beff
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "61688457"
---
# <a name="group-query-results"></a>对查询结果进行分组

分组是 LINQ 最强大的功能之一。 以下示例演示如何以各种方式对数据进行分组：

- 依据单个属性。

- 依据字符串属性的首字母。

- 依据计算出的数值范围。

- 依据布尔谓词或其他表达式。

- 依据组合键。

此外，最后两个查询将其结果投影到一个新的匿名类型中，该类型仅包含学生的名字和姓氏。 有关详细信息，请参阅 [group 子句](../language-reference/keywords/group-clause.md)。

## <a name="example"></a>示例

本主题中的所有示例都使用下列帮助程序类和数据源。

[!code-csharp[csProgGuideLINQ#15](~/samples/snippets/csharp/concepts/linq/how-to-group-query-results_1.cs)]

## <a name="example"></a>示例

以下示例演示如何通过使用元素的单个属性作为分组键对源元素进行分组。 在此示例中，键是 `string`，即学生的姓氏。 还可以使用子字符串作为键。 分组操作对该类型使用默认的相等比较器。

将以下方法粘贴到 `StudentClass` 类。 将 `Main` 方法中的调用语句更改为 `sc.GroupBySingleProperty()`。

[!code-csharp[csProgGuideLINQ#17](~/samples/snippets/csharp/concepts/linq/how-to-group-query-results_2.cs)]

## <a name="example"></a>示例

下例演示如何通过使用除对象属性以外的某个项作为分组键对源元素进行分组。 在此示例中，键是学生姓氏的第一个字母。

将以下方法粘贴到 `StudentClass` 类。 将 `Main` 方法中的调用语句更改为 `sc.GroupBySubstring()`。

[!code-csharp[csProgGuideLINQ#18](~/samples/snippets/csharp/concepts/linq/how-to-group-query-results_3.cs)]

## <a name="example"></a>示例

以下示例演示如何通过使用某个数值范围作为分组键对源元素进行分组。 然后，查询将结果投影到一个匿名类型中，该类型仅包含学生的名字和姓氏以及该学生所属的百分点范围。 使用匿名类型的原因是没有必要使用完整的 `Student` 对象来显示结果。 `GetPercentile` 是一个帮助程序函数，它根据学生的平均分数计算百分比。 该方法返回 0 到 10 之间的整数。

[!code-csharp[csProgGuideLINQ#50](~/samples/snippets/csharp/concepts/linq/how-to-group-query-results_4.cs)]

将以下方法粘贴到 `StudentClass` 类。 将 `Main` 方法中的调用语句更改为 `sc.GroupByRange()`。

[!code-csharp[csProgGuideLINQ#19](~/samples/snippets/csharp/concepts/linq/how-to-group-query-results_5.cs)]

## <a name="example"></a>示例

以下示例演示如何通过使用布尔比较表达式对源元素进行分组。 在此示例中，布尔表达式会测试学生的平均考试分数是否超过 75。 与上述示例一样，结果被投影到一个匿名类型中，因为不需要完整的源元素。 请注意，执行查询时，该匿名类型中的属性会变成 `Key` 成员上的属性，并且可以通过名称进行访问。

将以下方法粘贴到 `StudentClass` 类。 将 `Main` 方法中的调用语句更改为 `sc.GroupByBoolean()`。

[!code-csharp[csProgGuideLINQ#20](~/samples/snippets/csharp/concepts/linq/how-to-group-query-results_6.cs)]

## <a name="example"></a>示例

以下示例演示如何使用匿名类型来封装包含多个值的键。 在此示例中，第一个键值是学生姓氏的第一个字母。 第二个键值是一个布尔值，指定该学生在第一次考试中的得分是否超过了 85。 可以按照该键中的任何属性对组进行排序。

将以下方法粘贴到 `StudentClass` 类。 将 `Main` 方法中的调用语句更改为 `sc.GroupByCompositeKey()`。

[!code-csharp[csProgGuideLINQ#21](~/samples/snippets/csharp/concepts/linq/how-to-group-query-results_7.cs)]

## <a name="see-also"></a>另请参阅

- <xref:System.Linq.Enumerable.GroupBy%2A>
- <xref:System.Linq.IGrouping%602>
- [语言集成查询 (LINQ)](index.md)
- [group 子句](../language-reference/keywords/group-clause.md)
- [匿名类型](../programming-guide/classes-and-structs/anonymous-types.md)
- [对分组操作执行子查询](perform-a-subquery-on-a-grouping-operation.md)
- [创建嵌套组](create-a-nested-group.md)
- [数据分组](../programming-guide/concepts/linq/grouping-data.md)
