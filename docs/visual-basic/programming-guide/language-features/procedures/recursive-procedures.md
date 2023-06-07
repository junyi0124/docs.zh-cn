---
title: 递归过程
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic code, procedures
- procedures [Visual Basic], that call themselves
- procedures [Visual Basic], recursive
- procedures [Visual Basic], calling
- recursive procedures
- functions [Visual Basic], calling recursively
- recursion
ms.assetid: ba1d3962-b4c3-48d3-875e-96fdb4198327
ms.openlocfilehash: 646d4e29ed7a0b6367d4b35a7f8641bcf659e616
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352559"
---
# <a name="recursive-procedures-visual-basic"></a>递归过程 (Visual Basic)

*递归*过程是指调用自身的过程。 通常，这并不是编写 Visual Basic 代码的最有效方法。  
  
 下面的过程使用递归来计算其原始参数的阶乘。  
  
 [!code-vb[VbVbcnProcedures#51](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#51)]  
  
## <a name="considerations-with-recursive-procedures"></a>递归过程的注意事项

 **限制条件**。 必须设计一个递归过程来测试至少一个可以终止递归的条件，并且还必须处理在合理的递归调用中不满足此类条件的情况。 如果至少有一种情况不会失败，则您的过程会在无限循环中运行。

 **内存使用率**。 应用程序的本地变量空间量有限。 当过程每次调用自身时，它会使用更多的空间来获取其局部变量的其他副本。 如果此过程无限期继续，则最终会导致 <xref:System.StackOverflowException> 错误。

 **效率**。 几乎始终可以将递归替换为循环。 循环不会产生传递参数的开销、初始化附加存储以及返回值。 如果没有递归调用，性能可能会更好。

 **相互递归**。 如果两个过程相互调用，你可能会发现性能非常差，甚至会出现无限循环。 此类设计与单个递归过程表现出相同的问题，但可以更难检测和调试。

 **调用括号**。 如果 `Function` 过程以递归方式调用自身，则必须在过程名称后面加上括号，即使没有参数列表也是如此。 否则，函数名称将采用表示函数的返回值的形式。

 **测试**。 如果编写递归过程，应仔细测试该过程，以确保它始终满足某些限制条件。 你还应确保不会因为有太多递归调用而耗尽内存。

## <a name="see-also"></a>另请参阅

- <xref:System.StackOverflowException>
- [过程](index.md)
- [Sub 过程](sub-procedures.md)
- [Function 过程](function-procedures.md)
- [属性过程](property-procedures.md)
- [运算符过程](operator-procedures.md)
- [过程参数和自变量](procedure-parameters-and-arguments.md)
- [过程重载](procedure-overloading.md)
- [过程疑难解答](troubleshooting-procedures.md)
- [循环结构](../control-flow/loop-structures.md)
