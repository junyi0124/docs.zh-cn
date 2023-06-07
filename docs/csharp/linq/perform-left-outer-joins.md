---
title: 执行左外部联接（C# 中的 LINQ）
description: 了解如何使用 C# 中的 LINQ 执行左外部联接。
ms.date: 12/01/2016
ms.assetid: f542cee6-3169-4dcf-a631-3a6a79ccd473
ms.openlocfilehash: cc08a1c8670623a10d1e0bf10221d02037a8d7bc
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "61688574"
---
# <a name="perform-left-outer-joins"></a>执行左外部联接

左外部联接是这样定义的：返回第一个集合的每个元素，无论该元素在第二个集合中是否有任何相关元素。 可以使用 LINQ 通过对分组联接的结果调用 <xref:System.Linq.Enumerable.DefaultIfEmpty%2A> 方法来执行左外部联接。

## <a name="example"></a>示例

下面的示例演示如何对分组联接的结果调用 <xref:System.Linq.Enumerable.DefaultIfEmpty%2A> 方法来执行左外部联接。

若要生成两个集合的左外部联接，第一步是使用分组联接执行内联。 （有关此过程的说明，请参阅[执行内联](perform-inner-joins.md)。）在此示例中，`Person` 对象列表基于与 `Pet.Owner` 匹配的 `Person` 对象内联到 `Pet` 对象列表。

第二步是在结果集内包含第一个（左）集合的每个元素，即使该元素在右集合中没有匹配的元素也是如此。 这是通过对分组联接中的每个匹配元素序列调用 <xref:System.Linq.Enumerable.DefaultIfEmpty%2A> 来实现的。 此示例中，对每个匹配 `Pet` 对象的序列调用 <xref:System.Linq.Enumerable.DefaultIfEmpty%2A>。 如果对于任何 `Person` 对象，匹配 `Pet` 对象序列均为空，则该方法返回一个包含单个默认值的集合，从而确保结果集合中显示每个 `Person` 对象。

> [!NOTE]
> 引用类型的默认值为 `null`；因此，该示例在访问每个 `Pet` 集合的每个元素之前会先检查是否存在空引用。

[!code-csharp[CsLINQProgJoining#7](~/samples/snippets/csharp/concepts/linq/how-to-perform-left-outer-joins_1.cs)]

## <a name="see-also"></a>请参阅

- <xref:System.Linq.Enumerable.Join%2A>
- <xref:System.Linq.Enumerable.GroupJoin%2A>
- [执行内部联接](perform-inner-joins.md)
- [执行分组联接](perform-grouped-joins.md)
- [匿名类型](../programming-guide/classes-and-structs/anonymous-types.md)
