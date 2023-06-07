---
title: 构造函数
description: '了解如何在 F # 中定义和使用构造函数来创建和初始化类和结构对象。'
ms.date: 05/16/2016
ms.openlocfilehash: be8fc3f1d82a9a7c778a6d094139f14150a28813
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87303434"
---
# <a name="constructors"></a>构造函数

本文介绍如何定义和使用构造函数来创建和初始化类和结构对象。

## <a name="construction-of-class-objects"></a>类对象的构造

类类型的对象具有构造函数。 有两种类型的构造函数。 一个是主构造函数，其参数出现在括号中紧跟在类型名称后的括号中。 使用关键字指定其他可选的其他构造函数 `new` 。 任何此类附加构造函数都必须调用主构造函数。

主构造函数包含 `let` 和 `do` 绑定，它们显示在类定义的开头。 `let`绑定声明类的私有字段和方法; `do` 绑定执行代码。 有关 `let` 类构造函数中的绑定的详细信息，请参阅[ `let` 类中的绑定](let-bindings-in-classes.md)。 有关 `do` 构造函数中的绑定的详细信息，请参阅[ `do` 类中的绑定](do-bindings-in-classes.md)。

无论你要调用的构造函数是主构造函数还是附加构造函数，都可以通过使用 `new` 带有或不带可选关键字的表达式来创建对象 `new` 。 您可以将对象与构造函数参数一起初始化，方法是按顺序列出参数，并以逗号分隔并括在括号中，或者通过使用括号中的命名参数和值。 您还可以通过使用属性名称和赋值来设置对象的属性，就像使用命名构造函数参数一样。

下面的代码演示一个类，该类具有构造函数和用于创建对象的各种方式：

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet3501.fs)]

输出如下所示：

```console
Initialized object that has coordinates (1, 2, 3)
Initialized object that has coordinates (4, 5, 6)
Initialized object that has coordinates (7, 8, 9)
Initialized object that has coordinates (0, 0, 0)
```

## <a name="construction-of-structures"></a>结构的构造

结构遵循类的所有规则。 因此，您可以有一个主构造函数，并且可以通过使用提供其他构造函数 `new` 。 但结构和类之间存在一个重要的区别：结构可以有一个无参数的构造函数（即，没有参数的构造函数），即使未定义主构造函数也是如此。 无参数构造函数将所有字段初始化为该类型的默认值，通常为零或其等效的值。 为结构定义的任何构造函数都必须具有至少一个参数，使其不会与无参数构造函数发生冲突。

此外，结构通常包含使用关键字创建的字段 `val` ; 类也可以包含这些字段。 具有使用关键字定义的字段的结构和类 `val` 也可以通过使用记录表达式在其他构造函数中进行初始化，如以下代码所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet3502.fs)]

有关详细信息，请参阅[Explicit 字段： `val` 关键字](explicit-fields-the-val-keyword.md)。

## <a name="executing-side-effects-in-constructors"></a>在构造函数中执行副作用

类中的主构造函数可执行绑定中的代码 `do` 。 但是，如果必须在没有绑定的情况下在其他构造函数中执行代码怎么办 `do` ？ 为此，请使用 `then` 关键字。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet3503.fs)]

主构造函数的副作用仍在执行。 因此，输出如下所示：

```console
Created a person object.
Created a person object.
Created an invalid person object.
```

`then`需要而不是另一个的原因 `do` 是 `do` 关键字 `unit` 在附加构造函数的主体中存在时，其标准含义是界定返回的表达式。 它仅在主构造函数的上下文中具有特殊意义。

## <a name="self-identifiers-in-constructors"></a>构造函数中的自我标识符

在其他成员中，您可以在每个成员的定义中提供当前对象的名称。 你还可以使用 `as` 紧跟构造函数参数的关键字，将自标识符置于类定义的第一行。 下面的示例演示了此语法。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet3504.fs)]

在其他构造函数中，还可以通过 `as` 将子句置于构造函数参数之后来定义自定义标识符。 下面的示例演示了此语法：

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet3505.fs)]

当你尝试在完全定义对象之前使用对象时，可能会出现问题。 因此，使用 self 标识符可能导致编译器发出警告并插入其他检查，以确保在初始化对象之前不会访问对象的成员。 只应在主构造函数的绑定中使用自标识符 `do` ，或在 `then` 其他构造函数的关键字之后使用。

自身标识符的名称不必是 `this` 。 它可以是任何有效的标识符。

## <a name="assigning-values-to-properties-at-initialization"></a>在初始化时将值分配给属性

您可以通过将窗体赋值的列表追加 `property = value` 到构造函数的参数列表，将值分配给初始化代码中类对象的属性。 如下面的代码示例所示：

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet3506.fs)]

以下版本的前面的代码演示了一个构造函数调用中的普通参数、可选参数和属性设置的组合：

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet3507.fs)]

## <a name="constructors-in-inherited-class"></a>继承类中的构造函数

从具有构造函数的基类继承时，必须在 inherit 子句中指定其参数。 有关详细信息，请参阅[构造函数和继承](../inheritance.md#constructors-and-inheritance)。

## <a name="static-constructors-or-type-constructors"></a>静态构造函数或类型构造函数

除了指定用于创建对象的代码以外，还 `let` `do` 可以在类类型中创作静态和绑定，该类型首先用于在类型级别执行初始化。 有关详细信息，请参阅类中的[ `let` 绑定](let-bindings-in-classes.md)和[ `do` 类中的绑定](do-bindings-in-classes.md)。

## <a name="see-also"></a>请参阅

- [成员](index.md)
