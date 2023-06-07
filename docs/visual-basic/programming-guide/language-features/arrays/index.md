---
title: 数组
ms.date: 12/06/2017
f1_keywords:
- vb.Array
helpviewer_keywords:
- arrays [Visual Basic]
- Visual Basic, arrays
ms.assetid: dbf29737-b589-4443-bee6-a27588d9c67e
ms.openlocfilehash: 5093f28f05c5b72294dce9a4e69723acafb31a9f
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84413087"
---
# <a name="arrays-in-visual-basic"></a>Visual Basic 中的数组

数组是一组值，这些值是逻辑上相互关联的*元素*。 例如，数组可能包含语法学校每个年级的学生数;数组中的每个元素都是一年级的学生数。 同样，数组可能包含学生对类的成绩;数组的每个元素都是一个级别。

可以通过单独的变量存储我们的每个数据项。 例如，如果我们的应用程序分析学生评分，则可以为每位学生的成绩使用单独的变量，如 `englishGrade1` 、等 `englishGrade2` 。此方法有三个主要限制：

- 我们必须在设计时了解我们必须处理的成绩。
- 处理大量成绩很快就会变得难以处理。 这进而会使应用程序更有可能出现严重错误。
- 很难维护。 我们添加的每个新评分要求修改、重新编译和重新部署应用程序。

通过使用数组，可以按相同的名称引用这些相关的值，并使用名为*索引*或*下标*的数字来根据元素在数组中的位置来识别单个元素。 数组的索引范围为0到数组中元素总数减1之间的值。 如果使用 Visual Basic 语法来定义数组的大小，则指定其最高索引，而不是数组中元素的总数。 您可以将数组作为一个单元处理，并能够循环访问其元素，而无需确切了解它在设计时所包含的元素数目。

在进行说明之前，请看几个简单的示例：

```vb
' Declare a single-dimension array of 5 numbers.
Dim numbers(4) As Integer

' Declare a single-dimension array and set its 4 values.
Dim numbers = New Integer() {1, 2, 4, 8}

' Change the size of an existing array to 16 elements and retain the current values.
ReDim Preserve numbers(15)

' Redefine the size of an existing array and reset the values.
ReDim numbers(15)

' Declare a 6 x 6 multidimensional array.
Dim matrix(5, 5) As Double

' Declare a 4 x 3 multidimensional array and set array element values.
Dim matrix = New Integer(3, 2) {{1, 2, 3}, {2, 3, 4}, {3, 4, 5}, {4, 5, 6}}

' Declare a jagged array
Dim sales()() As Double = New Double(11)() {}
```

## <a name="array-elements-in-a-simple-array"></a>简单数组中的数组元素

让我们创建一个名为的数组 `students` ，用于存储语法学校每个年级的学生数量。 这些元素的索引范围为 0 到 6。 使用此数组比声明七个变量更简单。

下图显示了 `students` 数组。 对于数组的每个元素：

- 元素索引表示年级（索引 0 表示幼儿园）。

- 元素中包含的值表示每个年级的学生数量。

![显示学生数数组的关系图](./media/index/students-array-elements.gif)

下面的示例包含创建和使用数组的 Visual Basic 代码：

[!code-vb[simple-array](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/simple-array.vb)]

该示例执行三个操作：

- 它声明 `students` 包含七个元素的数组。 `6`数组声明中的数字指示数组中的最后一个索引; 它小于数组中元素的数目。
- 它将值分配给数组中的每个元素。 数组元素通过使用数组名称访问，并在括号中包含单个元素的索引。
- 它列出数组的每个值。 该示例使用 [`For`](../../../language-reference/statements/for-next-statement.md) 语句通过索引号来访问数组的每个元素。

`students`上面的示例中的数组是一维数组，因为它使用一个索引。 使用多个索引或下标的数组称为*多维*。 有关详细信息，请参阅本文的其余部分和[Visual Basic 中的数组维度](array-dimensions.md)。

## <a name="creating-an-array"></a>创建数组

可以通过多种方式定义数组的大小：

