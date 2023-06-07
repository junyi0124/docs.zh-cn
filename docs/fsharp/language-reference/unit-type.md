---
title: unit 类型
description: 了解F# "unit" 类型如何经常用于在无需任何值或需要任何值时保存语言语法需要值的位置。
ms.date: 05/16/2016
ms.openlocfilehash: a5960fb05af50486a78345d10a5ad913e65729e3
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2019
ms.locfileid: "73424030"
---
# <a name="unit-type"></a>unit 类型

`unit` 类型是指示缺少特定值的类型;`unit` 类型仅包含一个值，该值在不存在任何其他值或需要时充当占位符。

## <a name="syntax"></a>语法

```fsharp
// The value of the unit type.
()
```

## <a name="remarks"></a>备注

每F#个表达式的计算结果都必须为值。 对于不生成相关值的表达式，将使用 `unit` 类型的值。 `unit` 类型类似于C#和C++之类的语言 `void` 类型。

`unit` 类型具有单个值，并且该值由标记 `()`指示。

`unit` 类型的值通常用在编程中F# ，用于保存语言语法需要值的位置，但不需要或不需要任何值。 一个示例可能是 `printf` 函数的返回值。 由于函数中发生了 `printf` 操作的重要操作，因此该函数不必返回实际值。 因此，返回值的类型为 `unit`。

某些构造需要 `unit` 值。 例如，在模块的顶级 `do` 绑定或任何代码的计算结果应为 `unit` 值。 当某个模块的顶级 `do` 绑定或代码产生的结果不是未使用的 `unit` 值时，编译器将报告警告，如下面的示例中所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet901.fs)]

此警告是函数编程的一个特性;它不会显示在其他 .NET 编程语言中。 在纯粹的函数中，函数没有任何副作用，最终返回值是函数调用的唯一结果。 因此，如果忽略结果，则可能是编程错误。 尽管F#不是纯粹的函数编程语言，但最好尽可能遵循函数编程样式。

## <a name="see-also"></a>请参阅

- [基本](basic-types.md)
- [F# 语言参考](index.md)
