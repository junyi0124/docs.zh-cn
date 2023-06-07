---
title: Dim 语句
ms.date: 05/12/2018
f1_keywords:
- vb.Dim
- Dim
helpviewer_keywords:
- Public keyword [Visual Basic], in Dim statement
- Dim statement [Visual Basic]
- fixed-length strings [Visual Basic], declaring
- variables [Visual Basic], declaring
- WithEvents keyword [Visual Basic], Dim statement
- dynamic arrays [Visual Basic], Dim statement
- variables [Visual Basic], initializing
- '{} braces'
- fields [Visual Basic], as member variables
- declarations [Visual Basic], dynamic arrays
- member variables [Visual Basic]
- default values [Visual Basic]
- data types [Visual Basic], assigning
- braces {}
- As keyword [Visual Basic], in Dim statement
- arrays [Visual Basic], declaring
- New keyword [Visual Basic], Dim statement
- To keyword [Visual Basic], in Dim statement
- storage [Visual Basic], allocating
- local variables [Visual Basic]
- declaration statements [Visual Basic]
- Dim statement [Visual Basic], syntax
- variables [Visual Basic], member and local
ms.assetid: fae3eca1-f0b2-4400-994b-7aa58a848448
ms.openlocfilehash: 1b0c3089c366c417af926c8c0703cea021674432
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744723"
---
# <a name="dim-statement-visual-basic"></a>Dim 语句（Visual Basic）

声明和分配一个或多个变量的存储空间。

## <a name="syntax"></a>语法

```vb
[ <attributelist> ] [ accessmodifier ] [[ Shared ] [ Shadows ] | [ Static ]] [ ReadOnly ]
Dim [ WithEvents ] variablelist
```

## <a name="parts"></a>组成部分

- `attributelist`

  可选。 请参阅[特性列表](attribute-list.md)。

- `accessmodifier`

  可选。 可以是以下值之一：

  - 公共[Public](../modifiers/public.md)

  - [Protected](../modifiers/protected.md)

  - [Friend](../modifiers/friend.md)

  - 专用[Private](../modifiers/private.md)

  - [Protected Friend](../modifiers/protected-friend.md)

  - [Private Protected](../modifiers/private-protected.md)

  请参阅 [Access levels in Visual Basic](../../programming-guide/language-features/declared-elements/access-levels.md)。

- `Shared`

  可选。 请参阅[共享](../modifiers/shared.md)。

- `Shadows`

  可选。 请参阅[阴影](../modifiers/shadows.md)。

- `Static`

  可选。 请参阅[静态](../modifiers/static.md)。

- `ReadOnly`

  可选。 请参阅[ReadOnly](../modifiers/readonly.md)。

- `WithEvents`

  可选。 指定这些对象变量引用可以引发事件的类的实例。 请参阅[WithEvents](../modifiers/withevents.md)。

