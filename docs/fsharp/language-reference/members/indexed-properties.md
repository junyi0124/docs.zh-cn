---
title: 索引属性
description: 了解中F#的索引属性，该属性允许对按序的数据进行类似数组的访问。
ms.date: 11/04/2019
ms.openlocfilehash: f6cf3bfa737d2bf458e379594be5884696cee3e1
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2019
ms.locfileid: "73976601"
---
# <a name="indexed-properties"></a>索引属性

定义对按序的数据进行抽象的类时，有时提供对该数据的索引访问会很有帮助，而无需公开基础实现。 这是通过 `Item` 成员来完成的。

## <a name="syntax"></a>语法

```fsharp
// Indexed property that can be read and written to
member self-identifier.Item
    with get(index-values) =
        get-member-body
    and set index-values values-to-set =
        set-member-body

// Indexed property can only be read
member self-identifier.Item
    with get(index-values) =
        get-member-body

// Indexed property that can only be set
member self-identifier.Item
    with set index-values values-to-set =
        set-member-body
```

## <a name="remarks"></a>备注

上述语法的形式显示了如何定义同时具有 `get` 和 `set` 方法的索引属性、仅具有 `get` 方法或仅具有 `set` 方法。 你还可以将显示的语法和仅用于 get 的语法组合在一起，并生成同时具有 get 和 set 的语法。 后一种形式允许将不同的可访问性修饰符和特性置于 get 和 set 方法上。

通过使用名称 `Item`，编译器将属性视为默认的索引属性。 *默认索引属性*是一个属性，可通过对对象实例使用类似数组的语法来访问它。 例如，如果 `o` 为定义此属性的类型的对象，则使用语法 `o.[index]` 来访问属性。

用于访问非默认索引属性的语法是在括号中提供属性和索引的名称，就像常规成员一样。 例如，如果 `o` 上的属性称为 `Ordinal`，则可以编写 `o.Ordinal(index)` 来访问它。

无论使用哪种窗体，都应始终对索引属性使用 set 方法的扩充形式。 有关扩充函数的信息，请参阅[函数](../functions/index.md)。

## <a name="example"></a>示例

下面的代码示例演示了具有 get 和 set 方法的默认和非默认索引属性的定义和使用。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet3301.fs)]

## <a name="output"></a>Output

```console
ONE two three four five six seven eight nine ten
ONE first two second three third four fourth five fifth six 6th
seven seventh eight eighth nine ninth ten tenth
```

## <a name="indexed-properties-with-multiple-index-values"></a>具有多个索引值的索引属性

索引属性可以有多个索引值。 在这种情况下，如果使用属性，则值用逗号分隔。 此类属性中的 set 方法必须具有两个扩充参数，其中第一个参数是包含键的元组，第二个参数是要设置的值。

下面的代码演示如何使用具有多个索引值的索引属性。

```fsharp
open System.Collections.Generic

/// Basic implementation of a sparse matrix based on a dictionary
type SparseMatrix() =
    let table = new Dictionary<(int * int), float>()
    member _.Item
        // Because the key is comprised of two values, 'get' has two index values
        with get(key1, key2) = table.[(key1, key2)]

        // 'set' has two index values and a new value to place in the key's position
        and set (key1, key2) value = table.[(key1, key2)] <- value

let sm = new SparseMatrix()
for i in 1..1000 do
    sm.[i, i] <- float i * float i
```

## <a name="see-also"></a>请参阅

- [成员](index.md)
