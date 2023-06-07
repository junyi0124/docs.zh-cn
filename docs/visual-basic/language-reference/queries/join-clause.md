---
title: Join 子句
ms.date: 07/20/2015
f1_keywords:
- vb.QueryJoinIn
- vb.QueryJoin
- vb.QueryJoinOn
helpviewer_keywords:
- queries [Visual Basic], Join
- Join statement [Visual Basic]
- Join clause [Visual Basic]
ms.assetid: 6dd37936-b27c-4e00-98ad-154b23f4de64
ms.openlocfilehash: f73dc31bbbb9014a8a1a315de406c53fa58d1c65
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84359769"
---
# <a name="join-clause-visual-basic"></a>Join 子句 (Visual Basic)

将两个集合合并为单个集合。 联接运算基于匹配键，并使用 `Equals` 运算符。

## <a name="syntax"></a>语法

```vb
Join element In collection _
  [ joinClause _ ]
  [ groupJoinClause ... _ ]
On key1 Equals key2 [ And key3 Equals key4 [... ]
```

## <a name="parts"></a>组成部分

`element`（必需）。 要联接的集合的控件变量。

`collection`  
必需。 要与运算符左侧标识的集合组合的集合 `Join` 。 `Join`子句可以嵌套在另一个 `Join` 子句或 `Group Join` 子句中。

`joinClause`  
可选。 `Join`用于进一步优化查询的一个或多个其他子句。

`groupJoinClause`  
可选。 `Group Join`用于进一步优化查询的一个或多个其他子句。

`key1` `Equals` `key2`  
必需。 标识要联接的集合的键。 必须使用 `Equals` 运算符来比较要联接的集合中的键。 可以通过使用 `And` 运算符来标识多个键，从而组合联接条件。 `key1`必须是位于运算符左侧的集合中 `Join` 。 `key2`必须来自运算符右侧的集合 `Join` 。

联接条件中使用的键可以是包含集合中多个项的表达式。 但是，每个键表达式只能包含其各自集合中的项。

## <a name="remarks"></a>备注

`Join`子句组合了两个集合，这些集合基于要联接的集合中的匹配键值。 生成的集合可以包含运算符左侧标识的集合中的任何值组合 `Join` 和子句中标识的集合 `Join` 。 查询将只返回满足运算符指定的条件的结果 `Equals` 。 这等效于 `INNER JOIN` SQL 中的。

您可以 `Join` 在查询中使用多个子句，以将两个或多个集合联接到一个集合中。

您可以执行隐式联接以组合集合，而不使用 `Join` 子句。 为此，请 `In` 在子句中包含多个子句， `From` 并指定一个用于 `Where` 标识要用于联接的键的子句。

可以使用子句将 `Group Join` 集合合并为单个分层集合。 这类似于 `LEFT OUTER JOIN` SQL 中的。

## <a name="example"></a>示例

下面的代码示例将执行隐式联接，以将客户列表与订单组合在一起。

[!code-vb[VbSimpleQuerySamples#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#13)]

## <a name="example"></a>示例

下面的代码示例使用子句联接两个集合 `Join` 。

[!code-vb[VbSimpleQuerySamples#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples2.vb#12)]

此示例将生成与下面类似的输出：

`winlogon (968), Windows Logon`

`explorer (2424), File Explorer`

`cmd (5136), Command Window`

## <a name="example"></a>示例

下面的代码示例通过使用 `Join` 具有两个键列的子句来联接两个集合。

[!code-vb[VbSimpleQuerySamples#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples3.vb#17)]

该示例将生成类似于以下内容的输出：

`winlogon (968), Windows Logon, Priority = 13`

`cmd (700), Command Window, Priority = 8`

`explorer (2424), File Explorer, Priority = 8`

## <a name="see-also"></a>另请参阅

- [Visual Basic 中的 LINQ 简介](../../programming-guide/language-features/linq/introduction-to-linq.md)
- [查询](index.md)
- [Select 子句](select-clause.md)
- [From 子句](from-clause.md)
- [Group Join 子句](group-join-clause.md)
- [Where 子句](where-clause.md)
