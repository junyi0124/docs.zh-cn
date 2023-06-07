---
title: 解释表达式
description: 了解如何编写代码来检查表达式树的结构。
ms.date: 06/20/2016
ms.technology: csharp-advanced-concepts
ms.assetid: adf73dde-1e52-4df3-9929-2e0670e28e16
ms.openlocfilehash: 39baf32c9c53d57227d52b9370f8165ff92d708d
ms.sourcegitcommit: 67ebdb695fd017d79d9f1f7f35d145042d5a37f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92223675"
---
# <a name="interpreting-expressions"></a>解释表达式

[上一部分 -- 执行表达式](expression-trees-execution.md)

现在，让我们编写一些代码来检查 *表达式树* 的结构。 表达式树中的每个节点将是派生自 `Expression` 的类的对象。

该设计使得访问表达式树中的所有节点成为相对直接的递归操作。 常规策略是从根节点开始并确定它是哪种节点。

如果节点类型具有子级，则以递归方式访问该子级。 在每个子节点中，重复在根节点处使用的步骤：确定类型，且如果该类型具有子级，则访问每个子级。

## <a name="examining-an-expression-with-no-children"></a>检查不具有子级的表达式

让我们首先访问一个简单的表达式树中的每个节点。
下面是创建常数表达式然后检查其属性的代码：

```csharp
var constant = Expression.Constant(24, typeof(int));

Console.WriteLine($"This is a/an {constant.NodeType} expression type");
Console.WriteLine($"The type of the constant value is {constant.Type}");
Console.WriteLine($"The value of the constant value is {constant.Value}");
```

将打印以下内容：

```output
This is an Constant expression type
The type of the constant value is System.Int32
The value of the constant value is 24
```

现在，让我们来编写将检查此表达式的代码，并写出有关它的一些重要属性。 下面是该代码：

## <a name="examining-a-simple-addition-expression"></a>检查一个简单的加法表达式

让我们从本节简介处的加法示例开始。

```csharp
Expression<Func<int>> sum = () => 1 + 2;
```

> 我没有使用 `var` 来声明此表达式树，因为此操作无法执行，这是由于赋值右侧是隐式类型而导致的。

根节点是 `LambdaExpression`。 为了获得 `=>` 运算符右侧的有用代码，需要找到 `LambdaExpression` 的子级之一。 我们将通过本部分中的所有表达式来实现此目的。 父节点确实有助于找到 `LambdaExpression` 的返回类型。

若要检查此表达式中的每个节点，将需要以递归方式访问大量节点。 下面是一个简单的首次实现：

```csharp
Expression<Func<int, int, int>> addition = (a, b) => a + b;

Console.WriteLine($"This expression is a {addition.NodeType} expression type");
Console.WriteLine($"The name of the lambda is {((addition.Name == null) ? "<null>" : addition.Name)}");
Console.WriteLine($"The return type is {addition.ReturnType.ToString()}");
Console.WriteLine($"The expression has {addition.Parameters.Count} arguments. They are:");
foreach(var argumentExpression in addition.Parameters)
{
    Console.WriteLine($"\tParameter Type: {argumentExpression.Type.ToString()}, Name: {argumentExpression.Name}");
}

var additionBody = (BinaryExpression)addition.Body;
Console.WriteLine($"The body is a {additionBody.NodeType} expression");
Console.WriteLine($"The left side is a {additionBody.Left.NodeType} expression");
var left = (ParameterExpression)additionBody.Left;
Console.WriteLine($"\tParameter Type: {left.Type.ToString()}, Name: {left.Name}");
Console.WriteLine($"The right side is a {additionBody.Right.NodeType} expression");
var right= (ParameterExpression)additionBody.Right;
Console.WriteLine($"\tParameter Type: {right.Type.ToString()}, Name: {right.Name}");
```

此示例打印以下输出：

```output
This expression is a/an Lambda expression type
The name of the lambda is <null>
The return type is System.Int32
The expression has 2 arguments. They are:
        Parameter Type: System.Int32, Name: a
        Parameter Type: System.Int32, Name: b
The body is a/an Add expression
The left side is a Parameter expression
        Parameter Type: System.Int32, Name: a
The right side is a Parameter expression
        Parameter Type: System.Int32, Name: b
```

