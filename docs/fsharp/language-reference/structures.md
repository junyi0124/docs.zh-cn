---
title: 结构
description: '了解 F # 结构，精简对象类型通常比具有少量数据和简单行为的类型的类更有效。'
ms.date: 05/16/2016
ms.openlocfilehash: 1e9652cc4776e4d1d52eb20e41b6dd87a6c5ba05
ms.sourcegitcommit: 67ebdb695fd017d79d9f1f7f35d145042d5a37f7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92223818"
---
# <a name="structures"></a>结构

*结构*是一种紧凑对象类型，它比具有少量数据和简单行为的类型的类更有效。

## <a name="syntax"></a>语法

```fsharp
[ attributes ]
type [accessibility-modifier] type-name =
    struct
        type-definition-elements-and-members
    end
// or
[ attributes ]
[<StructAttribute>]
type [accessibility-modifier] type-name =
    type-definition-elements-and-members
```

## <a name="remarks"></a>备注

结构是 *值类型*，这意味着它们直接存储在堆栈上，或者作为字段或数组元素使用内联在父类型中。 与类和记录不同，结构具有按值传递语义。 这意味着它们主要用于经常访问和复制的少量聚合数据。

在前面的语法中，将显示两个窗体。 第一个窗体不是轻量级语法，但其仍然经常使用，因为当使用 `struct` 和 `end` 关键字时，你可以忽略第二个窗体中出现的 `StructAttribute` 特性。 你可以将 `StructAttribute` 缩写为 `Struct`。

前面的语法中的 *类型定义元素和成员* 表示成员声明和定义。 结构可以具有构造函数以及可变和不可变字段，并且可以声明成员和接口实现。 有关详细信息，请参阅 [成员](./members/index.md)。

结构不能参与继承、不能包含 `let` 或 `do` 绑定、不能以递归方式包含其自身类型的字段（但可以包含引用其自身类型的引用单元格）。

由于结构不允许 `let` 绑定，你必须通过使用 `val` 关键字在结构中声明字段。 `val` 关键字定义字段及其类型，但不允许初始化。 相反，`val` 声明会初始化零或 null。 因此，具有隐式构造函数（即，在声明中的结构名称后直接给定参数）的结构要求使用 `val` 特性批注 `DefaultValue` 声明。 具有定义构造函数的结构仍支持零初始化。 因此，`DefaultValue` 特性是此类零值对字段有效的声明。 由于此类型上不允许 `let` 和 `do` 绑定，结构的隐式构造函数不执行任何操作，但所传入的隐式构造函数参数值作为私有字段提供。

显式构造函数可能涉及字段值的初始化。 当你的结构具有显式构造函数时，它仍支持零初始化；但是，由于它与显式构造函数冲突，请勿在 `DefaultValue` 声明上使用 `val` 特性。 有关声明的详细信息 `val` ，请参阅 [显式字段： `val` 关键字](./members/explicit-fields-the-val-keyword.md)。

结构上允许特性和可访问性修饰符，并遵循与其他类型相同的规则。 有关详细信息，请参阅 [特性](attributes.md) 和 [访问控制](access-control.md)。

以下代码示例说明结构定义。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2501.fs)]

## <a name="byreflike-structs"></a>ByRefLike 结构

您可以定义自己的结构，这些结构可以遵循 `byref` 类似的语义：有关详细信息，请参阅 [byref](byrefs.md) 。 这是通过属性完成的 <xref:System.Runtime.CompilerServices.IsByRefLikeAttribute> ：

```fsharp
open System
open System.Runtime.CompilerServices

[<IsByRefLike; Struct>]
type S(count1: Span<int>, count2: Span<int>) =
    member x.Count1 = count1
    member x.Count2 = count2
```

`IsByRefLike` 不意味着 `Struct` 。 这两者都必须存在于类型上。

`byref`F # 中的 "like" 结构是堆栈绑定值类型。 永远不会在托管堆上分配。 `byref`类似的结构可用于实现高性能编程，因为它是针对生存期和非捕获的一组强检查强制实施的。 规则如下：

- 它们可用作函数参数、方法参数、局部变量、方法返回。
- 它们不能是类或普通结构的静态成员或实例成员。
- 不能通过任何闭包构造 (`async` 方法或 lambda 表达式来捕获它们) 。
- 它们不能用作泛型参数。

尽管这些规则非常严格地限制使用，但它们会以安全的方式满足高性能计算的承诺。

## <a name="readonly-structs"></a>ReadOnly 结构

您可以用属性批注结构 <xref:System.Runtime.CompilerServices.IsReadOnlyAttribute> 。 例如：

```fsharp
[<IsReadOnly; Struct>]
type S(count1: int, count2: int) =
    member x.Count1 = count1
    member x.Count2 = count2
```

`IsReadOnly` 不意味着 `Struct` 。 必须同时添加这两个，才能获得 `IsReadOnly` 结构。

使用此属性会发出元数据，让 F # 和 c # 知道将 `inref<'T>` 其 `in ref` 分别视为和。

在 readonly 结构内定义可变值将产生错误。

## <a name="struct-records-and-discriminated-unions"></a>结构记录和可区分联合

可以将 [记录](records.md) 和可 [区分联合](discriminated-unions.md) 表示为具有特性的结构 `[<Struct>]` 。  请参阅每篇文章了解详细信息。

## <a name="see-also"></a>另请参阅

- [F# 语言参考](index.md)
- [Classes](classes.md)
- [记录](records.md)
- [成员](./members/index.md)
