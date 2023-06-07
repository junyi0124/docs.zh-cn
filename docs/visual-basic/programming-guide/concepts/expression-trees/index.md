---
title: 表达式树
ms.date: 07/20/2015
ms.assetid: 8bbbb02d-7ffc-476b-8c25-118d82bf5d46
ms.openlocfilehash: 29f2545a3bc1d53e8ab28478f63ef7b0dfe7a15e
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91075403"
---
# <a name="expression-trees-visual-basic"></a>表达式树 (Visual Basic)

表达式树以树形数据结构表示代码，其中每一个节点都是一种表达式，比如方法调用和 `x < y` 这样的二元运算等。  
  
 你可以对表达式树中的代码进行编辑和运算。 这样能够动态修改可执行代码、在不同数据库中执行 LINQ 查询以及创建动态查询。 有关 LINQ 中表达式树的详细信息，请参阅[如何：使用表达式树生成动态查询 (Visual Basic)](how-to-use-expression-trees-to-build-dynamic-queries.md)。  
  
 表达式树还能用于动态语言运行时 (DLR) 以提供动态语言和 .NET Framework 之间的互操作性，同时保证编译器编写员能够发射表达式树而非 Microsoft 中间语言 (MSIL)。 有关 DLR 的详细信息，请参阅[动态语言运行时概述](../../../../framework/reflection-and-codedom/dynamic-language-runtime-overview.md)。  
  
 你可以基于匿名 lambda 表达式通过 C# 或者 Visual Basic 编译器创建表达式树，或者通过 <xref:System.Linq.Expressions> 名称空间手动创建。  
  
## <a name="creating-expression-trees-from-lambda-expressions"></a>根据 Lambda 表达式创建表达式树  

 若 lambda 表达式被分配给 <xref:System.Linq.Expressions.Expression%601> 类型的变量，则编译器可以发射代码以创建表示该 lambda 表达式的表达式树。  
  
 Visual Basic 编译器只能从表达式 Lambda（或单行 Lambda）生成表达式树。 它无法解析语句 lambda （或多行 lambda）。 有关 Visual Basic 中 Lambda 表达式的详细信息，请参阅 [Lambda 表达式](../../language-features/procedures/lambda-expressions.md)。  
  
 下列代码示例展示如何通过 Visual Basic 编译器创建表示 Lambda 表达式 `Function(num) num < 5` 的表达式树。  
  
```vb  
Dim lambda As Expression(Of Func(Of Integer, Boolean)) =  
    Function(num) num < 5  
```  
  
## <a name="creating-expression-trees-by-using-the-api"></a>通过 API 创建表达式树  

 通过 API 创建表达式树需要使用 <xref:System.Linq.Expressions.Expression> 类。 类包含创建特定类型表达式树节点的静态工厂方法，比如表示参数变量的 <xref:System.Linq.Expressions.ParameterExpression>，或者是表示方法调用的 <xref:System.Linq.Expressions.MethodCallExpression>。 <xref:System.Linq.Expressions.ParameterExpression> 名称空间还解释了 <xref:System.Linq.Expressions.MethodCallExpression>、<xref:System.Linq.Expressions>和另一种具体表达式类型。 这些类型来源于抽象类型 <xref:System.Linq.Expressions.Expression>。  
  
 下列代码示例展示如何使用 API 创建表示 Lambda 表达式 `Function(num) num < 5` 的表达式树。  
  
```vb  
' Import the following namespace to your project: System.Linq.Expressions  
  
' Manually build the expression tree for the lambda expression num => num < 5.  
Dim numParam As ParameterExpression = Expression.Parameter(GetType(Integer), "num")  
Dim five As ConstantExpression = Expression.Constant(5, GetType(Integer))  
Dim numLessThanFive As BinaryExpression = Expression.LessThan(numParam, five)  
Dim lambda1 As Expression(Of Func(Of Integer, Boolean)) =  
  Expression.Lambda(Of Func(Of Integer, Boolean))(  
        numLessThanFive,  
        New ParameterExpression() {numParam})  
```  
  
 在 .NET Framework 4 或更高版本中，表达式树 API 还支持赋值表达式和控制流表达式，例如循环、条件块和 `try-catch` 块等。 相对于通过 Visual Basic 编译器和 Lambda 表达式创建表达式树，还可利用 API 创建更加复杂的表达式树。 下列示例展示如何创建计算数字阶乘的表达式树。  
  
