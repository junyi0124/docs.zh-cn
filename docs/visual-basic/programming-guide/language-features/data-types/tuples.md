---
title: 元组
ms.date: 04/23/2017
helpviewer_keywords:
- tuples [Visual Basic]
ms.assetid: 3e66cd1b-3432-4e1d-8c37-5ebacae8f53f
ms.openlocfilehash: b169a1c13b3f20d7b5e2a1386cfb28a9cc093dcd
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88559084"
---
# <a name="tuples-visual-basic"></a>元组 (Visual Basic)

从 Visual Basic 2017 开始，Visual Basic 语言为元组提供内置支持，使创建元组和访问元组的元素变得更加容易。 元组是一种轻量级数据结构，它具有特定数量和值序列。 实例化元组时，可以定义每个值的数量和数据类型 (或元素) 。 例如，2元组 (或对) 具有两个元素。 第一个可能是一个 `Boolean` 值，而第二个是 `String` 。 因为元组可以轻松地将多个值存储在一个对象中，所以它们通常用作从方法返回多个值的一种简便方法。

> [!IMPORTANT]
> 元组支持需要 <xref:System.ValueTuple> 类型。 如果未安装 .NET Framework 4.7，你必须添加 nuget 包 `System.ValueTuple` ，它在 Nuget 库中可用。 如果不使用此包，可能会出现类似于 "预定义类型" ValueTuple (,,, ) "未定义或未导入" 的编译错误。

## <a name="instantiating-and-using-a-tuple"></a>实例化和使用元组

您可以通过将元组的逗号分隔值括在括号中来实例化元组。 然后，这些值将成为元组的字段。 例如，下面的代码定义了三 (或3元组) `Date` ，其第一个值为，第二个值为，另一个为第二个值 `String` `Boolean` 。

