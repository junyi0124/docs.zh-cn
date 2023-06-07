---
title: Return 语句
ms.date: 07/20/2015
f1_keywords:
- vb.Return
helpviewer_keywords:
- Return statement [Visual Basic], syntax
- control flow [Visual Basic], returning control to expressions
- Return statement [Visual Basic]
- expressions [Visual Basic], returning control to
ms.assetid: ac86e7f0-5a67-42c3-9834-0e0381efa3ec
ms.openlocfilehash: 3ca705409bc8233bc2562c64b8e7704f08dd7641
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90871811"
---
# <a name="return-statement-visual-basic"></a>Return 语句 (Visual Basic)

将控件返回到调用 `Function` 、 `Sub` 、 `Get` 、 `Set` 或 `Operator` 过程的代码。  
  
## <a name="syntax"></a>语法  
  
```vb  
Return  
' -or-  
Return expression  
```  
  
## <a name="part"></a>部件  

 `expression`  
 在 `Function` 、或过程中是必需的 `Get` `Operator` 。 表示要返回到调用代码的值的表达式。  
  
## <a name="remarks"></a>备注  

 在 `Sub` 或 `Set` 过程中， `Return` 语句等效于 `Exit Sub` 或 `Exit Property` 语句， `expression` 不能提供。  
  
 在 `Function` 、 `Get` 或过程中 `Operator` ， `Return` 语句必须包含 `expression` ，并且 `expression` 必须计算为可转换为过程返回类型的数据类型。 在 `Function` 或 `Get` 过程中，您还可以选择将表达式分配给过程名称以用作返回值，然后执行 `Exit Function` 或 `Exit Property` 语句。 在 `Operator` 过程中，必须使用 `Return expression` 。  
  
 可以 `Return` 在同一个过程中包含任意多个语句。  
  
> [!NOTE]
> 块中的代码在 `Finally` `Return` 遇到或块中的语句 `Try` 之后 `Catch` 、但在执行该语句之前运行 `Return` 。 `Return`语句不能包含在块中 `Finally` 。  
  
## <a name="example"></a>示例  

 下面的示例多次使用 `Return` 语句，以便在过程不需要执行任何其他操作时返回到调用代码。  
  
 [!code-vb[VbVbalrStatements#53](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#53)]  
  
## <a name="see-also"></a>另请参阅

- [Function 语句](function-statement.md)
- [Sub 语句](sub-statement.md)
- [Get 语句](get-statement.md)
- [Set 语句](set-statement.md)
- [Operator Statement](operator-statement.md)
- [Property Statement](property-statement.md)
- [Exit 语句](exit-statement.md)
- [尝试 .。。Catch .。。Finally 语句](try-catch-finally-statement.md)
