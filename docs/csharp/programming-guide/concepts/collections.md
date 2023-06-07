---
title: 集合 (C#)
description: 了解 C# 中的集合，这些集合用于处理对象组。 集合可随着应用程序更改的需要动态地放大和缩小。
ms.date: 07/20/2015
ms.assetid: 317d7dc3-8587-4873-8b3e-556f86497939
ms.openlocfilehash: 2375980e2915d4daa5a221a94eee2aea41959852
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/22/2020
ms.locfileid: "86924924"
---
# <a name="collections-c"></a>集合 (C#)

对于许多应用程序，你会想要创建和管理相关对象的组。 有两种方法对对象进行分组：通过创建对象的数组，以及通过创建对象的集合。

数组最适用于创建和使用固定数量的强类型化对象。 有关数组的信息，请参阅[数组](../arrays/index.md)。

集合提供更灵活的方式来使用对象组。 与数组不同，你使用的对象组随着应用程序更改的需要动态地放大和缩小。 对于某些集合，你可以为放入集合中的任何对象分配一个密钥，这样你便可以使用该密钥快速检索此对象。

集合是一个类，因此必须在向该集合添加元素之前，声明类的实例。

如果集合中只包含一种数据类型的元素，则可以使用 <xref:System.Collections.Generic?displayProperty=nameWithType> 命名空间中的一个类。 泛型集合强制类型安全，因此无法向其添加任何其他数据类型。 当你从泛型集合检索元素时，你无需确定其数据类型或对其进行转换。

> [!NOTE]
> 在本主题的示例中，针对 `System.Collections.Generic` 和 `System.Linq` 命名空间包括 [using](../../language-reference/keywords/using-directive.md) 指令。

 **在本主题中**

- [使用简单集合](#BKMK_SimpleCollection)

- [集合的类型](#BKMK_KindsOfCollections)

  - [System.Collections.Generic 类](#BKMK_Generic)

  - [System.Collections.Concurrent 类](#BKMK_Concurrent)

  - [System.Collections 类](#BKMK_Collections)

- [实现键/值对集合](#BKMK_KeyValuePairs)

- [使用 LINQ 访问集合](#BKMK_LINQ)

- [对集合排序](#BKMK_Sorting)

- [定义自定义集合](#BKMK_CustomCollection)

- [迭代器](#BKMK_Iterators)

<a name="BKMK_SimpleCollection"></a>

## <a name="using-a-simple-collection"></a>使用简单集合

本部分中的示例使用泛型 <xref:System.Collections.Generic.List%601> 类，通过此类可使用对象的强类型列表。

以下示例创建字符串列表，并通过使用 [foreach](../../language-reference/keywords/foreach-in.md) 语句循环访问字符串。

```csharp
// Create a list of strings.
var salmons = new List<string>();
salmons.Add("chinook");
salmons.Add("coho");
salmons.Add("pink");
salmons.Add("sockeye");

// Iterate through the list.
foreach (var salmon in salmons)
{
    Console.Write(salmon + " ");
}
// Output: chinook coho pink sockeye
```

如果集合中的内容是事先已知的，则可以使用集合初始值设定项来初始化集合。 有关详细信息，请参阅[对象和集合初始值设定项](../classes-and-structs/object-and-collection-initializers.md)。

以下示例与上一示例相同，除了有一个集合初始值设定项用于将元素添加到集合。

```csharp
// Create a list of strings by using a
// collection initializer.
var salmons = new List<string> { "chinook", "coho", "pink", "sockeye" };

// Iterate through the list.
foreach (var salmon in salmons)
{
    Console.Write(salmon + " ");
}
// Output: chinook coho pink sockeye
```

可以使用 [for](../../language-reference/keywords/for.md) 语句，而不是 `foreach` 语句来循环访问集合。 通过按索引位置访问集合元素实现此目的。 元素的索引开始于 0，结束于元素计数减 1。

以下示例通过使用 `for` 而不是 `foreach` 循环访问集合中的元素。

```csharp
// Create a list of strings by using a
// collection initializer.
var salmons = new List<string> { "chinook", "coho", "pink", "sockeye" };

for (var index = 0; index < salmons.Count; index++)
{
    Console.Write(salmons[index] + " ");
}
// Output: chinook coho pink sockeye
```

以下示例通过指定要删除的对象，从集合中删除一个元素。

```csharp
// Create a list of strings by using a
// collection initializer.
var salmons = new List<string> { "chinook", "coho", "pink", "sockeye" };

// Remove an element from the list by specifying
// the object.
salmons.Remove("coho");

// Iterate through the list.
foreach (var salmon in salmons)
{
    Console.Write(salmon + " ");
}
// Output: chinook pink sockeye
```

以下示例从一个泛型列表中删除元素。 使用以降序进行循环访问的 [for](../../language-reference/keywords/for.md) 语句，而非 `foreach` 语句。 这是因为 <xref:System.Collections.Generic.List%601.RemoveAt%2A> 方法将导致已移除的元素后的元素的索引值减小。

```csharp
var numbers = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };

// Remove odd numbers.
for (var index = numbers.Count - 1; index >= 0; index--)
{
    if (numbers[index] % 2 == 1)
    {
        // Remove the element by specifying
        // the zero-based index in the list.
        numbers.RemoveAt(index);
    }
}

// Iterate through the list.
// A lambda expression is placed in the ForEach method
// of the List(T) object.
numbers.ForEach(
    number => Console.Write(number + " "));
// Output: 0 2 4 6 8
```

对于 <xref:System.Collections.Generic.List%601> 中的元素类型，还可以定义自己的类。 在下面的示例中，由 <xref:System.Collections.Generic.List%601> 使用的 `Galaxy` 类在代码中定义。

```csharp
private static void IterateThroughList()
{
    var theGalaxies = new List<Galaxy>
        {
            new Galaxy() { Name="Tadpole", MegaLightYears=400},
            new Galaxy() { Name="Pinwheel", MegaLightYears=25},
            new Galaxy() { Name="Milky Way", MegaLightYears=0},
            new Galaxy() { Name="Andromeda", MegaLightYears=3}
        };

    foreach (Galaxy theGalaxy in theGalaxies)
    {
        Console.WriteLine(theGalaxy.Name + "  " + theGalaxy.MegaLightYears);
    }

    // Output:
    //  Tadpole  400
    //  Pinwheel  25
    //  Milky Way  0
    //  Andromeda  3
}

public class Galaxy
{
    public string Name { get; set; }
    public int MegaLightYears { get; set; }
}
```

<a name="BKMK_KindsOfCollections"></a>

## <a name="kinds-of-collections"></a>集合的类型

许多通用集合由 .NET 提供。 每种类型的集合用于特定的用途。

本部分介绍了一些通用集合类：

- <xref:System.Collections.Generic> 类

- <xref:System.Collections.Concurrent> 类

- <xref:System.Collections> 类

<a name="BKMK_Generic"></a>

### <a name="systemcollectionsgeneric-classes"></a>System.Collections.Generic 类

可以使用 <xref:System.Collections.Generic> 命名空间中的某个类来创建泛型集合。 当集合中的所有项都具有相同的数据类型时，泛型集合会非常有用。 泛型集合通过仅允许添加所需的数据类型，强制实施强类型化。

下表列出了 <xref:System.Collections.Generic?displayProperty=nameWithType> 命名空间中的一些常用类：

|类|说明|
|---|---|
|<xref:System.Collections.Generic.Dictionary%602>|表示基于键进行组织的键/值对的集合。|
|<xref:System.Collections.Generic.List%601>|表示可按索引访问的对象的列表。 提供用于对列表进行搜索、排序和修改的方法。|
|<xref:System.Collections.Generic.Queue%601>|表示对象的先进先出 (FIFO) 集合。|
|<xref:System.Collections.Generic.SortedList%602>|表示基于相关的 <xref:System.Collections.Generic.IComparer%601> 实现按键进行排序的键/值对的集合。|
|<xref:System.Collections.Generic.Stack%601>|表示对象的后进先出 (LIFO) 集合。|

有关其他信息，请参阅[常用集合类型](../../../standard/collections/commonly-used-collection-types.md)、[选择集合类](../../../standard/collections/selecting-a-collection-class.md)和 <xref:System.Collections.Generic>。

<a name="BKMK_Concurrent"></a>

### <a name="systemcollectionsconcurrent-classes"></a>System.Collections.Concurrent 类

在 .NET Framework 4 以及更新的版本中，<xref:System.Collections.Concurrent> 命名空间中的集合可提供高效的线程安全操作，以便从多个线程访问集合项。

只要多个线程同时访问集合，就应使用 <xref:System.Collections.Concurrent> 命名空间中的类，而不是 <xref:System.Collections.Generic?displayProperty=nameWithType> 和 <xref:System.Collections?displayProperty=nameWithType> 命名空间中的相应类型。 有关详细信息，请参阅[线程安全集合](../../../standard/collections/thread-safe/index.md)和 <xref:System.Collections.Concurrent>。

包含在 <xref:System.Collections.Concurrent> 命名空间中的一些类为 <xref:System.Collections.Concurrent.BlockingCollection%601>、<xref:System.Collections.Concurrent.ConcurrentDictionary%602>、<xref:System.Collections.Concurrent.ConcurrentQueue%601> 和 <xref:System.Collections.Concurrent.ConcurrentStack%601>。

<a name="BKMK_Collections"></a>

### <a name="systemcollections-classes"></a>System.Collections 类

<xref:System.Collections?displayProperty=nameWithType> 命名空间中的类不会将元素作为特别类型化的对象存储，而是作为 `Object` 类型的对象存储。

只要可能，则应使用 <xref:System.Collections.Generic?displayProperty=nameWithType> 命名空间或 <xref:System.Collections.Concurrent> 命名空间中的泛型集合，而不是 `System.Collections` 命名空间中的旧类型。

下表列出了 `System.Collections` 命名空间中的一些常用类：

|类|描述|
|---|---|
|<xref:System.Collections.ArrayList>|表示对象的数组，这些对象的大小会根据需要动态增加。|
|<xref:System.Collections.Hashtable>|表示根据键的哈希代码进行组织的键/值对的集合。|
|<xref:System.Collections.Queue>|表示对象的先进先出 (FIFO) 集合。|
|<xref:System.Collections.Stack>|表示对象的后进先出 (LIFO) 集合。|

<xref:System.Collections.Specialized> 命名空间提供专门类型化以及强类型化的集合类，例如只包含字符串的集合以及链接列表和混合字典。

<a name="BKMK_KeyValuePairs"></a>

## <a name="implementing-a-collection-of-keyvalue-pairs"></a>实现键/值对集合

<xref:System.Collections.Generic.Dictionary%602> 泛型集合可通过每个元素的键访问集合中的元素。 每次对字典的添加都包含一个值和与其关联的键。 通过使用键来检索值十分快捷，因为 `Dictionary` 类实现为哈希表。

以下示例创建 `Dictionary` 集合并通过使用 `foreach` 语句循环访问字典。

```csharp
private static void IterateThruDictionary()
{
    Dictionary<string, Element> elements = BuildDictionary();

    foreach (KeyValuePair<string, Element> kvp in elements)
    {
        Element theElement = kvp.Value;

        Console.WriteLine("key: " + kvp.Key);
        Console.WriteLine("values: " + theElement.Symbol + " " +
            theElement.Name + " " + theElement.AtomicNumber);
    }
}

private static Dictionary<string, Element> BuildDictionary()
{
    var elements = new Dictionary<string, Element>();

    AddToDictionary(elements, "K", "Potassium", 19);
    AddToDictionary(elements, "Ca", "Calcium", 20);
    AddToDictionary(elements, "Sc", "Scandium", 21);
    AddToDictionary(elements, "Ti", "Titanium", 22);

    return elements;
}

private static void AddToDictionary(Dictionary<string, Element> elements,
    string symbol, string name, int atomicNumber)
{
    Element theElement = new Element();

    theElement.Symbol = symbol;
    theElement.Name = name;
    theElement.AtomicNumber = atomicNumber;

    elements.Add(key: theElement.Symbol, value: theElement);
}

public class Element
{
    public string Symbol { get; set; }
    public string Name { get; set; }
    public int AtomicNumber { get; set; }
}
```

若要转而使用集合初始值设定项生成 `Dictionary` 集合，可使用以下方法替换 `BuildDictionary` 和 `AddToDictionary`。

```csharp
private static Dictionary<string, Element> BuildDictionary2()
{
    return new Dictionary<string, Element>
    {
        {"K",
            new Element() { Symbol="K", Name="Potassium", AtomicNumber=19}},
        {"Ca",
            new Element() { Symbol="Ca", Name="Calcium", AtomicNumber=20}},
        {"Sc",
            new Element() { Symbol="Sc", Name="Scandium", AtomicNumber=21}},
        {"Ti",
            new Element() { Symbol="Ti", Name="Titanium", AtomicNumber=22}}
    };
}
```

以下示例使用 <xref:System.Collections.Generic.Dictionary%602.ContainsKey%2A> 方法和 `Dictionary` 的 <xref:System.Collections.Generic.Dictionary%602.Item%2A> 属性按键快速查找某个项。 使用 `Item` 属性可通过 C# 中的 `elements[symbol]` 来访问 `elements` 集合中的项。

```csharp
private static void FindInDictionary(string symbol)
{
    Dictionary<string, Element> elements = BuildDictionary();

    if (elements.ContainsKey(symbol) == false)
    {
        Console.WriteLine(symbol + " not found");
    }
    else
    {
        Element theElement = elements[symbol];
        Console.WriteLine("found: " + theElement.Name);
    }
}
```

以下示例则使用 <xref:System.Collections.Generic.Dictionary%602.TryGetValue%2A> 方法按键快速查找某个项。

```csharp
private static void FindInDictionary2(string symbol)
{
    Dictionary<string, Element> elements = BuildDictionary();

    Element theElement = null;
    if (elements.TryGetValue(symbol, out theElement) == false)
        Console.WriteLine(symbol + " not found");
    else
        Console.WriteLine("found: " + theElement.Name);
}
```

<a name="BKMK_LINQ"></a>

## <a name="using-linq-to-access-a-collection"></a>使用 LINQ 访问集合

可以使用 LINQ（语言集成查询）来访问集合。 LINQ 查询提供筛选、排序和分组功能。 有关详细信息，请参阅 [C# 中的 LINQ 入门](linq/index.md)。

以下示例运行一个对泛型 `List` 的 LINQ 查询。 LINQ 查询返回一个包含结果的不同集合。

```csharp
private static void ShowLINQ()
{
    List<Element> elements = BuildList();

    // LINQ Query.
    var subset = from theElement in elements
                 where theElement.AtomicNumber < 22
                 orderby theElement.Name
                 select theElement;

    foreach (Element theElement in subset)
    {
        Console.WriteLine(theElement.Name + " " + theElement.AtomicNumber);
    }

    // Output:
    //  Calcium 20
    //  Potassium 19
    //  Scandium 21
}

private static List<Element> BuildList()
{
    return new List<Element>
    {
        { new Element() { Symbol="K", Name="Potassium", AtomicNumber=19}},
        { new Element() { Symbol="Ca", Name="Calcium", AtomicNumber=20}},
        { new Element() { Symbol="Sc", Name="Scandium", AtomicNumber=21}},
        { new Element() { Symbol="Ti", Name="Titanium", AtomicNumber=22}}
    };
}

public class Element
{
    public string Symbol { get; set; }
    public string Name { get; set; }
    public int AtomicNumber { get; set; }
}
```

<a name="BKMK_Sorting"></a>

## <a name="sorting-a-collection"></a>对集合排序

以下示例阐释了对集合排序的过程。 该示例对 <xref:System.Collections.Generic.List%601> 中存储的 `Car` 类的实例进行排序。 `Car` 类实现 <xref:System.IComparable%601> 接口，此操作需要实现 <xref:System.IComparable%601.CompareTo%2A> 方法。

每次对 <xref:System.IComparable%601.CompareTo%2A> 方法的调用均会执行用于排序的单一比较。 `CompareTo` 方法中用户编写的代码针对当前对象与另一个对象的每个比较返回一个值。 如果当前对象小于另一个对象，则返回的值小于零；如果当前对象大于另一个对象，则返回的值大于零；如果当前对象等于另一个对象，则返回的值等于零。 这使你可以在代码中定义大于、小于和等于条件。

在 `ListCars` 方法中，`cars.Sort()` 语句对列表进行排序。 对 <xref:System.Collections.Generic.List%601> 的 <xref:System.Collections.Generic.List%601.Sort%2A> 方法的此调用将导致为 `List` 中的 `Car` 对象自动调用 `CompareTo` 方法。

```csharp
private static void ListCars()
{
    var cars = new List<Car>
    {
        { new Car() { Name = "car1", Color = "blue", Speed = 20}},
        { new Car() { Name = "car2", Color = "red", Speed = 50}},
        { new Car() { Name = "car3", Color = "green", Speed = 10}},
        { new Car() { Name = "car4", Color = "blue", Speed = 50}},
        { new Car() { Name = "car5", Color = "blue", Speed = 30}},
        { new Car() { Name = "car6", Color = "red", Speed = 60}},
        { new Car() { Name = "car7", Color = "green", Speed = 50}}
    };

    // Sort the cars by color alphabetically, and then by speed
    // in descending order.
    cars.Sort();

    // View all of the cars.
    foreach (Car thisCar in cars)
    {
        Console.Write(thisCar.Color.PadRight(5) + " ");
        Console.Write(thisCar.Speed.ToString() + " ");
        Console.Write(thisCar.Name);
        Console.WriteLine();
    }

    // Output:
    //  blue  50 car4
    //  blue  30 car5
    //  blue  20 car1
    //  green 50 car7
    //  green 10 car3
    //  red   60 car6
    //  red   50 car2
}

public class Car : IComparable<Car>
{
    public string Name { get; set; }
    public int Speed { get; set; }
    public string Color { get; set; }

    public int CompareTo(Car other)
    {
        // A call to this method makes a single comparison that is
        // used for sorting.

        // Determine the relative order of the objects being compared.
        // Sort by color alphabetically, and then by speed in
        // descending order.

        // Compare the colors.
        int compare;
        compare = String.Compare(this.Color, other.Color, true);

        // If the colors are the same, compare the speeds.
        if (compare == 0)
        {
            compare = this.Speed.CompareTo(other.Speed);

            // Use descending order for speed.
            compare = -compare;
        }

        return compare;
    }
}
```

<a name="BKMK_CustomCollection"></a>

## <a name="defining-a-custom-collection"></a>定义自定义集合

可以通过实现 <xref:System.Collections.Generic.IEnumerable%601> 或 <xref:System.Collections.IEnumerable> 接口来定义集合。

尽管可以定义自定义集合，但通常最好使用包含在 .NET 中的集合，这在本文前面的[集合类型](#BKMK_KindsOfCollections)中进行了介绍。

以下示例定义一个名为 `AllColors` 的自定义集合类。 此类实现 <xref:System.Collections.IEnumerable> 接口，此操作需要实现 <xref:System.Collections.IEnumerable.GetEnumerator%2A> 方法。

`GetEnumerator` 方法返回 `ColorEnumerator` 类的一个实例。 `ColorEnumerator` 实现 <xref:System.Collections.IEnumerator> 接口，此操作需要实现 <xref:System.Collections.IEnumerator.Current%2A> 属性、<xref:System.Collections.IEnumerator.MoveNext%2A> 方法以及 <xref:System.Collections.IEnumerator.Reset%2A> 方法。

```csharp
private static void ListColors()
{
    var colors = new AllColors();

    foreach (Color theColor in colors)
    {
        Console.Write(theColor.Name + " ");
    }
    Console.WriteLine();
    // Output: red blue green
}

// Collection class.
public class AllColors : System.Collections.IEnumerable
{
    Color[] _colors =
    {
        new Color() { Name = "red" },
        new Color() { Name = "blue" },
        new Color() { Name = "green" }
    };

    public System.Collections.IEnumerator GetEnumerator()
    {
        return new ColorEnumerator(_colors);

        // Instead of creating a custom enumerator, you could
        // use the GetEnumerator of the array.
        //return _colors.GetEnumerator();
    }

    // Custom enumerator.
    private class ColorEnumerator : System.Collections.IEnumerator
    {
        private Color[] _colors;
        private int _position = -1;

        public ColorEnumerator(Color[] colors)
        {
            _colors = colors;
        }

        object System.Collections.IEnumerator.Current
        {
            get
            {
                return _colors[_position];
            }
        }

        bool System.Collections.IEnumerator.MoveNext()
        {
            _position++;
            return (_position < _colors.Length);
        }

        void System.Collections.IEnumerator.Reset()
        {
            _position = -1;
        }
    }
}

// Element class.
public class Color
{
    public string Name { get; set; }
}
```

<a name="BKMK_Iterators"></a>

## <a name="iterators"></a>迭代器

迭代器用于对集合执行自定义迭代。 迭代器可以是一种方法，或是一个 `get` 访问器。 迭代器使用 [yield return](../../language-reference/keywords/yield.md) 语句返回集合的每一个元素，每次返回一个元素。

通过使用 [foreach](../../language-reference/keywords/foreach-in.md) 语句调用迭代器。 `foreach` 循环的每次迭代都会调用迭代器。 迭代器中到达 `yield return` 语句时，会返回一个表达式，并保留当前在代码中的位置。 下次调用迭代器时，将从该位置重新开始执行。

有关详细信息，请参阅[迭代器 (C#)](./iterators.md)。

下面的示例使用迭代器方法。 迭代器方法具有位于 [for](../../language-reference/keywords/for.md) 循环中的 `yield return` 语句。 在 `ListEvenNumbers` 方法中，`foreach` 语句体的每次迭代都会创建对迭代器方法的调用，并将继续到下一个 `yield return` 语句。

```csharp
private static void ListEvenNumbers()
{
    foreach (int number in EvenSequence(5, 18))
    {
        Console.Write(number.ToString() + " ");
    }
    Console.WriteLine();
    // Output: 6 8 10 12 14 16 18
}

private static IEnumerable<int> EvenSequence(
    int firstNumber, int lastNumber)
{
    // Yield even numbers in the range.
    for (var number = firstNumber; number <= lastNumber; number++)
    {
        if (number % 2 == 0)
        {
            yield return number;
        }
    }
}
```

## <a name="see-also"></a>请参阅

- [对象和集合初始值设定项](../classes-and-structs/object-and-collection-initializers.md)
- [编程概念 (C#)](./index.md)
- [Option Strict 语句](../../../visual-basic/language-reference/statements/option-strict-statement.md)
- [LINQ to Objects (C#)](./linq/linq-to-objects.md)
- [并行 LINQ (PLINQ)](../../../standard/parallel-programming/introduction-to-plinq.md)
- [集合和数据结构](../../../standard/collections/index.md)
- [选择集合类](../../../standard/collections/selecting-a-collection-class.md)
- [集合内的比较和排序](../../../standard/collections/comparisons-and-sorts-within-collections.md)
- [何时使用泛型集合](../../../standard/collections/when-to-use-generic-collections.md)