你会注意到以上代码示例中的大量重复。
让我们将其清理干净，并生成一个更加通用的表达式节点访问者。 这将要求编写递归算法。 任何节点都可能是具有子级的类型。
具有子级的任何节点都要求访问这些子级并确定该节点是什么。 下面是利用递归访问加法运算的已清理的版本：

```csharp
// Base Visitor class:
public abstract class Visitor
{
    private readonly Expression node;

    protected Visitor(Expression node)
    {
        this.node = node;
    }

    public abstract void Visit(string prefix);

    public ExpressionType NodeType => this.node.NodeType;
    public static Visitor CreateFromExpression(Expression node)
    {
        switch(node.NodeType)
        {
            case ExpressionType.Constant:
                return new ConstantVisitor((ConstantExpression)node);
            case ExpressionType.Lambda:
                return new LambdaVisitor((LambdaExpression)node);
            case ExpressionType.Parameter:
                return new ParameterVisitor((ParameterExpression)node);
            case ExpressionType.Add:
                return new BinaryVisitor((BinaryExpression)node);
            default:
                Console.Error.WriteLine($"Node not processed yet: {node.NodeType}");
                return default(Visitor);
        }
    }
}

// Lambda Visitor
public class LambdaVisitor : Visitor
{
    private readonly LambdaExpression node;
    public LambdaVisitor(LambdaExpression node) : base(node)
    {
        this.node = node;
    }

    public override void Visit(string prefix)
    {
        Console.WriteLine($"{prefix}This expression is a {NodeType} expression type");
        Console.WriteLine($"{prefix}The name of the lambda is {((node.Name == null) ? "<null>" : node.Name)}");
        Console.WriteLine($"{prefix}The return type is {node.ReturnType.ToString()}");
        Console.WriteLine($"{prefix}The expression has {node.Parameters.Count} argument(s). They are:");
        // Visit each parameter:
        foreach (var argumentExpression in node.Parameters)
        {
            var argumentVisitor = Visitor.CreateFromExpression(argumentExpression);
            argumentVisitor.Visit(prefix + "\t");
        }
        Console.WriteLine($"{prefix}The expression body is:");
        // Visit the body:
        var bodyVisitor = Visitor.CreateFromExpression(node.Body);
        bodyVisitor.Visit(prefix + "\t");
    }
}

// Binary Expression Visitor:
public class BinaryVisitor : Visitor
{
    private readonly BinaryExpression node;
    public BinaryVisitor(BinaryExpression node) : base(node)
    {
        this.node = node;
    }

    public override void Visit(string prefix)
    {
        Console.WriteLine($"{prefix}This binary expression is a {NodeType} expression");
        var left = Visitor.CreateFromExpression(node.Left);
        Console.WriteLine($"{prefix}The Left argument is:");
        left.Visit(prefix + "\t");
        var right = Visitor.CreateFromExpression(node.Right);
        Console.WriteLine($"{prefix}The Right argument is:");
        right.Visit(prefix + "\t");
    }
}

// Parameter visitor:
public class ParameterVisitor : Visitor
{
    private readonly ParameterExpression node;
    public ParameterVisitor(ParameterExpression node) : base(node)
    {
        this.node = node;
    }

    public override void Visit(string prefix)
    {
        Console.WriteLine($"{prefix}This is an {NodeType} expression type");
        Console.WriteLine($"{prefix}Type: {node.Type.ToString()}, Name: {node.Name}, ByRef: {node.IsByRef}");
    }
}

// Constant visitor:
public class ConstantVisitor : Visitor
{
    private readonly ConstantExpression node;
    public ConstantVisitor(ConstantExpression node) : base(node)
    {
        this.node = node;
    }

    public override void Visit(string prefix)
    {
        Console.WriteLine($"{prefix}This is an {NodeType} expression type");
        Console.WriteLine($"{prefix}The type of the constant value is {node.Type}");
        Console.WriteLine($"{prefix}The value of the constant value is {node.Value}");
    }
}
```

