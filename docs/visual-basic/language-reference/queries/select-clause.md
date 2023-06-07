---
title: Select 子句
ms.date: 07/20/2015
f1_keywords:
- vb.QuerySelect
helpviewer_keywords:
- Select statement [Visual Basic]
- Select clause [Visual Basic]
- queries [Visual Basic], Select
ms.assetid: 27a3f61c-5960-4692-9b91-4d0c4b6178fe
ms.openlocfilehash: d96423efbee075a7ad257df72471c71e38e09b63
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90875745"
---
# <a name="select-clause-visual-basic"></a>Select 子句 (Visual Basic)

定义查询的结果。  
  
## <a name="syntax"></a>语法  
  
```vb  
Select [ var1 = ] fieldName1 [, [ var2 = ] fieldName2 [...] ]  
```  
  
## <a name="parts"></a>组成部分  

 `var1`  
 可选。 可用于引用列表达式的结果的别名。  
  
 `fieldName1`  
 必需。 要在查询结果中返回的字段的名称。  
  
## <a name="remarks"></a>备注  

 您可以使用 `Select` 子句来定义要从查询返回的结果。 这使您可以定义由查询创建的新匿名类型的成员，或指定查询所返回的命名类型的成员。 `Select`查询不需要子句。 如果未 `Select` 指定子句，则查询将基于为当前作用域标识的范围变量的所有成员返回一个类型。 有关详细信息，请参阅[匿名类型](../../programming-guide/language-features/objects-and-classes/anonymous-types.md)。 当查询创建命名类型时，它将返回类型为的结果， <xref:System.Collections.Generic.IEnumerable%601> 其中 `T` 是创建的类型。  
  
 `Select`子句可以引用当前范围内的任何变量。 这包括 `From` 子句中)  (或子句中标识的范围变量 `From` 。 它还包括使用别名创建的任何新变量、、 `Aggregate` `Let` `Group By` 或 `Group Join` 子句，或者 `Select` 查询表达式中上一个子句中的变量。 `Select`子句还可以包括静态值。 例如，下面的代码示例演示一个查询表达式，其中子句将 `Select` 查询结果定义为具有四个成员的新匿名类型： `ProductName` 、 `Price` 、 `Discount` 和 `DiscountedPrice` 。 `ProductName`和 `Price` 成员值取自子句中定义的产品范围变量 `From` 。 `DiscountedPrice`成员值在子句中计算 `Let` 。 该 `Discount` 成员是一个静态值。  
  
 [!code-vb[VbSimpleQuerySamples#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#27)]  
  
 `Select`子句为后面的查询子句引入了一组新的范围变量，并且以前的范围变量不再位于范围内。 `Select`查询表达式中的最后一个子句确定查询的返回值。 例如，以下查询将返回总计超过500的每个客户订单的公司名称和订单 ID。 第一个 `Select` 子句标识 `Where` 子句和第二个子句的范围变量 `Select` 。 第二个 `Select` 子句将查询返回的值标识为新的匿名类型。  
  
 [!code-vb[VbSimpleQuerySamples#28](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#28)]  
  
 如果 `Select` 子句标识要返回的单个项，则查询表达式返回该项的类型的集合。 如果 `Select` 子句标识多个要返回的项，则查询表达式将返回基于选定项的新匿名类型的集合。 例如，下面两个查询基于子句返回两个不同类型的集合 `Select` 。 第一个查询以字符串的形式返回公司名称的集合。 第二个查询返回 `Customer` 使用公司名称和地址信息填充的对象集合。  
  
 [!code-vb[VbSimpleQuerySamples#29](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#29)]  
  
## <a name="example"></a>示例  

 下面的查询表达式使用 `From` 子句为集合声明范围变量 `cust` `customers` 。 `Select`子句选择客户名称和 ID 值，并填充 `CompanyName` `CustomerID` 新范围变量的和列。 `For Each`语句在每个返回的对象上循环，并显示 `CompanyName` `CustomerID` 每个记录的和列。  
  
 [!code-vb[VbSimpleQuerySamples#30](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#30)]  
  
## <a name="see-also"></a>另请参阅

- [Visual Basic 中的 LINQ 简介](../../programming-guide/language-features/linq/introduction-to-linq.md)
- [查询](index.md)
- [From 子句](from-clause.md)
- [Where 子句](where-clause.md)
- [Order By 子句](order-by-clause.md)
- [匿名类型](../../programming-guide/language-features/objects-and-classes/anonymous-types.md)