```vb  
' Creating a parameter expression.  
Dim value As ParameterExpression =  
    Expression.Parameter(GetType(Integer), "value")  
  
' Creating an expression to hold a local variable.
Dim result As ParameterExpression =  
    Expression.Parameter(GetType(Integer), "result")  
  
' Creating a label to jump to from a loop.  
Dim label As LabelTarget = Expression.Label(GetType(Integer))  
  
' Creating a method body.  
Dim block As BlockExpression = Expression.Block(  
    New ParameterExpression() {result},  
    Expression.Assign(result, Expression.Constant(1)),  
    Expression.Loop(  
        Expression.IfThenElse(  
            Expression.GreaterThan(value, Expression.Constant(1)),  
            Expression.MultiplyAssign(result,  
                Expression.PostDecrementAssign(value)),  
            Expression.Break(label, result)  
        ),  
        label  
    )  
)  
  
' Compile an expression tree and return a delegate.  
Dim factorial As Integer =  
    Expression.Lambda(Of Func(Of Integer, Integer))(block, value).Compile()(5)  
  
Console.WriteLine(factorial)  
' Prints 120.  
```

有关详细信息，请参阅[在 Visual Studio 2010 中使用表达式树生成动态方法](https://devblogs.microsoft.com/csharpfaq/generating-dynamic-methods-with-expression-trees-in-visual-studio-2010/)，该方法也适用于 Visual Studio 的更高版本。
  
## <a name="parsing-expression-trees"></a>解析表达式树  

 下列代码示例展示如何分解表示 Lambda 表达式 `Function(num) num < 5` 的表达式树。  
  
```vb  
' Import the following namespace to your project: System.Linq.Expressions  
  
' Create an expression tree.  
Dim exprTree As Expression(Of Func(Of Integer, Boolean)) = Function(num) num < 5  
  
' Decompose the expression tree.  
Dim param As ParameterExpression = exprTree.Parameters(0)  
Dim operation As BinaryExpression = exprTree.Body  
Dim left As ParameterExpression = operation.Left  
Dim right As ConstantExpression = operation.Right  
  
Console.WriteLine(String.Format("Decomposed expression: {0} => {1} {2} {3}",  
                  param.Name, left.Name, operation.NodeType, right.Value))  
  
' This code produces the following output:  
'  
' Decomposed expression: num => num LessThan 5  
```  
  
## <a name="immutability-of-expression-trees"></a>表达式树永久性  

 表达式树应具有永久性。 这意味着如果你想修改某个表达式树，则必须复制该表达式树然后替换其中的节点来创建一个新的表达式树。 你可以使用表达式树访问者遍历现有表达式树。 有关详细信息，请参阅[如何：修改表达式树 (Visual Basic)](how-to-modify-expression-trees.md)。  
  
## <a name="compiling-expression-trees"></a>编译表达式树  

 <xref:System.Linq.Expressions.Expression%601> 类型提供了 <xref:System.Linq.Expressions.Expression%601.Compile%2A> 方法以将表达式树表示的代码编译成可执行委托。  
  
 下列代码示例展示如何编译表达式树并运行结果代码。  
  
```vb  
' Creating an expression tree.  
Dim expr As Expression(Of Func(Of Integer, Boolean)) =  
    Function(num) num < 5  
  
' Compiling the expression tree into a delegate.  
Dim result As Func(Of Integer, Boolean) = expr.Compile()  
  
' Invoking the delegate and writing the result to the console.  
Console.WriteLine(result(4))  
  
' Prints True.  
  
' You can also use simplified syntax  
' to compile and run an expression tree.  
' The following line can replace two previous statements.  
Console.WriteLine(expr.Compile()(4))  
  
' Also prints True.  
```  
  
 有关详细信息，请参阅[如何：执行表达式树 (Visual Basic)](how-to-execute-expression-trees.md)。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Linq.Expressions>
- [如何：执行表达式树 (Visual Basic)](how-to-execute-expression-trees.md)
- [如何：修改表达式树 (Visual Basic)](how-to-modify-expression-trees.md)
- [Lambda 表达式](../../language-features/procedures/lambda-expressions.md)
- [动态语言运行时概述](../../../../framework/reflection-and-codedom/dynamic-language-runtime-overview.md)
- [Visual Basic 的编程概念 () ](../index.md)
