---
title: 匿名记录
description: 了解如何使用构造和使用匿名记录，这种语言功能有助于处理数据。
ms.date: 06/12/2019
ms.openlocfilehash: 13950048f02ab74362f8174725f5f8615d9d7654
ms.sourcegitcommit: ecd9e9bb2225eb76f819722ea8b24988fe46f34c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2020
ms.locfileid: "96739810"
---
# <a name="anonymous-records"></a>匿名记录

匿名记录是命名值的简单聚合，在使用之前无需声明。 可以将它们声明为结构或引用类型。 默认情况下，它们是引用类型。

## <a name="syntax"></a>语法

下面的示例演示了匿名记录语法。 分隔的项 `[item]` 是可选的。

```fsharp
// Construct an anonymous record
let value-name = [struct] {| Label1: Type1; Label2: Type2; ...|}

// Use an anonymous record as a type parameter
let value-name = Type-Name<[struct] {| Label1: Type1; Label2: Type2; ...|}>

// Define a parameter with an anonymous record as input
let function-name (arg-name: [struct] {| Label1: Type1; Label2: Type2; ...|}) ...
```

## <a name="basic-usage"></a>基本用法

匿名记录被认为是 F # 记录类型，在实例化之前无需声明。

例如，在这里，你可以如何与生成匿名记录的函数进行交互：

```fsharp
open System

let getCircleStats radius =
    let d = radius * 2.0
    let a = Math.PI * (radius ** 2.0)
    let c = 2.0 * Math.PI * radius

    {| Diameter = d; Area = a; Circumference = c |}

let r = 2.0
let stats = getCircleStats r
printfn "Circle with radius: %f has diameter %f, area %f, and circumference %f"
    r stats.Diameter stats.Area stats.Circumference
```

下面的示例使用 `printCircleStats` 采用匿名记录作为输入的函数对上一个进行扩展：

```fsharp
open System

let getCircleStats radius =
    let d = radius * 2.0
    let a = Math.PI * (radius ** 2.0)
    let c = 2.0 * Math.PI * radius

    {| Diameter = d; Area = a; Circumference = c |}

let printCircleStats r (stats: {| Area: float; Circumference: float; Diameter: float |}) =
    printfn "Circle with radius: %f has diameter %f, area %f, and circumference %f"
        r stats.Diameter stats.Area stats.Circumference

let r = 2.0
let stats = getCircleStats r
printCircleStats r stats
```

如果调用 `printCircleStats` 的任何匿名记录类型不具有与输入类型相同的 "形状"，则将无法编译：

```fsharp
printCircleStats r {| Diameter = 2.0; Area = 4.0; MyCircumference = 12.566371 |}
// Two anonymous record types have mismatched sets of field names
// '["Area"; "Circumference"; "Diameter"]' and '["Area"; "Diameter"; "MyCircumference"]'
```

## <a name="struct-anonymous-records"></a>结构匿名记录

匿名记录还可以定义为带有可选关键字的结构 `struct` 。 下面的示例通过生成和使用结构匿名记录来补充前一个示例：

```fsharp
open System

let getCircleStats radius =
    let d = radius * 2.0
    let a = Math.PI * (radius ** 2.0)
    let c = 2.0 * Math.PI * radius

    // Note that the keyword comes before the '{| |}' brace pair
    struct {| Area = a; Circumference = c; Diameter = d |}

// the 'struct' keyword also comes before the '{| |}' brace pair when declaring the parameter type
let printCircleStats r (stats: struct {| Area: float; Circumference: float; Diameter: float |}) =
    printfn "Circle with radius: %f has diameter %f, area %f, and circumference %f"
        r stats.Diameter stats.Area stats.Circumference

let r = 2.0
let stats = getCircleStats r
printCircleStats r stats
```

### <a name="structness-inference"></a>Structness 推理

结构匿名记录还允许 "structness 推理"，在这种情况下，你无需在 `struct` 调用站点指定关键字。 在此示例中，在 `struct` 调用时 elide 关键字 `printCircleStats` ：

```fsharp

let printCircleStats r (stats: struct {| Area: float; Circumference: float; Diameter: float |}) =
    printfn "Circle with radius: %f has diameter %f, area %f, and circumference %f"
        r stats.Diameter stats.Area stats.Circumference

printCircleStats r {| Area = 4.0; Circumference = 12.6; Diameter = 12.6 |}
```

反向模式- `struct` 如果输入类型不是结构匿名记录，将无法进行编译。

## <a name="embedding-anonymous-records-within-other-types"></a>在其他类型中嵌入匿名记录

声明事例为记录的可 [区分联合](discriminated-unions.md) 很有用。 但是，如果记录中的数据与可区分联合的类型相同，则必须将所有类型定义为互相递归。 使用匿名记录可避免此限制。 下面是与模式匹配的示例类型和函数：

```fsharp
type FullName = { FirstName: string; LastName: string }

// Note that using a named record for Manager and Executive would require mutually recursive definitions.
type Employee =
    | Engineer of FullName
    | Manager of {| Name: FullName; Reports: Employee list |}
    | Executive of {| Name: FullName; Reports: Employee list; Assistant: Employee |}

let getFirstName e =
    match e with
    | Engineer fullName -> fullName.FirstName
    | Manager m -> m.Name.FirstName
    | Executive ex -> ex.Name.FirstName
```

## <a name="copy-and-update-expressions"></a>复制和更新表达式

匿名记录支持带有 [复制和更新表达式](copy-and-update-record-expressions.md)的构造。 例如，下面介绍了如何构造用于复制现有数据的匿名记录的新实例：

```fsharp
let data = {| X = 1; Y = 2 |}
let data' = {| data with Y = 3 |}
```

