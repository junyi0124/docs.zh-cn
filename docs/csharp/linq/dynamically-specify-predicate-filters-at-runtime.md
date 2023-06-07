---
title: 在运行时动态指定谓词筛选器（C# 中的 LINQ）
description: 了解如何使用 C# 中的 LINQ 在运行时动态指定谓词筛选器。
ms.date: 12/01/2016
ms.assetid: 90238470-0767-497c-916c-52d0d16845e0
ms.openlocfilehash: 314be8f98b9ff014f14bef11a1f3581eff8574b4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "61659938"
---
# <a name="dynamically-specify-predicate-filters-at-runtime"></a>在运行时动态指定谓词筛选器

在某些情况下，在运行时之前你不知道必须将多少个谓词应用于 `where` 子句中的源元素。 动态指定多个谓词筛选器的方法之一是使用 <xref:System.Linq.Enumerable.Contains%2A> 方法，如以下示例中所示。 该示例通过两种方式构造。 首先，通过对程序中提供的值进行筛选来运行项目。 然后通过使用在运行时提供的输入再次运行该项目。

## <a name="to-filter-by-using-the-contains-method"></a>使用 Contains 方法进行筛选

1. 打开一个新的控制台应用程序并将其命名为 `PredicateFilters`。

2. 从[查询对象的集合](query-a-collection-of-objects.md)复制 `StudentClass` 类，并将其粘贴到类 `Program` 下方的命名空间 `PredicateFilters`。 `StudentClass` 提供 `Student` 对象的列表。

3. 注释禁止 `StudentClass` 中的 `Main` 方法。

4. 将类 `Program` 替换为以下代码：

     [!code-csharp[csProgGuideLINQ#26](~/samples/snippets/csharp/concepts/linq/how-to-dynamically-specify-predicate-filters-at-runtime_1.cs)]

5. 将以下行添加到类 `DynamicPredicates` 中 `ids` 声明下的 `Main` 方法。

     ```csharp
     QueryById(ids);
     ```

6. 运行该项目。

7. 在控制台窗口中显示以下输出：

     Garcia: 114

     O'Donnell: 112

     Omelchenko: 111

8. 下一步是再次运行该项目，此次是通过使用在运行时输入的输入而非数组 `ids` 来执行此操作。 在 `Main` 方法中，将 `QueryByID(ids)` 更改为 `QueryByID(args)`。

9. 使用命令行参数 `122 117 120 115` 运行该项目。 运行项目时，这些值将成为 `args` 的元素，它们是 `Main` 方法的参数。

10. 在控制台窗口中显示以下输出：

     Adams: 120

     Feng: 117

     Garcia: 115

     Tucker: 122

## <a name="to-filter-by-using-a-switch-statement"></a>使用 switch 语句进行筛选

1. 可以使用 `switch` 语句在预先确定的备选查询中进行选择。 在下面的示例中，`studentQuery` 使用不同的 `where` 子句，具体取决于在运行时指定的等级级别或年。

2. 复制下面的方法，并将其粘贴到类 `DynamicPredicates`。

     [!code-csharp[csProgGuideLINQ#27](~/samples/snippets/csharp/concepts/linq//how-to-dynamically-specify-predicate-filters-at-runtime_2.cs)]

3. 在 `Main` 方法中，将对 `QueryByID` 的调用替换为以下调用，该调用将 `args` 数组中的第一个元素作为其参数发送：`QueryByYear(args[0])`。

4. 运行该项目，其命令行参数为介于 1 和 4 之间的整数值。

## <a name="see-also"></a>另请参阅

- [语言集成查询 (LINQ)](index.md)
- [where 子句](../language-reference/keywords/where-clause.md)
