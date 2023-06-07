---
title: 方法
description: '了解 F # 方法是与用于公开和实现对象和类型的功能和行为的类型相关联的函数。'
ms.date: 11/04/2019
ms.openlocfilehash: 6f5ae76ea450b07763eb58d0c95b18b30f634551
ms.sourcegitcommit: 67ebdb695fd017d79d9f1f7f35d145042d5a37f7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92224110"
---
# <a name="methods"></a>方法

*方法*是与类型相关联的函数。 在面向对象的编程中，方法用于公开和实现对象和类型的功能和行为。

## <a name="syntax"></a>语法

```fsharp
// Instance method definition.
[ attributes ]
member [inline] self-identifier.method-name parameter-list [ : return-type ] =
    method-body

// Static method definition.
[ attributes ]
static member [inline] method-name parameter-list [ : return-type ] =
    method-body

// Abstract method declaration or virtual dispatch slot.
[ attributes ]
abstract member method-name : type-signature

// Virtual method declaration and default implementation.
[ attributes ]
abstract member method-name : type-signature
[ attributes ]
default self-identifier.method-name parameter-list [ : return-type ] =
    method-body

// Override of inherited virtual method.
[ attributes ]
override self-identifier.method-name parameter-list [ : return-type ] =
    method-body

// Optional and DefaultParameterValue attributes on input parameters
[ attributes ]
[ modifier ] member [inline] self-identifier.method-name ([<Optional; DefaultParameterValue( default-value )>] input) [ : return-type ]
```

## <a name="remarks"></a>备注

在前面的语法中，可以看到各种形式的方法声明和定义。 在较长的方法体中，等号 (=) ，并缩进整个方法体。

特性可应用于任何方法声明。 它们位于方法定义的语法之前，通常在单独的行中列出。 有关更多信息，请参阅[特性](../attributes.md)。

可以标记方法 `inline` 。 有关 `inline` 的信息，请参阅[内联函数](../functions/inline-functions.md)。

非内联方法可在类型内递归使用;无需显式使用 `rec` 关键字。

## <a name="instance-methods"></a>实例方法

实例方法使用 `member` 关键字和 *自标识符*进行声明，后跟一个句点 (。 ) 和方法名称和参数。 与绑定的情况一样 `let` ， *参数列表* 可以是一种模式。 通常，将方法参数括在元组形式的括号中，这是在以其他 .NET Framework 语言创建方法时方法在 F # 中的显示方式。 不过，扩充格式 (参数由空格分隔) 也很常见，并且还支持其他模式。

下面的示例演示如何定义和使用非抽象实例方法。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet3401.fs)]

在实例方法中，不要使用 self 标识符访问使用 let bindings 定义的字段。 在访问其他成员和属性时使用 self 标识符。

## <a name="static-methods"></a>静态方法

关键字 `static` 用于指定可在没有实例的情况下调用方法，并且不与对象实例相关联。 否则，方法是实例方法。

下一节中的示例演示使用关键字声明的字段 `let` 、使用关键字声明的属性成员 `member` 以及使用关键字声明的静态方法 `static` 。

下面的示例演示如何定义和使用静态方法。 假设前面部分的类中有这些方法定义 `SomeType` 。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet3402.fs)]

## <a name="abstract-and-virtual-methods"></a>抽象方法和虚方法

关键字 `abstract` 指示方法具有虚拟调度槽，并且在类中可能不具有定义。 *虚拟调度槽*是在运行时用于在面向对象的类型中查找虚拟函数调用的函数的内部维护表中的条目。 虚拟调度机制是实现 *多态性*的机制，这是面向对象编程的一项重要功能。 至少具有一个没有定义的抽象方法的类是一个 *抽象类*，这意味着不能创建该类的任何实例。 有关抽象类的详细信息，请参阅 [抽象类](../abstract-classes.md)。

抽象方法声明不包括方法体。 相反，方法的名称后跟一个冒号 (： ) 和方法的类型签名。 当你将鼠标指针悬停在 Visual Studio Code 编辑器中的方法名称上（不包含参数名称）时，方法的类型签名与 IntelliSense 显示的类型签名相同。 当交互处理时，fsi.exe 的解释器也会显示类型签名。 方法的类型签名通过使用适当的分隔符符号列出参数的类型，后跟返回类型。 扩充参数由分隔 `->` ，元组参数由分隔 `*` 。 返回值始终与参数之间用 `->` 符号隔开。 括号可用于对复杂参数分组，如函数类型为参数时，或指示将元组视为单个参数而不是两个参数的时间。

还可以通过将定义添加到类并使用关键字来为抽象方法默认定义 `default` ，如本主题中的语法块中所示。 在同一个类中具有定义的抽象方法等效于其他 .NET Framework 语言的虚方法。 无论定义是否存在， `abstract` 关键字都会在类的虚拟函数表中创建新的调度槽。

无论基类是否实现其抽象方法，派生类都可以提供抽象方法的实现。 若要在派生类中实现抽象方法，请在派生类中定义具有相同名称和签名的方法，但使用 `override` 或 `default` 关键字，并提供方法体。 关键字 `override` 和 `default` 的含义完全相同。 `override`如果新方法重写基类实现，则使用; 在与 `default` 原始抽象声明相同的类中创建实现时使用。 不要 `abstract` 在实现基类中声明为抽象的方法的方法上使用关键字。

下面的示例演示 `Rotate` 具有默认实现的抽象方法，该方法等效于 .NET Framework 虚方法。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet3403.fs)]

下面的示例演示了一个重写基类方法的派生类。 在这种情况下，重写将更改行为，使方法不执行任何操作。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet3404.fs)]

## <a name="overloaded-methods"></a>重载方法

重载方法是在给定类型中具有相同名称但具有不同参数的方法。 在 F # 中，通常使用可选参数，而不是重载方法。 但是，如果参数采用的是元组格式，而不是扩充形式，则允许使用重载方法。

## <a name="optional-arguments"></a>可选实参

从 F # 4.1 开始，还可以在方法中使用带有默认参数值的可选参数。  这有助于简化与 c # 代码的互操作。  下面的示例演示了语法：

```fsharp
// A class with a method M, which takes in an optional integer argument.
type C() =
    _.M([<Optional; DefaultParameterValue(12)>] i) = i + 1
```

请注意，为传入的值 `DefaultParameterValue` 必须与输入类型匹配。  在上面的示例中，它是一个 `int` 。  尝试将一个非整数值传递到 `DefaultParameterValue` 将导致编译错误。

## <a name="example-properties-and-methods"></a>示例：属性和方法

下面的示例包含一个类型，该类型包含字段、私有函数、属性和静态方法的示例。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet3406.fs)]

## <a name="see-also"></a>另请参阅

- [成员](index.md)
