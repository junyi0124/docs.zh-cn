---
title: While...End While 语句
ms.date: 07/20/2015
f1_keywords:
- vb.While
- vb.While...EndWhile
helpviewer_keywords:
- While statement [Visual Basic], While...End While
- While statement [Visual Basic]
- While...End While statements [Visual Basic]
ms.assetid: b931d1ce-e8ed-44d8-a13d-92a4f5458a1e
ms.openlocfilehash: e3ab95f43e101a9ad8abe6fa61b94ae7542e409c
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90869477"
---
# <a name="whileend-while-statement-visual-basic"></a>While...End While 语句 (Visual Basic)

只要给定条件为，则运行一系列语句 `True` 。  
  
## <a name="syntax"></a>语法  
  
```vb  
While condition  
    [ statements ]  
    [ Continue While ]  
    [ statements ]  
    [ Exit While ]  
    [ statements ]  
End While  
```  
  
## <a name="parts"></a>组成部分  
  
|术语|定义|  
|---|---|  
|`condition`|必需。 `Boolean` 表达式。 如果 `condition` 为 `Nothing` ，则 Visual Basic 将其视为 `False` 。|  
|`statements`|可选。 后面的一个或多个语句 `While` ，每次运行 `condition` 都是 `True` 。|  
|`Continue While`|可选。 将控制转移到块的下一次迭代 `While` 。|  
|`Exit While`|可选。 将控制转移到 `While` 块。|  
|`End While`|必需。 终止 `While` 块的定义。|  
  
## <a name="remarks"></a>备注  

 `While...End While`如果您想要重复一组不确定次数的语句，只要存在条件，就可以使用结构 `True` 。 如果你想要更灵活地对条件进行测试或测试的结果，你可能更倾向 [于 .。。循环语句](do-loop-statement.md)。 如果要将语句重复一组次数 [，则下一条语句](for-next-statement.md) 通常是更好的选择。  
  
> [!NOTE]
> `While`关键字还用于[Do .。。Loop 语句](do-loop-statement.md)、 [Skip while 子句](../queries/skip-while-clause.md)和[Take while 子句](../queries/take-while-clause.md)。  
  
 如果 `condition` 为 `True` ，则在 `statements` 遇到语句之前，所有运行 `End While` 。 控件会返回到 `While` 语句，并 `condition` 再次选中。 如果 `condition` 仍为 `True` ，则将重复该过程。 如果是 `False` ，控制将传递到语句后面的语句 `End While` 。  
  
 `While`语句在开始循环之前始终检查条件。 当条件保留时，循环将继续 `True` 。 如果 `condition` 是 `False` 第一次进入循环，则它不会运行一次。  
  
 `condition`通常，比较两个值，但它可以是计算结果为[布尔数据类型](../data-types/boolean-data-type.md)值 (或) 的任何表达式 `True` `False` 。 此表达式可以包含已转换为的另一种数据类型的值，例如，数值类型 `Boolean` 。  
  
 可以 `While` 通过在另一个循环中放置循环来嵌套循环。 您还可以将不同种类的控制结构相互嵌套。 有关详细信息，请参阅 [嵌套控制结构](../../programming-guide/language-features/control-flow/nested-control-structures.md)。  
  
## <a name="exit-while"></a>退出时间  

 [Exit While](exit-statement.md)语句可以提供另一种退出循环的方法 `While` 。 `Exit While` 立即将控制转移到语句后面的语句 `End While` 。  
  
 `Exit While` (例如，在结构) 中计算某些条件后，通常使用 `If...Then...Else` 。 如果检测到可能导致不必要或无法继续迭代的条件（如错误值或终止请求），则可能需要退出循环。 `Exit While`当您测试可能导致*无限循环*的情况时，可以使用，这是一个循环，该循环可以运行极大规模甚至无限次。 然后，可以使用 `Exit While` 来转义循环。  
  
 可以将任意数量的 `Exit While` 语句置于循环中的任意位置 `While` 。  
  
 在嵌套循环内使用时 `While` ， `Exit While` 将控制转移出最内层循环，并将其转移到下一个更高的嵌套级别。  
  
 `Continue While`语句会立即将控制转移到循环的下一次迭代。 有关详细信息，请参阅 [Continue 语句](continue-statement.md)。  
  
## <a name="example"></a>示例  

 在下面的示例中，循环中的语句将继续运行，直到 `index` 变量大于10。  
  
 [!code-vb[VbVbalrStatements#171](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class14.vb#171)]  
  
## <a name="example"></a>示例  

 下面的示例演示如何使用 `Continue While` 和 `Exit While` 语句。  
  
 [!code-vb[VbVbalrStatements#172](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class14.vb#172)]  
  
## <a name="example"></a>示例  

 下面的示例读取文本文件中的所有行。 <xref:System.IO.File.OpenText%2A>方法打开文件并返回 <xref:System.IO.StreamReader> 读取字符的。 在 `While` 条件中，的 <xref:System.IO.StreamReader.Peek%2A> 方法 `StreamReader` 确定文件是否包含其他字符。  
  
 [!code-vb[VbVbalrStatements#173](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class14.vb#173)]  
  
## <a name="see-also"></a>另请参阅

- [循环结构](../../programming-guide/language-features/control-flow/loop-structures.md)
- [Do...Loop 语句](do-loop-statement.md)
- [For...Next 语句](for-next-statement.md)
- [布尔数据类型](../data-types/boolean-data-type.md)
- [嵌套的控件结构](../../programming-guide/language-features/control-flow/nested-control-structures.md)
- [Exit 语句](exit-statement.md)
- [Continue 语句](continue-statement.md)
