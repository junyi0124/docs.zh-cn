---
title: 如何执行表达式树 (C#)
description: 了解如何执行表达式树以返回值或执行类似于调用方法的操作。
ms.date: 07/20/2015
ms.assetid: b8c40db5-2464-4bb9-9001-8c2bc7f006c5
ms.openlocfilehash: 19c3e639d64a44d180c75964261569dc0d6c2d89
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91154010"
---
# <a name="how-to-execute-expression-trees-c"></a>如何执行表达式树 (C#)

本主题演示如何执行表达式树。 执行表达式树可能返回一个值，或者它可能只是执行操作，例如调用方法。  
  
 仅可以执行表示 lambda 表达式的表达式树。 表示 Lambda 表达式的表达式树的类型为 <xref:System.Linq.Expressions.LambdaExpression> 或 <xref:System.Linq.Expressions.Expression%601>。 若要执行这些表达式树，请调用 <xref:System.Linq.Expressions.LambdaExpression.Compile%2A> 方法来创建一个可执行的委托，然后调用该委托。  
  
> [!NOTE]
> 如果委托的类型未知，也就是说 Lambda 表达式的类型为 <xref:System.Linq.Expressions.LambdaExpression>，而不是 <xref:System.Linq.Expressions.Expression%601>，则必须对委托调用 <xref:System.Delegate.DynamicInvoke%2A> 方法，而不是直接调用委托。  
  
 如果表达式树不表示 Lambda 表达式，可以通过调用 <xref:System.Linq.Expressions.Expression.Lambda%60%601%28System.Linq.Expressions.Expression%2CSystem.Collections.Generic.IEnumerable%7BSystem.Linq.Expressions.ParameterExpression%7D%29> 方法创建一个新的 Lambda 表达式，此表达式的主体为原始表达式树。 然后，按本节前面所述执行此 lambda 表达式。  
  
## <a name="example"></a>示例  

 下面的代码示例演示如何通过创建 lambda 表达式并执行它来执行代表幂运算的表达式树。 示例最后显示幂运算的结果。  
  
```csharp  
// The expression tree to execute.  
BinaryExpression be = Expression.Power(Expression.Constant(2D), Expression.Constant(3D));  
  
// Create a lambda expression.  
Expression<Func<double>> le = Expression.Lambda<Func<double>>(be);  
  
// Compile the lambda expression.  
Func<double> compiledExpression = le.Compile();  
  
// Execute the lambda expression.  
double result = compiledExpression();  
  
// Display the result.  
Console.WriteLine(result);  
  
// This code produces the following output:  
// 8  
```  
  
## <a name="compiling-the-code"></a>编译代码  
  
- 包括 System.Linq.Expressions 命名空间。  
  
## <a name="see-also"></a>请参阅

- [表达式树 (C#)](./index.md)
- [如何修改表达式树 (C#)](./how-to-modify-expression-trees.md)