[!code-vb[Instantiate](../../../../../samples/snippets/visualbasic/programming-guide/language-features/data-types/tuple1.vb#1)]

默认情况下，元组中每个字段的名称都包含字符串 `Item` 和该字段在元组中的基于1的位置。 对于此3元组， `Date` 字段是 `Item1` ，字段为 `String` `Item2` ， `Boolean` 字段为 `Item3` 。 下面的示例显示了在上一行代码中实例化的元组字段的值。

[!code-vb[Instantiate](../../../../../samples/snippets/visualbasic/programming-guide/language-features/data-types/tuple1.vb#2)]

Visual Basic 元组的字段是读写的;实例化元组后，可以修改其值。 下面的示例修改了上一示例中创建的元组的三个字段中的两个，并显示结果。

[!code-vb[Instantiate](../../../../../samples/snippets/visualbasic/programming-guide/language-features/data-types/tuple1.vb#3)]

## <a name="instantiating-and-using-a-named-tuple"></a>实例化和使用命名元组

您可以通过将您自己的名称分配给元组的元素，来实例化 *命名元组* ，而不是使用元组字段的默认名称。 然后，可以通过其分配的名称 *或* 默认名称访问元组的字段。 下面的示例实例化与之前相同的3元组，只不过它显式命名第一个字段 `EventDate` 、第二个字段和第三个字段 `Name` `IsHoliday` 。 然后，它将显示字段值并对其进行修改，并再次显示字段值。

[!code-vb[Instantiate](../../../../../samples/snippets/visualbasic/programming-guide/language-features/data-types/tuple1.vb#4)]

## <a name="inferred-tuple-element-names"></a>推断元组元素名称

从 Visual Basic 15.3 开始，Visual Basic 可以推断元组元素的名称;无需显式分配它们。 当你从一组变量初始化元组，并且你希望元组元素名称与变量名称相同时，推理的元组名称很有用。

下面的示例创建一个 `stateInfo` 包含三个显式命名元素（、和）的元组 `state` `stateName` `capital` 。 请注意，在为元素命名时，元组初始化语句只将命名元素命名为同名变量的值。

[!code-vb[ExplicitlyNamed](../../../../../samples/snippets/visualbasic/programming-guide/language-features/data-types/named-tuples/program.vb#1)]

由于元素和变量具有相同的名称，因此 Visual Basic 编译器可以推断字段的名称，如下面的示例所示。

[!code-vb[ExplicitlyNamed](../../../../../samples/snippets/visualbasic/programming-guide/language-features/data-types/named-tuples/program.vb#2)]

若要启用推断元组元素名称，你必须定义要在 Visual Basic 项目中使用的 Visual Basic 编译器版本 (\* .vbproj) 文件：

```xml
<PropertyGroup>
  <LangVersion>15.3</LangVersion>
</PropertyGroup>
```

版本号可以是从15.3 开始的 Visual Basic 编译器的任何版本。 您还可以将 "最新版本" 指定为 `LangVersion` 使用您的系统上安装的 Visual Basic 编译器的最新版本，而不是对特定编译器版本进行硬编码。

有关详细信息，请参阅 [设置 Visual Basic 语言版本](../../../language-reference/configure-language-version.md)。

在某些情况下，Visual Basic 编译器无法从候选名称推断元组元素名称，并且只能使用其默认名称（如、等）来引用元组字段 `Item1` `Item2` 。其中包括：

- 候选名称与元组成员的名称相同，例如 `Item3` 、 `Rest` 或 `ToString` 。

- 候选名称在元组中重复。

当字段名称推理失败时，Visual Basic 不会生成编译器错误，也不会在运行时引发异常。 相反，元组字段必须由它们的预定义名称引用，例如 `Item1` 和 `Item2` 。

## <a name="tuples-versus-structures"></a>元组与结构

Visual Basic 元组是一个值类型，它是 **ValueTuple** 泛型类型之一的实例。 例如， `holiday` 在上一个示例中定义的元组是结构的实例 <xref:System.ValueTuple%603> 。 它设计为数据的轻型容器。 由于元组旨在使创建包含多个数据项的对象变得更加容易，因此它缺少自定义结构可能具有的某些功能。 其中包括：

- 自定义成员。 不能为元组定义自己的属性、方法或事件。

- 检查. 您无法验证分配给字段的数据。

- 不可变性. Visual Basic 元组是可变的。 与此相反，自定义结构允许您控制实例是可变的还是不可变的。

如果自定义成员、属性和字段验证或不可变性非常重要，则应使用 Visual Basic [结构](../../../language-reference/statements/structure-statement.md) 语句来定义自定义值类型。

Visual Basic 元组将继承其 **ValueTuple** 类型的成员。 除了其字段外，其中包括以下方法：

| 方法 | 说明 |
| ---|---|
| CompareTo | 将当前元组与具有相同数量的元素的另一个元组进行比较。 |
| 等于 | 确定当前元组是否等于另一个元组或对象。 |
| GetHashCode | 计算当前实例的哈希代码。 |
| ToString | 返回此元组的字符串表示形式，它采用格式 `(Item1, Item2...)` ，其中 `Item1` 和 `Item2` 表示元组字段的值。 |

此外， **ValueTuple** 类型实现 <xref:System.Collections.IStructuralComparable> 和 <xref:System.Collections.IStructuralEquatable> 接口，这允许您定义自定义比较器。

## <a name="assignment-and-tuples"></a>赋值和元组

Visual Basic 支持在具有相同数量的字段的元组类型之间进行分配。 如果满足以下任一条件，则可以转换字段类型：

- 源和目标字段的类型相同。

- 定义了源类型到目标类型的扩大 (或隐式) 转换。

- `Option Strict` 为 `On` ，并且定义了从源类型到目标类型的收缩 (或显式) 转换。 如果源值超出了目标类型的范围，则此转换可能会引发异常。

对于其他转换，不考虑进行赋值。 让我们看一下元组类型之间允许的赋值类型。

注意以下示例中使用的这些变量：

[!code-vb[Assign](../../../../../samples/snippets/visualbasic/programming-guide/language-features/data-types/tuple3.vb#1)]

前两个变量（ `unnamed` 和 `anonymous` ）没有为字段提供语义名称。 它们的字段名称是默认值 `Item1` 和 `Item2` 。 最后两个变量， `named` `differentName` 具有语义字段名称。 请注意，这两个元组具有不同的字段名称。

这四个元组具有相同数目的字段 (称为 "arity" ) ，这些字段的类型相同。 因此可进行以下赋值：

[!code-vb[Assign](../../../../../samples/snippets/visualbasic/programming-guide/language-features/data-types/tuple3.vb#2)]

请注意，元组的名称未赋值。 字段的赋值顺序遵循字段在元组中的顺序。

最后请注意，我们可以将 `named` 元组分配给 `conversion` 元组，即使的第一个字段 `named` 是 `Integer` ，第一个字段 `conversion` 是 `Long` 。 此分配成功，因为转换 `Integer` 为 `Long` 是扩大转换。

[!code-vb[Assign](../../../../../samples/snippets/visualbasic/programming-guide/language-features/data-types/tuple3.vb#3)]

具有不同数量字段的元组不可赋值：

```vb
' Does not compile.
' VB30311: Value of type '(Integer, Integer, Integer)' cannot be converted
'          to '(Answer As Integer, Message As String)'
var differentShape = (1, 2, 3)
named = differentShape
```

## <a name="tuples-as-method-return-values"></a>作为方法返回值的元组

一个方法只能返回一个值。 不过，通常会希望方法调用返回多个值。 有多种方法可以解决此限制：

- 你可以创建一个自定义类或结构，该类或结构的属性或字段表示方法返回的值。 因此，它是一个重型解决方案;它要求您定义一个仅用于从方法调用检索值的自定义类型。

- 你可以从方法返回单个值，并通过将引用传递给方法来返回其余值。 这涉及到实例化变量的开销以及意外覆盖按引用传递的变量值的风险。

- 可以使用元组，该元组提供用于检索多个返回值的轻型解决方案。

例如，.NET 中的 **TryParse** 方法返回一个 `Boolean` 值，该值指示分析操作是否成功。 分析操作的结果在通过引用传递给方法的变量中返回。 通常，对分析方法的调用如下所 <xref:System.Int32.TryParse%2A?displayProperty=nameWithType> 示：

[!code-vb[Return](../../../../../samples/snippets/visualbasic/programming-guide/language-features/data-types/tuple-returns.vb#1)]

如果我们在自己的方法中包装对方法的调用，我们可以从分析操作返回元组 <xref:System.Int32.TryParse%2A?displayProperty=nameWithType> 。 在下面的示例中， `NumericLibrary.ParseInteger` 调用 <xref:System.Int32.TryParse%2A?displayProperty=nameWithType> 方法并返回具有两个元素的命名元组。

[!code-vb[Return](../../../../../samples/snippets/visualbasic/programming-guide/language-features/data-types/tuple-returns.vb#2)]

然后，可以通过如下所示的代码调用该方法：

[!code-vb[Return](../../../../../samples/snippets/visualbasic/programming-guide/language-features/data-types/tuple-returns.vb#3)]

## <a name="visual-basic-tuples-and-tuples-in-the-net-framework"></a>.NET Framework 中的 Visual Basic 元组和元组

Visual Basic 元组是 .NET Framework 4.7 中引入的 ValueTuple 泛型类型之一的实例 **。** .NET Framework 还包括一组通用的 **系统元组** 类。 不过，这些类与 Visual Basic 元组和 **ValueTuple** 泛型类型的不同之处在于：

- **元组**类的元素是名为 `Item1` 、等的属性 `Item2` 。 在 Visual Basic 元组和 **ValueTuple** 类型中，元组元素是字段。

- 不能为 **元组** 实例或 **ValueTuple** 实例的元素分配有意义的名称。 Visual Basic 允许您分配用于传达字段含义的名称。

- **元组**实例的属性是只读的;元组是不可变的。 在 Visual Basic 元组和 **ValueTuple** 类型中，元组字段是读写的;元组是可变的。

- 泛型 **元组** 类型为引用类型。 使用这些 **元组** 类型意味着分配对象。 在热路径中，这可能会对应用程序性能产生明显的影响。 Visual Basic 元组和 **ValueTuple** 类型为值类型。

类中的扩展方法可以轻松地在 <xref:System.TupleExtensions> Visual Basic 元组和 .Net **元组** 对象之间进行转换。 **ToTuple**方法将 Visual Basic 元组转换为 .Net**元组**对象， **ToValueTuple**方法将 .net**元组**对象转换为 Visual Basic 元组。

下面的示例创建一个元组，将其转换为 .NET **元组** 对象，然后将其转换回 Visual Basic 元组。 然后，该示例将此元组与原始元组进行比较以确保它们相等。

[!code-vb[Convert](../../../../../samples/snippets/visualbasic/programming-guide/language-features/data-types/tuple2.vb#1)]

## <a name="see-also"></a>请参阅

- [Visual Basic 语言参考](index.md)
