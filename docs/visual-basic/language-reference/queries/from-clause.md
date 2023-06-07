---
title: From 子句
ms.date: 07/20/2015
f1_keywords:
- vb.QueryFrom
- vb.QueryFromIn
- vb.QueryFromLet
helpviewer_keywords:
- queries [Visual Basic], From
- From clause [Visual Basic]
- From statement [Visual Basic]
ms.assetid: 83e3665e-68a0-4540-a3a3-3d777a0f95d5
ms.openlocfilehash: 120ba6da11bffc3a0e81873d1fd606633724723d
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90875258"
---
# <a name="from-clause-visual-basic"></a>From 子句 (Visual Basic)

指定一个或多个范围变量以及要查询的集合。  
  
## <a name="syntax"></a>语法  
  
```vb  
From element [ As type ] In collection [ _ ]  
  [, element2 [ As type2 ] In collection2 [, ... ] ]  
```  
  
## <a name="parts"></a>组成部分  
  
|术语|定义|  
|---|---|  
|`element`|必需。 用于循环访问集合中的元素的 *范围变量* 。 `collection`当查询循环访问时，范围变量用于引用的每个成员 `collection` 。 必须为可枚举类型。|  
|`type`|可选。 `element` 的类型。 如果未 `type` 指定，则 `element` 将从中推断出的类型 `collection` 。|  
|`collection`|必需。 引用要查询的集合。 必须为可枚举类型。|  
  
## <a name="remarks"></a>备注  

 `From`子句用于标识查询的源数据和用于引用源集合中的元素的变量。 这些变量称为 *范围变量*。 `From`对于查询，子句是必需的，除非 `Aggregate` 使用子句标识仅返回聚合结果的查询。 有关详细信息，请参阅 [Aggregate 子句](aggregate-clause.md)。  
  
 您可以 `From` 在查询中指定多个子句，以标识要联接的多个集合。 指定多个集合时，它们将被单独循环访问，如果相关，可以联接它们。 您可以使用子句隐式联接集合 `Select` ，或使用 or 子句显式联接 `Join` 集合 `Group Join` 。 作为替代方法，您可以在单个子句中指定多个范围变量和集合 `From` ，其中每个相关范围变量和集合用逗号分隔开。 下面的代码示例演示了子句的两种语法选项 `From` 。  
  
 [!code-vb[VbSimpleQuerySamples#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#21)]  
  
 `From`子句定义查询的作用域，该作用域类似于循环的作用域 `For` 。 因此， `element` 查询作用域中的每个范围变量都必须具有唯一的名称。 由于可以为查询指定多个 `From` 子句，因此后续 `From` 子句可以引用子句中的范围变量 `From` ，或者可以引用上一个子句中的范围变量 `From` 。 例如，下面的示例显示一个嵌套 `From` 子句，其中第二个子句中的集合基于第一个子句中范围变量的属性。  
  
 [!code-vb[VbSimpleQuerySamples#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#22)]  
  
 每个 `From` 子句可以后跟其他查询子句的任意组合，以优化查询。 可以通过以下方式优化查询：  
  
- 使用和子句隐式组合多个集合 `From` `Select` ，或使用 `Join` or 子句显式组合多个集合 `Group Join` 。  
  
- 使用 `Where` 子句来筛选查询结果。  
  
- 使用子句对结果进行排序 `Order By` 。  
  
- 使用子句将相似结果组合在一起 `Group By` 。  
  
- 使用 `Aggregate` 子句标识要为整个查询结果计算的聚合函数。  
  
- 使用 `Let` 子句引入一个迭代变量，该变量的值由表达式而不是集合确定。  
  
- 使用 `Distinct` 子句可忽略重复的查询结果。  
  
- 使用 `Skip` 、 `Take` 、 `Skip While` 和子句标识要返回的结果的各个部分 `Take While` 。  
  
## <a name="example"></a>示例  

 下面的查询表达式使用 `From` 子句为 `cust` 集合中的每个对象声明一个范围变量 `Customer` `customers` 。 `Where`子句使用范围变量将输出限制为指定区域的客户。 该 `For Each` 循环将在查询结果中显示每个客户的公司名称。  
  
 [!code-vb[VbSimpleQuerySamples#23](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#23)]  
  
## <a name="see-also"></a>另请参阅

- [查询](index.md)
- [Visual Basic 中的 LINQ 简介](../../programming-guide/language-features/linq/introduction-to-linq.md)
- [For Each...Next 语句](../statements/for-each-next-statement.md)
- [For...Next 语句](../statements/for-next-statement.md)
- [Select 子句](select-clause.md)
- [Where 子句](where-clause.md)
- [Aggregate Clause](aggregate-clause.md)
- [Distinct 子句](distinct-clause.md)
- [Join 子句](join-clause.md)
- [Group Join 子句](group-join-clause.md)
- [Order By 子句](order-by-clause.md)
- [Let 子句](let-clause.md)
- [Skip 子句](skip-clause.md)
- [Take 子句](take-clause.md)
- [Skip While 子句](skip-while-clause.md)
- [Take While 子句](take-while-clause.md)
