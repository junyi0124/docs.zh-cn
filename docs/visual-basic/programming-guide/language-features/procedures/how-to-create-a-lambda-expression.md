---
title: 如何：创建 lambda 表达式
ms.date: 07/20/2015
helpviewer_keywords:
- lambda expressions [Visual Basic]
- expressions [Visual Basic], lambda
ms.assetid: 3279bd5c-80f7-410a-a7ba-f7085ed36aa5
ms.openlocfilehash: cc2de38f7375848d104edff6f419656d9caa9cb2
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91071919"
---
# <a name="how-to-create-a-lambda-expression-visual-basic"></a>如何：创建 Lambda 表达式 (Visual Basic)

*Lambda 表达式*是没有名称的函数或子例程。 如果委托类型有效，可以使用 lambda 表达式。  
  
### <a name="to-create-a-single-line-lambda-expression-function"></a>创建单行 lambda 表达式函数  
  
1. 在可以使用委托类型的任何情况下，键入关键字 `Function` ，如以下示例中所示：  
  
     `Dim add1 =`   `Function`  
  
2. 在括号中，直接 `Function` 键入函数的参数。 请注意，不能在之后指定名称 `Function` 。  
  
     `Dim add1 = Function`   `(num As Integer)`  
  
3. 在参数列表的后面键入一个表达式作为函数的主体。 表达式计算结果的值是函数返回的值。 不要使用 `As` 子句来指定返回类型。  
  
     [!code-vb[VbVbalrLambdas#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#1)]  
  
     通过传入整数参数来调用 lambda 表达式。  
  
     [!code-vb[VbVbalrLambdas#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#2)]  
  
4. 或者，通过以下示例实现相同的结果：  
  
     [!code-vb[VbVbalrLambdas#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#3)]  
  
### <a name="to-create-a-single-line-lambda-expression-subroutine"></a>创建单行 lambda 表达式子例程  
  
1. 在可以使用委托类型的任何情况下，键入关键字 `Sub` ，如下面的示例中所示。  
  
     `Dim add1 =`   `Sub`  
  
2. 在括号中，直接 `Sub` 键入子例程的参数。 请注意，不能在之后指定名称 `Sub` 。  
  
     `Dim add1 = Sub`   `(msg As String)`  
  
3. 在参数列表的后面键入单个语句作为子例程的主体。  
  
     [!code-vb[VbVbalrLambdas#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#17)]  
  
     通过传入字符串参数来调用 lambda 表达式。  
  
     [!code-vb[VbVbalrLambdas#18](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#18)]  
  
### <a name="to-create-a-multiline-lambda-expression-function"></a>创建多行 lambda 表达式函数  
  
1. 在可以使用委托类型的任何情况下，键入关键字 `Function` ，如下面的示例中所示。  
  
     `Dim add1 =`   `Function`  
  
2. 在括号中，直接 `Function` 键入函数的参数。 请注意，不能在之后指定名称 `Function` 。  
  
     `Dim add1 = Function`   `(index As Integer)`  
  
3. 按 Enter。 `End Function`语句会自动添加。  
  
4. 在函数的主体中，添加以下代码以创建表达式并返回值。 不要使用 `As` 子句来指定返回类型。  
  
     [!code-vb[VbVbalrLambdas#19](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#19)]  
  
     通过传入整数参数来调用 lambda 表达式。  
  
     [!code-vb[VbVbalrLambdas#20](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#20)]  
  
### <a name="to-create-a-multiline-lambda-expression-subroutine"></a>创建多行 lambda 表达式子例程  
  
1. 在可以使用委托类型的任何情况下，键入关键字 `Sub` ，如以下示例中所示：  
  
     `Dim add1 =`   `Sub`  
  
2. 在括号中，直接 `Sub` 键入子例程的参数。 请注意，不能在之后指定名称 `Sub` 。  
  
     `Dim add1 = Sub`  `(msg As String)`  
  
3. 按 Enter。 `End Sub`语句会自动添加。  
  
4. 在函数的主体中，添加以下代码，以便在调用子例程时执行。  
  
     [!code-vb[VbVbalrLambdas#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#21)]  
  
     通过传入字符串参数来调用 lambda 表达式。  
  
     [!code-vb[VbVbalrLambdas#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#22)]  
  
## <a name="example"></a>示例  

 Lambda 表达式的一个常见用途是定义一个函数，该函数可作为参数（其类型为）作为参数传入 `Delegate` 。 在下面的示例中， <xref:System.Diagnostics.Process.GetProcesses%2A> 方法返回在本地计算机上运行的进程的数组。 <xref:System.Linq.Enumerable.Where%2A>类中的方法 <xref:System.Linq.Enumerable> 要求 `Boolean` 委托作为其参数。 示例中的 lambda 表达式用于此目的。 它 `True` 为每个只有一个线程的进程返回，在中选择这些进程 `filteredList` 。  
  
 [!code-vb[VbVbalrLambdas#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class4.vb#10)]  
  
 前面的示例等效于以下代码，该代码用语言集成查询编写 (LINQ) 语法：  
  
 [!code-vb[VbVbalrLambdas#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class5.vb#11)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.Linq.Enumerable>
- [Lambda 表达式](./lambda-expressions.md)
- [Function 语句](../../../language-reference/statements/function-statement.md)
- [Sub 语句](../../../language-reference/statements/sub-statement.md)
- [委托](../delegates/index.md)
- [如何：在 Visual Basic 中将过程传递给另一过程](../delegates/how-to-pass-procedures-to-another-procedure.md)
- [Delegate 语句](../../../language-reference/statements/delegate-statement.md)
- [Visual Basic 中的 LINQ 简介](../linq/introduction-to-linq.md)
