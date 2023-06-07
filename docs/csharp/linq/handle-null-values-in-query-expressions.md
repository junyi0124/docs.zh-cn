---
title: 在查询表达式中处理 NULL 值（C# 中的 LINQ）
description: 了解如何在 C# 中的 LINQ 查询表达式中处理 NULL 值。
ms.date: 12/01/2016
ms.assetid: ac63ae8b-724d-4251-9334-528f4e884ae7
ms.openlocfilehash: 3da490b72bd518df7be8c14b34655af8c6f84929
ms.sourcegitcommit: 99b153b93bf94d0fecf7c7bcecb58ac424dfa47c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2020
ms.locfileid: "80249300"
---
# <a name="handle-null-values-in-query-expressions"></a>在查询表达式中处理 null 值

此示例显示如何在源集合中处理可能的 null 值。 <xref:System.Collections.Generic.IEnumerable%601> 等对象集合可包含值为 [null](../language-reference/keywords/null.md) 的元素。 如果源集合为 null 或包含值为 null 的元素，并且查询不处理 null 值，则在执行查询时将引发 <xref:System.NullReferenceException>。

## <a name="example"></a>示例

可采用防御方式进行编码，以避免空引用异常，如以下示例所示：

[!code-csharp[csProgGuideLINQ#82](~/samples/snippets/csharp/concepts/linq/how-to-handle-null-values-in-query-expressions_1.cs)]

在前面的示例中，`where` 子句筛选出类别序列中的所有 null 元素。 此方法独立于 join 子句中的 null 检查。 在此示例中，带有 null 的条件表达式有效，因为 `Products.CategoryID` 的类型为 `int?`，这是 `Nullable<int>` 的速记形式。

## <a name="example"></a>示例

在 join 子句中，如果只有一个比较键是可以为 null 的值类型，则可以在查询表达式中将另一个比较键转换为可以为 null 的值类型。 在以下示例中，假定 `EmployeeID` 是包含 `int?` 类型的值列：

[!code-csharp[csProgGuideLINQ#83](~/samples/snippets/csharp/concepts/linq/how-to-handle-null-values-in-query-expressions_2.cs)]

## <a name="see-also"></a>请参阅

- <xref:System.Nullable%601>
- [语言集成查询 (LINQ)](index.md)
- [可以为 null 的值类型](../language-reference/builtin-types/nullable-value-types.md)
