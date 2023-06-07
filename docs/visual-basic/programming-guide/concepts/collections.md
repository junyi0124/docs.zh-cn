---
title: 集合
ms.date: 07/20/2015
ms.assetid: 5f7749f3-aaf2-4319-b63c-bfa72e1e2b7a
ms.openlocfilehash: 91c6048caf622f21a02032bac31cb2ba5565c54c
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90551062"
---
# <a name="collections-visual-basic"></a>集合 (Visual Basic)

对于许多应用程序，你会想要创建和管理相关对象的组。 有两种方法对对象进行分组：通过创建对象的数组，以及通过创建对象的集合。

数组最适用于创建和使用固定数量的强类型化对象。 有关数组的信息，请参阅[数组](../language-features/arrays/index.md)。

集合提供更灵活的方式来使用对象组。 与数组不同，你使用的对象组随着应用程序更改的需要动态地放大和缩小。 对于某些集合，你可以为放入集合中的任何对象分配一个密钥，这样你便可以使用该密钥快速检索此对象。

集合是一个类，因此必须在向该集合添加元素之前，声明类的实例。

如果集合中只包含一种数据类型的元素，则可以使用 <xref:System.Collections.Generic?displayProperty=nameWithType> 命名空间中的一个类。 泛型集合强制类型安全，因此无法向其添加任何其他数据类型。 当你从泛型集合检索元素时，你无需确定其数据类型或对其进行转换。

> [!NOTE]
> 对于本主题中的示例，包括[Imports](../../language-reference/statements/imports-statement-net-namespace-and-type.md)用于 `System.Collections.Generic` 和命名空间的 Imports 语句 `System.Linq` 。

<a name="BKMK_SimpleCollection"></a>

## <a name="using-a-simple-collection"></a>使用简单集合

本部分中的示例使用泛型 <xref:System.Collections.Generic.List%601> 类，通过此类可使用对象的强类型列表。

下面的示例创建一个字符串列表，然后使用 [For Each .。。下一](../../language-reference/statements/for-each-next-statement.md) 语句。

```vb
' Create a list of strings.
Dim salmons As New List(Of String)
salmons.Add("chinook")
salmons.Add("coho")
salmons.Add("pink")
salmons.Add("sockeye")

' Iterate through the list.
For Each salmon As String In salmons
    Console.Write(salmon & " ")
Next
'Output: chinook coho pink sockeye
```

如果集合中的内容是事先已知的，则可以使用集合初始值设定项来初始化集合。 有关详细信息，请参阅[集合初始值设定项](../language-features/collection-initializers/index.md)。

以下示例与上一示例相同，除了有一个集合初始值设定项用于将元素添加到集合。

```vb
' Create a list of strings by using a
' collection initializer.
Dim salmons As New List(Of String) From
    {"chinook", "coho", "pink", "sockeye"}

For Each salmon As String In salmons
    Console.Write(salmon & " ")
Next
'Output: chinook coho pink sockeye
```

你可以使用 [For .。。Next](../../language-reference/statements/for-next-statement.md) 语句，而不是 `For Each` 语句来循环访问集合。 通过按索引位置访问集合元素实现此目的。 元素的索引开始于 0，结束于元素计数减 1。

以下示例通过使用 `For…Next` 而不是 `For Each` 循环访问集合中的元素。

```vb
Dim salmons As New List(Of String) From
    {"chinook", "coho", "pink", "sockeye"}

For index = 0 To salmons.Count - 1
    Console.Write(salmons(index) & " ")
Next
'Output: chinook coho pink sockeye
```

以下示例通过指定要删除的对象，从集合中删除一个元素。

```vb
' Create a list of strings by using a
' collection initializer.
Dim salmons As New List(Of String) From
    {"chinook", "coho", "pink", "sockeye"}

' Remove an element in the list by specifying
' the object.
salmons.Remove("coho")

For Each salmon As String In salmons
    Console.Write(salmon & " ")
Next
'Output: chinook pink sockeye
```

以下示例从一个泛型列表中删除元素。 而不是 `For Each` 语句， [For .。。使用下一个](../../language-reference/statements/for-next-statement.md) 循环访问顺序的语句。 这是因为 <xref:System.Collections.Generic.List%601.RemoveAt%2A> 方法将导致已移除的元素后的元素的索引值减小。