此算法是可以访问任意 `LambdaExpression` 的算法的基础。 其中有许多缺口，即我创建的代码仅查找它可能遇到的表达式树节点组的一小部分。 但是，你仍可以从其结果中获益匪浅。 （遇到新的节点类型时，`Visitor.CreateFromExpression` 方法中的默认 case 会将消息打印到错误控制台。 如此，你便知道要添加新的表达式类型。）

在上面所示的加法表达式中运行此访问者时，将获得以下输出：

```output
This expression is a/an Lambda expression type
The name of the lambda is <null>
The return type is System.Int32
The expression has 2 argument(s). They are:
        This is an Parameter expression type
        Type: System.Int32, Name: a, ByRef: False
        This is an Parameter expression type
        Type: System.Int32, Name: b, ByRef: False
The expression body is:
        This binary expression is a Add expression
        The Left argument is:
                This is an Parameter expression type
                Type: System.Int32, Name: a, ByRef: False
        The Right argument is:
                This is an Parameter expression type
                Type: System.Int32, Name: b, ByRef: False
```

现在既已构建更通用的访问者实现，便可以访问和处理更多不同类型的表达式了。

## <a name="examining-an-addition-expression-with-many-levels"></a>检查具有许多级别的加法表达式

让我们尝试更复杂的示例，但仍限制节点类型仅为加法：

```csharp
Expression<Func<int>> sum = () => 1 + 2 + 3 + 4;
```

在访问者算法上运行此表达式之前，请尝试思考可能的输出是什么。 请记住，`+` 运算符是 *二元运算符* ：它必须具有两个子级，分别表示左右操作数。 有几种可行的方法来构造可能正确的树：

```csharp
Expression<Func<int>> sum1 = () => 1 + (2 + (3 + 4));
Expression<Func<int>> sum2 = () => ((1 + 2) + 3) + 4;

Expression<Func<int>> sum3 = () => (1 + 2) + (3 + 4);
Expression<Func<int>> sum4 = () => 1 + ((2 + 3) + 4);
Expression<Func<int>> sum5 = () => (1 + (2 + 3)) + 4;
```

可以看到可能的答案分为两种，以便着重于最有可能正确的答案。 第一种表示 *右结合* 表达式。 第二种表示 *左结合* 表达式。
这两种格式的优点是，格式可以缩放为任意数量的加法表达式。

如果确实通过该访问者运行此表达式，则将看到此输出，它验证简单的加法表达式是否为左结合。

为了运行此示例并查看完整的表达式树，我不得不对源表达式树进行一次更改。 当表达式树包含所有常量时，所得到的树仅包含 `10` 的常量值。 编译器执行所有加法运算，并将表达式缩减为其最简单的形式。 只需在表达式中添加一个变量即可看到原始的树：

```csharp
Expression<Func<int, int>> sum = (a) => 1 + a + 3 + 4;
```

创建可得出此总和的访问者并运行该访问者，则会看到以下输出：

```output
This expression is a/an Lambda expression type
The name of the lambda is <null>
The return type is System.Int32
The expression has 1 argument(s). They are:
        This is an Parameter expression type
        Type: System.Int32, Name: a, ByRef: False
The expression body is:
        This binary expression is a Add expression
        The Left argument is:
                This binary expression is a Add expression
                The Left argument is:
                        This binary expression is a Add expression
                        The Left argument is:
                                This is an Constant expression type
                                The type of the constant value is System.Int32
                                The value of the constant value is 1
                        The Right argument is:
                                This is an Parameter expression type
                                Type: System.Int32, Name: a, ByRef: False
                The Right argument is:
                        This is an Constant expression type
                        The type of the constant value is System.Int32
                        The value of the constant value is 3
        The Right argument is:
                This is an Constant expression type
                The type of the constant value is System.Int32
                The value of the constant value is 4
```

还可以通过访问者代码运行任何其他示例，并查看其表示的树。 下面是上述 `sum3` 表达式（使用附加参数来阻止编译器计算常量）的一个示例：

```csharp
Expression<Func<int, int, int>> sum3 = (a, b) => (1 + a) + (3 + b);
```

下面是访问者的输出：

