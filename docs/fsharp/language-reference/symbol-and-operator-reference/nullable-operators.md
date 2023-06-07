---
title: 可以为 null 的运算符
description: '了解 F # 编程语言中可用的可以为 null 的运算符。'
ms.date: 05/16/2016
ms.openlocfilehash: 9ac6afc2c3f4277ee6e93b1ccb3d21f892926b4b
ms.sourcegitcommit: ecd9e9bb2225eb76f819722ea8b24988fe46f34c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2020
ms.locfileid: "96740362"
---
# <a name="nullable-operators"></a>可以为 null 的运算符

可以为 null 的运算符是在一侧或两侧使用可以为 null 的算术类型的二进制算法或比较运算符。 如果你使用的数据源（例如允许 null 代替实际值的数据库）使用数据，则会经常出现可为 null 的类型。 在查询表达式中经常使用可以为 null 的运算符。 除了用于算术和比较的可以为 null 的运算符以外，转换运算符还可用于在可以为 null 的类型之间进行转换。 某些查询运算符还可以是可以为 null 的版本。

## <a name="table-of-nullable-operators"></a>可以为 Null 的运算符表

下表列出了 F # 语言中支持的可以为 null 的运算符。

|左空|在右侧可以为 null|两边可以为 null|
|---|---|---|
|[？ >=](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?%3E=%20))|[>=？](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20%3E=?%20))|[？ >=？](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?%3E=?%20))|
|[？ >](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?%3E%20))|[>？](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20%3E?%20))|[？ >？](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?%3E?%20))|
|[？ <=](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?%3C=%20))|[<=？](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20%3C=?%20))|[？ <=？](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?%3C=?%20))|
|[？ <](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?%3C%20))|[<？](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20%3C?%20))|[？ <？](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?%3C?%20))|
|[?=](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?=%20))|[=?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20=?%20))|[?=?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?=?%20))|
|[？ <>](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?%3C%3E%20))|[<>？](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20%3C%3E?%20))|[？ <>？](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?%3C%3E?%20))|
|[?+](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?+%20))|[+?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20+?%20))|[?+?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?+?%20))|
|[?-](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?-%20))|[-?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20-?%20))|[?-?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?-?%20))|
|[?*](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?*%20))|[*?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20*?%20))|[?*?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?*?%20))|
|[?/](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?/%20))|[/?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20/?%20))|[?/?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?/?%20))|
|[?%](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?%%20))|[%?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20%?%20))|[?%?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?%?%20))|

## <a name="remarks"></a>备注

可以为 null 的运算符包含在命名空间[fsharp.core](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq.html)的[NullableOperators](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html)模块中。 可为 null 的数据的类型为 `System.Nullable<'T>` 。

在查询表达式中，从允许空值而不是值的数据源中选择数据时，可以为 null 的类型。 在 SQL Server 数据库中，表中的每个数据列都有一个属性，该属性指示是否允许 null 值。 如果允许使用 null，则从数据库返回的数据可能包含不能用基元数据类型（例如、等）表示的 null 值 `int` `float` 。 因此，数据将返回为而不是 `System.Nullable<int>` ，而 `int` 不是 `System.Nullable<float>` `float` 。 可以通过使用属性从对象中获取实际值 `System.Nullable<'T>` `Value` ，并且可以 `System.Nullable<'T>` 通过调用方法来确定对象是否具有值 `HasValue` 。 另一种有用的方法是 `System.Nullable<'T>.GetValueOrDefault` 方法，该方法允许您获取适当类型的值或默认值。 默认值为 "零" 值的某种形式，例如0、0.0 或 `false` 。

可以为 null 的类型使用常用转换运算符（如或）转换为不可为 null 的基元类型 `int` `float` 。 使用可以为 null 的类型的转换运算符，还可以从一个可以为 null 的类型转换为另一个可以为 null 的类型。 适当的转换运算符与标准的转换运算符具有相同的名称，但它们位于单独的模块中，这是[fsharp.core](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq.html)命名空间中的[可为 null](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullablemodule.html)的模块。 通常，在使用查询表达式时打开此命名空间。 在这种情况下，可以通过将前缀添加到相应的转换运算符来使用可以为 null 的转换运算符 `Nullable.` ，如下面的代码所示。

```fsharp
open Microsoft.FSharp.Linq

let nullableInt = new System.Nullable<int>(10)

// Use the Nullable.float conversion operator to convert from one nullable type to another nullable type.
let nullableFloat = Nullable.float nullableInt

// Use the regular non-nullable float operator to convert to a non-nullable float.
printfn $"%f{float nullableFloat}"
```

输出为 `10.000000`。

可以为 null 的数据字段（如）的查询运算符 `sumByNullable` 也存在于查询表达式中。 不可以为 null 的类型的查询运算符与可为 null 的类型不兼容，因此，当你使用可以为 null 的数据值时，必须使用适当查询运算符的可为 null 版本。 有关详细信息，请参阅 [查询表达式](../query-expressions.md)。

下面的示例演示如何在 F # 查询表达式中使用可以为 null 的运算符。 第一个查询显示了如何在没有可为 null 的运算符的情况下编写查询;第二个查询显示使用可以为 null 的运算符的等效查询。 有关完整的上下文，包括如何将数据库设置为使用此示例代码，请参阅 [演练：使用类型提供程序访问 SQL 数据库](../../tutorials/type-providers/index.md)。

```fsharp
open System
open System.Data
open System.Data.Linq
open Microsoft.FSharp.Data.TypeProviders
open Microsoft.FSharp.Linq

[<Generate>]
type dbSchema = SqlDataConnection<"Data Source=MYSERVER\INSTANCE;Initial Catalog=MyDatabase;Integrated Security=SSPI;">

let db = dbSchema.GetDataContext()

query {
    for row in db.Table2 do
    where (row.TestData1.HasValue && row.TestData1.Value > 2)
    select row
} |> Seq.iter (fun row -> printfn $"%d{row.TestData1.Value} %s{row.Name}")

query {
    for row in db.Table2 do
    // Use a nullable operator ?>
    where (row.TestData1 ?> 2)
    select row
} |> Seq.iter (fun row -> printfn "%d{row.TestData1.GetValueOrDefault()} %s{row.Name}")
```

## <a name="see-also"></a>请参阅

- [类型提供程序](../../tutorials/type-providers/index.md)
- [查询表达式](../query-expressions.md)
