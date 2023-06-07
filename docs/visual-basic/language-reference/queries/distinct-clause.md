---
title: Distinct 子句
ms.date: 07/20/2015
f1_keywords:
- vb.QueryDistinct
helpviewer_keywords:
- Distinct clause [Visual Basic]
- Distinct statement [Visual Basic]
- queries [Visual Basic], Distinct
ms.assetid: 86f42614-0d8f-4ffc-b888-ce8a37a8d36a
ms.openlocfilehash: 49eaab2e6a8c48d61518ea51ef2f644b6eb48314
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90875276"
---
# <a name="distinct-clause-visual-basic"></a>Distinct 子句 (Visual Basic)

限制当前范围变量的值，以消除后面的查询子句中的重复值。  
  
## <a name="syntax"></a>语法  
  
```vb  
Distinct  
```  
  
## <a name="remarks"></a>备注  

 您可以使用 `Distinct` 子句返回唯一项的列表。 `Distinct`子句使查询忽略重复的查询结果。 `Distinct`子句适用于子句指定的所有返回字段的重复值 `Select` 。 如果未 `Select` 指定子句，则将 `Distinct` 子句应用于子句中标识的查询的范围变量 `From` 。 如果范围变量不是不可变类型，则当该类型的所有成员均与现有查询结果匹配时，该查询将忽略查询结果。  
  
## <a name="example"></a>示例  

 下面的查询表达式将联接列表和客户订单列表。 `Distinct`包含子句是为了返回唯一客户名称和订单日期的列表。  
  
 [!code-vb[VbSimpleQuerySamples#20](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#20)]  
  
## <a name="see-also"></a>另请参阅

- [Visual Basic 中的 LINQ 简介](../../programming-guide/language-features/linq/introduction-to-linq.md)
- [查询](index.md)
- [From 子句](from-clause.md)
- [Select 子句](select-clause.md)
- [Where 子句](where-clause.md)
