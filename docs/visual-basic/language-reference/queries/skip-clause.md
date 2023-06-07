---
title: Skip 子句
ms.date: 07/20/2015
f1_keywords:
- vb.QuerySkip
helpviewer_keywords:
- queries [Visual Basic], Skip
- Skip statement [Visual Basic]
- Skip clause [Visual Basic]
ms.assetid: f00eb172-3907-4c43-9745-d8546ab86234
ms.openlocfilehash: 40e89160baf663f7d6785e5d3e09ad6cc4eefbde
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90866306"
---
# <a name="skip-clause-visual-basic"></a>Skip 子句 (Visual Basic)

绕过集合中指定数量的元素，然后返回剩余的元素。  
  
## <a name="syntax"></a>语法  
  
```vb  
Skip count  
```  
  
## <a name="parts"></a>组成部分  

 `count`  
 必需。 值或计算结果为要跳过的序列的元素数的表达式。  
  
## <a name="remarks"></a>备注  

 `Skip`子句使查询绕过结果列表开头的元素，并返回剩余的元素。 要跳过的元素数由 `count` 参数标识。  
  
 可以将 `Skip` 子句与子句结合使用， `Take` 以便从查询的任何段返回数据范围。 为此，请将范围中第一个元素的索引传递给 `Skip` 子句，并将范围的大小传递到 `Take` 子句。  
  
 在 `Skip` 查询中使用子句时，您可能还需要确保按使 `Skip` 子句跳过预期结果的顺序返回结果。 有关对查询结果进行排序的详细信息，请参阅 [Order By 子句](order-by-clause.md)。  
  
 您可以使用 `SkipWhile` 子句来指定仅忽略某些元素，具体取决于所提供的条件。  
  
## <a name="example"></a>示例  

 下面的代码示例将 `Skip` 子句与子句一起使用 `Take` ，以从页的查询返回数据。 `GetCustomers`函数使用 `Skip` 子句跳过列表中的客户，直至提供的起始索引值，并使用 `Take` 子句返回从该索引值开始的客户页面。  
  
 [!code-vb[VbSimpleQuerySamples#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#1)]  
  
## <a name="see-also"></a>另请参阅

- [Visual Basic 中的 LINQ 简介](../../programming-guide/language-features/linq/introduction-to-linq.md)
- [查询](index.md)
- [Select 子句](select-clause.md)
- [From 子句](from-clause.md)
- [Order By 子句](order-by-clause.md)
- [Skip While 子句](skip-while-clause.md)
- [Take 子句](take-clause.md)