```output
This expression is a/an Lambda expression type
The name of the lambda is <null>
The return type is System.Int32
The expression has 2 argument(s). They are:
        This is an Parameter expression type
        Type: System.Int32, Name: a, ByRef: False
        This is an Parameter expression type
        Type: System.Int32, Name: b, ByRef: False
The expression body is:
        This binary expression is a Add expression
        The Left argument is:
                This binary expression is a Add expression
                The Left argument is:
                        This is an Constant expression type
                        The type of the constant value is System.Int32
                        The value of the constant value is 1
                The Right argument is:
                        This is an Parameter expression type
                        Type: System.Int32, Name: a, ByRef: False
        The Right argument is:
                This binary expression is a Add expression
                The Left argument is:
                        This is an Constant expression type
                        The type of the constant value is System.Int32
                        The value of the constant value is 3
                The Right argument is:
                        This is an Parameter expression type
                        Type: System.Int32, Name: b, ByRef: False
```

请注意，括号不是输出的一部分。 表达式树中不存在表示输入表达式中的括号的节点。 表达式树的结构包含传达优先级所需的所有信息。

## <a name="extending-from-this-sample"></a>从此示例扩展

此示例仅处理最基本的表达式树。 在本部分中看到的代码仅处理常量整数和二进制 `+` 运算符。 作为最后一个示例，让我们更新访问者以处理更加复杂的表达式。 让我们这样来改进它：

```csharp
Expression<Func<int, int>> factorial = (n) =>
    n == 0 ?
    1 :
    Enumerable.Range(1, n).Aggregate((product, factor) => product * factor);
```

此代码表示数学 *阶乘* 函数的一个可能的实现。 编写此代码的方式强调了通过将 lambda 表达式分配到表达式来生成表达式树的两个限制。 首先，lambda 语句是不允许的。 这意味着无法使用循环、块、if / else 语句和 C# 中常用的其他控件结构。 我只能使用表达式。 其次，不能以递归方式调用同一表达式。
如果该表达式已是一个委托，则可以通过递归方式进行调用，但不能在其表达式树的形式中调用它。 在有关[生成表达式树](expression-trees-building.md)的部分中，你将了解克服这些限制的技巧。

在此表达式中，将遇到所有这些类型的节点：

1. Equal（二进制表达式）
2. Multiply（二进制表达式）
3. Conditional（? : 表达式）
4. 方法调用表达式（调用 `Range()` 和 `Aggregate()`）

修改访问者算法的其中一个方法是持续执行它，并在每次到达 `default` 子句时编写节点类型。 经过几次迭代之后，便将看到每个可能的节点。 这样便万事俱备了。 结果类似于：

```csharp
public static Visitor CreateFromExpression(Expression node)
{
    switch(node.NodeType)
    {
        case ExpressionType.Constant:
            return new ConstantVisitor((ConstantExpression)node);
        case ExpressionType.Lambda:
            return new LambdaVisitor((LambdaExpression)node);
        case ExpressionType.Parameter:
            return new ParameterVisitor((ParameterExpression)node);
        case ExpressionType.Add:
        case ExpressionType.Equal:
        case ExpressionType.Multiply:
            return new BinaryVisitor((BinaryExpression)node);
        case ExpressionType.Conditional:
            return new ConditionalVisitor((ConditionalExpression)node);
        case ExpressionType.Call:
            return new MethodCallVisitor((MethodCallExpression)node);
        default:
            Console.Error.WriteLine($"Node not processed yet: {node.NodeType}");
            return default(Visitor);
    }
}
```

ConditionalVisitor 和 MethodCallVisitor 将处理这两个节点：

