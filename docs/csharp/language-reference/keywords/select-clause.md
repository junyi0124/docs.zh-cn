---
description: select 子句 - C# 参考
title: select 子句 - C# 参考
ms.date: 07/20/2015
f1_keywords:
- select_CSharpKeyword
- select
helpviewer_keywords:
- select keyword [C#]
- select clause [C#]
ms.assetid: df01e266-5781-4aaa-80c4-67cf28ea093f
ms.openlocfilehash: d67c99cc841c08a63cc83843a07a46e80199b9d1
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "89136897"
---
# <a name="select-clause-c-reference"></a>select 子句（C# 参考）

在查询表达式中，`select` 子句指定在执行查询时产生的值的类型。 根据计算所有以前的子句以及根据 `select` 子句本身的所有表达式得出结果。 查询表达式必须以 `select` 子句或 [group](group-clause.md) 子句结尾。

以下示例演示查询表达式中的简单的 `select` 子句。

[!code-csharp[cscsrefQueryKeywords#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/Select.cs#8)]  

`select` 子句生成的序列的类型确定查询变量 `queryHighScores` 的类型。 在最简单的情况下，`select` 子句仅指定范围变量。 这将导致返回的序列包含与数据源类型相同的元素。 有关详细信息，请参阅 [LINQ 查询操作中的类型关系](../../programming-guide/concepts/linq/type-relationships-in-linq-query-operations.md)。 但是，`select` 子句还提供了强大的机制，用于将源数据转换（或投影）为新类型。 有关详细信息，请参阅[使用 LINQ 进行数据转换 (C#)](../../programming-guide/concepts/linq/data-transformations-with-linq.md)。

## <a name="example"></a>示例

以下示例展示 `select` 子句可能采用的所有不同窗体。 在每个查询中，请注意 `select` 子句和查询变量（`studentQuery1`、`studentQuery2` 等）类型之间的关系。

[!code-csharp[cscsrefQueryKeywords#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/Select.cs#9)]

如前面示例中的 `studentQuery8` 所示，有时可能想要返回序列的元素仅包含一部分源元素属性。 通过让返回序列尽可能变小，可以减少内存需求并提高执行查询的速度。 在 `select` 子句中创建匿名类型并使用对象初始值设定项通过源元素中的相应属性初始化该类型可以完成此操作。 有关如何执行此操作的示例，请参阅[对象和集合初始值设定项](../../programming-guide/classes-and-structs/object-and-collection-initializers.md)。

## <a name="remarks"></a>备注

在编译时，`select` 子句被转换为 <xref:System.Linq.Enumerable.Select%2A> 标准查询运算符的方法调用。

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [查询关键字 (LINQ)](query-keywords.md)
- [from 子句](from-clause.md)
- [分部（方法）（C# 参考）](partial-method.md)
- [匿名类型](../../programming-guide/classes-and-structs/anonymous-types.md)
- [C# 中的 LINQ](../../linq/index.md)
- [语言集成查询 (LINQ)](../../programming-guide/concepts/linq/index.md)
