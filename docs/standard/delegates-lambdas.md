---
title: 委托和 lambda
description: 了解委托（定义指定特定方法签名的类型）如何直接进行调用或传递到另一种方法进行调用。
author: richlander
ms.author: wiwagn
ms.date: 06/20/2016
ms.assetid: fe2e4b4c-6483-4106-a4b4-a33e2e306591
ms.openlocfilehash: 9be4fcdc29eac433dc7416578acbc664ac2e431c
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94821679"
---
# <a name="delegates-and-lambdas"></a>委托和 lambda

委托定义指定特定方法签名的类型。 可将满足此签名的方法（静态或实例）分配给该类型的变量，然后（使用适当参数）直接调用该方法，或将其作为参数本身传递给另一方法再进行调用。 以下示例演示了委托的用法。

```csharp
using System;
using System.Linq;

public class Program
{
    public delegate string Reverse(string s);

    static string ReverseString(string s)
    {
        return new string(s.Reverse().ToArray());
    }

    static void Main(string[] args)
    {
        Reverse rev = ReverseString;

        Console.WriteLine(rev("a string"));
    }
}
```

* `public delegate string Reverse(string s);` 行创建特定签名的委托类型，在本例中即接受字符串参数并返回字符串参数的方法。
* `static string ReverseString(string s)` 方法与定义的委托类型具有完全相同的签名，用于实现委托。
* `Reverse rev = ReverseString;` 行显示可将方法分配给相应委托类型的变量。
* `Console.WriteLine(rev("a string"));` 行演示如何使用委托类型的变量来调用委托。

为简化开发过程，.NET 包含一组委托类型，程序员可重用这些类型而无需创建新类型。 这些类型是 `Func<>`、`Action<>` 和 `Predicate<>`，可以使用它们而无需定义新的委托类型。 这三种类型之间有一些区别，与它们的预期使用方式有关：

* `Action<>` 用于需要使用委托参数执行操作的情况。 它所封装的方法不返回值。
* `Func<>` 通常用于现有转换的情况，也就是说需要将委托参数转换为其他结果时。 投影是一个很好的示例。 它所封装的方法返回指定值。
* `Predicate<>` 用于需要确定参数是否满足委托条件的情况。 它也可以编写为 `Func<T, bool>`，这意味着方法返回布尔值。

现在可使用 `Func<>` 委托而非自定义类型重新编写上述示例。 程序将照旧继续运行。

```csharp
using System;
using System.Linq;

public class Program
{
    static string ReverseString(string s)
    {
        return new string(s.Reverse().ToArray());
    }

    static void Main(string[] args)
    {
        Func<string, string> rev = ReverseString;

        Console.WriteLine(rev("a string"));
    }
}
```

对于此简单示例而言，在 `Main` 方法之外定义方法似乎有些多余。 .NET Framework 2.0 引入了匿名委托的概念，使你可以创建“内联”委托，而无需指定任何其他类型或方法。

在下面的示例中，匿名委托将列表筛选为只包含偶数，然后将它们打印到控制台。

```csharp
using System;
using System.Collections.Generic;

public class Program
{
    public static void Main(string[] args)
    {
        List<int> list = new List<int>();

        for (int i = 1; i <= 100; i++)
        {
            list.Add(i);
        }

        List<int> result = list.FindAll(
          delegate (int no)
          {
              return (no % 2 == 0);
          }
        );

        foreach (var item in result)
        {
            Console.WriteLine(item);
        }
    }
}
```

如你所见，该委托的正文只是一组表达式，其他所有委托也是如此。 但它并非单独定义，而是在调用 <xref:System.Collections.Generic.List%601.FindAll%2A?displayProperty=nameWithType> 方法时临时引入。

但是，即使使用此方法，仍有许多可以丢弃的代码。 此时就需要使用 *lambda 表达式*。 lambda 表达式（或简称“lambda”）在 C# 3.0 中作为语言集成查询 (LINQ) 的核心构建基块被引入。 这种表达式只是使用委托的更方便的语法。 它们将声明签名和方法正文，但在分配到委托之前没有自己的正式标识。 与委托不同，可将其作为事件注册的右侧内容或在各种 LINQ 子句和方法中直接分配。

由于 lambda 表达式只是指定委托的另一种方式，因此应可重新编写上述示例，令其使用 lambda 表达式而不是匿名委托。

```csharp
using System;
using System.Collections.Generic;

public class Program
{
    public static void Main(string[] args)
    {
        List<int> list = new List<int>();

        for (int i = 1; i <= 100; i++)
        {
            list.Add(i);
        }

        List<int> result = list.FindAll(i => i % 2 == 0);

        foreach (var item in result)
        {
            Console.WriteLine(item);
        }
    }
}
```

在前面的示例中，所使用的 Lambda 表达式为 `i => i % 2 == 0`。 同样，这只是使用委托的便捷语法。 内部原理与匿名委托相似。

再次强调，lambda 只是委托，这意味着可将其顺利用作事件处理程序，如以下代码片段所示。

```csharp
public MainWindow()
{
    InitializeComponent();

    Loaded += (o, e) =>
    {
        this.Title = "Loaded";
    };
}
```

此上下文中的 `+=` 运算符用于订阅[事件](../csharp/language-reference/keywords/event.md)。 有关详细信息，请参阅[如何订阅和取消订阅事件](../csharp/programming-guide/events/how-to-subscribe-to-and-unsubscribe-from-events.md)。

## <a name="further-reading-and-resources"></a>其他阅读材料和资源

* [委托](../csharp/programming-guide/delegates/index.md)
* [匿名函数](../csharp/programming-guide/statements-expressions-operators/anonymous-functions.md)
* [Lambda 表达式](../csharp/language-reference/operators/lambda-expressions.md)