- 可在声明数组时指定大小：

  [!code-vb[creating1](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/create-array.vb#1)]

- `New`创建数组时，可以使用子句提供数组的大小：

  [!code-vb[creating2](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/create-array.vb#2)]

如果你有现有的数组，则可以通过使用语句来重新定义其大小 [`ReDim`](../../../language-reference/statements/redim-statement.md) 。 您可以指定该 `ReDim` 语句保留数组中的值，也可以指定它创建一个空数组。 下面的示例演示了用 `ReDim` 语句来修改现有数组的大小的不同用法。

[!code-vb[redimensioning](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/create-array.vb#3)]

有关详细信息，请参阅[ReDim 语句](../../../language-reference/statements/redim-statement.md)。

## <a name="storing-values-in-an-array"></a>在数组中存储值

你可以通过使用类型 `Integer`的索引访问数组中的每个位置。 你可以使用括号内的索引来引用每个数组位置，从而存储和检索数组中的值。 多维数组的索引用逗号（，）分隔。 每个数组维度都需要一个索引。

下面的示例演示在数组中存储和检索值的一些语句。

[!code-vb[store-and-retrieve](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/store-and-retrieve.vb)]

## <a name="populating-an-array-with-array-literals"></a>使用数组文本填充数组

通过使用数组文本，可以在创建数组时，使用初始值集来填充数组。 数组文本包含用逗号分隔的值列表，这些值被括在括号内 (`{}`)。

通过使用数组文本创建数组时，可以提供数组类型或使用类型推理功能来确定数组类型。 下面的示例演示了这两个选项。

[!code-vb[create-with-literals](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/create-array.vb#4)]

使用类型推理时，数组的类型由文本值列表中的*主导类型*确定。 主导类型是数组中所有其他类型可以扩大到的类型。 如果无法确定此唯一类型，基准类型是数组中所有其他类型可以缩小到的唯一类型。 如果无法确定为这两种唯一类型之一，则基准类型是 `Object`。 例如，如果提供给数组文本的值的列表包含 `Integer`、 `Long`和 `Double`类型的值，则生成的数组类型是 `Double`。 由于 `Integer` 和 `Long` 仅扩大到 `Double` ，因此 `Double` 是基准类型。 有关详细信息，请参阅 [Widening and Narrowing Conversions](../data-types/widening-and-narrowing-conversions.md)。

> [!NOTE]
> 只能对定义为类型成员中的局部变量的数组使用类型推理。 如果不存在显式类型定义，则在类级别使用数组文本定义的数组的类型为 `Object[]` 。 有关详细信息，请参阅[局部类型推理](../variables/local-type-inference.md)。

请注意，上面的示例将定义 `values` 为类型的数组， `Double` 即使所有数组文本都为类型，也是如此 `Integer` 。 你可以创建此数组，因为数组文本中的值可以扩大到 `Double` 值。

还可以使用*嵌套的数组文本*来创建和填充多维数组。 嵌套的数组文本必须具有与生成的数组一致的多个维度。 下面的示例通过使用嵌套的数组文本创建一个二维整数数组。

[!code-vb[nested-array-literals](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/create-array.vb#5)]

使用嵌套的数组文本创建和填充数组时，如果嵌套数组文本中的元素数目不匹配，则会发生错误。 如果显式声明数组变量的维数不同于数组文本，也会出现错误。

正如可以对一维数组使用的一样，在使用嵌套的数组文本创建多维数组时，可以依赖类型推理。 推断出的类型是所有嵌套级别的所有数组文本中的所有值的基准类型。 下面的示例 `Double[,]` 从类型为和的值创建一个类型为的二维数组 `Integer` `Double` 。

[!code-vb[nested-type-inference](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/create-array.vb#6)]

有关其他示例，请参阅[如何：在 Visual Basic 中初始化数组变量](how-to-initialize-an-array-variable.md)。

## <a name="iterating-through-an-array"></a>循环访问数组

循环访问数组时，可将数组中的每个元素从最小索引访问到最高级别或从最高到最低。 通常，使用[For .。。下一语句](../../../language-reference/statements/for-next-statement.md)或[每个 .。。Next 语句](../../../language-reference/statements/for-each-next-statement.md)来循环访问数组的元素。 如果不知道数组的上限，可以调用 <xref:System.Array.GetUpperBound%2A?displayProperty=nameWithType> 方法来获取索引的最大值。 尽管最低索引值几乎始终为0，但您可以调用 <xref:System.Array.GetLowerBound%2A?displayProperty=nameWithType> 方法来获取索引的最小值。

下面的示例通过使用语句循环访问一维数组 [`For...Next`](../../../language-reference/statements/for-next-statement.md) 。

[!code-vb[iterate-one-dimensional-array](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/iterate1d.vb)]

下面的示例通过使用语句循环访问多维数组 [`For...Next`](../../../language-reference/statements/for-next-statement.md) 。 <xref:System.Array.GetUpperBound%2A> 方法具有用于指定维度的参数。 `GetUpperBound(0)`返回第一个维度的最高索引，并 `GetUpperBound(1)` 返回第二个维度的最高索引。

[!code-vb[iterate-two-dimensional-array](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/iterate2d.vb)]

下面的示例使用[For Each .。。](../../../language-reference/statements/for-each-next-statement.md)用于循环访问一维数组和二维数组的 Next 语句。

[!code-vb[iterate-for-each-next](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/iterate-for-each-next.vb)]

## <a name="array-size"></a>数组大小

数组的大小是数组所有维度的长度的产物。 它表示数组中当前所包含的元素总数。  例如，下面的示例声明一个二维数组，每个维度中包含四个元素。 如示例中的输出所示，数组的大小为16（或（3 + 1） * （3 + 1）。

[!code-vb[array-size](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/array-size.vb)]

> [!NOTE]
> 对数组大小的讨论不适用于交错数组。 有关交错数组和确定交错数组大小的信息，请参阅[交错](#jagged-arrays)数组部分。

可以通过使用 <xref:System.Array.Length%2A?displayProperty=nameWithType> 属性查找数组大小。 您可以通过使用方法查找多维数组的每个维度的长度 <xref:System.Array.GetLength%2A?displayProperty=nameWithType> 。

可以通过向数组变量分配新数组对象或使用[ `ReDim` 语句](../../../language-reference/statements/redim-statement.md)语句来调整其大小。 下面的示例使用 `ReDim` 语句将100元素数组更改为51元素数组。

[!code-vb[resize-an-array](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/array-size2.vb)]

当处理数组大小时，需要记住几件事情。

|||
|---|---|
|维度长度|每个维度的索引都是从0开始的，这意味着它的范围介于0到其上限之间。 因此，给定维度的长度比该维度已声明的上限大1。|
|长度限制|数组的每个维度的长度限制为数据类型的最大值 `Integer` ，即 <xref:System.Int32.MaxValue?displayProperty=nameWithType> 或（2 ^ 31）-1。 但是，数组的总大小还受到系统上可用的内存限制。 如果你尝试初始化的数组超过可用内存量，则运行时将引发 <xref:System.OutOfMemoryException> 。|
|大小和元素大小|数组的大小独立于其元素的数据类型。 大小始终表示元素总数，而不是在内存中使用的字节数。|
|内存消耗|做出关于数组如何存储在内存中的假设是不可靠的。 由于不同数据宽度的平台上的存储会有所变化，因此同一数组在 64 位系统上可以占用比在 32 位系统上更多的内存。 具体取决于数组初始化时的系统配置，公共语言运行时 (CLR) 可以尽可能地将存储分配到靠近包元素的地方，或者将它们全部在自然硬件边界上对齐。 此外，数组需要存储开销的控制信息，而且每添加一个维度，这种开销随之增加。|

## <a name="the-array-type"></a>数组类型

每个数组都具有一个数据类型，该数据类型不同于其元素的数据类型。 没有一种数据类型能用于所有数组。 相反，数组的数据类型由数组的维度数量或 *“排名”*，以及数组中元素的数据类型确定。 只有当两个数组变量具有相同的秩并且它们的元素具有相同的数据类型时，它们才具有相同的数据类型。 数组的维度长度不会影响数组数据类型。

每个数组都继承自 <xref:System.Array?displayProperty=nameWithType> 类，并且你可以声明一个类型为 `Array` 的变量，但不能创建一个类型为 `Array` 的数组。 例如，虽然下面的代码将变量声明 `arr` 为类型 `Array` 并调用 <xref:System.Array.CreateInstance%2A?displayProperty=nameWithType> 方法来实例化数组，但数组的类型会证明是对象 []。

[!code-vb[array-class](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/array-class.vb)]

此外，[ReDim 语句](../../../language-reference/statements/redim-statement.md)无法对声明为类型 `Array` 的变量执行运算。 出于这些原因，以及对于类型安全，建议将每个数组声明为特定类型。

你可以通过几种方式了解到数组及其元素的数据类型。

- 您可以对 <xref:System.Object.GetType%2A> 变量调用方法来获取 <xref:System.Type> 表示变量的运行时类型的对象。 <xref:System.Type> 对象可在其属性和方法中保存大量信息。
- 可以将变量传递给 <xref:Microsoft.VisualBasic.Information.TypeName%2A> 函数，以获取 `String` 具有运行时类型名称的。

下面的示例调用 `GetType` 方法和 `TypeName` 函数来确定数组的类型。 数组的类型为 `Byte(,)` 。 请注意， <xref:System.Type.BaseType%2A?displayProperty=nameWithType> 属性还指示字节数组的基类型是 <xref:System.Array> 类。

[!code-vb[array-type](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/array-type.vb)]

## <a name="arrays-as-return-values-and-parameters"></a>作为返回值和参数的数组

若要通过 `Function` 过程返回数组，请将数组数据类型和维度数指定为 [Function 语句](../../../language-reference/statements/function-statement.md)的返回类型。 在函数内，声明一个具有相同数据类型和维度数的本地数组变量。 在 [Return 语句](../../../language-reference/statements/return-statement.md)中，添加不带括号的局部数组变量。

若要将作为参数的数组指定到 `Sub` 或 `Function` 步骤中，可将此参数定义为具有指定数据类型和维度数量的数组。 在对过程的调用中，传递具有相同数据类型和维度数量的数组变量。

在下面的示例中， `GetNumbers` 函数返回一个 `Integer()` 类型为的一维数组 `Integer` 。 `ShowNumbers` 过程接受 `Integer()` 参数。

[!code-vb[return-value-and-params](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/return-values-and-params.vb)]

在下面的示例中， `GetNumbersMultiDim` 函数返回一个 `Integer(,)` 类型为的二维数组 `Integer` 。  `ShowNumbersMultiDim` 过程接受 `Integer(,)` 参数。

[!code-vb[multidimensional-return-value](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/return-values-and-params-2d.vb)]

## <a name="jagged-arrays"></a>交错数组

有时应用程序中的数据结构是二维而不是矩形。 例如，你可以使用一个数组来存储有关每个月中每一天的高温数据。 数组的第一个维度表示月份，但第二个维度表示天数，月份中的天数不统一。 *交错数组*（也称为*数组*）是为这种情况设计的。 交错数组是指其元素也是数组的数组。 交错数组和交错数组中的每个元素都可以具有一个或多个维度。

下面的示例使用月份数组，其中的每个元素是天数的数组。 该示例使用交错数组，因为不同的月份有不同的天数。  该示例演示如何创建交错数组，为其赋值，以及检索和显示其值。

[!code-vb[jagged-arrays](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/jagged.vb)]

前面的示例使用循环将值逐个元素地分配给交错数组 `For...Next` 。 还可以使用嵌套的数组文本将值分配给交错数组的元素。 不过，尝试使用嵌套的数组文本（例如 `Dim valuesjagged = {{1, 2}, {2, 3, 4}}` ）会生成编译器错误[BC30568](../../../misc/bc30568.md)。 若要更正此错误，请将内部数组文本括在括号中。 括号强制计算数组文本表达式，并将生成的值用于外部数组文本，如下面的示例所示。

[!code-vb[jagged-array-initialization](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/jagged-assign.vb)]

交错数组是其元素包含数组的一维数组。 因此， <xref:System.Array.Length%2A?displayProperty=nameWithType> 属性和方法将 `Array.GetLength(0)` 返回一维数组中的元素数，并 `Array.GetLength(1)` 引发， <xref:System.IndexOutOfRangeException> 因为交错数组不是多维的。 通过检索每个子数组的属性的值，可以确定每个子数组中的元素数 <xref:System.Array.Length%2A?displayProperty=nameWithType> 。 下面的示例演示如何确定交错数组中的元素数。

[!code-vb[jagged-array-size](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/jagged-length.vb)]

## <a name="zero-length-arrays"></a>长度为零的数组

Visual Basic 区分未初始化的数组（值为的数组 `Nothing` ）和*零长度数组*或空数组（没有元素的数组）。未初始化的数组是指尚未对其进行尺寸或分配任何值的数组。 例如：

```vb
Dim arr() As String
```

使用维度-1 声明长度为零的数组。 例如：

```vb
Dim arrZ(-1) As String
```

在下列情况下，你可能需要创建一个零长度数组：

- 如果不冒 <xref:System.NullReferenceException> 异常，你的代码必须访问类的成员（ <xref:System.Array> 如 <xref:System.Array.Length%2A> 或 <xref:System.Array.Rank%2A> ），或调用 Visual Basic 函数（如） <xref:Microsoft.VisualBasic.Information.UBound%2A> 。

- 您希望通过无需将作为特殊情况进行检查来简化您的代码 `Nothing` 。

- 你的代码与应用程序编程接口 (API) 交互，该接口要求你将一个零长度数组传递到一个或多个过程，或将一个零长度数组返回到一个或多个过程。

## <a name="splitting-an-array"></a>拆分数组

在某些情况下，可能需要将单个数组拆分为多个数组。 这涉及到识别要拆分数组的点，然后将该数组 spitting 为两个或多个单独的数组。

> [!NOTE]
> 本部分不讨论根据某些分隔符将单个字符串拆分为字符串数组。 有关拆分字符串的信息，请参阅 <xref:System.String.Split%2A?displayProperty=nameWithType> 方法。

拆分数组最常见的条件是：

- 数组中的元素数。 例如，你可能想要将多个指定数量的元素的数组拆分为一些大致相等的部分。 为此，可以使用或方法返回的值 <xref:System.Array.Length%2A?displayProperty=nameWithType> <xref:System.Array.GetLength%2A?displayProperty=nameWithType> 。

- 元素的值，它用作指示应拆分数组的位置的分隔符。 可以通过调用和方法搜索特定值 <xref:System.Array.FindIndex%2A?displayProperty=nameWithType> <xref:System.Array.FindLastIndex%2A?displayProperty=nameWithType> 。

确定了应拆分数组的索引后，可以通过调用方法来创建各个数组 <xref:System.Array.Copy%2A?displayProperty=nameWithType> 。

下面的示例将一个数组拆分为大小大致相等的两个数组。 （如果数组元素的总数为奇数，则第一个数组的元素数比第二个多。）

[!code-vb[splitting-an-array-by-length](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/split1.vb)]

下面的示例基于是否存在值为 "zzz" 的元素，将字符串数组拆分为两个数组，该元素充当数组分隔符。 新数组不包括包含分隔符的元素。

[!code-vb[splitting-an-array-by-delimiter](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/split2.vb)]

## <a name="joining-arrays"></a>联接数组

还可以将多个数组合并成一个更大的数组。 为此，您还可以使用 <xref:System.Array.Copy%2A?displayProperty=nameWithType> 方法。

> [!NOTE]
> 本部分不讨论如何将字符串数组联接成单个字符串。 有关联接字符串数组的信息，请参见 <xref:System.String.Join%2A?displayProperty=nameWithType> 方法。

在将每个数组的元素复制到新数组中之前，必须先确保已初始化数组，使其足以容纳新的数组。 可以通过两种方法执行此操作：

- 在 [`ReDim Preserve`](../../../language-reference/statements/redim-statement.md) 将新元素添加到数组之前，请使用语句动态展开数组。 这是最简单的方法，但它可能会导致性能下降，并在复制大型数组时消耗过多的内存。
- 计算新的大型数组所需的元素总数，然后将每个源数组的元素添加到其中。

下面的示例使用第二种方法将四个数组分别添加到一个数组中。

[!code-vb[joining-an-array](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/join.vb)]

由于在这种情况下，源数组都是小的，因此我们还可以在将每个新数组的元素添加到数组时，动态扩展数组。 以下示例执行该操作。

[!code-vb[joining-an-array-dynamically](~/samples/snippets/visualbasic/programming-guide/language-features/arrays/join2.vb)]

## <a name="collections-as-an-alternative-to-arrays"></a>作为数组的替代方法的集合

数组最适用于创建和使用固定数量的强类型化对象。 集合提供更灵活的方式来使用对象组。 与数组不同，数组要求使用[ `ReDim` 语句](../../../language-reference/statements/redim-statement.md)显式更改数组的大小，集合随着应用程序更改的需要动态地增长和收缩。

使用 `ReDim` 对数组进行重维数时，Visual Basic 会创建一个新数组，并释放以前的数组。 这需要执行时间。 因此，如果你正在使用的项目数量频繁变化，或者你无法预测所需的最大项目数，则通常可以使用集合获得更好的性能。

对于某些集合，你可以为放入集合中的任何对象分配一个密钥，这样你便可以使用该密钥快速检索此对象。

如果集合中只包含一种数据类型的元素，则可以使用 <xref:System.Collections.Generic?displayProperty=nameWithType> 命名空间中的一个类。 泛型集合强制类型安全，因此无法向其添加任何其他数据类型。

有关集合的详细信息，请参阅[集合](../../concepts/collections.md)。

## <a name="related-topics"></a>相关主题

|术语|定义|
|----------|----------------|
|[Array Dimensions in Visual Basic](array-dimensions.md)|在数组中解释级别和维度。|
|[如何：在 Visual Basic 中初始化数组变量](how-to-initialize-an-array-variable.md)|说明如何用初始值填充数组。|
|[如何：在 Visual Basic 中对数组进行排序](how-to-sort-an-array.md)|显示如何按字母先后顺序对数组元素进行排序。|
|[如何：将一个数组赋给另一个数组](how-to-assign-one-array-to-another-array.md)|说明将数组分配到另一个数组变量的规则和步骤。|
|[数组疑难解答](troubleshooting-arrays.md)|讨论在使用数组时出现的一些常见问题。|

## <a name="see-also"></a>另请参阅

- <xref:System.Array?displayProperty=nameWithType>
- [Dim 语句](../../../language-reference/statements/dim-statement.md)
- [ReDim 语句](../../../language-reference/statements/redim-statement.md)