```vb
Dim numbers As New List(Of Integer) From
    {0, 1, 2, 3, 4, 5, 6, 7, 8, 9}

' Remove odd numbers.
For index As Integer = numbers.Count - 1 To 0 Step -1
    If numbers(index) Mod 2 = 1 Then
        ' Remove the element by specifying
        ' the zero-based index in the list.
        numbers.RemoveAt(index)
    End If
Next

' Iterate through the list.
' A lambda expression is placed in the ForEach method
' of the List(T) object.
numbers.ForEach(
    Sub(number) Console.Write(number & " "))
' Output: 0 2 4 6 8
```

对于 <xref:System.Collections.Generic.List%601> 中的元素类型，还可以定义自己的类。 在下面的示例中，由 <xref:System.Collections.Generic.List%601> 使用的 `Galaxy` 类在代码中定义。

```vb
Private Sub IterateThroughList()
    Dim theGalaxies As New List(Of Galaxy) From
        {
            New Galaxy With {.Name = "Tadpole", .MegaLightYears = 400},
            New Galaxy With {.Name = "Pinwheel", .MegaLightYears = 25},
            New Galaxy With {.Name = "Milky Way", .MegaLightYears = 0},
            New Galaxy With {.Name = "Andromeda", .MegaLightYears = 3}
        }

    For Each theGalaxy In theGalaxies
        With theGalaxy
            Console.WriteLine(.Name & "  " & .MegaLightYears)
        End With
    Next

    ' Output:
    '  Tadpole  400
    '  Pinwheel  25
    '  Milky Way  0
    '  Andromeda  3
End Sub

Public Class Galaxy
    Public Property Name As String
    Public Property MegaLightYears As Integer
End Class
```

<a name="BKMK_KindsOfCollections"></a>

## <a name="kinds-of-collections"></a>集合的类型

许多通用集合由 .NET Framework 提供。 每种类型的集合用于特定的用途。

本部分介绍了一些通用集合类：

- <xref:System.Collections.Generic> 类

- <xref:System.Collections.Concurrent> 类

- <xref:System.Collections> 类

- Visual Basic `Collection` 类

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

有关其他信息，请参阅[常用集合类型](../../../standard/collections/commonly-used-collection-types.md)、[选择集合类](../../../standard/collections/selecting-a-collection-class.md)和 <xref:System.Collections.Generic?displayProperty=nameWithType>。

<a name="BKMK_Concurrent"></a>

### <a name="systemcollectionsconcurrent-classes"></a>System.Collections.Concurrent 类

在 .NET Framework 4 或更新的版本中，<xref:System.Collections.Concurrent> 命名空间中的集合可提供高效的线程安全操作，以便从多个线程访问集合项。

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

<a name="BKMK_VisualBasic"></a>

### <a name="visual-basic-collection-class"></a>Visual Basic 集合类

可以使用 Visual Basic <xref:Microsoft.VisualBasic.Collection> 类以使用数字索引或 `String` 键访问集合项。 你可以向集合对象中添加一个已经或还未指定键的项。 如果你添加了不带有键的项，必须使用其数字索引来访问它。

Visual Basic `Collection` 类将其所有元素存储为 `Object` 类型，因此你可以添加任何数据类型的项。 没有任何保护措施来防止添加不适当的数据类型。

当你使用 Visual Basic `Collection` 类时，集合中的第一项的索引为 1。 这不同于 .NET Framework 集合类，其起始索引为 0。

只要可能，则应使用 <xref:System.Collections.Generic?displayProperty=nameWithType> 命名空间或 <xref:System.Collections.Concurrent> 命名空间中的泛型集合而不是 Visual Basic `Collection` 类。

有关详细信息，请参阅 <xref:Microsoft.VisualBasic.Collection>。

<a name="BKMK_KeyValuePairs"></a>

## <a name="implementing-a-collection-of-keyvalue-pairs"></a>实现键/值对集合

<xref:System.Collections.Generic.Dictionary%602> 泛型集合可通过每个元素的键访问集合中的元素。 每次对字典的添加都包含一个值和与其关联的键。 通过使用键来检索值十分快捷，因为 `Dictionary` 类实现为哈希表。

以下示例创建 `Dictionary` 集合并通过使用 `For Each` 语句循环访问字典。

