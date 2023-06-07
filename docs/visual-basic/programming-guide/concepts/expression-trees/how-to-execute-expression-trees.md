---
title: 如何：执行表达式树
ms.date: 07/20/2015
ms.assetid: 9dfb5ab3-f48f-417e-975f-f8f6f1cdc18d
ms.openlocfilehash: 703a2dc49ba905c81948267f6eec3ca7578fc308
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91077327"
---
# <a name="how-to-execute-expression-trees-visual-basic"></a>如何：执行表达式树 (Visual Basic)

本主题演示如何执行表达式树。 执行表达式树可能返回一个值，或者它可能只是执行操作，例如调用方法。  
  
 仅可以执行表示 lambda 表达式的表达式树。 表示 Lambda 表达式的表达式树的类型为 <xref:System.Linq.Expressions.LambdaExpression> 或 <xref:System.Linq.Expressions.Expression%601>。 若要执行这些表达式树，请调用 <xref:System.Linq.Expressions.LambdaExpression.Compile%2A> 方法来创建一个可执行的委托，然后调用该委托。  
  
> [!NOTE]
> 如果委托的类型未知，也就是说 Lambda 表达式的类型为 <xref:System.Linq.Expressions.LambdaExpression>，而不是 <xref:System.Linq.Expressions.Expression%601>，则必须对委托调用 <xref:System.Delegate.DynamicInvoke%2A> 方法，而不是直接调用委托。  
  
 如果表达式树不表示 Lambda 表达式，可以通过调用 <xref:System.Linq.Expressions.Expression.Lambda%60%601%28System.Linq.Expressions.Expression%2CSystem.Collections.Generic.IEnumerable%7BSystem.Linq.Expressions.ParameterExpression%7D%29> 方法创建一个新的 Lambda 表达式，此表达式的主体为原始表达式树。 然后，按本节前面所述执行此 lambda 表达式。  
  
## <a name="example"></a>示例  

 下面的代码示例演示如何通过创建 lambda 表达式并执行它来执行代表幂运算的表达式树。 示例最后显示幂运算的结果。  
  
```vb  
' The expression tree to execute.  
Dim be As BinaryExpression = Expression.Power(Expression.Constant(2.0R), Expression.Constant(3.0R))  
  
' Create a lambda expression.  
Dim le As Expression(Of Func(Of Double)) = Expression.Lambda(Of Func(Of Double))(be)  
  
' Compile the lambda expression.  
Dim compiledExpression As Func(Of Double) = le.Compile()  
  
' Execute the lambda expression.  
Dim result As Double = compiledExpression()  
  
' Display the result.  
MsgBox(result)  
  
' This code produces the following output:  
' 8  
```  
  
## <a name="compile-the-code"></a>编译代码  
  
- 包括 System.Linq.Expressions 命名空间。  
  
## <a name="see-also"></a>请参阅

- [表达式树 (Visual Basic)](index.md)
- [如何：修改表达式树 (Visual Basic)](how-to-modify-expression-trees.md)
