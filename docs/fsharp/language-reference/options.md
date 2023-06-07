---
title: 选项
description: '了解当命名值或变量可能不存在实际值时如何使用 F # 选项类型。'
ms.date: 08/13/2020
ms.openlocfilehash: 0618590c10f6ecac51a23483ca0ab6cd66f2df4f
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88557563"
---
# <a name="options"></a>选项

当命名值或变量可能不存在实际值时，将使用 F # 中的选项类型。 选项具有基础类型并且可以保存该类型的值，或它可能没有值。

## <a name="remarks"></a>备注

下面的代码演示了一个生成选项类型的函数。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1404.fs)]

正如您所看到的，如果输入 `a` 大于0， `Some(a)` 则生成。  否则， `None` 将生成。

`None`当选项没有实际值时，使用值。 否则，表达式将 `Some( ... )` 为选项提供一个值。 值 `Some` 和 `None` 可用于模式匹配，如下面的函数中所示 `exists` ， `true` 如果选项具有值，则返回; `false` 否则返回。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1401.fs)]

## <a name="using-options"></a>使用选项

如果搜索不返回匹配的结果，通常会使用选项，如下面的代码所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1403.fs)]

在前面的代码中，将以递归方式搜索列表。 函数 `tryFindMatch` 采用一个谓词函数， `pred` 该函数返回布尔值和要搜索的列表。 如果找到满足该谓词的元素，则递归结束，并且函数以表达式形式返回值 `Some(head)` 。 当匹配空列表时，递归结束。 此时， `head` 未找到值， `None` 返回。

许多 F # 库函数在集合中搜索可能存在也可能不存在的值将返回该 `option` 类型。 按照约定，这些函数以前缀开头 `try` ，例如 [`Seq.tryFindIndex`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#tryFindIndex) 。

当某个值可能不存在时（例如，如果在尝试构造值时可能会引发异常），选项也可能有用。 下面的代码示例阐释了这一点。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1402.fs)]

`openFile`上一示例中的函数具有类型 `string -> File option` `File` ，因为如果文件成功打开并且发生异常，则它将返回一个对象 `None` 。 根据具体的情况，它可能不是捕获异常而不允许它传播的合适设计选项。

此外，还可以传递或值为 null 的值，这是 `null` `Some` 一个选项。 通常，这是为了避免这种情况，通常在常规 F # 编程中，但可能是因为 .NET 中的引用类型的性质。

## <a name="option-properties-and-methods"></a>选项属性和方法

选项类型支持以下属性和方法。

|属性或方法|类型|说明|
|------------------|----|-----------|
|无|`'T option`|一个静态属性，可用于创建一个具有值的选项值 `None` 。|
|[IsNone](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-fsharpoption-1.html#IsNone)|`bool`|如果选项的值为，则返回 `true` `None` 。|
|[IsSome](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-fsharpoption-1.html#IsSome)|`bool`|`true`如果选项的值不为，则返回 `None` 。|
|[Some](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-fsharpoption-1.html#Some)|`'T option`|一个静态成员，该成员创建的选项的值不是 `None` 。|
|[值](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-fsharpoption-1.html#Value)|`'T`|返回基础值， `System.NullReferenceException` 如果值为，则引发 `None` 。|

## <a name="option-module"></a>选项模块

有一个模块 [选项](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-optionmodule.html)，它包含对选项执行操作的有用函数。 某些函数会重复属性的功能，但在需要函数的上下文中很有用。 [IsSome](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-optionmodule.html#isSome) 和 [isNone](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-optionmodule.html#isNone) 都是测试选项是否包含值的模块函数。 [Option 获取](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-optionmodule.html#get) 值（如果有）。 如果没有值，则会引发 `System.ArgumentException` 。

如果有值，则该 [方法](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-optionmodule.html#bind) 会对值执行函数。 函数必须仅采用一个自变量，并且其参数类型必须为选项类型。 函数的返回值是另一个选项类型。

选项模块还包括与可用于列表、数组、序列和其他集合类型的函数相对应的函数。 这些函数包括 [`Option.map`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-optionmodule.html#map) 、 [`Option.iter`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-optionmodule.html#iter) 、 [`Option.forall`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-optionmodule.html#forall) 、 [`Option.exists`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-optionmodule.html#exists) 、 [`Option.foldBack`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-optionmodule.html#foldBack) 、 [`Option.fold`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-optionmodule.html#fold) 和 [`Option.count`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-optionmodule.html#count) 。 这些函数可用于将选项用作零个或一个元素的集合。 有关详细信息和示例，请参阅 " [列表](lists.md)中的集合函数讨论"。

## <a name="converting-to-other-types"></a>转换为其他类型

选项可以转换为列表或数组。 当某个选项转换为这些数据结构中的任何一个时，生成的数据结构将包含零个或一个元素。 若要将选项转换为数组，请使用 [`Option.toArray`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-optionmodule.html#toArray) 。 若要将选项转换为列表，请使用 [`Option.toList`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-optionmodule.html#toList) 。

## <a name="see-also"></a>另请参阅

- [F# 语言参考](index.md)
- [F# 类型](fsharp-types.md)
