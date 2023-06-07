---
title: Take 子句
ms.date: 07/20/2015
f1_keywords:
- vb.QueryTake
helpviewer_keywords:
- Take statement [Visual Basic]
- queries [Visual Basic], Take
- Take clause [Visual Basic]
ms.assetid: 77bf87b2-1476-4456-957f-fee922fbad8c
ms.openlocfilehash: f2377d8d1635912885a310b2b0429a6a00083b47
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90869668"
---
# <a name="take-clause-visual-basic"></a>Take 子句 (Visual Basic)

从集合的开头返回指定数量的连续元素。  
  
## <a name="syntax"></a>语法  
  
```vb  
Take count  
```  
  
## <a name="parts"></a>组成部分  

 `count`  
 必需。 值或计算结果为要返回的序列的元素数的表达式。  
  
## <a name="remarks"></a>备注  

 `Take`子句使查询包括从结果列表的开头开始的指定数量的连续元素。 要包括的元素数是由参数指定的 `count` 。  
  
 可以将 `Take` 子句与子句结合使用， `Skip` 以便从查询的任何段返回数据范围。 为此，请将范围中第一个元素的索引传递给 `Skip` 子句，并将范围的大小传递到 `Take` 子句。 在这种情况下， `Take` 子句必须在子句之后指定 `Skip` 。  
  
 在 `Take` 查询中使用子句时，您可能还需要确保按使 `Take` 子句包含预期结果的顺序返回结果。 有关对查询结果进行排序的详细信息，请参阅 [Order By 子句](order-by-clause.md)。  
  
 您可以使用 `TakeWhile` 子句指定仅返回某些元素，具体取决于所提供的条件。  
  
## <a name="example"></a>示例  

 下面的代码示例将 `Take` 子句与子句一起使用 `Skip` ，以从页的查询返回数据。 GetCustomers 函数使用 `Skip` 子句跳过列表中的客户，直至提供的起始索引值，并使用 `Take` 子句返回从该索引值开始的客户页面。  
  
 [!code-vb[VbSimpleQuerySamples#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#1)]  
  
## <a name="see-also"></a>另请参阅

- [Visual Basic 中的 LINQ 简介](../../programming-guide/language-features/linq/introduction-to-linq.md)
- [查询](index.md)
- [Select 子句](select-clause.md)
- [From 子句](from-clause.md)
- [Order By 子句](order-by-clause.md)
- [Take While 子句](take-while-clause.md)
- [Skip 子句](skip-clause.md)
