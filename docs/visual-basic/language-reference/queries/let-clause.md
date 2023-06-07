---
title: Let 子句
ms.date: 07/20/2015
f1_keywords:
- vb.QueryLet
helpviewer_keywords:
- queries [Visual Basic], Let
- Let clause [Visual Basic]
- Let statement [Visual Basic]
ms.assetid: 981aa516-16eb-4c53-b1f1-5aa3e82f316e
ms.openlocfilehash: a6ff3fc80a2b6752c61a8b8f7d4ce62b5a46baad
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90869887"
---
# <a name="let-clause-visual-basic"></a>Let 子句 (Visual Basic)

计算值并将其分配给查询中的新变量。  
  
## <a name="syntax"></a>语法  
  
```vb  
Let variable = expression [, ...]  
```  
  
## <a name="parts"></a>组成部分  
  
|术语|定义|  
|---|---|  
|`variable`|必需。 可用于引用所提供表达式的结果的别名。|  
|`expression`|必需。 将进行计算并分配给指定变量的表达式。|  
  
## <a name="remarks"></a>备注  

 `Let`使用子句可以计算每个查询结果的值，并使用别名引用这些值。 别名可用于其他子句，如 `Where` 子句。 `Let`使用子句可以创建更易于阅读的查询语句，因为您可以为查询中包括的 expression 子句指定一个别名，并在每次使用 expression 子句时替换该别名。  
  
 可以在子句中包含任意数量的 `variable` 和 `expression` 赋值 `Let` 。 用逗号分隔每个分配 (，) 。  
  
## <a name="example"></a>示例  

 下面的代码示例使用 `Let` 子句来计算产品10% 的折扣。  
  
 [!code-vb[VbSimpleQuerySamples#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#16)]  
  
## <a name="see-also"></a>另请参阅

- [Visual Basic 中的 LINQ 简介](../../programming-guide/language-features/linq/introduction-to-linq.md)
- [查询](index.md)
- [Select 子句](select-clause.md)
- [From 子句](from-clause.md)
- [Where 子句](where-clause.md)
