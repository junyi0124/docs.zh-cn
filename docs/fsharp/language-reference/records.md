---
title: 记录
description: '了解 F # 记录如何表示命名值的简单聚合，还可以选择包含成员。'
ms.date: 08/15/2020
ms.openlocfilehash: 2da31da0ec830d458a370e64ca105048181f5d74
ms.sourcegitcommit: 88fbb019b84c2d044d11fb4f6004aec07f2b25b1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/05/2021
ms.locfileid: "97899634"
---
# <a name="records"></a>记录

记录表示已命名值的简单聚合，可选择包含成员。 它们可以是结构或引用类型。  默认情况下，它们是引用类型。

## <a name="syntax"></a>语法

```fsharp
[ attributes ]
type [accessibility-modifier] typename =
    { [ mutable ] label1 : type1;
      [ mutable ] label2 : type2;
      ... }
    [ member-list ]
```

## <a name="remarks"></a>备注

在前面的语法中， *typename* 是记录类型的名称， *label1* 和 *label2* 是值的名称（称为 *标签*）， *type1* 和 *type2* 是这些值的类型。 *成员列表* 是此类型的成员的可选列表。  您可以使用 `[<Struct>]` 属性来创建结构记录，而不是引用类型的记录。

下面是一些示例。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1901.fs)]

每个标签都在单独的行上时，分号是可选的。

您可以在称为 *记录表达式* 的表达式中设置值。 如果标签与其他记录类型) 的 (完全不同，则编译器将从使用的标签推断类型。 大括号 ( {} ) 将记录表达式括起来。 下面的代码演示了一个记录表达式，该表达式使用带有标签、和的三个 float 元素初始化记录 `x` `y` `z` 。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1907.fs)]

如果有另一种类型也具有相同标签，请不要使用缩写形式。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1903.fs)]

最近声明的类型的标签优先于以前声明的类型的标签，因此在前面的示例中， `mypoint3D` 将推断为 `Point3D` 。 可以显式指定记录类型，如下面的代码所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1908.fs)]

与类类型一样，可以为记录类型定义方法。

## <a name="creating-records-by-using-record-expressions"></a>使用记录表达式创建记录

您可以通过使用记录中定义的标签来初始化记录。 这样做的表达式称为 " *记录表达式*"。 使用大括号将记录表达式括起来，并使用分号作为分隔符。

下面的示例演示如何创建记录。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1904.fs)]

记录表达式和类型定义中最后一个字段后面的分号是可选的，不管这些字段是否都在同一行中。

创建记录时，必须为每个字段提供值。 不能引用任何字段的初始化表达式中其他字段的值。

在下面的代码中，的类型 `myRecord2` 是从字段的名称推断出来的。 还可以选择显式指定类型名称。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1905.fs)]

当必须复制现有记录，并且可能更改某些字段值时，另一种形式的记录构造会很有用。 下面的代码行阐释了这一点。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1906.fs)]

这种形式的记录表达式称为 " *复制和更新记录表达式*"。

记录在默认情况下是不可变的;不过，您可以使用复制和更新表达式轻松创建修改后的记录。 还可以显式指定可变字段。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1909.fs)]

不要将 DefaultValue 属性用于记录字段。 更好的方法是使用初始化为默认值的字段定义记录的默认实例，然后使用复制和更新记录表达式来设置不同于默认值的任何字段。

```fsharp
// Rather than use [<DefaultValue>], define a default record.
type MyRecord =
    { Field1 : int
      Field2 : int }

let defaultRecord1 = { Field1 = 0; Field2 = 0 }
let defaultRecord2 = { Field1 = 1; Field2 = 25 }

// Use the with keyword to populate only a few chosen fields
// and leave the rest with default values.
let rr3 = { defaultRecord1 with Field2 = 42 }
```

## <a name="creating-mutually-recursive-records"></a>创建相互递归记录

在创建记录时，可能需要让它依赖于以后要定义的另一种类型。 这是一种编译错误，除非你将记录类型定义为 "互相递归"。

定义相互递归的记录是通过关键字来完成的 `and` 。 这使你可以将两个或更多个记录类型链接在一起。

例如，下面的代码将 `Person` 和类型定义 `Address` 为互相递归：

```fsharp
// Create a Person type and use the Address type that is not defined
type Person =
  { Name: string
    Age: int
    Address: Address }
// Define the Address type which is used in the Person record
and Address =
  { Line1: string
    Line2: string
    PostCode: string
    Occupant: Person }
```

如果要定义前面的示例而不包含 `and` 关键字，则不会进行编译。 `and`对于相互递归定义，关键字是必需的。

## <a name="pattern-matching-with-records"></a>与记录匹配的模式

记录可以与模式匹配一起使用。 可以显式指定某些字段，并为其他字段提供在发生匹配时将分配的变量。 下面的代码示例阐释了这一点。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1910.fs)]

此代码的输出如下所示。

```console
Point is at the origin.
Point is on the x-axis. Value is 100.000000.
Point is at (10.000000, 0.000000, -1.000000).
```

## <a name="records-and-members"></a>记录和成员

您可以在记录上指定成员，这一点与类的处理方式相同。 不支持字段。 常见的方法是定义一个 `Default` 静态成员以便于构建简单的记录：

```fsharp
type Person =
  { Name: string
    Age: int
    Address: string }

    static member Default =
        { Name = "Phillip"
          Age = 12
          Address = "123 happy fun street" }

let defaultPerson = Person.Default
```

如果使用自我标识符，则该标识符引用其成员被调用的记录的实例：

```fsharp
type Person =
  { Name: string
    Age: int
    Address: string }

    member this.WeirdToString() =
        this.Name + this.Address + string this.Age

let p = { Name = "a"; Age = 12; Address = "abc123" }
let weirdString = p.WeirdToString()
```

## <a name="differences-between-records-and-classes"></a>记录和类之间的差异

记录字段与类字段的不同之处在于，它们会自动作为属性公开，并在创建和复制记录时使用。 记录构造也不同于类构造。 在记录类型中，不能定义构造函数。 相反，本主题中介绍的构造语法适用。 类在构造函数参数、字段和属性之间没有直接关系。

与联合和结构类型一样，记录具有结构相等性语义。 类具有引用相等性语义。 下面的代码示例展示了此操作。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1911.fs)]

此代码的输出如下所示：

```console
The records are equal.
```

如果你用类编写相同的代码，则两个类对象将不相等，因为这两个值将表示堆上的两个对象，并且仅 (比较地址，除非类类型重写 `System.Object.Equals`) 的方法。

如果需要记录的引用相等性，请在 `[<ReferenceEquality>]` 记录上方添加属性。

## <a name="see-also"></a>另请参阅

- [F# 类型](fsharp-types.md)
- [Classes](classes.md)
- [F# 语言参考](index.md)
- [引用相等](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-referenceequalityattribute.html)
- [模式匹配](pattern-matching.md)
