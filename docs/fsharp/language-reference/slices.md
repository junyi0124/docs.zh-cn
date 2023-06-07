---
title: 切片
description: '了解如何使用现有 F # 数据类型的切片，以及如何为其他数据类型定义自己的切片。'
ms.date: 11/20/2020
ms.openlocfilehash: b776058df5a174dfe48dbf513bf17281036dd83e
ms.sourcegitcommit: ecd9e9bb2225eb76f819722ea8b24988fe46f34c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2020
ms.locfileid: "96740375"
---
# <a name="slices"></a>切片

本文介绍如何从现有的 F # 类型中获取切片，以及如何定义自己的切片。

在 F # 中，切片是任何数据类型的子集。  切片与 [索引器](./members/indexed-properties.md)相似，但它不是从基础数据结构产生单个值，而是生成多个值。 切片使用 `..` 运算符语法来选择数据类型中指定索引的范围。 有关详细信息，请参阅 [循环表达式参考文章](./loops-for-in-expression.md)。

F # 目前对切片字符串、列表、数组和多维 (2D、3D、4D) 数组的内部支持。 切片最常用于 F # 数组和列表。 您可以通过使用 `GetSlice` 类型定义中的方法或作用域内 [类型扩展](type-extensions.md)，将切片添加到您的自定义数据类型中。

## <a name="slicing-f-lists-and-arrays"></a>对 F # 列表和数组进行切片

切片最常见的数据类型是 F # 列表和数组。  下面的示例演示如何对列表进行切片：

```fsharp
// Generate a list of 100 integers
let fullList = [ 1 .. 100 ]

// Create a slice from indices 1-5 (inclusive)
let smallSlice = fullList.[1..5]
printfn $"Small slice: {smallSlice}"

// Create a slice from the beginning to index 5 (inclusive)
let unboundedBeginning = fullList.[..5]
printfn $"Unbounded beginning slice: {unboundedBeginning}"

// Create a slice from an index to the end of the list
let unboundedEnd = fullList.[94..]
printfn $"Unbounded end slice: {unboundedEnd}"
```

切片数组与切片列表类似：

```fsharp
// Generate an array of 100 integers
let fullArray = [| 1 .. 100 |]

// Create a slice from indices 1-5 (inclusive)
let smallSlice = fullArray.[1..5]
printfn $"Small slice: {smallSlice}"

// Create a slice from the beginning to index 5 (inclusive)
let unboundedBeginning = fullArray.[..5]
printfn $"Unbounded beginning slice: {unboundedBeginning}"

// Create a slice from an index to the end of the list
let unboundedEnd = fullArray.[94..]
printfn $"Unbounded end slice: {unboundedEnd}"
```

## <a name="slicing-multidimensional-arrays"></a>切片多维数组

F # 支持 F # 核心库中的多维数组。 与一维数组一样，多维数组的切片也很有用。 但是，附加维度的引入要求使用略微不同的语法，以便能够获取特定行和列的切片。

下面的示例演示如何切分二维数组：

```fsharp
// Generate a 3x3 2D matrix
let A = array2D [[1;2;3];[4;5;6];[7;8;9]]
printfn $"Full matrix:\n {A}"

// Take the first row
let row0 = A.[0,*]
printfn $"{row0}"

// Take the first column
let col0 = A.[*,0]
printfn $"{col0}"

// Take all rows but only two columns
let subA = A.[*,0..1]
printfn $"{subA}"

// Take two rows and all columns
let subA' = A.[0..1,*]
printfn $"{subA}"

// Slice a 2x2 matrix out of the full 3x3 matrix
let twoByTwo = A.[0..1,0..1]
printfn $"{twoByTwo}"
```

## <a name="defining-slices-for-other-data-structures"></a>为其他数据结构定义切片

F # 核心库定义了有限类型集的切片。 如果要定义更多数据类型的切片，可以在类型定义本身或类型扩展中执行此操作。

例如，下面介绍了如何为类定义切片， <xref:System.ArraySegment%601> 以方便进行数据操作：