```csharp
public class ConditionalVisitor : Visitor
{
    private readonly ConditionalExpression node;
    public ConditionalVisitor(ConditionalExpression node) : base(node)
    {
        this.node = node;
    }

    public override void Visit(string prefix)
    {
        Console.WriteLine($"{prefix}This expression is a {NodeType} expression");
        var testVisitor = Visitor.CreateFromExpression(node.Test);
        Console.WriteLine($"{prefix}The Test for this expression is:");
        testVisitor.Visit(prefix + "\t");
        var trueVisitor = Visitor.CreateFromExpression(node.IfTrue);
        Console.WriteLine($"{prefix}The True clause for this expression is:");
        trueVisitor.Visit(prefix + "\t");
        var falseVisitor = Visitor.CreateFromExpression(node.IfFalse);
        Console.WriteLine($"{prefix}The False clause for this expression is:");
        falseVisitor.Visit(prefix + "\t");
    }
}

public class MethodCallVisitor : Visitor
{
    private readonly MethodCallExpression node;
    public MethodCallVisitor(MethodCallExpression node) : base(node)
    {
        this.node = node;
    }

    public override void Visit(string prefix)
    {
        Console.WriteLine($"{prefix}This expression is a {NodeType} expression");
        if (node.Object == null)
            Console.WriteLine($"{prefix}This is a static method call");
        else
        {
            Console.WriteLine($"{prefix}The receiver (this) is:");
            var receiverVisitor = Visitor.CreateFromExpression(node.Object);
            receiverVisitor.Visit(prefix + "\t");
        }

        var methodInfo = node.Method;
        Console.WriteLine($"{prefix}The method name is {methodInfo.DeclaringType}.{methodInfo.Name}");
        // There is more here, like generic arguments, and so on.
        Console.WriteLine($"{prefix}The Arguments are:");
        foreach(var arg in node.Arguments)
        {
            var argVisitor = Visitor.CreateFromExpression(arg);
            argVisitor.Visit(prefix + "\t");
        }
    }
}
```

且表达式树的输出为：

```output
This expression is a/an Lambda expression type
The name of the lambda is <null>
The return type is System.Int32
The expression has 1 argument(s). They are:
        This is an Parameter expression type
        Type: System.Int32, Name: n, ByRef: False
The expression body is:
        This expression is a Conditional expression
        The Test for this expression is:
                This binary expression is a Equal expression
                The Left argument is:
                        This is an Parameter expression type
                        Type: System.Int32, Name: n, ByRef: False
                The Right argument is:
                        This is an Constant expression type
                        The type of the constant value is System.Int32
                        The value of the constant value is 0
        The True clause for this expression is:
                This is an Constant expression type
                The type of the constant value is System.Int32
                The value of the constant value is 1
        The False clause for this expression is:
                This expression is a Call expression
                This is a static method call
                The method name is System.Linq.Enumerable.Aggregate
                The Arguments are:
                        This expression is a Call expression
                        This is a static method call
                        The method name is System.Linq.Enumerable.Range
                        The Arguments are:
                                This is an Constant expression type
                                The type of the constant value is System.Int32
                                The value of the constant value is 1
                                This is an Parameter expression type
                                Type: System.Int32, Name: n, ByRef: False
                        This expression is a Lambda expression type
                        The name of the lambda is <null>
                        The return type is System.Int32
                        The expression has 2 arguments. They are:
                                This is an Parameter expression type
                                Type: System.Int32, Name: product, ByRef: False
                                This is an Parameter expression type
                                Type: System.Int32, Name: factor, ByRef: False
                        The expression body is:
                                This binary expression is a Multiply expression
                                The Left argument is:
                                        This is an Parameter expression type
                                        Type: System.Int32, Name: product, ByRef: False
                                The Right argument is:
                                        This is an Parameter expression type
                                        Type: System.Int32, Name: factor, ByRef: False
```

## <a name="extending-the-sample-library"></a>扩展示例库

本部分中的示例演示访问和检查表达式树中的节点的核心技术。 我略过了很多可能需要的操作，以便专注于访问表达式树中的节点这一核心任务。

首先，访问者只处理整数常量。 常量值可以是任何其他数值类型，且 C# 语言支持这些类型之间的转换和提升。 此代码的更可靠版本可反映所有这些功能。

即使最后一个示例也只可识别可能的节点类型的一部分。
你仍可以向其添加许多将导致其失败的表达式。
完整的实现包含在名为 <xref:System.Linq.Expressions.ExpressionVisitor> 的 .NET Standard 中，且可以处理所有可能的节点类型。

最后，在本文中所使用的库是为演示和学习目的而生成。 它未进行优化。 我编写它是为了让所使用的结构清晰，以及强调用于访问节点和对此进行分析的技术。 生产实现将更加注重性能。

即使存在这些限制，在编写阅读和理解表达式树的算法这方面应当是没有问题的。

[下一部分 -- 生成表达式](expression-trees-building.md)
