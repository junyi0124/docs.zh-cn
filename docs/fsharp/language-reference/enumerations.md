---
title: 枚举
description: '了解如何使用 F # 枚举替代文本，使代码更具可读性和更易维护性。'
ms.date: 08/15/2020
ms.openlocfilehash: 5f298691ce48a06c203930c7742cf007c819dc33
ms.sourcegitcommit: 9c45035b781caebc63ec8ecf912dc83fb6723b1f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2020
ms.locfileid: "88812102"
---
# <a name="enumerations"></a>枚举

*枚举*（也称为 *枚举*）是整型类型，其中标签分配给值的子集。 可以使用它们来代替文本，使代码更具可读性且更易维护。

## <a name="syntax"></a>语法

```fsharp
type enum-name =
| value1 = integer-literal1
| value2 = integer-literal2
...
```

## <a name="remarks"></a>备注

枚举非常类似于具有简单值的可区分联合，只不过可以指定这些值。 值通常是从0或1开始的整数，或者是表示位位置的整数。 如果枚举旨在表示位位置，还应使用 [Flags](xref:System.FlagsAttribute) 特性。

枚举的基础类型是根据所使用的文本来确定的，因此，例如，可以 `1u` `2u` 对无符号整数 () 类型使用带有后缀的文本（例如、等） `uint32` 。

引用命名值时，必须使用枚举类型本身的名称作为限定符，即，而 `enum-name.value1` 不只是 `value1` 。 此行为不同于可区分联合的行为。 这是因为枚举始终具有 [RequireQualifiedAccess](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-requirequalifiedaccessattribute.html) 属性。

下面的代码演示了枚举的声明和使用。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2101.fs)]

使用适当的运算符可以轻松地将枚举转换为基础类型，如下面的代码所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2102.fs)]

枚举类型可以具有以下基础类型之一： `sbyte` 、 `byte` 、 `int16` 、、、、、、 `uint16` `int32` `uint32` `int64` `uint16` `uint64` 和 `char` 。 枚举类型在 .NET Framework 中表示为从继承的类型 `System.Enum` ，而后者又继承自 `System.ValueType` 。 因此，它们是位于堆栈上或内联在包含对象中的值类型，并且基础类型的任何值都是枚举的有效值。 这在对枚举值进行模式匹配时很重要，因为您必须提供一个模式来捕获未命名的值。

`enum`F # 库中的函数可用于生成枚举值，甚至是预定义的命名值之一之外的值。 `enum`函数如下所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2103.fs)]

默认 `enum` 函数适用于类型 `int32` 。 因此，它不能与具有其他基础类型的枚举类型一起使用。 请改用以下各项。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2104.fs)]

此外，枚举事例始终作为发出 `public` 。 这是为了使它们与 c # 和 .NET 平台的其余部分保持一致。

## <a name="see-also"></a>另请参阅

- [F# 语言参考](index.md)
- [强制转换和转换](casting-and-conversions.md)
