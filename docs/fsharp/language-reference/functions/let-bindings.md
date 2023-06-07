---
title: let 绑定
description: '了解如何使用 F # "let" 绑定，该绑定将标识符与值或函数关联起来。'
ms.date: 05/16/2016
ms.openlocfilehash: 6f2396f480c5e6c631d0022f4732419ee5b07db6
ms.sourcegitcommit: 9c45035b781caebc63ec8ecf912dc83fb6723b1f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2020
ms.locfileid: "88812219"
---
# <a name="let-bindings"></a>let 绑定

*绑定*将标识符与值或函数关联。 使用 `let` 关键字可将名称绑定到值或函数。

## <a name="syntax"></a>语法

```fsharp
// Binding a value:
let identifier-or-pattern [: type] =expressionbody-expression
// Binding a function value:
let identifier parameter-list [: return-type ] =expressionbody-expression
```

## <a name="remarks"></a>备注

`let`关键字用于定义一个或多个名称的值或函数值的绑定表达式。 表达式的最简单形式是将 `let` 名称绑定到简单值，如下所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1101.fs)]

如果使用新行将表达式与标识符分隔开，则必须缩进表达式的每一行，如以下代码所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1102.fs)]

例如，可以指定包含名称的模式，而不只是名称，如下面的代码所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1103.fs)]

*主体表达式*是使用其名称的表达式。 主体表达式显示在其自己的行上，并与关键字中的第一个字符完全对齐 `let` ：

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1104.fs)]

`let`绑定可以出现在模块级别、类类型的定义或本地范围中，如函数定义中的。 `let`位于模块或类类型中的顶级绑定不需要具有主体表达式，而在其他范围级别，则需要主体表达式。 绑定名称可在定义点后使用，但在 `let` 绑定出现之前不会出现，如以下代码所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1105.fs)]

## <a name="function-bindings"></a>函数绑定

函数绑定遵循值绑定规则，只不过函数绑定包括函数名称和参数，如以下代码所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1106.fs)]

通常，参数是模式，如元组模式：

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1107.fs)]

`let`绑定表达式的计算结果为最后一个表达式的值。 因此，在下面的代码示例中，的值 `result` 从计算得出 `100 * function3 (1, 2)` ，其计算结果为 `300` 。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1109.fs)]

有关详细信息，请参阅[函数](index.md)。

## <a name="type-annotations"></a>类型批注

您可以通过包含冒号 (： ) 后跟类型名称，并将其括在括号中来指定参数的类型。 还可以通过在最后一个参数后面追加冒号和类型来指定返回值的类型。 的完整类型注释 `function1` （带有整数作为参数类型）将如下所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1108.fs)]

如果没有显式类型参数，则使用类型推理来确定函数的参数类型。 这可能包括自动将参数类型通用化为泛型。

有关详细信息，请参阅 [自动泛化](../generics/automatic-generalization.md) 和 [类型推理](../type-inference.md)。

## <a name="let-bindings-in-classes"></a>类中的 let 绑定

`let`绑定可以出现在类类型中，但不能出现在结构或记录类型中。 若要在类类型中使用 let 绑定，此类必须具有主构造函数。 构造函数参数必须出现在类定义中的类型名称之后。 `let`类类型中的绑定定义此类类型的私有字段和成员，并且在类型中 `do` 构成类型的主构造函数的代码。 下面的代码示例显示了 `MyClass` 具有私有字段和的类 `field1` `field2` 。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1110.fs)]

和的作用 `field1` 域 `field2` 仅限于在其中声明它们的类型。 有关详细信息，请参阅类和[类](../classes.md) [ `let` 中的绑定](../members/let-bindings-in-classes.md)。

## <a name="type-parameters-in-let-bindings"></a>Let 绑定中的类型参数

`let`模块级别、类型或计算表达式中的绑定可以具有显式类型参数。 表达式中的 let 绑定，如函数定义中的，不能具有类型参数。 有关详细信息，请参阅[泛型](../generics/index.md)。

## <a name="attributes-on-let-bindings"></a>Let 绑定上的属性

特性可应用于模块中的顶级 `let` 绑定，如下面的代码所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1111.fs)]

## <a name="scope-and-accessibility-of-let-bindings"></a>Let 绑定的作用域和可访问性

使用 let 绑定声明的实体的作用域仅限于包含作用域的部分 (例如，绑定显示后的函数、模块、文件或类) 。 因此，可以说，let binding 将名称引入到作用域中。 在模块中，只要模块可访问，模块的客户端就可以访问一个允许绑定值或函数，因为模块中的 let 绑定将编译为模块的公共函数。 与此相反，让类中的绑定对于类是私有的。

通常情况下，模块中的函数必须由客户端代码使用时的模块名称限定。 例如，如果模块 `Module1` 具有函数 `function1` ，则用户将指定 `Module1.function1` 以引用函数。

模块用户可以使用导入声明使该模块内的函数可供使用，而无需由模块名称限定。 在上述示例中，在这种情况下，模块的用户可以打开该模块，方法是使用导入声明打开 `Module1` ，然后 `function1` 直接引用。

```fsharp
module Module1 =
    let function1 x = x + 1.0

module Module2 =
    let function2 x =
        Module1.function1 x

open Module1

let function3 x =
    function1 x
```

某些模块的属性为 [RequireQualifiedAccess](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-requirequalifiedaccessattribute.html)，这意味着它们公开的函数必须使用模块的名称进行限定。 例如，F # List 模块具有此属性。

有关模块和访问控制的详细信息，请参阅 [模块](../modules.md) 和 [访问控制](../access-control.md)。

## <a name="see-also"></a>请参阅

- [函数](index.md)
- [`let` 类中的绑定](../members/let-bindings-in-classes.md)
