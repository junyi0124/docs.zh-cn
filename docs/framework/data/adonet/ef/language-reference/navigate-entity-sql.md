---
title: NAVIGATE (Entity SQL)
ms.date: 03/30/2017
ms.assetid: f107f29d-005f-4e39-a898-17f163abb1d0
ms.openlocfilehash: 09128a367a02e1f9b206a9cc068987166c76381b
ms.sourcegitcommit: 628e8147ca10187488e6407dab4c4e6ebe0cac47
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/15/2019
ms.locfileid: "72319541"
---
# <a name="navigate-entity-sql"></a>NAVIGATE (Entity SQL)

导航实体之间建立的关系。

## <a name="syntax"></a>语法

```sql
navigate(instance-expression, [relationship-type], [to-end [, from-end] ])
```

## <a name="arguments"></a>自变量

`instance-expression` 实体的实例。

从概念架构定义语言（CSDL）文件 `relationship-type` 关系的类型名称。 @No__t_0 被限定为 \<namespace >。\<relationship 类型名称 >。

`to` 关系的结束。

`from` 关系的开头。

## <a name="return-value"></a>返回值

如果结束端的基数为 1，返回值将为 `Ref<T>`。 如果结束端的基数为 n，返回值将为 `Collection<Ref<T>>`。

## <a name="remarks"></a>备注

关系是实体数据模型（EDM）中的第一类构造。 可以在两个或更多实体类型之间建立关系，用户可以通过关系从一端（实体）导航到另一端。 当关系中的名称解析没有歧义时，`from` 和 `to` 为有条件可选。

NAVIGATE 在 O 和 C 空间中有效。

导航构造的常规形式如下：

navigate(`instance-expression`, `relationship-type`, [ `to-end` [, `from-end` ] ] )

例如:

```sql
Select o.Id, navigate(o, OrderCustomer, Customer, Order)
From LOB.Orders as o
```

其中 OrderCustomer 为 `relationship`，Customer 和 Order 为关系的 `to-end` （客户）和 `from-end` （订单）。 如果 OrderCustomer 是 n：1关系，那么导航表达式的结果类型为 Ref \<Customer >。

此表达式的简单形式如下：

```sql
Select o.Id, navigate(o, OrderCustomer)
From LOB.Orders as o
```

同样，在以下形式的查询中，导航表达式会生成一个集合 < 引用 \<Order > >。

```sql
Select c.Id, navigate(c, OrderCustomer, Order, Customer)
From LOB.Customers as c
```

实例表达式必须为 entity/ref 类型。

## <a name="example"></a>示例

下面的 Entity SQL 查询使用 NAVIGATE 运算符来导航建立在 Address 和 SalesOrderHeader 实体类型之间的关系。 此查询基于 AdventureWorks 销售模型。 若要编译并运行此查询，请执行下列步骤：

1. 执行 [How to: Execute a Query that Returns StructuralType Results](../how-to-execute-a-query-that-returns-structuraltype-results.md)中的过程。

2. 将以下查询作为参数传递给 `ExecuteStructuralTypeQuery` 方法：

 [!code-sql[DP EntityServices Concepts#NAVIGATE](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#navigate)]

## <a name="see-also"></a>请参阅

- [实体 SQL 引用](entity-sql-reference.md)
- [如何：利用导航运算符导航关系](navigate-entity-sql.md)