```vb
Private Sub IterateThroughDictionary()
    Dim elements As Dictionary(Of String, Element) = BuildDictionary()

    For Each kvp As KeyValuePair(Of String, Element) In elements
        Dim theElement As Element = kvp.Value

        Console.WriteLine("key: " & kvp.Key)
        With theElement
            Console.WriteLine("values: " & .Symbol & " " &
                .Name & " " & .AtomicNumber)
        End With
    Next
End Sub

Private Function BuildDictionary() As Dictionary(Of String, Element)
    Dim elements As New Dictionary(Of String, Element)

    AddToDictionary(elements, "K", "Potassium", 19)
    AddToDictionary(elements, "Ca", "Calcium", 20)
    AddToDictionary(elements, "Sc", "Scandium", 21)
    AddToDictionary(elements, "Ti", "Titanium", 22)

    Return elements
End Function

Private Sub AddToDictionary(ByVal elements As Dictionary(Of String, Element),
ByVal symbol As String, ByVal name As String, ByVal atomicNumber As Integer)
    Dim theElement As New Element

    theElement.Symbol = symbol
    theElement.Name = name
    theElement.AtomicNumber = atomicNumber

    elements.Add(Key:=theElement.Symbol, value:=theElement)
End Sub

Public Class Element
    Public Property Symbol As String
    Public Property Name As String
    Public Property AtomicNumber As Integer
End Class
```

若要转而使用集合初始值设定项生成 `Dictionary` 集合，可使用以下方法替换 `BuildDictionary` 和 `AddToDictionary`。

```vb
Private Function BuildDictionary2() As Dictionary(Of String, Element)
    Return New Dictionary(Of String, Element) From
        {
            {"K", New Element With
                {.Symbol = "K", .Name = "Potassium", .AtomicNumber = 19}},
            {"Ca", New Element With
                {.Symbol = "Ca", .Name = "Calcium", .AtomicNumber = 20}},
            {"Sc", New Element With
                {.Symbol = "Sc", .Name = "Scandium", .AtomicNumber = 21}},
            {"Ti", New Element With
                {.Symbol = "Ti", .Name = "Titanium", .AtomicNumber = 22}}
        }
End Function
```

以下示例使用 <xref:System.Collections.Generic.Dictionary%602.ContainsKey%2A> 方法和 `Dictionary` 的 <xref:System.Collections.Generic.Dictionary%602.Item%2A> 属性按键快速查找某个项。 `Item`属性使你能够 `elements` 使用 Visual Basic 中的代码访问集合中的项 `elements(symbol)` 。

```vb
Private Sub FindInDictionary(ByVal symbol As String)
    Dim elements As Dictionary(Of String, Element) = BuildDictionary()

    If elements.ContainsKey(symbol) = False Then
        Console.WriteLine(symbol & " not found")
    Else
        Dim theElement = elements(symbol)
        Console.WriteLine("found: " & theElement.Name)
    End If
End Sub
```

以下示例则使用 <xref:System.Collections.Generic.Dictionary%602.TryGetValue%2A> 方法按键快速查找某个项。

```vb
Private Sub FindInDictionary2(ByVal symbol As String)
    Dim elements As Dictionary(Of String, Element) = BuildDictionary()

    Dim theElement As Element = Nothing
    If elements.TryGetValue(symbol, theElement) = False Then
        Console.WriteLine(symbol & " not found")
    Else
        Console.WriteLine("found: " & theElement.Name)
    End If
End Sub
```

<a name="BKMK_LINQ"></a>

## <a name="using-linq-to-access-a-collection"></a>使用 LINQ 访问集合

可以使用 LINQ（语言集成查询）来访问集合。 LINQ 查询提供筛选、排序和分组功能。 有关详细信息，请参阅 [Visual Basic 中的入门 LINQ](linq/getting-started-with-linq.md)。

以下示例运行一个对泛型 `List` 的 LINQ 查询。 LINQ 查询返回一个包含结果的不同集合。

```vb
Private Sub ShowLINQ()
    Dim elements As List(Of Element) = BuildList()

    ' LINQ Query.
    Dim subset = From theElement In elements
                  Where theElement.AtomicNumber < 22
                  Order By theElement.Name

    For Each theElement In subset
        Console.WriteLine(theElement.Name & " " & theElement.AtomicNumber)
    Next

    ' Output:
    '  Calcium 20
    '  Potassium 19
    '  Scandium 21
End Sub

Private Function BuildList() As List(Of Element)
    Return New List(Of Element) From
        {
            {New Element With
                {.Symbol = "K", .Name = "Potassium", .AtomicNumber = 19}},
            {New Element With
                {.Symbol = "Ca", .Name = "Calcium", .AtomicNumber = 20}},
            {New Element With
                {.Symbol = "Sc", .Name = "Scandium", .AtomicNumber = 21}},
            {New Element With
                {.Symbol = "Ti", .Name = "Titanium", .AtomicNumber = 22}}
        }
End Function

Public Class Element
    Public Property Symbol As String
    Public Property Name As String
    Public Property AtomicNumber As Integer
End Class
```

