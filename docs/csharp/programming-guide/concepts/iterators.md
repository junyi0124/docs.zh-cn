---
title: 循环访问 C# 中的集合
description: 了解如何使用迭代器来逐步迭代集合，例如列表和数组。 通过 foreach 语句或 LINQ 查询从客户端代码中使用迭代器。
ms.date: 08/14/2018
ms.assetid: c93f6dd4-e72a-4a06-be1c-a98b3255b734
ms.openlocfilehash: 310fff68a242812620357517c212ddd5f053775c
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2020
ms.locfileid: "87104251"
---
# <a name="iterators-c"></a>迭代器 (C#)

迭代器可用于逐步迭代集合，例如列表和数组。

迭代器方法或 `get` 访问器可对集合执行自定义迭代。 迭代器方法使用 [yield return](../../language-reference/keywords/yield.md) 语句返回元素，每次返回一个。 到达 `yield return` 语句时，会记住当前在代码中的位置。 下次调用迭代器函数时，将从该位置重新开始执行。

通过 [foreach](../../language-reference/keywords/foreach-in.md) 语句或 LINQ 查询从客户端代码中使用迭代器。

在以下示例中，`foreach` 循环的首次迭代导致 `SomeNumbers` 迭代器方法继续执行，直至到达第一个 `yield return` 语句。 此迭代返回的值为 3，并保留当前在迭代器方法中的位置。 在循环的下次迭代中，迭代器方法的执行将从其暂停的位置继续，直至到达 `yield return` 语句后才会停止。 此迭代返回的值为 5，并再次保留当前在迭代器方法中的位置。 到达迭代器方法的结尾时，循环便已完成。

```csharp
static void Main()
{
    foreach (int number in SomeNumbers())
    {
        Console.Write(number.ToString() + " ");
    }
    // Output: 3 5 8
    Console.ReadKey();
}

public static System.Collections.IEnumerable SomeNumbers()
{
    yield return 3;
    yield return 5;
    yield return 8;
}
```

迭代器方法或 `get` 访问器的返回类型可以是 <xref:System.Collections.IEnumerable>、<xref:System.Collections.Generic.IEnumerable%601>、<xref:System.Collections.IEnumerator> 或 <xref:System.Collections.Generic.IEnumerator%601>。

可以使用 `yield break` 语句来终止迭代。

> [!NOTE]
> 对于本主题中除简单迭代器示例以外的所有示例，请为 `System.Collections` 和 `System.Collections.Generic` 命名空间加入 [using](../../language-reference/keywords/using-directive.md) 指令。

## <a name="simple-iterator"></a>简单的迭代器

下例包含一个位于 [for](../../language-reference/keywords/for.md) 循环内的 `yield return` 语句。 在 `Main` 中，`foreach` 语句体的每次迭代都会创建一个对迭代器函数的调用，并将继续到下一个 `yield return` 语句。

```csharp
static void Main()
{
    foreach (int number in EvenSequence(5, 18))
    {
        Console.Write(number.ToString() + " ");
    }
    // Output: 6 8 10 12 14 16 18
    Console.ReadKey();
}

public static System.Collections.Generic.IEnumerable<int>
    EvenSequence(int firstNumber, int lastNumber)
{
    // Yield even numbers in the range.
    for (int number = firstNumber; number <= lastNumber; number++)
    {
        if (number % 2 == 0)
        {
            yield return number;
        }
    }
}
```

## <a name="creating-a-collection-class"></a>创建集合类

在以下示例中，`DaysOfTheWeek` 类实现 <xref:System.Collections.IEnumerable> 接口，此操作需要 <xref:System.Collections.IEnumerable.GetEnumerator%2A> 方法。 编译器隐式调用 `GetEnumerator` 方法，此方法返回 <xref:System.Collections.IEnumerator>。

`GetEnumerator` 方法通过使用 `yield return` 语句每次返回 1 个字符串。

```csharp
static void Main()
{
    DaysOfTheWeek days = new DaysOfTheWeek();

    foreach (string day in days)
    {
        Console.Write(day + " ");
    }
    // Output: Sun Mon Tue Wed Thu Fri Sat
    Console.ReadKey();
}

public class DaysOfTheWeek : IEnumerable
{
    private string[] days = { "Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat" };

    public IEnumerator GetEnumerator()
    {
        for (int index = 0; index < days.Length; index++)
        {
            // Yield each day of the week.
            yield return days[index];
        }
    }
}
```

下例创建了一个包含动物集合的 `Zoo` 类。

