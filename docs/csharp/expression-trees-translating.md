---
title: 转换表达式树
description: 介绍如何访问表达式树中的每个节点，同时生成该表达式树的已修改副本。
ms.date: 06/20/2016
ms.technology: csharp-advanced-concepts
ms.assetid: b453c591-acc6-4e08-8175-97e5bc65958e
ms.openlocfilehash: cb64e79d915a5c5567d5a3d25f1d747df9687c87
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90537458"
---
# <a name="translate-expression-trees"></a>转换表达式树

[上一部分 -- 生成表达式](expression-trees-building.md)

在此最后一节中，将介绍如何访问表达式树中的每个节点，同时生成该表达式树的已修改副本。 以下是在两个重要方案中将使用的技巧。 第一种是了解表达式树表示的算法，以便可以将其转换到另一个环境中。 第二种是何时更改已创建的算法。 这可能是为了添加日志记录、拦截方法调用并跟踪它们，或其他目的。

## <a name="translating-is-visiting"></a>转换即访问

生成的用于转换表达式树的代码是你已看到的用于访问树中所有节点的代码的扩展。 转换表达式树时，会访问所有节点，并在访问它们的同时生成新树。 新树可包含对原始节点的引用或已放置在树中的新节点。

让我们通过访问表达式树，并创建具有一些替换节点的新树，来查看其工作原理。 在此示例中，我们将任何常数替换为其十倍大的常数。
要么就保持表达式树不变。 我们通过将常数节点替换为执行乘法运算的新节点来进行此替换，而不必阅读常数的值并将其替换为新的常数。

此处，在找到常数节点后，创建新乘法节点（其子节点是原始常数和常数 `10`）：

```csharp
private static Expression ReplaceNodes(Expression original)
{
    if (original.NodeType == ExpressionType.Constant)
    {
        return Expression.Multiply(original, Expression.Constant(10));
    }
    else if (original.NodeType == ExpressionType.Add)
    {
        var binaryExpression = (BinaryExpression)original;
        return Expression.Add(
            ReplaceNodes(binaryExpression.Left),
            ReplaceNodes(binaryExpression.Right));
    }
    return original;
}
```

通过替换原始节点，将形成一个包含修改的新树。 可以通过编译并执行替换的树对此进行验证。

```csharp
var one = Expression.Constant(1, typeof(int));
var two = Expression.Constant(2, typeof(int));
var addition = Expression.Add(one, two);
var sum = ReplaceNodes(addition);
var executableFunc = Expression.Lambda(sum);

var func = (Func<int>)executableFunc.Compile();
var answer = func();
Console.WriteLine(answer);
```

生成新树是两者的结合：访问现有树中的节点，和创建新节点并将其插入树中。

此示例演示了表达式树不可变这一点的重要性。 请注意，上面创建的新树混合了新创建的节点和现有树中的节点。 这是安全的，因为现有树中的节点无法进行修改。 这可以极大提高内存效率。
相同的节点可能会在整个树或多个表达式树中遍历使用。 由于不能修改节点，因此可以在需要时随时重用相同的节点。

## <a name="traversing-and-executing-an-addition"></a>遍历并执行加法

让我们通过生成遍历加法节点的树并计算结果的第二个访问者来对此进行验证。 可以通过对目前见到的访问者进行一些修改来执行此操作。 在此新版本中，访问者将返回到目前为止加法运算的部分总和。 对于常数表达式，该总和即为常数表达式的值。 对于加法表达式，遍历这些树后，其结果为左操作数和右操作数的总和。

```csharp
var one = Expression.Constant(1, typeof(int));
var two = Expression.Constant(2, typeof(int));
var three= Expression.Constant(3, typeof(int));
var four = Expression.Constant(4, typeof(int));
var addition = Expression.Add(one, two);
var add2 = Expression.Add(three, four);
var sum = Expression.Add(addition, add2);

// Declare the delegate, so we can call it
// from itself recursively:
Func<Expression, int> aggregate = null;
// Aggregate, return constants, or the sum of the left and right operand.
// Major simplification: Assume every binary expression is an addition.
aggregate = (exp) =>
    exp.NodeType == ExpressionType.Constant ?
    (int)((ConstantExpression)exp).Value :
    aggregate(((BinaryExpression)exp).Left) + aggregate(((BinaryExpression)exp).Right);

var theSum = aggregate(sum);
Console.WriteLine(theSum);
```

