---
title: 委托
description: 了解如何在中F#使用委托。
ms.date: 05/16/2016
ms.openlocfilehash: 65875897d5fc4b2ac66f1dfbe913f29fb74137cd
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2019
ms.locfileid: "68630366"
---
# <a name="delegates"></a>委托

委托将函数调用表示为对象。 在F#中, 通常应使用函数值将函数表示为第一类值;但是, 在 .NET Framework 中使用委托, 因此在与预期它们的 Api 进行互操作时需要使用委托。 在创作旨在使用其他 .NET Framework 语言的库时, 还可以使用它们。

## <a name="syntax"></a>语法

```fsharp
type delegate-typename = delegate of type1 -> type2
```

## <a name="remarks"></a>备注

在前面的语法中`type1` , 表示参数类型或类型, `type2`表示返回类型。 所表示`type1`的参数类型会自动扩充。 这表明, 对于此类型, 如果目标函数的参数为扩充, 则使用元组窗体, 并为已在元组格式中的参数使用带括号的元组。 自动 currying 将删除一组括号, 并保留与目标方法匹配的元组参数。 有关应在每种情况下使用的语法, 请参阅代码示例。

委托可以附加到F#函数值、静态方法或实例方法。 F#函数值可以作为参数直接作为委托构造函数的参数进行传递。 对于静态方法, 您可以使用类的名称和方法构造委托。 对于实例方法, 请在一个参数中提供对象实例和方法。 在这两种情况下, 都使用`.`成员访问运算符 ()。

委托`Invoke`类型上的方法调用封装的函数。 此外, 可以通过引用调用方法名称 (不带括号) 将委托作为函数值进行传递。

下面的代码演示了用于创建委托的语法, 这些委托表示类中的各种方法。 根据该方法是静态方法还是实例方法, 以及它是否具有元组格式或扩充形式的参数, 声明和分配委托的语法略有不同。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4201.fs)]

下面的代码演示了一些可以使用委托的不同方式。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4202.fs)]

前面的代码示例的输出如下所示。

```console
aaaaa
bbbbb
ccccc
[|"aaa"; "bbb"|]
```

## <a name="see-also"></a>请参阅

- [F# 语言参考](index.md)
- [参数和自变量](parameters-and-arguments.md)
- [事件](./members/events.md)