引用类实例 (`theZoo`) 的 `foreach` 语句隐式调用 `GetEnumerator` 方法。 引用 `Birds` 和 `Mammals` 属性的 `foreach` 语句使用 `AnimalsForType` 命名迭代器方法。

```csharp
static void Main()
{
    Zoo theZoo = new Zoo();

    theZoo.AddMammal("Whale");
    theZoo.AddMammal("Rhinoceros");
    theZoo.AddBird("Penguin");
    theZoo.AddBird("Warbler");

    foreach (string name in theZoo)
    {
        Console.Write(name + " ");
    }
    Console.WriteLine();
    // Output: Whale Rhinoceros Penguin Warbler

    foreach (string name in theZoo.Birds)
    {
        Console.Write(name + " ");
    }
    Console.WriteLine();
    // Output: Penguin Warbler

    foreach (string name in theZoo.Mammals)
    {
        Console.Write(name + " ");
    }
    Console.WriteLine();
    // Output: Whale Rhinoceros

    Console.ReadKey();
}

public class Zoo : IEnumerable
{
    // Private members.
    private List<Animal> animals = new List<Animal>();

    // Public methods.
    public void AddMammal(string name)
    {
        animals.Add(new Animal { Name = name, Type = Animal.TypeEnum.Mammal });
    }

    public void AddBird(string name)
    {
        animals.Add(new Animal { Name = name, Type = Animal.TypeEnum.Bird });
    }

    public IEnumerator GetEnumerator()
    {
        foreach (Animal theAnimal in animals)
        {
            yield return theAnimal.Name;
        }
    }

    // Public members.
    public IEnumerable Mammals
    {
        get { return AnimalsForType(Animal.TypeEnum.Mammal); }
    }

    public IEnumerable Birds
    {
        get { return AnimalsForType(Animal.TypeEnum.Bird); }
    }

    // Private methods.
    private IEnumerable AnimalsForType(Animal.TypeEnum type)
    {
        foreach (Animal theAnimal in animals)
        {
            if (theAnimal.Type == type)
            {
                yield return theAnimal.Name;
            }
        }
    }

    // Private class.
    private class Animal
    {
        public enum TypeEnum { Bird, Mammal }

        public string Name { get; set; }
        public TypeEnum Type { get; set; }
    }
}
```

## <a name="using-iterators-with-a-generic-list"></a>对泛型列表使用迭代器

在以下示例中，<xref:System.Collections.Generic.Stack%601> 泛型类实现 <xref:System.Collections.Generic.IEnumerable%601> 泛型接口。 <xref:System.Collections.Generic.Stack%601.Push%2A> 方法将值分配给类型为 `T` 的数组。 <xref:System.Collections.Generic.IEnumerable%601.GetEnumerator%2A> 方法通过使用 `yield return` 语句返回数组值。

除了泛型 <xref:System.Collections.Generic.IEnumerable%601.GetEnumerator%2A> 方法，还必须实现非泛型 <xref:System.Collections.IEnumerable.GetEnumerator%2A> 方法。 这是因为从 <xref:System.Collections.IEnumerable> 继承了 <xref:System.Collections.Generic.IEnumerable%601>。 非泛型实现遵从泛型实现的规则。

本示例使用命名迭代器来支持通过各种方法循环访问同一数据集合。 这些命名迭代器为 `TopToBottom` 和 `BottomToTop` 属性，以及 `TopN` 方法。

`BottomToTop` 属性在 `get` 访问器中使用迭代器。

