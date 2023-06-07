---
title: 类中的 let 绑定
description: 了解如何通过在类定义中使用 "let F# " 绑定来定义类的私有字段和私有函数。
ms.date: 05/16/2016
ms.openlocfilehash: 1366ab8f1f4f606fe5947a8fc4df10de49346b3e
ms.sourcegitcommit: 56f1d1203d0075a461a10a301459d3aa452f4f47
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2019
ms.locfileid: "71216532"
---
# <a name="let-bindings-in-classes"></a>类中的 let 绑定

您可以使用`let`类定义中的绑定为F#类定义私有字段和私有函数。

## <a name="syntax"></a>语法

```fsharp
// Field.
[static] let [ mutable ] binding1 [ and ... binding-n ]

// Function.
[static] let [ rec ] binding1 [ and ... binding-n ]
```

## <a name="remarks"></a>备注

前面的语法出现在类标题和继承声明之后，但在任何成员定义之前。 语法类似于`let`类之外的绑定，但类中定义的名称的作用域限制为类。 `let`绑定创建私有字段或函数; 若要公开数据或函数，请声明属性或成员方法。

`let`非静态`let`绑定称为实例绑定。 创建`let`对象时将执行实例绑定。 静态`let`绑定是类的静态初始值设定项的一部分，可保证在第一次使用该类型之前执行。

实例`let`绑定中的代码可以使用主构造函数的参数。

类中的`let`绑定不允许使用属性和可访问性修饰符。

下面的代码示例演示类中的`let`几种类型的绑定。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet3001.fs)]

输出如下所示。

```console
10 52 1 204
```

## <a name="alternative-ways-to-create-fields"></a>创建字段的替代方法

你还可以使用`val`关键字来创建私有字段。 使用`val`关键字时，在创建对象时不会为该字段指定值，而是使用默认值对其进行初始化。 有关详细信息, 请[参阅显式字段:Val 关键字](explicit-fields-the-val-keyword.md)。

还可以通过使用成员定义并向定义中添加关键字`private` ，在类中定义私有字段。 如果希望在不重写代码的情况下更改成员的可访问性，这会很有用。 有关详细信息，请参阅[访问控制](../access-control.md)。

## <a name="see-also"></a>请参阅

- [成员](index.md)
- [类中的 `do` 绑定](do-bindings-in-classes.md)
- [`let`绑定](../functions/let-bindings.md)
