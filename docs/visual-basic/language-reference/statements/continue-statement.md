---
title: Continue 语句
ms.date: 07/20/2015
f1_keywords:
- vb.continue
helpviewer_keywords:
- Continue statement [Visual Basic]
- loops, transferring to next iteration
ms.assetid: 3ad00103-358b-4af3-a3a8-1b9ea0e995d3
ms.openlocfilehash: cf73ea1b3d402609c9966980dcab9ddd9bc096c2
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90874968"
---
# <a name="continue-statement-visual-basic"></a>Continue 语句 (Visual Basic)

将控制立即传输到循环的下一次迭代。  
  
## <a name="syntax"></a>语法  
  
```vb  
Continue { Do | For | While }  
```  
  
## <a name="remarks"></a>备注  

 可以从 `Do` 、或循环内传输 `For` `While` 到循环的下一次迭代。 控制权会立即传递到循环条件测试，这等效于将转换为 `For` 或 `While` 语句，或 `Do` `Loop` 包含或子句的或语句 `Until` `While` 。  
  
 可以在 `Continue` 允许传输的循环中的任意位置使用。 允许传输控件的规则与 [GoTo 语句](goto-statement.md)相同。  
  
 例如，如果循环完全包含在 `Try` 块、 `Catch` 块或 `Finally` 块中，则可以使用 `Continue` 来传出循环。 另一方面，如果 `Try` `End Try` 循环中包含 ... 结构，则不能使用 `Continue` 将控件从块中传输出去 `Finally` ，并且 `Try` `Catch` 仅当完全从 `Try` ... `End Try` 结构中传输时，才能使用它来传出或阻止。  
  
 如果有相同类型的嵌套循环（例如 `Do` 另一个循环内的循环），则 `Do` 语句将 `Continue Do` 跳到包含它的最内层循环的下一次迭代 `Do` 。 不能使用 `Continue` 跳到相同类型的包含循环的下一次迭代。  
  
 如果具有不同类型的嵌套循环（例如 `Do` 循环内的循环）， `For` 可以使用或跳到任一循环的下一次迭代 `Continue Do` `Continue For` 。  
  
## <a name="example"></a>示例  

 下面的代码示例使用 `Continue While` 语句跳转到数组的下一列（如果除数为零）。 在 `Continue While` `For` 循环内。 它转移到 `While col < lastcol` 语句，后者是包含循环的最内层循环的下一次迭代 `While` `For` 。  
  
 [!code-vb[VbVbalrStatements#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#14)]  
  
## <a name="see-also"></a>另请参阅

- [Do...Loop 语句](do-loop-statement.md)
- [For...Next 语句](for-next-statement.md)
- [While...End While 语句](while-end-while-statement.md)
- [尝试 .。。Catch .。。Finally 语句](try-catch-finally-statement.md)
