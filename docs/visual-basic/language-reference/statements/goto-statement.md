---
title: GoTo 语句
ms.date: 07/20/2015
f1_keywords:
- vb.GoTo
helpviewer_keywords:
- GoTo statement [Visual Basic]
- control flow [Visual Basic], branching
- unconditional branching [Visual Basic]
- branching [Visual Basic]
- branching [Visual Basic], unconditional
- branching [Visual Basic], conditional
- conditional statements [Visual Basic], GoTo statement
- GoTo statement [Visual Basic], syntax
ms.assetid: 313274c2-8ab3-4b9c-9ba3-0fd6798e4f6d
ms.openlocfilehash: 000f6754575bcce6b2d79d85541e755219aca956
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90866620"
---
# <a name="goto-statement"></a>GoTo 语句

无条件地分支到过程中的指定行。  
  
## <a name="syntax"></a>语法  
  
```vb  
GoTo line  
```  
  
## <a name="part"></a>部件  

 `line`  
 必需。 任何行标签。  
  
## <a name="remarks"></a>备注  

 `GoTo`语句只能分支到其出现的过程中的行。 该行必须具有可引用的行标签 `GoTo` 。 有关详细信息，请参阅 [如何：标签语句](../../programming-guide/program-structure/how-to-label-statements.md)。  
  
> [!NOTE]
> `GoTo` 语句可以使代码难以阅读和维护。 请尽可能使用控制结构。 有关详细信息，请参阅 [Control Flow](../../programming-guide/language-features/control-flow/index.md)。  
  
 不能使用 `GoTo` 语句从 `For` ...、 `Next` `For Each` ... `Next` `SyncLock` `End SyncLock` `Try` `Catch` 、... ... ... ... ... ... .。。... `Finally` 、 `With` ... `End With` 或 `Using` ... `End Using` 构造中的标签。  
  
## <a name="branching-and-try-constructions"></a>分支和尝试构造  

 在 `Try` ... `Catch`...`Finally` 构造，以下规则适用于使用语句的分支 `GoTo` 。  
  
|块或区域|从外部分支|从内部分支出|  
|---------------------|-------------------------------|-------------------------------|  
|`Try` 模块|仅从 `Catch` 同一构造的块 <sup>1</sup>|仅限于整个构造之外|  
|`Catch` 模块|不允许|仅对整个构造以外的或 `Try` 相同构造<sup>1</sup>的块|  
|`Finally` 模块|不允许|不允许|  
  
 <sup>1</sup> （如果 `Try` 有 `Catch` ） .。。...`Finally` 构造嵌套在另一个块中， `Catch` 块可以 `Try` 在其自身的嵌套级别（而不是其他块）分支到块 `Try` 。 嵌套 `Try` ... `Catch`...`Finally` 构造必须完全包含在 `Try` `Catch` 它所嵌套到的构造内或块中。  
  
 下图显示了一个 `Try` 嵌套在另一个构造内的构造。 这两个构造的块中的各种分支都指示为有效或无效。  
  
 ![Try 结构分支示意图](./media/goto-statement/try-construction-branching.gif)  
  
## <a name="example"></a>示例  

 下面的示例使用 `GoTo` 语句分支到过程中的行标签。  
  
 [!code-vb[VbVbalrStatements#31](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#31)]  
  
## <a name="see-also"></a>另请参阅

- [Do...Loop 语句](do-loop-statement.md)
- [For...Next 语句](for-next-statement.md)
- [For Each...Next 语句](for-each-next-statement.md)
- [If...Then...Else 语句](if-then-else-statement.md)
- [Select...Case 语句](select-case-statement.md)
- [Try...Catch...Finally 语句](try-catch-finally-statement.md)
- [While...End While 语句](while-end-while-statement.md)
- [With...End With 语句](with-end-with-statement.md)
