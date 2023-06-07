---
title: Where 子句
ms.date: 07/20/2015
f1_keywords:
- vb.QueryWhere
helpviewer_keywords:
- Where statement [Visual Basic]
- queries [Visual Basic], Where
- Where clause [Visual Basic]
ms.assetid: 48b5c2c5-3181-429c-8545-894296798c89
ms.openlocfilehash: 3a43554fb25592bf413525a2df109010e4868492
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90869641"
---
# <a name="where-clause-visual-basic"></a>Where 子句 (Visual Basic)

指定查询的筛选条件。  
  
## <a name="syntax"></a>语法  
  
```vb  
Where condition  
```  
  
## <a name="parts"></a>组成部分  

 `condition`  
 必需。 确定集合中当前项的值是否包含在输出集合中的表达式。 表达式的计算结果必须是值 `Boolean` 或值的等效值 `Boolean` 。 如果条件的计算结果为 `True` ，则元素将包含在查询结果中; 否则，将从查询结果中排除元素。  
  
## <a name="remarks"></a>备注  

 使用 `Where` 子句可以通过仅选择符合特定条件的元素来筛选查询数据。 其值导致 `Where` 子句计算结果的元素 `True` 包含在查询结果中，其他元素将被排除。 子句中使用的表达式的 `Where` 计算结果必须为或的 `Boolean` 等效项 `Boolean` ，例如， `False` 如果其值为零，则计算结果为。 您可以 `Where` 通过使用逻辑运算符（例如、、、 `And` `Or` `AndAlso` `OrElse` 、 `Is` 和 `IsNot` ）组合子句中的多个表达式。  
  
 默认情况下，在访问查询表达式之前，不会对其进行计算（例如，当它们在循环中进行数据绑定或循环访问时） `For` 。 因此，在 `Where` 访问查询之前，不会计算子句的值。 如果在子句中使用的查询外部存在值 `Where` ，请确保在执行查询时在子句中使用相应的值 `Where` 。 有关查询执行的详细信息，请参阅 [编写第一个 LINQ 查询](../../programming-guide/concepts/linq/writing-your-first-linq-query.md)。  
  
 可以在子句中调用函数 `Where` ，以对集合中的当前元素中的值执行计算或运算。 如果在子句中调用函数，则 `Where` 可能会导致查询在定义时立即执行，而不是在访问时执行。 有关查询执行的详细信息，请参阅 [编写第一个 LINQ 查询](../../programming-guide/concepts/linq/writing-your-first-linq-query.md)。  
  
## <a name="example"></a>示例  

 下面的查询表达式使用 `From` 子句为 `cust` 集合中的每个对象声明一个范围变量 `Customer` `customers` 。 `Where`子句使用范围变量将输出限制为指定区域的客户。 该 `For Each` 循环将在查询结果中显示每个客户的公司名称。  
  
 [!code-vb[VbSimpleQuerySamples#23](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#23)]  
  
## <a name="example"></a>示例  

 下面的示例 `And` `Or` 在子句中使用和逻辑运算符 `Where` 。  
  
 [!code-vb[VbSimpleQuerySamples#31](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#31)]  
  
## <a name="see-also"></a>另请参阅

- [Visual Basic 中的 LINQ 简介](../../programming-guide/language-features/linq/introduction-to-linq.md)
- [查询](index.md)
- [From 子句](from-clause.md)
- [Select 子句](select-clause.md)
- [For Each...Next 语句](../statements/for-each-next-statement.md)
