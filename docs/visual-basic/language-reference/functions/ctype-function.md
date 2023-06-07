---
title: CType Function
ms.date: 07/20/2015
f1_keywords:
- vb.CType
helpviewer_keywords:
- expression conversion results
- explicit data type conversions [Visual Basic]
- CType function
- conversions [Visual Basic], expression
ms.assetid: dd4b29e7-6fa1-428c-877e-69955420bb72
ms.openlocfilehash: 88d609146648fe1b0c3124b99a65e85293fc0707
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84406425"
---
# <a name="ctype-function-visual-basic"></a>CType 函数 (Visual Basic)

返回将表达式显式转换为指定的数据类型、对象、结构、类或接口的结果。

## <a name="syntax"></a>语法

```vb
CType(expression, typename)
```

## <a name="parts"></a>组成部分

`expression`任何有效的表达式。 如果的值 `expression` 超出了所允许的范围 `typename` ，Visual Basic 会引发异常。

`typename`语句中子句内合法的任何表达式，即 `As` `Dim` 任何数据类型、对象、结构、类或接口的名称。

## <a name="remarks"></a>备注

> [!TIP]
> 你还可以使用以下函数来执行类型转换：
>
> - 类型转换函数，例如 `CByte` 、 `CDbl` 和， `CInt` 它们执行到特定数据类型的转换。 有关详细信息，请参阅 [Type Conversion Functions](type-conversion-functions.md)（类型转换函数）。
> - [DirectCast 运算符](../operators/directcast-operator.md)或[TryCast 运算符](../operators/trycast-operator.md)。 这些运算符要求一个类型继承自或实现另一个类型。 与在 `CType` 数据类型之间进行转换相比，它们可以提供更好的性能 `Object` 。

`CType`是内联编译的，这意味着转换代码是计算表达式的代码的一部分。 在某些情况下，代码运行速度更快，因为没有调用任何过程来执行转换。

如果未定义从到的 `expression` 转换 `typename` （例如，从到 `Integer` `Date` ），Visual Basic 将显示编译时错误消息。

如果转换在运行时失败，则会引发相应的异常。 如果收缩转换失败， <xref:System.OverflowException> 最常见的结果是。 如果未定义转换，则会 <xref:System.InvalidCastException> 引发。 例如，如果 `expression` 的类型为 `Object` 且其运行时类型没有转换为，则可能会发生这种情况 `typename` 。

如果或的数据类型 `expression` `typename` 是你定义的类或结构，则可以 `CType` 将该类或结构的定义为转换运算符。 这会使作为 `CType` *重载运算符*。 如果执行此操作，则可以控制与类或结构的转换的行为，包括可能引发的异常。

## <a name="overloading"></a>重载

`CType`还可以在代码外部定义的类或结构上重载运算符。 如果你的代码在此类或结构之间进行转换，请确保了解其运算符的行为 `CType` 。 有关详细信息，请参阅 [Operator Procedures](../../programming-guide/language-features/procedures/operator-procedures.md)。

## <a name="converting-dynamic-objects"></a>转换动态对象

动态对象的类型转换由使用或方法的用户定义的动态转换执行 <xref:System.Dynamic.DynamicObject.TryConvert%2A> <xref:System.Dynamic.DynamicMetaObject.BindConvert%2A> 。 如果使用的是动态对象，请使用 <xref:Microsoft.VisualBasic.Conversion.CTypeDynamic%2A> 方法转换动态对象。

## <a name="example"></a>示例

下面的示例使用 `CType` 函数将表达式转换为 `Single` 数据类型。

[!code-vb[VbVbalrFunctions#24](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrFunctions/VB/Class1.vb#24)]

有关其他示例，请参阅[隐式和显式转换](../../programming-guide/language-features/data-types/implicit-and-explicit-conversions.md)。

## <a name="see-also"></a>另请参阅

- <xref:System.OverflowException>
- <xref:System.InvalidCastException>
- [Type Conversion Functions](type-conversion-functions.md)
- [转换函数](conversion-functions.md)
- [Operator Statement](../statements/operator-statement.md)
- [如何：定义转换运算符](../../programming-guide/language-features/procedures/how-to-define-a-conversion-operator.md)
- [.NET Framework 中的类型转换](../../../standard/base-types/type-conversion.md)
