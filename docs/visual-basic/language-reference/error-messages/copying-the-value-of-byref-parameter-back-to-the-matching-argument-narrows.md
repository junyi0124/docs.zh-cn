---
title: 将“ByRef”参数“<parametername>”的值复制回匹配的参数将导致从类型“<typename1>”到类型“<typename2>”的收缩转换
ms.date: 07/20/2015
f1_keywords:
- bc32053
- vbc32053
helpviewer_keywords:
- BC32053
ms.assetid: 281564b7-99f7-451f-b10d-f985e831bb25
ms.openlocfilehash: b9a38d3eb4e25d5c9ac765adf47df72e45fd082a
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92160927"
---
# <a name="bc32053-copying-the-value-of-byref-parameter-parametername-back-to-the-matching-argument-narrows-from-type-typename1-to-type-typename2"></a>BC32053：将 "ByRef" 参数 "" 的值复制 \<parametername> 回匹配的参数将导致从类型 " \<typename1> " 到类型 " \<typename2> " 的收缩

使用扩展到相应参数类型的自变量调用过程，并从参数到自变量的转换为收缩。

 在定义类或结构时，可以定义一个或多个转换运算符来将该类或结构类型转换为其他类型。 也可以定义反向转换运算符来将这些其他类型转换回类或结构类型。 当你在过程调用中使用你的类或结构类型时，Visual Basic 可以使用这些转换运算符将参数的类型转换为其对应参数的类型。

 如果传递参数 [ByRef](../modifiers/byref.md)，Visual Basic 有时会将参数值复制到过程的局部变量中，而不是传递引用。 在这种情况下，当过程返回时，Visual Basic 必须随后将局部变量值复制回调用代码中的参数。

 如果将 `ByRef` 实参值复制到过程中，并且实参与形参为同一类型，则不必进行转换。 但是，如果类型不同，则 Visual Basic 必须双向转换。 如果其中一个类型是你的类或结构类型，则 Visual Basic 必须将其转换为另一种类型。 如果其中一个转换为扩大，则反向转换可能会收缩。

 **错误 ID：** BC32053

## <a name="to-correct-this-error"></a>更正此错误

- 如果可能，请使用与过程参数相同的类型的调用参数，这样 Visual Basic 不需要进行任何转换。

- 如果需要调用实参类型与形参类型不同的过程，但不需要将值返回到调用实参中，请将形参定义为 [ByVal](../modifiers/byval.md) 而不是 `ByRef`。

- 如果需要将值返回到调用自变量中，请尽可能将反向转换运算符定义为 [扩大](../modifiers/widening.md)转换运算符。

## <a name="see-also"></a>另请参阅

- [过程](../../programming-guide/language-features/procedures/index.md)
- [过程形参和实参](../../programming-guide/language-features/procedures/procedure-parameters-and-arguments.md)
- [按值和按引用传递参数](../../programming-guide/language-features/procedures/passing-arguments-by-value-and-by-reference.md)
- [运算符过程](../../programming-guide/language-features/procedures/operator-procedures.md)
- [Operator Statement](../statements/operator-statement.md)
- [如何：定义运算符](../../programming-guide/language-features/procedures/how-to-define-an-operator.md)
- [如何：定义转换运算符](../../programming-guide/language-features/procedures/how-to-define-a-conversion-operator.md)
- [Visual Basic 中的类型转换](../../programming-guide/language-features/data-types/type-conversions.md)
- [Widening and Narrowing Conversions](../../programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)
