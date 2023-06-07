---
title: 将“ByRef”参数“<typename1>”的值复制回匹配参数时，发生从“<typename2>”到“<parametername>”的隐式转换。
ms.date: 07/20/2015
f1_keywords:
- vbc41999
- bc41999
helpviewer_keywords:
- BC41999
ms.assetid: ae48c738-dff8-4c0f-8931-bbb70b2c8b03
ms.openlocfilehash: a95a4b792742efcc165f7c7a9592582d34618f11
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92162799"
---
# <a name="bc41999-implicit-conversion-from-typename1-to-typename2-in-copying-the-value-of-byref-parameter-parametername-back-to-the-matching-argument"></a>BC41999： \<typename1> 将 \<typename2> "ByRef" 参数 "" 的值复制 \<parametername> 回匹配参数时，从 "" 到 "" 的隐式转换。

使用与对应参数的类型不同的 [ByRef](../modifiers/byref.md) 参数调用过程。

 如果传递参数 `ByRef` ，Visual Basic 有时会将参数值复制到过程的局部变量中，而不是传递引用。 在这种情况下，当过程返回时，Visual Basic 必须随后将局部变量值复制回调用代码中的参数。

 如果将 `ByRef` 实参值复制到过程中，并且实参与形参为同一类型，则不必进行转换。 但是，如果类型不同，则 Visual Basic 必须双向转换。 由于不能 `CType` 在过程参数或参数上使用或任何其他转换关键字，因此此类转换始终是隐式的。

 默认情况下，此消息是一个警告。 有关隐藏警告或将警告视为错误的信息，请参见 [Configuring Warnings in Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic)。

 **错误 ID：** BC41999

## <a name="to-correct-this-error"></a>更正此错误

- 如果可能，请使用与过程参数相同的类型的调用参数，这样 Visual Basic 不需要进行任何转换。

- 如果需要调用实参类型与形参类型不同的过程，但不需要将值返回到调用实参中，请将形参定义为 [ByVal](../modifiers/byval.md) 而不是 `ByRef`。

## <a name="see-also"></a>另请参阅

- [过程](../../programming-guide/language-features/procedures/index.md)
- [过程形参和实参](../../programming-guide/language-features/procedures/procedure-parameters-and-arguments.md)
- [按值和按引用传递参数](../../programming-guide/language-features/procedures/passing-arguments-by-value-and-by-reference.md)
- [隐式转换和显式转换](../../programming-guide/language-features/data-types/implicit-and-explicit-conversions.md)