```csharp
static void Main()
{
    Stack<int> theStack = new Stack<int>();

    //  Add items to the stack.
    for (int number = 0; number <= 9; number++)
    {
        theStack.Push(number);
    }

    // Retrieve items from the stack.
    // foreach is allowed because theStack implements IEnumerable<int>.
    foreach (int number in theStack)
    {
        Console.Write("{0} ", number);
    }
    Console.WriteLine();
    // Output: 9 8 7 6 5 4 3 2 1 0

    // foreach is allowed, because theStack.TopToBottom returns IEnumerable(Of Integer).
    foreach (int number in theStack.TopToBottom)
    {
        Console.Write("{0} ", number);
    }
    Console.WriteLine();
    // Output: 9 8 7 6 5 4 3 2 1 0

    foreach (int number in theStack.BottomToTop)
    {
        Console.Write("{0} ", number);
    }
    Console.WriteLine();
    // Output: 0 1 2 3 4 5 6 7 8 9

    foreach (int number in theStack.TopN(7))
    {
        Console.Write("{0} ", number);
    }
    Console.WriteLine();
    // Output: 9 8 7 6 5 4 3

    Console.ReadKey();
}

public class Stack<T> : IEnumerable<T>
{
    private T[] values = new T[100];
    private int top = 0;

    public void Push(T t)
    {
        values[top] = t;
        top++;
    }
    public T Pop()
    {
        top--;
        return values[top];
    }

    // This method implements the GetEnumerator method. It allows
    // an instance of the class to be used in a foreach statement.
    public IEnumerator<T> GetEnumerator()
    {
        for (int index = top - 1; index >= 0; index--)
        {
            yield return values[index];
        }
    }

    IEnumerator IEnumerable.GetEnumerator()
    {
        return GetEnumerator();
    }

    public IEnumerable<T> TopToBottom
    {
        get { return this; }
    }

    public IEnumerable<T> BottomToTop
    {
        get
        {
            for (int index = 0; index <= top - 1; index++)
            {
                yield return values[index];
            }
        }
    }

    public IEnumerable<T> TopN(int itemsFromTop)
    {
        // Return less than itemsFromTop if necessary.
        int startIndex = itemsFromTop >= top ? 0 : top - itemsFromTop;

        for (int index = top - 1; index >= startIndex; index--)
        {
            yield return values[index];
        }
    }

}
```

## <a name="syntax-information"></a>语法信息

迭代器可用作一种方法，或一个 `get` 访问器。 不能在事件、实例构造函数、静态构造函数或静态终结器中使用迭代器。

必须存在从 `yield return` 语句中的表达式类型到迭代器返回的 `IEnumerable<T>` 类型参数的隐式转换。

在 C# 中，迭代器方法不能有任何 `in`、`ref` 或 `out` 参数。

在 C# 中，`yield` 不是保留字，只有在 `return` 或 `break` 关键字之前使用时才有特殊含义。

## <a name="technical-implementation"></a>技术实现

即使将迭代器编写成方法，编译器也会将其转换为实际上是状态机的嵌套类。 只要客户端代码中的 `foreach` 循环继续，此类就会跟踪迭代器的位置。

若要查看编译器执行的操作，可使用 Ildasm.exe 工具查看为迭代器方法生成的 Microsoft 中间语言代码。

为[类](../../language-reference/keywords/class.md)或[结构](../../language-reference/builtin-types/struct.md)创建迭代器时，不必实现整个 <xref:System.Collections.IEnumerator> 接口。 编译器检测到迭代器时，会自动生成 <xref:System.Collections.IEnumerator> 或 <xref:System.Collections.Generic.IEnumerator%601> 接口的 `Current`、`MoveNext` 和 `Dispose` 方法。

在 `foreach` 循环（或对 `IEnumerator.MoveNext` 的直接调用）的每次后续迭代中，下一个迭代器代码体都会在上一个 `yield return` 语句之后恢复。 然后继续下一个 `yield return` 语句，直至到达迭代器体的结尾，或直至遇到 `yield break` 语句。

迭代器不支持 <xref:System.Collections.IEnumerator.Reset%2A?displayProperty=nameWithType> 方法。 若要从头开始重新迭代，必须获取新的迭代器。 在迭代器方法返回的迭代器上调用 <xref:System.Collections.IEnumerator.Reset%2A> 会引发 <xref:System.NotSupportedException>。

有关其他信息，请参阅 [C# 语言规范](~/_csharplang/spec/classes.md#iterators)。

## <a name="use-of-iterators"></a>迭代器的使用

需要使用复杂代码填充列表序列时，使用迭代器可保持 `foreach` 循环的简单性。 需执行以下操作时，这可能很有用：

- 在第一次 `foreach` 循环迭代之后，修改列表序列。

- 避免在 `foreach` 循环的第一次迭代之前完全加载大型列表。 一个示例是用于加载一批表格行的分页提取。 另一个示例是 <xref:System.IO.DirectoryInfo.EnumerateFiles%2A> 方法，该方法在 .NET 中实现迭代器。

- 在迭代器中封装生成列表。 使用迭代器方法，可生成该列表，然后在循环中产出每个结果。

## <a name="see-also"></a>请参阅

- <xref:System.Collections.Generic>
- <xref:System.Collections.Generic.IEnumerable%601>
- [foreach, in](../../language-reference/keywords/foreach-in.md)
- [yield](../../language-reference/keywords/yield.md)
- [对数组使用 foreach](../arrays/using-foreach-with-arrays.md)
- [泛型](../generics/index.md)
