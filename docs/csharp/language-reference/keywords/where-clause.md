---
description: where 子句 - C# 参考
title: where 子句 - C# 参考
ms.date: 07/20/2015
f1_keywords:
- whereclause_CSharpKeyword
helpviewer_keywords:
- where keyword [C#]
- where clause [C#]
ms.assetid: 7f9bf952-7744-4f91-b676-cddb55d107c3
ms.openlocfilehash: 58a8dc226bb2720b6a8251f028712a80f74e893c
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "89141681"
---
# <a name="where-clause-c-reference"></a>where 子句（C# 参考）

`where` 子句用在查询表达式中，用于指定将在查询表达式中返回数据源中的哪些元素。 它将一个布尔条件（谓词）应用于每个源元素（由范围变量引用），并返回满足指定条件的元素。 一个查询表达式可以包含多个 `where` 子句，一个子句可以包含多个谓词子表达式。

## <a name="example"></a>示例

在下面的示例中，`where` 子句筛选出除小于五的数字外的所有数字。 如果删除 `where` 子句，则会返回数据源中的所有数字。 表达式 `num < 5` 是应用于每个元素的谓词。

[!code-csharp[cscsrefQueryKeywords#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/Where.cs#5)]

## <a name="example"></a>示例

在单个 `where` 子句中，可以使用 [&&](../operators/boolean-logical-operators.md#conditional-logical-and-operator-) 和 [&#124;&#124;](../operators/boolean-logical-operators.md#conditional-logical-or-operator-) 运算符根据需要指定任意多个谓词。 在下面的示例中，查询将指定两个谓词，以便只选择小于五的偶数。

[!code-csharp[cscsrefQueryKeywords#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/Where.cs#6)]  

## <a name="example"></a>示例

一个 `where` 子句可以包含一个或多个返回布尔值的方法。 在下面的示例中，`where` 子句使用一种方法来确定范围变量的当前值是偶数还是奇数。

[!code-csharp[cscsrefQueryKeywords#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/Where.cs#7)]

## <a name="remarks"></a>备注

`where` 子句是一种筛选机制。 除了不能是第一个或最后一个子句外，它几乎可以放在查询表达式中的任何位置。 `where` 子句可以出现在 [group](group-clause.md) 子句的前面或后面，具体取决于时必须在对源元素进行分组之前还是之后来筛选源元素。

如果指定的谓词对于数据源中的元素无效，则会发生编译时错误。 这是 LINQ 提供的强类型检查的一个优点。

在编译时，`where` 关键字将转换为对 <xref:System.Linq.Enumerable.Where%2A> 标准查询运算符方法的调用。

## <a name="see-also"></a>另请参阅

- [查询关键字 (LINQ)](query-keywords.md)
- [from 子句](from-clause.md)
- [select 子句](select-clause.md)
- [筛选数据](../../programming-guide/concepts/linq/filtering-data.md)
- [C# 中的 LINQ](../../linq/index.md)
- [语言集成查询 (LINQ)](../../programming-guide/concepts/linq/index.md)