- `variablelist`

  必需。 在此语句中声明的变量的列表。

  `variable [ , variable ... ]`

  每个 `variable` 都具有以下语法和部件：

  `variablename [ ( [ boundslist ] ) ] [ As [ New ] datatype [ With`{`[ .propertyname = propinitializer [ , ... ] ] } ] ] [ = initializer ]`

  |组成部分|说明|
  |---|---|
  |`variablename`|必需。 变量的名称。 请参阅 [Declared Element Names](../../programming-guide/language-features/declared-elements/declared-element-names.md)。|
  |`boundslist`|可选。 数组变量的每个维度的界限列表。|
  |`New`|可选。 当 `Dim` 语句运行时，创建类的新实例。|
  |`datatype`|可选。 变量的数据类型。|
  |`With`|可选。 引入对象初始值设定项列表。|
  |`propertyname`|可选。 要生成其实例的类中的属性的名称。|
  |`propinitializer`|`propertyname` = 后必需。 计算并分配给属性名称的表达式。|
  |`initializer`|如果未指定 `New`，则为可选。 创建变量时计算并分配给该变量的表达式。|

## <a name="remarks"></a>备注

Visual Basic 编译器使用 `Dim` 语句来确定变量的数据类型和其他信息，例如哪些代码可以访问该变量。 下面的示例声明一个变量以保存 `Integer` 值。

```vb
Dim numberOfStudents As Integer
```

您可以指定任何数据类型或枚举、结构、类或接口的名称。

```vb
Dim finished As Boolean
Dim monitorBox As System.Windows.Forms.Form
```

对于引用类型，可以使用 `New` 关键字创建由数据类型指定的类或结构的新实例。 如果使用 `New`，则不使用初始值设定项表达式。 相反，可以向从中创建变量的类的构造函数提供参数（如果需要）。

```vb
Dim bottomLabel As New System.Windows.Forms.Label
```

您可以在过程、块、类、结构或模块中声明变量。 不能在源文件、命名空间或接口中声明变量。 有关详细信息，请参阅[声明上下文和默认访问级别](declaration-contexts-and-default-access-levels.md)。

在模块级别，在任何过程外部声明的变量是*成员变量*或*字段*。 成员变量在其类、结构或模块中的作用域内。 在过程级别声明的变量是*局部变量*。 局部变量仅在其过程或块范围内。

以下访问修饰符用于声明过程之外的变量： `Public`、`Protected`、`Friend`、`Protected Friend`和 `Private`。 有关详细信息，请参阅[Visual Basic 中的访问级别](../../programming-guide/language-features/declared-elements/access-levels.md)。

如果指定以下任何修饰符，则 `Dim` 关键字是可选的，通常省略此关键字： `Public`、`Protected`、`Friend`、`Protected Friend`、`Private`、`Shared`、`Shadows`、`Static`、`ReadOnly`或 `WithEvents`。

```vb
Public maximumAllowed As Double
Protected Friend currentUserName As String
Private salary As Decimal
Static runningTotal As Integer
```

如果 `Option Explicit` 为 on （默认值），则编译器要求使用的每个变量的声明。 有关详细信息，请参阅[Option Explicit 语句](option-explicit-statement.md)。

## <a name="specifying-an-initial-value"></a>指定初始值

可以在创建变量时为该变量分配值。 对于值类型，使用*初始值设定项*提供要分配给变量的表达式。 表达式的计算结果必须为可在编译时计算的常数。

```vb
Dim quantity As Integer = 10
Dim message As String = "Just started"
```

如果指定了初始值设定项并且 `As` 子句中未指定数据类型，则使用*类型推理*从初始值设定项推断数据类型。 在下面的示例中，`num1` 和 `num2` 都作为整数强类型化。 在第二个声明中，类型推理从值3推断类型。

```vb
' Use explicit typing.
Dim num1 As Integer = 3

' Use local type inference.
Dim num2 = 3
```

类型推理适用于过程级别。 它不会应用于类、结构、模块或接口中的过程外部。 有关类型推理的详细信息，请参阅[选项推断语句](option-infer-statement.md)和[局部类型推理](../../programming-guide/language-features/variables/local-type-inference.md)。

有关未指定数据类型或初始值设定项时所发生情况的信息，请参阅本主题后面的[默认数据类型和值](dim-statement.md#default)。

可以使用*对象初始值设定项*声明命名类型和匿名类型的实例。 下面的代码创建 `Student` 类的实例，并使用对象初始值设定项来初始化属性。

```vb
Dim student1 As New Student With {.First = "Michael",
                                  .Last = "Tucker"}
```

有关对象初始值设定项的详细信息，请参阅[如何：使用对象初始值设定项声明对象](../../programming-guide/language-features/objects-and-classes/how-to-declare-an-object-by-using-an-object-initializer.md)、[对象初始值设定项：命名类型和匿名](../../programming-guide/language-features/objects-and-classes/object-initializers-named-and-anonymous-types.md)类型和[匿名类型](../../programming-guide/language-features/objects-and-classes/anonymous-types.md)。

## <a name="declaring-multiple-variables"></a>声明多个变量

可以在一个声明语句中声明多个变量，并为每个变量指定变量名称，并在每个数组名称后面加上括号。 以逗号分隔多个变量。

```vb
Dim lastTime, nextTime, allTimes() As Date
```

如果使用一个 `As` 子句声明多个变量，则不能为该变量组提供初始值设定项。

您可以为不同的变量指定不同的数据类型，方法是为每个声明的变量使用单独的 `As` 子句。 每个变量都采用在其 `variablename` 部分后遇到的第一个 `As` 子句中指定的数据类型。

```vb
Dim a, b, c As Single, x, y As Double, i As Integer
' a, b, and c are all Single; x and y are both Double
```

## <a name="arrays"></a>阵列

您可以声明一个变量来保存一个*数组*，该数组可以包含多个值。 若要指定某个变量包含数组，请在其 `variablename` 后跟括号。 有关数组的详细信息，请参阅[数组](../../programming-guide/language-features/arrays/index.md)。

您可以指定数组的每个维度的下限和上限。 为此，请在括号内包含一个 `boundslist`。 对于每个维度，`boundslist` 指定上限，还可以选择下限。 无论是否指定，下限始终为零。 每个索引的大小均为0到其上限值。

下面两个语句是等效的。 每个语句声明21个 `Integer` 元素的数组。 访问数组时，索引的取值可能为0到20。

```vb
Dim totals(20) As Integer
Dim totals(0 To 20) As Integer
```

下面的语句声明一个 `Double`类型的二维数组。 数组具有4个行（3 + 1）每个行（5 + 1）。 请注意，上限表示索引的可能的最大值，而不是维度的长度。 维度的长度为上限加1。

```vb
Dim matrix2(3, 5) As Double
```

数组的维数可以是1到32。

可以在数组声明中保留所有界限为空。 如果执行此操作，数组将具有指定的维度数，但未初始化。 它的值为 `Nothing`，直到至少初始化其部分元素。 `Dim` 语句必须为所有维度或无维度指定界限。

```vb
' Declare an array with blank array bounds.
Dim messages() As String
' Initialize the array.
ReDim messages(4)
```

如果数组具有多个维度，则必须在括号之间包含逗号以指示维数。

```vb
Dim oneDimension(), twoDimensions(,), threeDimensions(,,) As Byte
```

可以通过将数组的一个维度声明为-1，来声明*长度为零的数组*。 保存零长度数组的变量不具有 `Nothing`的值。 某些公共语言运行时函数需要零长度数组。 如果尝试访问此类数组，则会发生运行时异常。 有关详细信息，请参阅 [array](../../programming-guide/language-features/arrays/index.md)。

可以通过使用数组文本初始化数组的值。 为此，请将初始化值用大括号（`{}`）括起来。

```vb
Dim longArray() As Long = {0, 1, 2, 3}
```

对于多维数组，每个单独维度的初始化都括在外部维度中的大括号内。 元素按行主顺序指定。

```vb
Dim twoDimensions(,) As Integer = {{0, 1, 2}, {10, 11, 12}}
```

有关数组文本的详细信息，请参阅[数组](../../programming-guide/language-features/arrays/index.md)。

## <a name="default"></a>默认数据类型和值

下表描述了指定 `Dim` 语句中数据类型和初始值设定项的各种组合的结果。

|是否指定数据类型？|是否指定初始值设定项？|示例|结果|
|---|---|---|---|
|是|是|`Dim qty`|如果[Option Strict](option-strict-statement.md)为 off （默认值），则变量设置为 `Nothing`。<br /><br /> 如果 `Option Strict` 处于打开状态，则发生编译时错误。|
|是|是|`Dim qty = 5`|如果[选项推断](option-infer-statement.md)为 on （默认值），则变量使用初始值设定项的数据类型。 请参阅[局部类型推理](../../programming-guide/language-features/variables/local-type-inference.md)。<br /><br /> 如果 `Option Infer` 和 `Option Strict` 均处于关闭状态，则变量采用 `Object` 的数据类型。<br /><br /> 如果 `Option Infer` 处于关闭状态但 `Option Strict` 处于打开状态，则发生编译时错误。|
|是|是|`Dim qty As Integer`|将变量初始化为数据类型的默认值。 请参阅本部分后面的表。|
|是|是|`Dim qty  As Integer = 5`|如果初始值设定项的数据类型不可转换为指定数据类型，则会发生编译时错误。|

如果指定数据类型但未指定初始值设定项，Visual Basic 会将变量初始化为其数据类型的默认值。 下表显示了默认的初始化值。

|数据类型|默认值|
|---|---|
|所有数值类型（包括 `Byte` 和 `SByte`）|0|
|`Char`|二进制0|
|所有引用类型（包括 `Object`、`String`和所有数组）|`Nothing`|
|`Boolean`|`False`|
|`Date`|12:00 年1月1日上午（01/01/0001 12:00:00 AM）|

将结构的每个元素初始化为一个单独的变量。 如果声明数组的长度，但不初始化其元素，则会将每个元素初始化为单独的变量。

## <a name="static-local-variable-lifetime"></a>静态局部变量生存期

`Static` 局部变量的生存期比声明它的过程长。 变量生存期的边界取决于声明过程的位置以及是否 `Shared`。

|过程声明|变量已初始化|变量停止了现有|
|---|---|---|
|在模块中|第一次调用该过程时|当程序停止执行时|
|在类或结构中，过程是 `Shared`|第一次在特定实例或类或结构自身上调用该过程时|当程序停止执行时|
|在类或结构中，过程不 `Shared`|第一次在特定实例上调用该过程时|当发布实例进行垃圾回收（GC）时|

## <a name="attributes-and-modifiers"></a>特性和修饰符

仅可将属性应用于成员变量，而不能应用于局部变量。 特性向程序集的元数据提供信息，这对于临时存储（如局部变量）没有意义。

在模块级别，不能使用 `Static` 修饰符来声明成员变量。 在过程级别，不能使用 `Shared`、`Shadows`、`ReadOnly`、`WithEvents`或任何访问修饰符来声明局部变量。

可以通过提供 `accessmodifier`来指定哪些代码可以访问变量。 类和模块成员变量（在任何过程外部）默认为私有访问，结构成员变量默认为公共访问。 您可以使用访问修饰符调整其访问级别。 不能对本地变量（在过程中）使用访问修饰符。

只能在成员变量上指定 `WithEvents`，而不能指定过程中的局部变量。 如果指定 `WithEvents`，则变量的数据类型必须是特定的类类型，而不是 `Object`。 不能使用 `WithEvents`声明数组。 有关事件的详细信息，请参阅[事件](../../programming-guide/language-features/events/index.md)。

> [!NOTE]
> 类、结构或模块外的代码必须使用该类、结构或模块的名称来限定成员变量的名称。 过程或块外的代码不能引用该过程或块中的任何局部变量。

## <a name="releasing-managed-resources"></a>释放托管资源

.NET Framework 垃圾回收器会释放托管资源，而不会对你的部分进行任何额外编码。 但是，您可以强制处置托管资源，而不是等待垃圾回收器。

如果类包含在特别宝贵的资源（例如数据库连接或文件句柄）上，则您可能不希望等到下一次垃圾回收来清理不再使用的类实例。 类可以实现 <xref:System.IDisposable> 接口，以提供一种在垃圾回收之前释放资源的方法。 实现该接口的类公开了一个 `Dispose` 方法，可以调用该方法来强制立即释放有价值的资源。

`Using` 语句会自动获取资源，执行一组语句，然后释放资源。 但是，资源必须实现 <xref:System.IDisposable> 接口。 有关详细信息，请参阅 [Using 语句](using-statement.md)。

## <a name="example"></a>示例

下面的示例通过使用带有各种选项的 `Dim` 语句来声明变量。

[!code-vb[VbVbalrStatements#141](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class11.vb#141)]

## <a name="example"></a>示例

下面的示例列出了介于1和30之间的质数。 在代码注释中介绍了局部变量的作用域。

[!code-vb[VbVbalrStatements#142](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class11.vb#142)]

## <a name="example"></a>示例

在下面的示例中，在类级别声明 `speedValue` 变量。 `Private` 关键字用于声明变量。 `Car` 类中的任何过程都可以访问该变量。

[!code-vb[VbVbalrStatements#144](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class11.vb#144)]

[!code-vb[VbVbalrStatements#145](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class11.vb#145)]

## <a name="see-also"></a>另请参阅

- [Const 语句](const-statement.md)
- [ReDim 语句](redim-statement.md)
- [Option Explicit 语句](option-explicit-statement.md)
- [Option Infer 语句](option-infer-statement.md)
- [Option Strict 语句](option-strict-statement.md)
- [“项目设计器”->“编译”页 (Visual Basic)](/visualstudio/ide/reference/compile-page-project-designer-visual-basic)
- [变量声明](../../programming-guide/language-features/variables/variable-declaration.md)
- [数组](../../programming-guide/language-features/arrays/index.md)
- [对象初始值设定项：命名类型和匿名类型](../../programming-guide/language-features/objects-and-classes/object-initializers-named-and-anonymous-types.md)
- [匿名类型](../../programming-guide/language-features/objects-and-classes/anonymous-types.md)
- [对象初始值设定项：命名类型和匿名类型](../../programming-guide/language-features/objects-and-classes/object-initializers-named-and-anonymous-types.md)
- [如何：使用对象初始值设定项声明对象](../../programming-guide/language-features/objects-and-classes/how-to-declare-an-object-by-using-an-object-initializer.md)
- [局部类型推理](../../programming-guide/language-features/variables/local-type-inference.md)
