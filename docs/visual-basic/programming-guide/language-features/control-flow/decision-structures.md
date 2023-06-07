---
title: 决策结构
ms.date: 07/20/2015
helpviewer_keywords:
- statements [Visual Basic], control flow
- If statement [Visual Basic], decision structures
- If statement [Visual Basic], If...Then...Else
- control flow [Visual Basic], decision structures
- decision structures [Visual Basic]
- conditional statements [Visual Basic], decision structures
ms.assetid: 2e2e0895-4483-442a-b17c-26aead751ec2
ms.openlocfilehash: 79c4949cd4d5b07d1b1d666b21467bf8db41ab3d
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91095611"
---
# <a name="decision-structures-visual-basic"></a>决策结构 (Visual Basic)

Visual Basic 允许您测试条件并根据该测试的结果执行不同的操作。 对于表达式的各个值，或者在执行一系列语句时生成的各种异常，可以测试条件是 true 还是 false。  
  
 下图显示了一个决策结构，该结构用于测试条件是否为 true，并执行不同的操作，具体取决于它是 true 还是 false。  
  
 ![If ... 的流程图Then .。。Else 构造。](./media/decision-structures/if-then-else-construction.gif)  
  
## <a name="ifthenelse-construction"></a>If .。。Then .。。Else 构造  

 `If...Then...Else` 构造使你可以测试一个或多个条件并根据每个条件运行一个或多个语句。 可以通过以下方式测试条件并采取措施：  
  
- 如果条件为，则运行一个或多个语句 `True`  
  
- 如果条件为，则运行一个或多个语句 `False`  
  
- 如果条件为，则运行某些语句; 如果条件为，则运行 `True` 其他语句 `False`  
  
- 如果先前的条件为，则测试其他条件 `False`  
  
 提供所有这些可能的控制结构是 [If .。。Then .。。Else 语句](../../../language-reference/statements/if-then-else-statement.md)。 如果仅有一个测试和一个语句要运行，则可以使用单行版本。 如果有更复杂的条件和操作集，则可以使用多行版本。  
  
## <a name="selectcase-construction"></a>选择 .。。案例构造  

 使用 `Select...Case` 构造，可以计算一次表达式，并基于不同的可能值运行不同的语句集。 有关详细信息，请参阅 [Select .。。Case 语句](../../../language-reference/statements/select-case-statement.md)。  
  
## <a name="trycatchfinally-construction"></a>尝试 .。。Catch .。。最终构造  

 `Try...Catch...Finally` 构造使你可以在某个环境下运行一组语句，如果你的任何一个语句导致异常，该环境将保留控制权。 对于不同的异常，可以执行不同的操作。 您可以选择指定在退出整个构造之前运行的代码块 `Try...Catch...Finally` ，而不管发生了什么情况。 有关详细信息，请参阅 [Try...Catch...Finally 语句](../../../language-reference/statements/try-catch-finally-statement.md)。  
  
> [!NOTE]
> 对于许多控制结构，当您单击某个关键字时，将突出显示该结构中的所有关键字。 例如，单击构造时，将 `If` `If...Then...Else` `If` `Then` `ElseIf` `Else` `End If` 突出显示构造中的、、、和的所有实例。 若要移动到下一个或上一个突出显示的关键字，请按 CTRL + SHIFT + 向下键或 CTRL + SHIFT + 向上键。  
  
## <a name="see-also"></a>请参阅

- [控制流](index.md)
- [循环结构](loop-structures.md)
- [其他控件结构](other-control-structures.md)
- [嵌套的控件结构](nested-control-structures.md)
- [If 运算符](../../../language-reference/operators/if-operator.md)
