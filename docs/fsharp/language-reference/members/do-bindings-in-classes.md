---
title: 类中的 do 绑定
description: 了解如何在类定义F#中使用 "do" 绑定, 此类定义在构造对象或首次使用类型时执行操作。
ms.date: 05/16/2016
ms.openlocfilehash: ced4f1bb17d9e23bf51cc79b5a275cc334cca013
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2019
ms.locfileid: "68627585"
---
# <a name="do-bindings-in-classes"></a>类中的 do 绑定

构造`do`对象时, 类定义中的绑定将执行操作; 对于静态`do`绑定, 在首次使用该类型时执行操作。

## <a name="syntax"></a>语法

```fsharp
[static] do expression
```

## <a name="remarks"></a>备注

绑定与`let`绑定一起显示, 但在类定义中的成员定义之前。 `do` 尽管关键字对于`do`模块级别的绑定是可选的, 但它对于`do`类定义中的绑定是不可选的。 `do`

对于任何给定类型的每个对象的构造, 将按照它们`do`在类定义中的`let`显示顺序来执行非静态绑定和非静态绑定。 一`do`种类型可以发生多个绑定。 非静态`let`绑定和非静态`do`绑定成为主构造函数的主体。 非静态`do`绑定部分中的代码可以引用主构造函数参数以及在 "绑定" `let`部分中定义的任何值或函数。

如果类具有`do` `as`由类标题中的关键字定义的自我标识符, 则非静态绑定可以访问类的成员, 只要这些成员的所有使用都是用类的 self 标识符限定的。

由于`let`绑定初始化类的私有字段, 通常需要确保成员按预期方式运行, 因此绑定通常置于绑定`do`后`let` , 以便`do`绑定中的代码可以使用完全初始化的对象执行。 如果你的代码在初始化完成之前尝试使用成员, 则会引发 InvalidOperationException。

静态`do`绑定可以引用封闭类的静态成员或字段, 而不能引用实例成员或字段。 静态`do`绑定成为类的静态初始值设定项的一部分, 保证在第一次使用类之前执行。

对于类型中的`do`绑定, 将忽略属性。 如果在`do`绑定中执行的代码需要特性, 则必须将该特性应用于主构造函数。

在下面的代码中, 类具有静态`do`绑定和非静态`do`绑定。 对象具有一个构造函数, 该构造函数具有`a`两`b`个参数: 和, 并且在类的`let`绑定中定义了两个私有字段。 还定义了两个属性。 所有这些都在非静态`do`绑定部分的范围内, 如打印所有这些值的行所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet3101.fs)]

输出如下所示。

```console
Initializing MyType.
Initializing object 1 2 2 4 8 16
```

## <a name="see-also"></a>请参阅

- [成员](index.md)
- [类](../classes.md)
- [构造函数](constructors.md)
- [类中的 `let` 绑定](let-bindings-in-classes.md)
- [`do`绑定](../functions/do-Bindings.md)
