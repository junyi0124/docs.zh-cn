---
title: Aggregate Clause
ms.date: 08/28/2018
f1_keywords:
- vb.QueryAggregateIn
- vb.QueryAggregate
- vb.QueryAggregateInto
helpviewer_keywords:
- Aggregate clause [Visual Basic]
- Aggregate statement [Visual Basic]
- queries [Visual Basic], Aggregate
ms.assetid: 1315a814-5db6-4077-b34b-b141e11cc0eb
ms.openlocfilehash: be2e401c7931b2637c14a3ea3b742a2c09917939
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90869983"
---
# <a name="aggregate-clause-visual-basic"></a>Aggregate 子句 (Visual Basic)

将一个或多个聚合函数应用于集合。  
  
## <a name="syntax"></a>语法  
  
```vb  
Aggregate element [As type] In collection _  
  [, element2 [As type2] In collection2, [...]]  
  [ clause ]  
  Into expressionList  
```  
  
## <a name="parts"></a>组成部分  
  
|术语|定义|  
|---|---|  
|`element`|必需。 用于循环访问集合中的元素的变量。|  
|`type`|可选。 `element` 的类型。 如果未指定类型， `element` 则将从中推断出的类型 `collection` 。|  
|`collection`|必需。 引用要对其执行操作的集合。|  
|`clause`|可选。 一个或多个查询子句（如 `Where` 子句），用于优化要将聚合子句或子句应用于的查询结果。|  
|`expressionList`|必需。 一个或多个以逗号分隔的表达式，用于标识要应用于集合的聚合函数。 您可以向聚合函数应用别名，以指定查询结果的成员名称。 如果未提供别名，则使用聚合函数的名称。 有关示例，请参阅本主题后面的有关聚合函数的部分。|  
  
## <a name="remarks"></a>备注  

 `Aggregate`子句可用于在查询中包括聚合函数。 聚合函数对一组值执行检查和计算，并返回单个值。 您可以使用查询结果类型的成员访问计算值。 可使用的标准聚合函数包括 `All` 、 `Any` 、、 `Average` `Count` 、、、 `LongCount` `Max` `Min` 和 `Sum` 函数。 这些函数对熟悉 SQL 中聚合的开发人员非常熟悉。 本主题的下一节将对其进行介绍。  
  
 聚合函数的结果作为查询结果类型的字段包含在查询结果中。 您可以为聚合函数结果提供别名，以指定将保存聚合值的查询结果类型的成员名称。 如果未提供别名，则使用聚合函数的名称。  
  
 `Aggregate`子句可以开始查询，也可以在查询中将其作为附加子句包括在内。 如果 `Aggregate` 子句开始查询，则结果为单个值，该值是在子句中指定的聚合函数的结果 `Into` 。 如果在子句中指定了多个聚合函数 `Into` ，则查询将返回单一类型，该类型具有一个单独的属性，用于引用子句中每个聚合函数的结果 `Into` 。 如果 `Aggregate` 子句作为附加子句包含在查询中，则查询集合中返回的类型将有一个单独的属性来引用子句中每个聚合函数的结果 `Into` 。  
  
## <a name="aggregate-functions"></a>聚合函数

下面是可以与子句一起使用的标准聚合函数 `Aggregate` 。  
  
### <a name="all"></a>全部