<a name="BKMK_Sorting"></a>

## <a name="sorting-a-collection"></a>对集合排序

以下示例阐释了对集合排序的过程。 该示例对 <xref:System.Collections.Generic.List%601> 中存储的 `Car` 类的实例进行排序。 `Car` 类实现 <xref:System.IComparable%601> 接口，此操作需要实现 <xref:System.IComparable%601.CompareTo%2A> 方法。

每次对 <xref:System.IComparable%601.CompareTo%2A> 方法的调用均会执行用于排序的单一比较。 `CompareTo` 方法中用户编写的代码针对当前对象与另一个对象的每个比较返回一个值。 如果当前对象小于另一个对象，则返回的值小于零；如果当前对象大于另一个对象，则返回的值大于零；如果当前对象等于另一个对象，则返回的值等于零。 这使你可以在代码中定义大于、小于和等于条件。

在 `ListCars` 方法中，`cars.Sort()` 语句对列表进行排序。 对 <xref:System.Collections.Generic.List%601> 的 <xref:System.Collections.Generic.List%601.Sort%2A> 方法的此调用将导致为 `List` 中的 `Car` 对象自动调用 `CompareTo` 方法。

```vb
Public Sub ListCars()

    ' Create some new cars.
    Dim cars As New List(Of Car) From
    {
        New Car With {.Name = "car1", .Color = "blue", .Speed = 20},
        New Car With {.Name = "car2", .Color = "red", .Speed = 50},
        New Car With {.Name = "car3", .Color = "green", .Speed = 10},
        New Car With {.Name = "car4", .Color = "blue", .Speed = 50},
        New Car With {.Name = "car5", .Color = "blue", .Speed = 30},
        New Car With {.Name = "car6", .Color = "red", .Speed = 60},
        New Car With {.Name = "car7", .Color = "green", .Speed = 50}
    }

    ' Sort the cars by color alphabetically, and then by speed
    ' in descending order.
    cars.Sort()

    ' View all of the cars.
    For Each thisCar As Car In cars
        Console.Write(thisCar.Color.PadRight(5) & " ")
        Console.Write(thisCar.Speed.ToString & " ")
        Console.Write(thisCar.Name)
        Console.WriteLine()
    Next

    ' Output:
    '  blue  50 car4
    '  blue  30 car5
    '  blue  20 car1
    '  green 50 car7
    '  green 10 car3
    '  red   60 car6
    '  red   50 car2
End Sub

Public Class Car
    Implements IComparable(Of Car)

    Public Property Name As String
    Public Property Speed As Integer
    Public Property Color As String

    Public Function CompareTo(ByVal other As Car) As Integer _
        Implements System.IComparable(Of Car).CompareTo
        ' A call to this method makes a single comparison that is
        ' used for sorting.

        ' Determine the relative order of the objects being compared.
        ' Sort by color alphabetically, and then by speed in
        ' descending order.

        ' Compare the colors.
        Dim compare As Integer
        compare = String.Compare(Me.Color, other.Color, True)

        ' If the colors are the same, compare the speeds.
        If compare = 0 Then
            compare = Me.Speed.CompareTo(other.Speed)

            ' Use descending order for speed.
            compare = -compare
        End If

        Return compare
    End Function
End Class
```

<a name="BKMK_CustomCollection"></a>

## <a name="defining-a-custom-collection"></a>定义自定义集合

可以通过实现 <xref:System.Collections.Generic.IEnumerable%601> 或 <xref:System.Collections.IEnumerable> 接口来定义集合。 有关其他信息，请参阅 [枚举集合](/previous-versions/dotnet/netframework-4.0/hwyysy67(v=vs.100))。

