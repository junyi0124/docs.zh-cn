---
title: Skip While 子句
ms.date: 07/20/2015
f1_keywords:
- vb.QuerySkipWhile
helpviewer_keywords:
- Skip While statement [Visual Basic]
- Skip While clause [Visual Basic]
- queries [Visual Basic], Skip While
ms.assetid: 5dee8350-7520-4f1a-b00d-590cacd572d6
ms.openlocfilehash: af722f7aee021f244b411cdc61619b7de3c20607
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90866231"
---
# <a name="skip-while-clause-visual-basic"></a>Skip While 子句 (Visual Basic)

跳过集合中指定条件为 `true` 的任何元素，然后返回剩余元素。  
  
## <a name="syntax"></a>语法  
  
```vb  
Skip While expression  
```  
  
## <a name="parts"></a>组成部分  
  
|术语|定义|  
|---|---|  
|`expression`|必需。 表示要为其测试元素的条件的表达式。 表达式必须返回一个 `Boolean` 值或函数等效项，如 `Integer` 要计算为的 `Boolean` 。|  
  
## <a name="remarks"></a>备注  

 `Skip While`子句会绕过查询结果开头的元素，直到提供的 `expression` 返回 `false` 。 `expression`返回后 `false` ，该查询将返回所有剩余元素。 `expression`对于剩余的结果，将忽略。  
  
 `Skip While`子句与子句的不同之处 `Where` 在于， `Where` 子句可用于从查询中排除不满足特定条件的所有元素。 `Skip While`子句仅排除元素，直到第一次未满足条件。 `Skip While`当使用排序的查询结果时，子句最有用。  
  
 您可以使用子句从查询结果的开头跳过特定数目的结果 `Skip` 。  
  
## <a name="example"></a>示例  

 下面的代码示例使用 `Skip While` 子句跳过结果，直到找到美国中的第一个客户。  
  
 [!code-vb[VbSimpleQuerySamples#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#3)]  
  
## <a name="see-also"></a>另请参阅

- [Visual Basic 中的 LINQ 简介](../../programming-guide/language-features/linq/introduction-to-linq.md)
- [查询](index.md)
- [Select 子句](select-clause.md)
- [From 子句](from-clause.md)
- [Skip 子句](skip-clause.md)
- [Take While 子句](take-while-clause.md)
- [Where 子句](where-clause.md)
