---
title: Do...Loop 语句
ms.date: 07/20/2015
f1_keywords:
- vb.Do
- vb.Loop
- vb.Until
helpviewer_keywords:
- conditional statements [Visual Basic], Do�Loop
- while statement [Visual Basic], Do...Loop
- execution [Visual Basic], conditional
- Do loops
- Until keyword [Visual Basic], Do...Loop statement
- Do...Loop statement
- instructions, repeating
- Do statement [Visual Basic]
- Exit statement [Visual Basic], in Do...Loop statements
- loop structures [Visual Basic], Do�Loop statements
- do-while statements [Visual Basic]
- loops, exiting
- Loop keyword [Visual Basic], Do...Loop statement
ms.assetid: 892f9096-b3e2-4aee-834d-83bc4e2c379d
ms.openlocfilehash: 86a702aefeea1e5e359a579a3f29e9c06f1c619c
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90865931"
---
# <a name="doloop-statement-visual-basic"></a>Do...Loop 语句 (Visual Basic)

当 `Boolean` 条件为 `True` 或在条件变为之前，重复语句块 `True` 。  
  
## <a name="syntax"></a>语法  
  
```vb  
Do { While | Until } condition  
    [ statements ]  
    [ Continue Do ]  
    [ statements ]  
    [ Exit Do ]  
    [ statements ]  
Loop  
' -or-  
Do  
    [ statements ]  
    [ Continue Do ]  
    [ statements ]  
    [ Exit Do ]  
    [ statements ]  
Loop { While | Until } condition  
```  
  
## <a name="parts"></a>组成部分  
  
|术语|定义|  
|---|---|  
|`Do`|必需。 开始循环的定义 `Do` 。|  
|`While`|必选项（除非使用了 `Until`）。 重复循环，直到 `condition` 为为止 `False` 。|  
|`Until`|必选项（除非使用了 `While`）。 重复循环，直到 `condition` 为为止 `True` 。|  
|`condition`|可选。 `Boolean` 表达式。 如果 `condition` 为 `Nothing` ，则 Visual Basic 将其视为 `False` 。|  
|`statements`|可选。 一个或多个重复的语句，或在之前重复 `condition` `True` 。|  
|`Continue Do`|可选。 将控制转移到循环的下一次迭代 `Do` 。|  
|`Exit Do`|可选。 将控制转移到 `Do` 循环外。|  
|`Loop`|必需。 终止循环的定义 `Do` 。|  
  
## <a name="remarks"></a>备注  

 `Do...Loop`如果希望在满足条件之前重复一组语句，请使用结构。 如果要将语句重复一组次数 [，则下一条语句](for-next-statement.md) 通常是更好的选择。  
  
 您可以使用 `While` 或 `Until` 来指定 `condition` ，但不能同时使用两者。  
  
 只能 `condition` 在循环的开头或结尾测试一次。 如果在 `condition` 语句) 中测试循环 (`Do` ，则循环可能不会运行一次。 如果在语句) 中测试循环 (`Loop` ，则循环始终运行至少一次。  
  
 这种情况通常是由两个值比较导致的，但它可以是计算结果为 [布尔数据类型](../data-types/boolean-data-type.md) 值 (或) 的任何表达式 `True` `False` 。 这包括已转换为的其他数据类型（如数值类型）的值 `Boolean` 。  
  
 可以 `Do` 通过在另一个循环中放置循环来嵌套循环。 您还可以在彼此之间嵌套不同种类的控制结构。 有关详细信息，请参阅 [嵌套控制结构](../../programming-guide/language-features/control-flow/nested-control-structures.md)。  
  
> [!NOTE]
> 此 `Do...Loop` 结构提供的灵活性比 [.。。End While 语句](while-end-while-statement.md) ，因为它可用于决定是在 `condition` 停止时 `True` 还是在第一次变为时结束循环 `True` 。 它还使你能够 `condition` 在循环的开头或结尾进行测试。  
  
## <a name="exit-do"></a>退出 Do  

 [Exit Do](exit-statement.md)语句可以提供退出的替代方法 `Do…Loop` 。 `Exit Do` 将控制立即传输到语句后面的语句 `Loop` 。  
  
 `Exit Do` 通常在计算某些条件后（例如在结构中）使用 `If...Then...Else` 。 如果检测到可能导致不必要或无法继续迭代的条件（如错误值或终止请求），则可能需要退出循环。 的一种用途 `Exit Do` 是测试可能导致 *无限循环*的情况，这是一个可运行很大甚至无限次数的循环。 您可以使用 `Exit Do` 来转义循环。  
  
 可以在中的任意位置包含任意数量的 `Exit Do` 语句 `Do…Loop` 。  
  
 在嵌套循环内使用时 `Do` ， `Exit Do` 将控制转移出最内层循环，并将其转移到下一个更高的嵌套级别。  
  
## <a name="example"></a>示例  

 在下面的示例中，循环中的语句将继续运行，直到 `index` 变量大于10。 `Until`子句位于循环的结尾。  
  
 [!code-vb[VbVbalrStatements#131](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class10.vb#131)]  
  
## <a name="example"></a>示例  

 下面的示例使用 `While` 子句而不是 `Until` 子句，并 `condition` 在循环的开头而不是在结束时进行测试。  
  
 [!code-vb[VbVbalrStatements#132](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class10.vb#132)]  
  
## <a name="example"></a>示例  

 在下面的示例中， `condition` 当 `index` 变量大于100时停止循环。 `If`但是，循环中的语句会导致语句在 `Exit Do` 索引变量大于10时停止循环。  
  
 [!code-vb[VbVbalrStatements#133](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class10.vb#133)]  
  
## <a name="example"></a>示例  

 下面的示例读取文本文件中的所有行。 <xref:System.IO.File.OpenText%2A>方法打开文件并返回 <xref:System.IO.StreamReader> 读取字符的。 在 `Do...Loop` 条件中，的 <xref:System.IO.StreamReader.Peek%2A> 方法 `StreamReader` 确定是否有任何其他字符。  
  
 [!code-vb[VbVbalrStatements#134](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class10.vb#134)]  
  
## <a name="see-also"></a>另请参阅

- [循环结构](../../programming-guide/language-features/control-flow/loop-structures.md)
- [For...Next 语句](for-next-statement.md)
- [布尔数据类型](../data-types/boolean-data-type.md)
- [嵌套的控件结构](../../programming-guide/language-features/control-flow/nested-control-structures.md)
- [Exit 语句](exit-statement.md)
- [While...End While 语句](while-end-while-statement.md)