`true`如果集合中的所有元素都满足指定的条件，则返回; 否则返回 `false` 。 下面是一个示例：

 [!code-vb[VbSimpleQuerySamples#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#5)]

### <a name="any"></a>任意

`true`如果集合中的任何元素满足指定的条件，则返回; 否则返回 `false` 。 下面是一个示例：

 [!code-vb[VbSimpleQuerySamples#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#6)]

### <a name="average"></a>平均值

计算集合中所有元素的平均值，或为集合中的所有元素计算提供的表达式。 下面是一个示例：

 [!code-vb[VbSimpleQuerySamples#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#7)]

### <a name="count"></a>Count

计算集合中元素的数目。 您可以提供一个可选 `Boolean` 表达式，以便仅对集合中满足条件的元素数进行计数。 下面是一个示例：

 [!code-vb[VbSimpleQuerySamples#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#8)]

### <a name="group"></a>组

指作为 or 子句的结果进行分组的查询结果 `Group By` `Group Join` 。 `Group`函数仅在 `Into` 或子句的子句中有效 `Group By` `Group Join` 。 有关详细信息和示例，请参阅 [Group By 子句](group-by-clause.md) 和 [group Join 子句](group-join-clause.md)。

### <a name="longcount"></a>LongCount

计算集合中元素的数目。 您可以提供一个可选 `Boolean` 表达式，以便仅对集合中满足条件的元素数进行计数。 以的形式返回结果 `Long` 。 有关示例，请参阅 `Count` 聚合函数。

### <a name="max"></a>最大值

计算集合中的最大值，或为集合中的所有元素计算提供的表达式。 下面是一个示例：

 [!code-vb[VbSimpleQuerySamples#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#9)]

### <a name="min"></a>Min

计算集合中的最小值，或为集合中的所有元素计算提供的表达式。 下面是一个示例：

 [!code-vb[VbSimpleQuerySamples#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#10)]

### <a name="sum"></a>Sum

计算集合中所有元素的和，或为集合中的所有元素计算提供的表达式。 下面是一个示例：

 [!code-vb[VbSimpleQuerySamples#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#15)]

## <a name="example"></a>示例  

下面的示例演示如何使用子句将 `Aggregate` 聚合函数应用于查询结果。  
  
 [!code-vb[VbSimpleQuerySamples#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#4)]  
  
## <a name="creating-user-defined-aggregate-functions"></a>创建用户定义聚合函数

 您可以通过向类型添加扩展方法，在查询表达式中包含您自己的自定义聚合函数 <xref:System.Collections.Generic.IEnumerable%601> 。 然后，您的自定义方法可以对引用了聚合函数的可枚举集合执行计算或操作。 有关扩展方法的详细信息，请参阅[扩展方法](../../programming-guide/language-features/procedures/extension-methods.md)。  
  
 例如，下面的示例显示了一个自定义聚合函数，该函数计算数字集合的中间值。 扩展方法有两个重载 `Median` 。 第一次重载接受类型为的集合作为输入 `IEnumerable(Of Double)` 。 如果为 `Median` 类型的查询字段调用聚合函数 `Double` ，则将调用此方法。 方法的第二个重载 `Median` 可传递到任何泛型类型。 方法的泛型重载 `Median` 使用第二个参数，该参数引用 `Func(Of T, Double)` lambda 表达式，以便为集合中的类型 (值投影) 为类型的相应值 `Double` 。 然后，它将中间值的计算委托给该方法的其他重载 `Median` 。 有关 lambda 表达式的详细信息，请参阅 [Lambda 表达式](../../programming-guide/language-features/procedures/lambda-expressions.md)。  
  
 [!code-vb[VbSimpleQuerySamples#18](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/UserDefinedAggregates.vb#18)]  
  
 下面的示例演示了 `Median` 在类型的集合上调用聚合函数的示例查询， `Integer` 以及类型的集合 `Double` 。 对类型为的集合调用聚合函数的查询将 `Median` `Double` 调用接受的 `Median` 方法（作为输入的集合）的重载 `Double` 。 对 `Median` 类型集合调用聚合函数的查询将 `Integer` 调用方法的泛型重载 `Median` 。  
  
 [!code-vb[VbSimpleQuerySamples#19](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/UserDefinedAggregates.vb#19)]  
  
## <a name="see-also"></a>另请参阅

- [Visual Basic 中的 LINQ 简介](../../programming-guide/language-features/linq/introduction-to-linq.md)
- [查询](index.md)
- [Select 子句](select-clause.md)
- [From 子句](from-clause.md)
- [Where 子句](where-clause.md)
- [Group By 子句](group-by-clause.md)
