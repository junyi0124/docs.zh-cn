---
title: GROUPPARTITION (Entity SQL)
ms.date: 03/30/2017
ms.assetid: d0482e9b-086c-451c-9dfa-ccb024a9efb6
ms.openlocfilehash: 11abebeac682fed9e3a007986bb2f5c7bdb80f16
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91204471"
---
# <a name="grouppartition-entity-sql"></a>GROUPPARTITION (Entity SQL)

返回从聚合与之相关的当前组分区提取的参数值集合。 `GroupPartition` 聚合是基于组的聚合，没有基于集合的形式。  
  
## <a name="syntax"></a>语法  
  
```sql  
GROUPPARTITION( [ALL|DISTINCT] expression )  
```  
  
## <a name="arguments"></a>参数  

 `expression`  
 任何 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 表达式。  
  
## <a name="remarks"></a>备注  

 以下查询生成产品列表以及每个产品的订单行数量的集合。  
  
```sql  
SELECT p, GroupPartition(ol.Quantity) FROM LOB.OrderLines AS ol
  GROUP BY ol.Product AS p
```  
  
 以下两个查询在语义上是相同的：  
  
```sql  
SELECT p, Sum(GroupPartition(ol.Quantity)) FROM LOB.OrderLines AS ol
  GROUP BY ol.Product AS p
SELET p, Sum(ol.Quantity) FROM LOB.OrderLines AS ol
  group by ol.Product as p  
```  
  
 `GROUPPARTITION` 运算符可以与用户定义的聚合函数结合使用。  
  
`GROUPPARTITION` 是一个特殊的聚合运算符，它保留对于分组的输入集的引用。 此引用可以用在 GROUP BY 处于作用域内的查询中的任何位置。 例如：
  
```sql  
SELECT p, GroupPartition(ol.Quantity) FROM LOB.OrderLines AS ol GROUP BY ol.Product AS p
```  
  
 对于常规 `GROUP BY` ，将隐藏分组的结果。 您只能在聚合函数中使用结果。 为了能够看到分组的结果，必须使用子查询使分组的结果与输入集相关。 下面两个查询是等效的：  
  
```sql  
SELET p, (SELECT q FROM GroupPartition(ol.Quantity) AS q) FROM LOB.OrderLines AS ol GROUP BY ol.Product AS p
SELECT p, (SELECT ol.Quantity AS q FROM LOB.OrderLines AS ol2 WHERE ol2.Product = p) FROM LOB.OrderLines AS ol GROUP BY ol.Product AS p
```  
  
 如示例中所示，通过 GROUPPARTITION 聚合运算符，可以在分组后更轻松地获得对输入集的引用。  
  
 当您使用 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 参数时，GROUPPARTITION 运算符可以在运算符输入中指定任何 `expression` 表达式。  
  
 例如，下面针对组分区的所有输入表达式都是有效的：  
  
```sql  
SELECT groupkey, GroupPartition(b) FROM {1,2,3} AS a INNER JOIN {4,5,6} AS b ON true GROUP BY a AS groupkey
SELECT groupkey, GroupPartition(1) FROM {1,2,3} AS a INNER JOIN {4,5,6} AS b ON true GROUP BY a AS groupkey
SELECT groupkey, GroupPartition(a + b) FROM {1,2,3} AS a INNER JOIN {4,5,6} AS b ON true GROUP BY a AS groupkey
SELECT groupkey, GroupPartition({a + b}) FROM {1,2,3} AS a INNER JOIN {4,5,6} AS b ON true GROUP BY a AS groupkey  
SELECT groupkey, GroupPartition({42}) FROM {1,2,3} AS a INNER JOIN {4,5,6} AS b ON true GROUP BY a AS groupkey  
SELECT groupkey, GroupPartition(b > a) FROM {1,2,3} AS a INNER JOIN {4,5,6} AS b ON true GROUP BY a AS groupkey  
```  
  
## <a name="example"></a>示例  

 下面的示例演示如何将 GROUPPARTITION 子句与 GROUP BY 子句一起使用。 GROUP BY 子句按照 `SalesOrderHeader` 实体的 `Contact`对这些实体进行分组。 然后，GROUPPARTITION 子句投影每个组的 `TotalDue` 属性，而生成一个十进制值集合。  
  
 [!code-sql[DP EntityServices Concepts#Collection_GroupPartition](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#collection_grouppartition)]
