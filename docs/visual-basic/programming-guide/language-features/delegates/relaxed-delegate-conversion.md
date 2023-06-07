---
title: 宽松委托转换
ms.date: 07/20/2015
helpviewer_keywords:
- relaxed delegate conversion [Visual Basic]
- delegates [Visual Basic], relaxed conversion
- conversions [Visual Basic], relaxed delegate
ms.assetid: 64f371d0-5416-4f65-b23b-adcbf556e81c
ms.openlocfilehash: b914d0479f160199744a8f9923c0bebc87321329
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91086070"
---
# <a name="relaxed-delegate-conversion-visual-basic"></a>宽松委托转换 (Visual Basic)

通过宽松委托转换，您可以将 sub 和函数分配给委托或处理程序，即使它们的签名不相同。 因此，绑定到委托将与已允许方法调用的绑定一致。  
  
## <a name="parameters-and-return-type"></a>参数和返回类型  

 宽松转换要求在设置为以下条件时满足以下条件 `Option Strict` `On` ：  
  
- 必须将每个委托参数的数据类型的扩大转换为分配的函数或的对应参数的数据类型 `Sub` 。 在下面的示例中，委托 `Del1` 具有一个参数，即 `Integer` 。 `m`指定 lambda 表达式中的参数必须有一个数据类型，该数据类型的扩大转换来自 `Integer` ，如 `Long` 或 `Double` 。  
  
     [!code-vb[VbVbalrRelaxedDelegates#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#1)]  
  
     [!code-vb[VbVbalrRelaxedDelegates#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#2)]  
  
     仅当设置为时，才允许收缩转换 `Option Strict` `Off` 。  
  
     [!code-vb[VbVbalrRelaxedDelegates#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module2.vb#8)]  
  
- 扩大转换必须与指定函数的返回类型或 `Sub` 委托的返回类型在相反方向上存在。 在下面的示例中，每个分配的 lambda 表达式的主体的计算结果都必须是扩大到的数据类型， `Integer` 因为的返回类型 `del1` 是 `Integer` 。  
  
     [!code-vb[VbVbalrRelaxedDelegates#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#3)]  
  
 如果 `Option Strict` 设置为，则将 `Off` 在这两个方向上删除扩大限制。  
  
 [!code-vb[VbVbalrRelaxedDelegates#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module2.vb#4)]  
  
## <a name="omitting-parameter-specifications"></a>省略参数规范  

 宽松委托还允许您完全省略分配的方法中的参数规范：  
  
 [!code-vb[VbVbalrRelaxedDelegates#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#5)]  
  
 [!code-vb[VbVbalrRelaxedDelegates#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#6)]  
  
 请注意，不能指定某些参数并忽略其他参数。  
  
 [!code-vb[VbVbalrRelaxedDelegates#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#15)]  
  
 在某些情况下，忽略参数的功能非常有用，例如定义一个事件处理程序，其中涉及多个复杂参数。 不使用某些事件处理程序的参数。 相反，处理程序将直接访问在其上注册事件的控件的状态，并忽略参数。 宽松的委托允许在没有歧义结果时省略此类声明中的参数。 在下面的示例中，可以将完全指定的方法 `OnClick` 重写为 `RelaxedOnClick` 。  
  
```vb  
Sub OnClick(ByVal sender As Object, ByVal e As EventArgs) Handles b.Click  
    MessageBox.Show("Hello World from" + b.Text)  
End Sub  
  
Sub RelaxedOnClick() Handles b.Click  
    MessageBox.Show("Hello World from" + b.Text)  
End Sub  
```  
  
## <a name="addressof-examples"></a>AddressOf 示例  

 前面的示例中使用了 Lambda 表达式，使类型关系易于查看。 但是，允许使用、或的委托分配使用相同的 `AddressOf` 松弛法 `Handles` `AddHandler` 。  
  
 在下面的示例中，函数 `f1` 、、 `f2` 和都 `f3` `f4` 可以分配给 `Del1` 。  
  
 [!code-vb[VbVbalrRelaxedDelegates#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#1)]  
  
 [!code-vb[VbVbalrRelaxedDelegates#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#7)]  
  
 [!code-vb[VbVbalrRelaxedDelegates#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#9)]  
  
 仅当设置为时，下面的示例才有效 `Option Strict` `Off` 。  
  
 [!code-vb[VbVbalrRelaxedDelegates#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module2.vb#14)]  
  
## <a name="dropping-function-returns"></a>删除函数返回  

 通过宽松委托转换，您可以将函数分配给 `Sub` 委托，从而有效地忽略函数的返回值。 但是，不能将分配 `Sub` 给函数委托。 在下面的示例中，函数的地址 `doubler` 分配给 `Sub` 委托 `Del3` 。  
  
 [!code-vb[VbVbalrRelaxedDelegates#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#10)]  
  
 [!code-vb[VbVbalrRelaxedDelegates#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#11)]  
  
## <a name="see-also"></a>请参阅

- [Lambda 表达式](../procedures/lambda-expressions.md)
- [Widening and Narrowing Conversions](../data-types/widening-and-narrowing-conversions.md)
- [委托](index.md)
- [如何：在 Visual Basic 中将过程传递给另一过程](how-to-pass-procedures-to-another-procedure.md)
- [局部类型推理](../variables/local-type-inference.md)
- [Option Strict 语句](../../../language-reference/statements/option-strict-statement.md)
