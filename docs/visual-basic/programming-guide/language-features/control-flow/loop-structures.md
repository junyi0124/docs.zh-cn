---
title: 循环结构
ms.date: 07/20/2015
helpviewer_keywords:
- control flow [Visual Basic], loops
- For keyword [Visual Basic], loop structures
- loops
- loop structures [Visual Basic]
- statements [Visual Basic], loop
- Do statement [Visual Basic], Do loops
- conditional statements [Visual Basic], loop structures
ms.assetid: ecacb09b-a4c9-42be-98b2-a15d368b5db8
ms.openlocfilehash: 5019eaf219ad70f9c667356636d05ab69fc5a187
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91077210"
---
# <a name="loop-structures-visual-basic"></a>循环结构 (Visual Basic)

Visual Basic 循环结构使您可以重复运行一个或多个代码行。 您可以重复循环结构中的语句，直到条件为、 `True` `False` 指定次数或为集合中的每个元素指定一次。  
  
 下图显示了一个循环结构，它运行一组语句，直到条件变为 true：  
  
 ![显示 ... 的流程图Until 循环。](./media/loop-structures/do-until-loop-true-condition.gif)  
  
## <a name="while-loops"></a>While 循环  

 `While` `End While` 只要语句中指定的条件为，... 构造将运行一组语句 `While` `True` 。 有关详细信息，请参阅 [While .。。End While 语句](../../../language-reference/statements/while-end-while-statement.md)。  
  
## <a name="do-loops"></a>Do 循环  

 使用 `Do` ... 构造， `Loop` 可以在循环结构的开始或结束时测试条件。 您还可以指定是否在条件保持 `True` 或变为之前重复执行循环 `True` 。 有关详细信息，请参阅 [Do .。。循环语句](../../../language-reference/statements/do-loop-statement.md)。  
  
## <a name="for-loops"></a>For 循环  

 `For`... `Next` 构造按设置的次数执行循环。 它使用循环控制变量（也称为 *计数器*）跟踪重复。 您可以指定此计数器的起始值和结束值，还可以选择指定从一个重复到下一个重复的量。 有关详细信息，请参阅 [For .。。下一语句](../../../language-reference/statements/for-next-statement.md)。  
  
## <a name="for-each-loops"></a>对于每个循环  

 `For Each`... `Next` 构造为集合中的每个元素运行一组语句。 指定循环控制变量，但不需要确定其起始值或终止值。 有关详细信息，请参阅 [For Each .。。下一语句](../../../language-reference/statements/for-each-next-statement.md)。  
  
## <a name="see-also"></a>请参阅

- [控制流](index.md)
- [决策结构](decision-structures.md)
- [其他控件结构](other-control-structures.md)
- [嵌套的控件结构](nested-control-structures.md)