```fsharp
open System

type ArraySegment<'TItem> with
    member segment.GetSlice(start, finish) =
        let start = defaultArg start 0
        let finish = defaultArg finish segment.Count
        ArraySegment(segment.Array, segment.Offset + start, finish - start)

let arr = ArraySegment [| 1 .. 10 |]
let slice = arr.[2..5] //[ 3; 4; 5]
```

使用和类型的另一个示例 <xref:System.Span%601> <xref:System.ReadOnlySpan%601> ：

```fsharp
open System

type ReadOnlySpan<'T> with
    member sp.GetSlice(startIdx, endIdx) =
        let s = defaultArg startIdx 0
        let e = defaultArg endIdx sp.Length
        sp.Slice(s, e - s)

type Span<'T> with
    member sp.GetSlice(startIdx, endIdx) =
        let s = defaultArg startIdx 0
        let e = defaultArg endIdx sp.Length
        sp.Slice(s, e - s)

let printSpan (sp: Span<int>) =
    let arr = sp.ToArray()
    printfn $"{arr}"

let sp = [| 1; 2; 3; 4; 5 |].AsSpan()
printSpan sp.[0..] // [|1; 2; 3; 4; 5|]
printSpan sp.[..5] // [|1; 2; 3; 4; 5|]
printSpan sp.[0..3] // [|1; 2; 3|]
printSpan sp.[1..3] // |2; 3|]
```

## <a name="built-in-f-slices-are-end-inclusive"></a>内置 F # 切片包含结尾

F # 中的所有内部切片都是结尾的;也就是说，切片中包括上限。 对于具有起始索引 `x` 和结束索引的给定切片 `y` ，生成的切片将包含 *yth* 值。

```fsharp
// Define a new list
let xs = [1 .. 10]

printfn $"{xs.[2..5]}" // Includes the 5th index
```

## <a name="built-in-f-empty-slices"></a>内置 F # 空切片

如果语法可能生成不存在的切片，则 F # 列表、数组、序列、字符串、多维 (2D、3D、4D) 数组将生成一个空切片。

请看下面的示例：

```fsharp
let l = [ 1..10 ]
let a = [| 1..10 |]
let s = "hello!"

let emptyList = l.[-2..(-1)]
let emptyArray = a.[-2..(-1)]
let emptyString = s.[-2..(-1)]
```

> [!IMPORTANT]
> C # 开发人员可能希望它们引发异常，而不是生成空切片。 这是一个以 F # 编写的空集合为基础的设计决策。 空 F # 列表可以用另一个 F # 列表撰写，空字符串可添加到现有字符串，等等。 基于作为参数传入的值拍摄切片可能很常见，并且通过生成一个空集合（与 F # 代码的复合特性相匹配）来容忍超出界限 >。

## <a name="fixed-index-slices-for-3d-and-4d-arrays"></a>用于3D 和4D 数组的固定索引切片

对于 F # 3D 和4D 数组，可以 "修复" 特定索引，并切分已修复索引的其他维度。

为了说明这一点，请考虑下面的三维数组：

*z = 0*

| x\y   | 0 | 1 |
|-------|---|---|
| **0** | 0 | 1 |
| **1** | 2 | 3 |

*z = 1*

| x\y   | 0 | 1 |
|-------|---|---|
| **0** | 4 | 5 |
| **1** | 6 | 7 |

如果要 `[| 4; 5 |]` 从数组中提取切片，请使用固定索引切片。

```fsharp
let dim = 2
let m = Array3D.zeroCreate<int> dim dim dim

let mutable count = 0

for z in 0..dim-1 do
    for y in 0..dim-1 do
        for x in 0..dim-1 do
            m.[x,y,z] <- count
            count <- count + 1

// Now let's get the [4;5] slice!
m.[*, 0, 1]
```

最后一行修复了 `y` `z` 三维数组的和索引，并采用了对应于矩阵的其余 `x` 值。

## <a name="see-also"></a>请参阅

- [索引属性](./members/indexed-properties.md)