尽管可以定义自定义集合，但通常最好使用包含在 .NET Framework 中的集合，这在本主题前面的[集合类型](#kinds-of-collections)中进行了介绍。

以下示例定义一个名为 `AllColors` 的自定义集合类。 此类实现 <xref:System.Collections.IEnumerable> 接口，此操作需要实现 <xref:System.Collections.IEnumerable.GetEnumerator%2A> 方法。

`GetEnumerator` 方法返回 `ColorEnumerator` 类的一个实例。 `ColorEnumerator` 实现 <xref:System.Collections.IEnumerator> 接口，此操作需要实现 <xref:System.Collections.IEnumerator.Current%2A> 属性、<xref:System.Collections.IEnumerator.MoveNext%2A> 方法以及 <xref:System.Collections.IEnumerator.Reset%2A> 方法。

```vb
Public Sub ListColors()
    Dim colors As New AllColors()

    For Each theColor As Color In colors
        Console.Write(theColor.Name & " ")
    Next
    Console.WriteLine()
    ' Output: red blue green
End Sub

' Collection class.
Public Class AllColors
    Implements System.Collections.IEnumerable

    Private _colors() As Color =
    {
        New Color With {.Name = "red"},
        New Color With {.Name = "blue"},
        New Color With {.Name = "green"}
    }

    Public Function GetEnumerator() As System.Collections.IEnumerator _
        Implements System.Collections.IEnumerable.GetEnumerator

        Return New ColorEnumerator(_colors)

        ' Instead of creating a custom enumerator, you could
        ' use the GetEnumerator of the array.
        'Return _colors.GetEnumerator
    End Function

    ' Custom enumerator.
    Private Class ColorEnumerator
        Implements System.Collections.IEnumerator

        Private _colors() As Color
        Private _position As Integer = -1

        Public Sub New(ByVal colors() As Color)
            _colors = colors
        End Sub

        Public ReadOnly Property Current() As Object _
            Implements System.Collections.IEnumerator.Current
            Get
                Return _colors(_position)
            End Get
        End Property

        Public Function MoveNext() As Boolean _
            Implements System.Collections.IEnumerator.MoveNext
            _position += 1
            Return (_position < _colors.Length)
        End Function

        Public Sub Reset() Implements System.Collections.IEnumerator.Reset
            _position = -1
        End Sub
    End Class
End Class

' Element class.
Public Class Color
    Public Property Name As String
End Class
```

<a name="BKMK_Iterators"></a>

## <a name="iterators"></a>迭代器

迭代器用于对集合执行自定义迭代。 迭代器可以是一种方法，或是一个 `get` 访问器。 迭代器使用 [Yield](../../language-reference/statements/yield-statement.md) 语句每次返回集合中的每个元素。

您可以通过 [对每个 .。。下一](../../language-reference/statements/for-each-next-statement.md) 语句。 `For Each` 循环的每次迭代都会调用迭代器。 迭代器中到达 `Yield` 语句时，会返回一个表达式，并保留当前在代码中的位置。 下次调用迭代器时，将从该位置重新开始执行。

有关详细信息，请参阅 [迭代器 (Visual Basic) ](iterators.md)。

下面的示例使用迭代器方法。 迭代器方法的 `Yield` 语句位于 [For .。。下一个](../../language-reference/statements/for-next-statement.md) 循环。 在 `ListEvenNumbers` 方法中，`For Each` 语句体的每次迭代都会创建对迭代器方法的调用，并将继续到下一个 `Yield` 语句。

```vb
Public Sub ListEvenNumbers()
    For Each number As Integer In EvenSequence(5, 18)
        Console.Write(number & " ")
    Next
    Console.WriteLine()
    ' Output: 6 8 10 12 14 16 18
End Sub

Private Iterator Function EvenSequence(
ByVal firstNumber As Integer, ByVal lastNumber As Integer) _
As IEnumerable(Of Integer)

' Yield even numbers in the range.
    For number = firstNumber To lastNumber
        If number Mod 2 = 0 Then
            Yield number
        End If
    Next
End Function
```

## <a name="see-also"></a>请参阅

- [集合初始值设定项](../language-features/collection-initializers/index.md)
- [Visual Basic 的编程概念 () ](index.md)
- [Option Strict 语句](../../language-reference/statements/option-strict-statement.md)
- [LINQ to Objects (Visual Basic)](linq/linq-to-objects.md)
- [并行 LINQ (PLINQ)](../../../standard/parallel-programming/introduction-to-plinq.md)
- [集合和数据结构](../../../standard/collections/index.md)
- [选择集合类](../../../standard/collections/selecting-a-collection-class.md)
- [集合内的比较和排序](../../../standard/collections/comparisons-and-sorts-within-collections.md)
- [何时使用泛型集合](../../../standard/collections/when-to-use-generic-collections.md)
