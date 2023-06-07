---
title: 本地函数 - C# 编程指南
description: C# 中的本地函数是嵌套在另一成员中并且可以从其包含成员中调用的私有方法。
ms.date: 10/16/2020
helpviewer_keywords:
- local functions [C#]
ms.openlocfilehash: 75accda2e40443073274ece4d8964c13a0945dad
ms.sourcegitcommit: dfcbc096ad7908cd58a5f0aeabd2256f05266bac
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2020
ms.locfileid: "92332895"
---
# <a name="local-functions-c-programming-guide"></a>本地函数（C# 编程指南）

从 C# 7.0 开始，C# *支持本地函数* 。 本地函数是一种嵌套在另一成员中的类型的私有方法。 仅能从其包含成员中调用它们。 可以在以下位置中声明和调用本地函数：

- 方法（尤其是迭代器方法和异步方法）
- 构造函数
- 属性访问器
- 事件访问器
- 匿名方法
- Lambda 表达式
- 终结器
- 其他本地函数

但是，不能在 expression-bodied 成员中声明本地函数。

> [!NOTE]
> 在某些情况下，可以使用 lambda 表达式实现本地函数也支持的功能。 有关比较，请参阅[本地函数与 lambda 表达式](#local-functions-vs-lambda-expressions)。

本地函数可使代码意图明确。 任何读取代码的人都可以看到，此方法不可调用，包含方法除外。 对于团队项目，它们也使得其他开发人员无法直接从类或结构中的其他位置错误调用此方法。

## <a name="local-function-syntax"></a>本地函数语法

本地函数被定义为包含成员中的嵌套方法。 其定义具有以下语法：

```csharp
<modifiers> <return-type> <method-name> <parameter-list>
```

可以将以下修饰符用于本地函数：

- [`async`](../../language-reference/keywords/async.md)
- [`unsafe`](../../language-reference/keywords/unsafe.md)
- [`static`](../../language-reference/keywords/static.md)（在 C# 8.0 和更高版本中）。 静态本地函数无法捕获局部变量或实例状态。
- [`extern`](../../language-reference/keywords/extern.md)（在 C# 9.0 和更高版本中）。 外部本地函数必须为 `static`。

在包含成员中定义的所有本地变量（包括其方法参数）都可在非静态本地函数中访问。

与方法定义不同，本地函数定义不能包含成员访问修饰符。 因为所有本地函数都是私有的，包括访问修饰符（如 `private` 关键字）会生成编译器错误 CS0106“修饰符‘private’对于此项无效”。

以下示例定义了一个名为 `AppendPathSeparator` 的本地函数，该函数对于名为 `GetText` 的方法是私有的：

:::code language="csharp" source="snippets/local-functions/Program.cs" id="Basic" :::

从 C# 9.0 开始，你可以将属性应用于本地函数、其参数和类型参数，如以下示例所示：

:::code language="csharp" source="snippets/local-functions/Program.cs" id="WithAttributes" :::

前面的示例使用[特殊属性](../../language-reference/attributes/nullable-analysis.md)来帮助编译器在可为空的上下文中进行静态分析。

## <a name="local-functions-and-exceptions"></a>本地函数和异常

本地函数的一个实用功能是可以允许立即显示异常。 对于方法迭代器，仅在枚举返回的序列时才显示异常，而非在检索迭代器时。 对于异步方法，在等待返回的任务时，将观察到异步方法中引发的任何异常。

以下示例定义 `OddSequence` 方法，用于枚举指定范围中的奇数。 因为它会将一个大于 100 的数字传递到 `OddSequence` 迭代器方法，该方法将引发 <xref:System.ArgumentOutOfRangeException>。 如示例中的输出所示，仅当循环访问数字时才显示异常，而非检索迭代器时。

:::code language="csharp" source="snippets/local-functions/IteratorWithoutLocal.cs" :::

如果将迭代器逻辑放入本地函数，则在检索枚举器时会引发参数验证异常，如下面的示例所示：

:::code language="csharp" source="snippets/local-functions/IteratorWithLocal.cs" :::

你可以通过类似于异步操作的方式来使用本地函数。 等待相应的任务时，异步方法图面中引发的异常。 本地函数允许代码快速失败，并允许同步引发和观察异常。

以下示例使用名为 `GetMultipleAsync` 的异步方法暂停指定的秒数并返回一个值，该值是该秒数的任意倍数。 最大延迟为 5 秒；如果该值大于 5，则结果为 <xref:System.ArgumentOutOfRangeException>。 如下面的示例所示，仅当任务处于等待状态时，才会观察到将值 6 传递到 `GetMultipleAsync` 方法时引发的异常。

:::code language="csharp" source="snippets/local-functions/AsyncWithoutLocal.cs" :::

与方法迭代器类似，你可以重构前面的示例，将异步操作的代码放入本地函数。 如以下示例中的输出所示，调用 `GetMultiple` 方法后，会引发 <xref:System.ArgumentOutOfRangeException>。

:::code language="csharp" source="snippets/local-functions/AsyncWithLocal.cs" :::

## <a name="local-functions-vs-lambda-expressions"></a>本地函数与 Lambda 表达式

乍看之下，本地函数和 [lambda 表达式](../../language-reference/operators/lambda-expressions.md)非常相似。 在许多情况下，选择使用 Lambda 表达式还是本地函数是风格和个人偏好的问题。 但是，应该注意，从两者中选用一种的时机和条件其实是存在差别的。

让我们检查一下阶乘算法的本地函数实现和 lambda 表达式实现之间的差异。 下面是使用本地函数的版本：

:::code language="csharp" source="snippets/local-functions/Program.cs" id="FactorialWithLocal" :::

此版本使用 Lambda 表达式：

:::code language="csharp" source="snippets/local-functions/Program.cs" id="FactorialWithLambda" :::

### <a name="naming"></a>命名

本地函数的命名方式与方法相同。 Lambda 表达式是一种匿名方法，需要分配给 `delegate` 类型的变量，通常是 `Action` 或 `Func` 类型。 声明本地函数时，此过程类似于编写普通方法；声明一个返回类型和一个函数签名。

### <a name="function-signatures-and-lambda-expression-types"></a>函数签名和 Lambda 表达式类型

Lambda 表达式依赖于为其分配的 `Action`/`Func` 变量的类型来确定参数和返回类型。 在本地函数中，因为语法非常类似于编写常规方法，所以参数类型和返回类型已经是函数声明的一部分。

### <a name="definite-assignment"></a>明确赋值

Lambda 表达式是在运行时声明和分配的对象。 若要使用 Lambda 表达式，需要对其进行明确赋值：必须声明要分配给它的 `Action`/`Func` 变量，并为其分配 Lambda 表达式。 请注意，`LambdaFactorial` 必须先声明和初始化 Lambda 表达式 `nthFactorial`，然后再对其进行定义。 否则，会导致分配前引用 `nthFactorial` 时出现编译时错误。

本地函数在编译时定义。 由于未将它们分配给变量，因此可以从范围内的任意代码位置引用它们；在第一个示例 `LocalFunctionFactorial` 中，我们可以在 `return` 语句的上方或下方声明本地函数，而不会触发任何编译器错误。

这些区别意味着使用本地函数创建递归算法会更轻松。 你可以声明和定义一个调用自身的本地函数。 必须声明 Lambda 表达式，赋给默认值，然后才能将其重新赋给引用相同 Lambda 表达式的主体。

### <a name="implementation-as-a-delegate"></a>实现为委托

Lambda 表达式在声明时转换为委托。 本地函数更加灵活，可以像传统方法一样编写，也可以作为委托编写。 只有在用作委托时，本地函数才转换为委托。

如果声明了本地函数，但只是通过像调用方法一样调用该函数来引用该函数，它将不会转换成委托。

### <a name="variable-capture"></a>变量捕获

[明确分配](../../../../_csharplang/spec/variables.md#definite-assignment)的规则也会影响本地函数或 Lambda 表达式捕获的任何变量。 编译器可以执行静态分析，因此本地函数能够在封闭范围内明确分配捕获的变量。 请看以下示例：

```csharp
int M()
{
    int y;
    LocalFunction();
    return y;

    void LocalFunction() => y = 0;
}
```

编译器可以确定 `LocalFunction` 在调用时明确分配 `y`。 因为在 `return` 语句之前调用了 `LocalFunction`，所以在 `return` 语句中明确分配了 `y`。

请注意，当本地函数捕获封闭范围中的变量时，本地函数将作为委托类型实现。

### <a name="heap-allocations"></a>堆分配

根据它们的用途，本地函数可以避免 Lambda 表达式始终需要的堆分配。 如果本地函数永远不会转换为委托，并且本地函数捕获的变量都不会被其他转换为委托的 lambda 或本地函数捕获，则编译器可以避免堆分配。

请看以下异步示例：

:::code language="csharp" source="snippets/local-functions/Program.cs" id="AsyncWithLambda" :::

该 lambda 表达式的闭包包含 `address`、`index` 和 `name` 变量。 就本地函数而言，实现闭包的对象可能为 `struct` 类型。 该结构类型将通过引用传递给本地函数。 实现中的这个差异将保存在分配上。

Lambda 表达式所需的实例化意味着额外的内存分配，后者可能是时间关键代码路径中的性能因素。 本地函数不会产生这种开销。 在以上示例中，本地函数版本具有的分配比 Lambda 表达式版本少 2 个。

如果你知道本地函数不会转换为委托，并且本地函数捕获的变量都不会被其他转换为委托的 lambda 或本地函数捕获，则可以通过将本地函数声明为 `static` 本地函数来确保避免在堆上对其进行分配。 请注意，此功能在 C# 8.0 及更高版本中提供。

> [!NOTE]
> 等效于此方法的本地函数还将类用于闭包。 实现详细信息包括本地函数的闭包是作为 `class` 还是 `struct` 实现。 本地函数可能使用 `struct`，而 lambda 将始终使用 `class`。

:::code language="csharp" source="snippets/local-functions/Program.cs" id="AsyncWithLocal" :::

### <a name="usage-of-the-yield-keyword"></a>`yield` 关键字的用法

在本示例中尚未演示的最后一个优点是，可将本地函数作为迭代器实现，使用 `yield return` 语法生成一系列值。

:::code language="csharp" source="snippets/local-functions/Program.cs" id="YieldReturn" :::

Lambda 表达式中不允许使用 `yield return` 语句，请参阅[编译器错误 CS1621](../../misc/cs1621.md)。

虽然本地函数对 lambda 表达式可能有点冗余，但实际上它们的目的和用法都不一样。 如果想要编写仅从上下文或其他方法中调用的函数，则使用本地函数更高效。

## <a name="see-also"></a>请参阅

- [方法](methods.md)
