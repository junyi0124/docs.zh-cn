---
title: Take While 子句
ms.date: 07/20/2015
f1_keywords:
- vb.QueryTakeWhile
helpviewer_keywords:
- queries [Visual Basic], Take While
- Take While clause [Visual Basic]
- Take While statement [Visual Basic]
ms.assetid: db8f9f2f-fc9f-4a6c-b0b8-1bf048147e11
ms.openlocfilehash: 632e9e2195f21a3aa1d1ffd28e9838905c471156
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90869658"
---
# <a name="take-while-clause-visual-basic"></a>Take While 子句 (Visual Basic)

包括集合中指定条件为 `true` 的任何元素，并绕过剩余元素。  
  
## <a name="syntax"></a>语法  
  
```vb  
Take While expression  
```  
  
## <a name="parts"></a>组成部分  
  
|术语|定义|  
|---|---|  
|`expression`|必需。 表示要为其测试元素的条件的表达式。 表达式必须返回一个 `Boolean` 值或函数等效项，如 `Integer` 要计算为的 `Boolean` 。|  
  
## <a name="remarks"></a>备注  

 `Take While`子句包括从查询结果的开头开始直到提供的返回的元素 `expression` `false` 。 返回后 `expression` `false` ，查询将跳过所有剩余的元素。 `expression`对于剩余的结果，将忽略。  
  
 `Take While`子句与子句的不同之处 `Where` 在于， `Where` 子句可用于包括查询中满足特定条件的所有元素。 `Take While`子句仅包含元素，直到第一次未满足条件。 `Take While`当使用排序的查询结果时，子句最有用。  
  
## <a name="example"></a>示例  

 下面的代码示例使用 `Take While` 子句检索结果，直到找到第一个不含任何订单的客户。  
  
 [!code-vb[VbSimpleQuerySamples#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#2)]  
  
## <a name="see-also"></a>另请参阅

- [Visual Basic 中的 LINQ 简介](../../programming-guide/language-features/linq/introduction-to-linq.md)
- [查询](index.md)
- [Select 子句](select-clause.md)
- [From 子句](from-clause.md)
- [Take 子句](take-clause.md)
- [Skip While 子句](skip-while-clause.md)
- [Where 子句](where-clause.md)
