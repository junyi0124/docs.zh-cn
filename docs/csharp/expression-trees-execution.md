---
title: 执行表达式树
description: 介绍通过将表达式树转换为可执行的中间语言 (IL) 指令，执行表达式树。
ms.date: 06/20/2016
ms.technology: csharp-advanced-concepts
ms.assetid: 109e0ac5-2a9c-48b4-ac68-9b6219cdbccf
ms.openlocfilehash: 802a83f52f9c05a99c3f49f8f6511eff81ef3eaa
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "79146016"
---
# <a name="executing-expression-trees"></a>执行表达式树

[上一部分 -- 支持表达式树的框架类型](expression-classes.md)

*表达式树*是表示一些代码的数据结构。
它不是已编译且可执行的代码。 如果想要执行由表达式树表示的 .NET 代码，则必须将其转换为可执行的 IL 指令。

## <a name="lambda-expressions-to-functions"></a>Lambda 表达式到函数

可以将任何 LambdaExpression 或派生自 LambdaExpression 的任何类型转换为可执行的 IL。 其他表达式类型不能直接转换为代码。 此限制在实践中影响不大。 Lambda 表达式是你可通过转换为可执行的中间语言 (IL) 来执行的唯一表达式类型。 （思考直接执行 `ConstantExpression` 意味着什么。 这是否意味着任何用处？）`LambdaExpression` 或派生自 `LambdaExpression` 的类型的任何表达式树均可转换为 IL。
表达式类型 `Expression<TDelegate>` 是 .NET Core 库中的唯一具体示例。 它用于表示映射到任何委托类型的表达式。 由于此类型映射到一个委托类型，因此 .NET 可以检查表达式，并为匹配 lambda 表达式签名的适当委托生成 IL。

在大多数情况下，这将在表达式和其对应的委托之间创建简单映射。 例如，由 `Expression<Func<int>>` 表示的表达式树将被转换为 `Func<int>` 类型的委托。 对于具有任何返回类型和参数列表的 Lambda 表达式，存在这样的委托类型：该类型是由该 Lambda 表达式表示的可执行代码的目标类型。

`LambdaExpression` 类型包含用于将表达式树转换为可执行代码的 `Compile` 和 `CompileToMethod` 成员。 `Compile` 方法创建委托。 `CompileToMethod` 方法通过表示表达式树的已编译输出的 IL 更新 `MethodBuilder` 对象。 请注意，`CompileToMethod` 仅在完整的桌面框架中可用，不能用于 .NET Core。

还可以选择性地提供 `DebugInfoGenerator`，它将接收生成的委托对象的符号调试信息。 这让你可以将表达式树转换为委托对象，并拥有生成的委托的完整调试信息。

使用下面的代码将表达式转换为委托：

```csharp
Expression<Func<int>> add = () => 1 + 2;
var func = add.Compile(); // Create Delegate
var answer = func(); // Invoke Delegate
Console.WriteLine(answer);
```

请注意，该委托类型基于表达式类型。 如果想要以强类型的方式使用委托对象，则必须知道返回类型和参数列表。 `LambdaExpression.Compile()` 方法返回 `Delegate` 类型。 必须将其转换为正确的委托类型，以便使任何编译时工具检查参数列表或返回类型。

## <a name="execution-and-lifetimes"></a>执行和生存期

通过调用在调用 `LambdaExpression.Compile()` 时创建的委托来执行代码。 可以在上面进行查看，其中 `add.Compile()` 返回了一个委托。 通过调用 `func()` 调用该委托将执行代码。

该委托表示表达式树中的代码。 可以保留该委托的句柄并在稍后调用它。 不需要在每次想要执行表达式树所表示的代码时编译表达式树。 （请记住，表达式树是不可变的，且在之后编译同一表达式树将创建执行相同代码的委托。）

在此提醒你不要通过避免不必要的编译调用尝试创建用于提高性能的任何更复杂的缓存机制。 比较两个任意的表达式树，以确定如果它们表示相同的算法，是否也会花费很长的时间来执行。 你可能会发现，通过避免对 `LambdaExpression.Compile()` 的任何额外调用所节省的计算时间将多于执行代码（该代码确定可导致相同可执行代码的两个不同表达式树）所花费的时间。

## <a name="caveats"></a>注意事项

将 lambda 表达式编译为委托并调用该委托是可对表达式树执行的最简单的操作之一。 但是，即使是执行这个简单的操作，也存在一些必须注意的事项。

Lambda 表达式将对表达式中引用的任何局部变量创建闭包。 必须保证作为委托的一部分的任何变量在调用 `Compile` 的位置处和执行结果委托时可用。

一般情况下，编译器会确保这一点。 但是，如果表达式访问实现 `IDisposable` 的变量，则代码可能在表达式树仍保留有对象时释放该对象。

例如，此代码工作正常，因为 `int` 不实现 `IDisposable`：

```csharp
private static Func<int, int> CreateBoundFunc()
{
    var constant = 5; // constant is captured by the expression tree
    Expression<Func<int, int>> expression = (b) => constant + b;
    var rVal = expression.Compile();
    return rVal;
}
```

委托已捕获对局部变量 `constant` 的引用。
在稍后执行 `CreateBoundFunc` 返回的函数之后，可随时访问该变量。

但是，请考虑实现 `IDisposable` 的此（人为设计的）类：

```csharp
public class Resource : IDisposable
{
    private bool isDisposed = false;
    public int Argument
    {
        get
        {
            if (!isDisposed)
                return 5;
            else throw new ObjectDisposedException("Resource");
        }
    }

    public void Dispose()
    {
        isDisposed = true;
    }
}
```

如果将其用于如下所示的表达式中，则在执行 `Resource.Argument` 属性引用的代码时将出现 `ObjectDisposedException`：

```csharp
private static Func<int, int> CreateBoundResource()
{
    using (var constant = new Resource()) // constant is captured by the expression tree
    {
        Expression<Func<int, int>> expression = (b) => constant.Argument + b;
        var rVal = expression.Compile();
        return rVal;
    }
}
```

从此方法返回的委托已对释放了的 `constant` 对象闭包。 （它已被释放，因为它已在 `using` 语句中进行声明。）

现在，在执行从此方法返回的委托时，将在执行时引发 `ObjectDisposedException`。

出现表示编译时构造的运行时错误确实很奇怪，但这是使用表达式树时的正常现象。

此问题存在大量的排列，因此很难提供用于避免此问题的一般性指导。 定义表达式时，请谨慎访问局部变量，且在创建可由公共 API 返回的表达式树时，谨慎访问当前对象（由 `this` 表示）中的状态。

表达式中的代码可能引用其他程序集中的方法或属性。 对表达式进行定义、编译或在调用结果委托时，该程序集必须可访问。 在它不存在的情况下，将遇到 `ReferencedAssemblyNotFoundException`。

## <a name="summary"></a>总结

可以编译表示 lambda 表达式的表达式树，以创建可执行的委托。 这提供了一种机制，用于执行表达式树所表示的代码。

表达式树表示会为创建的任意给定构造执行的代码。 只要编译和执行代码的环境匹配创建表达式的环境，则一切将按预期进行。 如果未按预期进行，那么错误也是很容易预知的，并且将在使用表达式树的任何代码的第一个测试中捕获这些错误。

[下一部分 -- 解释表达式](expression-trees-interpreting.md)
