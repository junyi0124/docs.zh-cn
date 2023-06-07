---
title: Order By 子句
ms.date: 07/20/2015
f1_keywords:
- vb.QueryOrderBy
- vb.QueryAscending
- vb.QueryDescending
helpviewer_keywords:
- queries [Visual Basic], Order By
- Order By clause [Visual Basic]
- Order By statement [Visual Basic]
ms.assetid: fa911282-6b81-44c7-acfa-46b5bb93df75
ms.openlocfilehash: 05fa720237f4b0185b5c07217362c99b5dbf4303
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90869786"
---
# <a name="order-by-clause-visual-basic"></a>Order By 子句 (Visual Basic)

指定查询结果的排序顺序。  
  
## <a name="syntax"></a>语法  
  
```vb  
Order By orderExp1 [ Ascending | Descending ] [, orderExp2 [...] ]  
```  
  
## <a name="parts"></a>组成部分  

 `orderExp1`  
 必需。 当前查询结果中的一个或多个字段，用于确定如何对返回的值进行排序。 字段名称必须用逗号分隔 (，) 。 您可以使用或关键字，将每个字段标识为按升序或降序排序 `Ascending` `Descending` 。 如果未 `Ascending` `Descending` 指定 or 关键字，则默认排序顺序为升序。 排序顺序字段的优先级从左到右。  
  
## <a name="remarks"></a>备注  

 您可以使用 `Order By` 子句对查询结果进行排序。 `Order By`子句只能根据当前作用域的范围变量对结果进行排序。 例如， `Select` 子句在查询表达式中引入了新的作用域，该作用域具有新的迭代变量。 在查询中的子句之前定义的范围变量在 `Select` 子句之后不可用 `Select` 。 因此，如果要按子句中不提供的字段对结果进行排序 `Select` ，则必须将子句放在 `Order By` `Select` 子句之前。 需要执行此操作的一个示例是，如果要按不作为结果的一部分返回的字段对查询进行排序。  
  
 字段的升序和降序由接口的数据类型的实现确定 <xref:System.IComparable> 。 如果数据类型未实现 <xref:System.IComparable> 接口，则忽略排序顺序。  
  
## <a name="example"></a>示例  

 下面的查询表达式使用 `From` 子句为集合声明范围变量 `book` `books` 。 `Order By`子句按照默认)  (按升序对查询结果进行排序。 价格相同的书籍按标题的升序排序。 `Select`子句选择 `Title` 和 `Price` 属性作为查询返回的值。  
  
 [!code-vb[VbSimpleQuerySamples#24](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#24)]  
  
## <a name="example"></a>示例  

 下面的查询表达式使用 `Order By` 子句按照价格以降序对查询结果进行排序。 价格相同的书籍按标题的升序排序。  
  
 [!code-vb[VbSimpleQuerySamples#25](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#25)]  
  
## <a name="example"></a>示例  

 下面的查询表达式使用 `Select` 子句来选择书名、价格、发布日期和作者。 然后，它将 `Title` 填充 `Price` `PublishDate` 新范围的范围变量的、、和 `Author` 字段。 `Order By`子句按作者姓名、书籍标题和价格对新范围变量进行排序。 每列按默认顺序排序 (升序) 。  
  
 [!code-vb[VbSimpleQuerySamples#26](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#26)]  
  
## <a name="see-also"></a>另请参阅

- [Visual Basic 中的 LINQ 简介](../../programming-guide/language-features/linq/introduction-to-linq.md)
- [查询](index.md)
- [Select 子句](select-clause.md)
- [From 子句](from-clause.md)
