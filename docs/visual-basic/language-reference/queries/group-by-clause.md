---
title: Group By 子句
ms.date: 07/20/2015
f1_keywords:
- vb.QueryGroupByInto
- vb.QueryGroupBy
- vb.QueryGroupRef
- vb.QueryGroupInto
- vb.QueryGroup
helpviewer_keywords:
- queries [Visual Basic], Group By
- Group By statement [Visual Basic]
- Group By clause [Visual Basic]
ms.assetid: b1b5dcea-6654-473b-a2db-01f7e4c265d7
ms.openlocfilehash: b60f6759ada845d8eab048bceb1e47f9546ee7d0
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90869951"
---
# <a name="group-by-clause-visual-basic"></a>Group By 子句 (Visual Basic)

对查询结果的元素进行分组。 也可用于将聚合函数应用于每个组。 分组运算基于一个或多个键。  
  
## <a name="syntax"></a>语法  
  
```vb  
Group [ listField1 [, listField2 [...] ] By keyExp1 [, keyExp2 [...] ]  
  Into aggregateList  
```  
  
## <a name="parts"></a>组成部分  
  
- `listField1`, `listField2`  
  
     可选。 分组结果中将包括一个或多个显式标识字段的查询变量的一个或多个字段。 如果未指定字段，则分组结果中将包括一个或多个查询变量的所有字段。  
  
- `keyExp1`  
  
     必需。 标识键以用于确定元素所在的组的表达式。 可以指定多个键以形成组合键。  
  
- `keyExp2`  
  
     可选。 与 `keyExp1` 结合以形成组合键的一个或多个其他键。  
  
- `aggregateList`  
  
     必需。 标识如何对组进行聚合的一个或多个表达式。 若要标识分组结果的成员名称，请使用 `Group` 关键字，它可以采用以下任意一种形式：  
  
    ```vb  
    Into Group  
    ```  
  
     - 或 -  
  
    ```vb  
    Into <alias> = Group  
    ```  
  
     还可以包含聚合函数以将其应用于该组。  
  
## <a name="remarks"></a>备注  

 可以使用 `Group By` 子句来将查询的结果分解为组。 分组基于某个键或包含多个键的组合键。 与匹配的键值相关联的元素包括在同一组中。  
  
 使用 `aggregateList` 子句的 `Into` 参数和 `Group` 关键字来标识用于引用该组的成员名称。 还可以将聚合函数包括在 `Into` 子句中，以计算分组元素的值。 有关标准聚合函数的列表，请参阅 [Aggregate Clause](aggregate-clause.md)。  
  
## <a name="example"></a>示例  

 下面的代码示例基于其位置 (国家/地区) 对客户列表进行分组，并提供每个组中的客户的计数。 按国家/地区名称对结果进行排序。 按城市名称对分组结果进行排序。  
  
 [!code-vb[VbSimpleQuerySamples#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#11)]  
  
## <a name="see-also"></a>另请参阅

- [Visual Basic 中的 LINQ 简介](../../programming-guide/language-features/linq/introduction-to-linq.md)
- [查询](index.md)
- [Select 子句](select-clause.md)
- [From 子句](from-clause.md)
- [Order By 子句](order-by-clause.md)
- [Aggregate Clause](aggregate-clause.md)
- [Group Join 子句](group-join-clause.md)