但是，与命名记录不同，匿名记录允许您用复制和更新表达式构造完全不同的窗体。 下面的示例使用上一示例中的相同匿名记录，并将其扩展为新的匿名记录：

```fsharp
let data = {| X = 1; Y = 2 |}
let expandedData = {| data with Z = 3 |} // Gives {| X=1; Y=2; Z=3 |}
```

还可以通过命名记录的实例构造匿名记录：

```fsharp
type R = { X: int }
let data = { X = 1 }
let data' = {| data with Y = 2 |} // Gives {| X=1; Y=2 |}
```

您还可以将数据复制到引用和结构匿名记录：

```fsharp
// Copy data from a reference record into a struct anonymous record
type R1 = { X: int }
let r1 = { X = 1 }

let data1 = struct {| r1 with Y = 1 |}

// Copy data from a struct record into a reference anonymous record
[<Struct>]
type R2 = { X: int }
let r2 = { X = 1 }

let data2 = {| r1 with Y = 1 |}

// Copy the reference anonymous record data into a struct anonymous record
let data3 = struct {| data2 with Z = r2.X |}
```

## <a name="properties-of-anonymous-records"></a>匿名记录的属性

匿名记录有很多特性，这些特性对于完全理解它们的使用方式至关重要。

### <a name="anonymous-records-are-nominal"></a>匿名记录为名义

匿名记录是 [名义类型](https://en.wikipedia.org/wiki/Nominal_type_system)。 它们最适用于已命名的 [记录](records.md) 类型 (，这些类型也是不需要前期声明的名义) 。

请考虑以下示例，其中包含两个匿名记录声明：

```fsharp
let x = {| X = 1 |}
let y = {| Y = 1 |}
```

`x`和 `y` 值具有不同的类型，并且彼此不兼容。 它们不是可相等的，也不是可比较的。 为了说明这一点，请考虑等效的命名记录：

```fsharp
type X = { X: int }
type Y = { Y: int }

let x = { X = 1 }
let y = { Y = 1 }
```

如果匿名记录与与类型等效性或比较相关的已命名记录等效项进行比较，则没有什么差异。

### <a name="anonymous-records-use-structural-equality-and-comparison"></a>匿名记录使用结构相等性和比较

与记录类型类似，匿名记录是结构可相等和可比较的。 如果所有构成类型都支持相等和比较（如记录类型），则此值为 true。 若要支持相等或比较，两个匿名记录必须具有相同的 "形状"。

```fsharp
{| a = 1+1 |} = {| a = 2 |} // true
{| a = 1+1 |} > {| a = 1 |} // true

// error FS0001: Two anonymous record types have mismatched sets of field names '["a"]' and '["a"; "b"]'
{| a = 1 + 1 |} = {| a = 2;  b = 1|}
```

### <a name="anonymous-records-are-serializable"></a>匿名记录可序列化

可以像处理命名记录一样序列化匿名记录。 下面是一个使用 [Newtonsoft.Js的](https://www.nuget.org/packages/Newtonsoft.Json/)示例：

```fsharp
open Newtonsoft.Json

let phillip' = {| name="Phillip"; age=28 |}
let philStr = JsonConvert.SerializeObject(phillip')

let phillip = JsonConvert.DeserializeObject<{|name: string; age: int|}>(philStr)
printfn $"Name: {phillip.name} Age: %d{phillip.age}"
```

匿名记录适用于通过网络发送轻型数据，无需为前面的序列化/反序列化类型定义域。

### <a name="anonymous-records-interoperate-with-c-anonymous-types"></a>匿名记录与 c # 匿名类型互操作

可以使用需要使用 [c # 匿名类型](../../csharp/programming-guide/classes-and-structs/anonymous-types.md)的 .net API。 C # 匿名类型与通过使用匿名记录与进行互操作非常简单。 下面的示例演示如何使用匿名记录调用需要匿名类型的 [LINQ](../../csharp/programming-guide/concepts/linq/index.md) 重载：

```fsharp
open System.Linq

let names = [ "Ana"; "Felipe"; "Emilia"]
let nameGrouping = names.Select(fun n -> {| Name = n; FirstLetter = n.[0] |})
for ng in nameGrouping do
    printfn $"{ng.Name} has first letter {ng.FirstLetter}"
```

在整个 .NET 中使用的其他许多 Api 需要使用传入匿名类型。 匿名记录是用于处理它们的工具。

## <a name="limitations"></a>限制

匿名记录在使用时有一些限制。 有些是其设计固有的，但其他一些则适合更改。

### <a name="limitations-with-pattern-matching"></a>模式匹配的限制

匿名记录不支持模式匹配，这与命名记录不同。 有以下三个原因：

1. 与命名的记录类型不同，模式必须为匿名记录的每个字段提供帐户。 这是因为匿名记录不支持结构子类型化–它们是名义类型。
2. 由于 (1) ，因此不能在模式匹配表达式中添加其他模式，因为每个 distinct 模式将意味着不同的匿名记录类型。
3. 由于 (3) ，任何匿名记录模式比使用 "点" 表示法更详细。

有一个开放语言建议， [允许在有限的上下文中匹配模式](https://github.com/fsharp/fslang-suggestions/issues/713)。

### <a name="limitations-with-mutability"></a>可变性限制

目前不能定义包含数据的匿名记录 `mutable` 。 有一种 [开放语言建议](https://github.com/fsharp/fslang-suggestions/issues/732) ，允许使用可变数据。

### <a name="limitations-with-struct-anonymous-records"></a>结构匿名记录的限制

不能将结构匿名记录声明为 `IsByRefLike` 或 `IsReadOnly` 。 对于和匿名记录，有一种 [开放语言建议](https://github.com/fsharp/fslang-suggestions/issues/712) `IsByRefLike` `IsReadOnly` 。