此处有相当多的代码，但这些概念是非常容易理解的。
此代码访问首次深度搜索后的子级。 当它遇到常数节点时，访问者将返回该常数的值。 访问者访问这两个子级之后，这些子级将计算出为该子树计算的总和。 加法节点现在可以计算其总和。
在访问了表达式树中的所有节点后，将计算出总和。 可以通过在调试器中运行示例并跟踪执行来跟踪执行。

让我们通过遍历树，来更轻松地跟踪如何分析节点以及如何计算总和。 下面是包含大量跟踪信息的聚合方法的更新版本：

```csharp
private static int Aggregate(Expression exp)
{
    if (exp.NodeType == ExpressionType.Constant)
    {
        var constantExp = (ConstantExpression)exp;
        Console.Error.WriteLine($"Found Constant: {constantExp.Value}");
        return (int)constantExp.Value;
    }
    else if (exp.NodeType == ExpressionType.Add)
    {
        var addExp = (BinaryExpression)exp;
        Console.Error.WriteLine("Found Addition Expression");
        Console.Error.WriteLine("Computing Left node");
        var leftOperand = Aggregate(addExp.Left);
        Console.Error.WriteLine($"Left is: {leftOperand}");
        Console.Error.WriteLine("Computing Right node");
        var rightOperand = Aggregate(addExp.Right);
        Console.Error.WriteLine($"Right is: {rightOperand}");
        var sum = leftOperand + rightOperand;
        Console.Error.WriteLine($"Computed sum: {sum}");
        return sum;
    }
    else throw new NotSupportedException("Haven't written this yet");
}
```

在同一表达式中运行该版本将生成以下输出：

```output
10
Found Addition Expression
Computing Left node
Found Addition Expression
Computing Left node
Found Constant: 1
Left is: 1
Computing Right node
Found Constant: 2
Right is: 2
Computed sum: 3
Left is: 3
Computing Right node
Found Addition Expression
Computing Left node
Found Constant: 3
Left is: 3
Computing Right node
Found Constant: 4
Right is: 4
Computed sum: 7
Right is: 7
Computed sum: 10
10
```

跟踪输出，并在上面的代码中跟随。 应当能够看出代码如何在遍历树的同时访问代码和计算总和，并得出总和。

现在，让我们来看看另一个运行，其表达式由 `sum1` 给出：

```csharp
Expression<Func<int> sum1 = () => 1 + (2 + (3 + 4));
```

下面是通过检查此表达式得到的输出：

```output
Found Addition Expression
Computing Left node
Found Constant: 1
Left is: 1
Computing Right node
Found Addition Expression
Computing Left node
Found Constant: 2
Left is: 2
Computing Right node
Found Addition Expression
Computing Left node
Found Constant: 3
Left is: 3
Computing Right node
Found Constant: 4
Right is: 4
Computed sum: 7
Right is: 7
Computed sum: 9
Right is: 9
Computed sum: 10
10
```

虽然最终结果是相同的，但树遍历完全不同。 节点的访问顺序不同，因为树是以首先发生的不同运算构造的。

## <a name="learning-more"></a>了解更多信息

此示例演示了要生成的用于遍历和解释表达式树表示的算法的代码的一小部分。 有关生成将表达式树转换为其他语言的通用库所需的所有工作的完整讨论，请阅读 Matt Warren 的[这一系列](/archive/blogs/mattwar/linq-building-an-iqueryable-provider-series)。 它详细介绍了如何转换可能在表达式树中找到的任意代码。

希望你现在已经见识到了表达式树的真正强大功能。
你可以检查一组代码、对该代码进行任意更改，并执行更改后的版本。 由于表达式树是不可变的，你可以通过使用现有树的组件创建新树。 这样可使创建修改的表达式树所需的内存量最小。

[下一部分 -- 总结](expression-trees-summary.md)
