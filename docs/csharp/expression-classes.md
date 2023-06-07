---
title: 支持表达式树的框架类型
description: 了解支持表达式树的框架类型、创建表达式树和使用表达式树 API 的方法。
ms.date: 06/20/2016
ms.technology: csharp-advanced-concepts
ms.assetid: e9c85021-0d36-48af-91b7-aaaa66f22654
ms.openlocfilehash: 548f5ba6a2de00d9556621791515555b6f6a325c
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91180434"
---
# <a name="framework-types-supporting-expression-trees"></a>支持表达式树的框架类型

[上一步 - 已解释的表达式树](expression-trees-explained.md)

存在可与表达式树配合使用的 .NET Core framework 中的类的大型列表。
可以在 <xref:System.Linq.Expressions> 查看完整列表。
让我们来了解一下 framework 类的设计方式，而不是逐一查看完整列表。

在语言设计中，表达式是可计算并返回值的代码主体。 表达式可能非常简单：常数表达式 `1` 返回常数值 1。 也可能较复杂：表达式 `(-B + Math.Sqrt(B*B - 4 * A * C)) / (2 * A)` 返回二次方程的一个根（若方程有解）。  

## <a name="it-all-starts-with-systemlinqexpression"></a>这一切都始于 System.Linq.Expression

使用表达式树的其中一个难点在于许多不同类型的表达式在程序中的许多位置均有效。 请思考一个赋值表达式。 赋值的右侧可以是常数值、变量、方法调用表达式或其他内容。 语言灵活性意味着，遍历表达式树时，可能会在树的节点中的任意位置遇到许多不同的表达式类型。 因此，使用基表达式类型时，理解起来最简单。 但是，有时你需要了解更多内容。
为此，基表达式类包含 `NodeType` 属性。
它将返回 `ExpressionType`，这是可能的表达式类型的枚举。
知道节点的类型后，可以将其转换为该类型，并执行特定操作（如果知道表达式节点的类型）。 可以搜索特定的节点类型，然后使用这种表达式的特定属性。

例如，此代码将打印变量访问表达式的变量的名称。 我的做法是，先查看节点类型，再转换为变量访问表达式，然后查看特定表达式类型的属性：

```csharp
Expression<Func<int, int>> addFive = (num) => num + 5;

if (addFive.NodeType == ExpressionType.Lambda)
{
    var lambdaExp = (LambdaExpression)addFive;

    var parameter = lambdaExp.Parameters.First();

    Console.WriteLine(parameter.Name);
    Console.WriteLine(parameter.Type);
}
```

## <a name="creating-expression-trees"></a>创建表达式树

`System.Linq.Expression` 类还包含许多创建表达式的静态方法。 这些方法使用为子节点提供的参数创建表达式节点。 通过这种方式，可以从其叶节点构建一个表达式。 例如，此代码将生成一个 Add 表达式：

```csharp
// Addition is an add expression for "1 + 2"
var one = Expression.Constant(1, typeof(int));
var two = Expression.Constant(2, typeof(int));
var addition = Expression.Add(one, two);
```

从这个简单的示例中，你会发现创建和使用表达式树涉及了许多类型。 该复杂性是提供由 C# 语言提供的丰富词汇的功能所必需的。

## <a name="navigating-the-apis"></a>导航 API

存在映射到 C# 语言的几乎所有语法元素的表达式节点类型。 每种类型都有针对该种语言元素的特定方法。 需要一次性记住的内容很多。 我不会记住所有内容，而是会采用有关使用表达式树的技巧，如下所示：

1. 查看 `ExpressionType` 枚举的成员以确定应检查的可能节点。 如果想要遍历和理解表达式树，这将非常有用。
2. 查看 `Expression` 类的静态成员以生成表达式。 这些方法可以从其子节点集生成任何表达式类型。
3. 查看 `ExpressionVisitor` 类，以生成一个经过修改的表达式树。

如果查看这三个部分的每个部分，可以发现更多内容。 通过使用这三个步骤中的任意一个步骤，你一定会发现所需的内容。

 [下一步 - 执行表达式树](expression-trees-execution.md)
