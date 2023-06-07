---
title: 使用组合键进行联接（C# 中的 LINQ）
description: 了解如何在 LINQ 中使用组合键进行联接。
ms.date: 12/01/2016
ms.assetid: da70b54d-3213-45eb-8437-fbe75cbcf935
ms.openlocfilehash: 460a52da7e0c0a47b77d4c64e76641bae9da7cd6
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "61659873"
---
# <a name="join-by-using-composite-keys"></a>使用组合键进行联接

此示例演示如何执行想要使用多个键来定义匹配的联接操作。 使用组合键来完成此操作。 以匿名类型或包含要比较值的命名类型的形式来创建组合键。 若要跨方法边界传递查询变量，请为该键使用重写 <xref:System.Object.Equals%2A> 和 <xref:System.Object.GetHashCode%2A> 的命名类型。 属性的名称以及属性出现的顺序在每个键中必须相同。

## <a name="example"></a>示例

以下示例演示如何使用组合键联接 3 个表中的数据：

```csharp
var query = from o in db.Orders
    from p in db.Products
    join d in db.OrderDetails
        on new {o.OrderID, p.ProductID} equals new {d.OrderID, d.ProductID} into details
        from d in details
        select new {o.OrderID, p.ProductID, d.UnitPrice};
```

组合键上的类型推理取决于这些键中属性的名称，以及属性出现的顺序。 如果源序列中属性的名称不同，则必须在键中分配新名称。 例如，如果 `Orders` 表和 `OrderDetails` 表为各自的列分别使用不同的名称，则可通过在匿名类型中分配相同的名称来创建组合键：

```csharp
join...on new {Name = o.CustomerName, ID = o.CustID} equals
    new {Name = d.CustName, ID = d.CustID }
```

还可以在 `group` 子句中使用组合键。

## <a name="see-also"></a>另请参阅

- [语言集成查询 (LINQ)](index.md)
- [join 子句](../language-reference/keywords/join-clause.md)
- [group 子句](../language-reference/keywords/group-clause.md)
