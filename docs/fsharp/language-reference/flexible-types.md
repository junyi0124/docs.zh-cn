---
title: 可变类型
description: '了解如何使用 F # 挠性类型批注，该批注指示参数、变量或值具有与指定类型兼容的类型。'
ms.date: 08/15/2020
ms.openlocfilehash: 44241ad082cd7f3de9e0cc6a48b8a8946e7b33d3
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88557745"
---
# <a name="flexible-types"></a>可变类型

*可变类型批注*指示参数、变量或值具有与指定类型兼容的类型，其中的兼容性由类或接口的面向对象的层次结构中的位置确定。 当不发生类型层次结构中较高的类型的自动转换但仍希望使你的功能可以与层次结构中的任何类型或实现接口的任何类型一起使用时，灵活类型特别有用。

## <a name="syntax"></a>语法

```fsharp
#type
```

## <a name="remarks"></a>备注

在前面的语法中， *类型* 表示基类型或接口。

灵活类型等效于具有约束的泛型类型，该约束将允许的类型限制为与基类型或接口类型兼容的类型。 也就是说，以下两行代码是等效的。

```fsharp
#SomeType

'T when 'T :> SomeType
```

灵活类型在多种情况下都很有用。 例如，如果您的 order 函数 (将函数作为参数) 的函数，则使函数返回灵活类型通常很有用。 在下面的示例中，在中使用带序列参数的灵活类型 `iterate2` 使高阶函数能够使用生成序列、数组、列表和任何其他可枚举类型的函数。

请考虑以下两个函数，其中一个函数返回一个序列，另一个返回一个可变类型。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4101.fs)]

作为另一个示例，请考虑 [Seq](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#concat) library 函数：

```fsharp
val concat: sequences:seq<#seq<'T>> -> seq<'T>
```

可以将以下任一可枚举序列传递到此函数：

- 列表列表
- 数组的列表
- 列表的数组
- 序列数组
- 可枚举序列的任何其他组合

下面的代码使用 `Seq.concat` 灵活的类型来演示可支持的方案。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4102.fs)]

输出如下所示。

```console
seq [1; 2; 3; 4; ...]
seq [1; 2; 3; 4; ...]
seq [1; 2; 3; 4; ...]
seq [1; 2; 3; 4; ...]
seq [1; 2; 3; 4; ...]
```

在 F # 中，与在其他面向对象的语言中一样，某些上下文中实现接口的派生类型或类型会自动转换为基类型或接口类型。 这些自动转换在直接参数中进行，但当类型位于从属位置时，不会发生在作为更复杂类型（如函数类型的返回类型）或类型参数的一部分的情况下。 因此，当您要将它应用到的类型是更复杂类型的一部分时，灵活的类型表示法主要非常有用。

## <a name="see-also"></a>另请参阅

- [F# 语言参考](index.md)
- [泛型](./generics/index.md)
