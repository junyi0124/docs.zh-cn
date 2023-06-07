---
title: 隐式转换和显式转换
ms.date: 07/20/2015
helpviewer_keywords:
- conversions [Visual Basic], type
- variables [Visual Basic], changing data type
- casting
- conversions [Visual Basic], data type
- type conversion [Visual Basic], implicit conversions
- CType function [Visual Basic], conversions
- casting, data types
- data type conversion [Visual Basic], explicit
- type conversion [Visual Basic], explicit conversions
- data types [Visual Basic], casting
- conversions [Visual Basic], implicit
- explicit data type conversions [Visual Basic]
- conversions [Visual Basic]
- changing data types [Visual Basic]
- conversions [Visual Basic], explicit
- data type conversion [Visual Basic], implicit
- implicit data type conversions [Visual Basic]
ms.assetid: 77de1659-af8a-492c-967e-e7ef60ccce66
ms.openlocfilehash: 2858dafd2531bd846ad89672348d103f385c4511
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84393826"
---
# <a name="implicit-and-explicit-conversions-visual-basic"></a>隐式转换和显式转换 (Visual Basic)

*隐式转换*在源代码中无需任何特殊语法。 在下面的示例中，Visual Basic 将值隐式转换 `k` 为单精度浮点值，然后再将其赋值给 `q` 。

```vb
Dim k As Integer
Dim q As Double
' Integer widens to Double, so you can do this with Option Strict On.
k = 432
q = k
```

*显式转换*使用类型转换关键字。 Visual Basic 提供了几个这样的关键字，这些关键字将括号中的表达式强制转换为所需的数据类型。 这些关键字的作用类似于函数，但编译器将生成内联代码，因此执行速度要比使用函数调用稍快。

在前面的示例的以下扩展中， `CInt` 关键字将值转换为 `q` 整数，然后将其分配给 `k` 。

```vb
' q had been assigned the value 432 from k.
q = Math.Sqrt(q)
k = CInt(q)
' k now has the value 21 (rounded square root of 432).
```

## <a name="conversion-keywords"></a>转换关键字

下表显示了可用的转换关键字。

|类型转换关键字|将表达式转换为数据类型|允许转换的表达式的数据类型|
|---|---|---|
|`CBool`|[布尔数据类型](../../../language-reference/data-types/boolean-data-type.md)|任何数值类型（包括 `Byte` 、 `SByte` 、和枚举类型）、 `String` 、`Object`|
|`CByte`|[Byte 数据类型](../../../language-reference/data-types/byte-data-type.md)|任何数值类型（包括 `SByte` 和枚举类型）、 `Boolean` 、 `String` 、`Object`|
|`CChar`|[Char 数据类型](../../../language-reference/data-types/char-data-type.md)|`String`, `Object`|
|`CDate`|[Date 数据类型](../../../language-reference/data-types/date-data-type.md)|`String`, `Object`|
|`CDbl`|[Double 数据类型](../../../language-reference/data-types/double-data-type.md)|任何数值类型（包括 `Byte` 、 `SByte` 、和枚举类型）、 `Boolean` 、 `String` 、`Object`|
|`CDec`|[Decimal 数据类型](../../../language-reference/data-types/decimal-data-type.md)|任何数值类型（包括 `Byte` 、 `SByte` 、和枚举类型）、 `Boolean` 、 `String` 、`Object`|
|`CInt`|[Integer 数据类型](../../../language-reference/data-types/integer-data-type.md)|任何数值类型（包括 `Byte` 、 `SByte` 、和枚举类型）、 `Boolean` 、 `String` 、`Object`|
|`CLng`|[Long 数据类型](../../../language-reference/data-types/long-data-type.md)|任何数值类型（包括 `Byte` 、 `SByte` 、和枚举类型）、 `Boolean` 、 `String` 、`Object`|
|`CObj`|[Object Data Type](../../../language-reference/data-types/object-data-type.md)|任何类型|
|`CSByte`|[SByte 数据类型](../../../language-reference/data-types/sbyte-data-type.md)|任何数值类型（包括 `Byte` 和枚举类型）、 `Boolean` 、 `String` 、`Object`|
|`CShort`|[Short 数据类型](../../../language-reference/data-types/short-data-type.md)|任何数值类型（包括 `Byte` 、 `SByte` 、和枚举类型）、 `Boolean` 、 `String` 、`Object`|
|`CSng`|[Single 数据类型](../../../language-reference/data-types/single-data-type.md)|任何数值类型（包括 `Byte` 、 `SByte` 、和枚举类型）、 `Boolean` 、 `String` 、`Object`|
|`CStr`|[String 数据类型](../../../language-reference/data-types/string-data-type.md)|任何数值类型（包括 `Byte` 、 `SByte` 、和枚举类型）、 `Boolean` 、 `Char` 、 `Char` 数组、 `Date` 、`Object`|
|`CType`|在逗号（）之后指定的类型 `,`|转换为*基本数据类型*（包括基本类型的数组）时，为相应的转换关键字允许的相同类型<br /><br /> 转换为*复合数据类型*时，它实现的接口和它所继承的类<br /><br /> 转换为您在其上重载的类或结构时 `CType` ，该类或结构|
|`CUInt`|[UInteger 数据类型](../../../language-reference/data-types/uinteger-data-type.md)|任何数值类型（包括 `Byte` 、 `SByte` 、和枚举类型）、 `Boolean` 、 `String` 、`Object`|
|`CULng`|[ULong 数据类型](../../../language-reference/data-types/ulong-data-type.md)|任何数值类型（包括 `Byte` 、 `SByte` 、和枚举类型）、 `Boolean` 、 `String` 、`Object`|
|`CUShort`|[UShort 数据类型](../../../language-reference/data-types/ushort-data-type.md)|任何数值类型（包括 `Byte` 、 `SByte` 、和枚举类型）、 `Boolean` 、 `String` 、`Object`|

## <a name="the-ctype-function"></a>CType 函数

[CType 函数](../../../language-reference/functions/ctype-function.md)对两个参数进行运算。 第一个是要转换的表达式，第二个是目标数据类型或对象类。 请注意，第一个参数必须是表达式，而不能是类型。

`CType`是*内联函数*，这意味着编译的代码通常会进行转换，而不会生成函数调用。 从而可以提高性能。

有关 `CType` 与其他类型转换关键字的比较，请参阅[DirectCast 运算符](../../../language-reference/operators/directcast-operator.md)和[TryCast 运算符](../../../language-reference/operators/trycast-operator.md)。

### <a name="elementary-types"></a>基本类型

以下示例演示了 `CType` 的用法。

```vb
k = CType(q, Integer)
' The following statement coerces w to the specific object class Label.
f = CType(w, Label)
```

### <a name="composite-types"></a>复合类型

您可以使用将 `CType` 值转换为复合数据类型和基本类型。 你还可以使用它将对象类强制转换为它的某个接口的类型，如下例所示。

```vb
' Assume class cZone implements interface iZone.
Dim h As Object
' The first argument to CType must be an expression, not a type.
Dim cZ As cZone
' The following statement coerces a cZone object to its interface iZone.
h = CType(cZ, iZone)
```

### <a name="array-types"></a>数组类型

`CType`还可以转换数组数据类型，如下面的示例中所示。

```vb
Dim v() As classV
Dim obArray() As Object
' Assume some object array has been assigned to obArray.
' Check for run-time type compatibility.
If TypeOf obArray Is classV()
    ' obArray can be converted to classV.
    v = CType(obArray, classV())
End If
```

有关详细信息和示例，请参阅[数组转换](array-conversions.md)。

### <a name="types-defining-ctype"></a>定义 CType 的类型

你可以 `CType` 对已定义的类或结构进行定义。 这使你可以将值转换为类或结构的类型。 有关详细信息和示例，请参阅[如何：定义转换运算符](../procedures/how-to-define-a-conversion-operator.md)。

> [!NOTE]
> 与转换关键字一起使用的值必须对目标数据类型有效，否则会发生错误。 例如，如果尝试将转换为 `Long` `Integer` ，则的值 `Long` 必须在数据类型的有效范围内 `Integer` 。

> [!CAUTION]
> `CType`如果源类型不是从目标类型派生的，则指定从一个类类型转换为另一个类类型会在运行时失败。 此类故障会引发 <xref:System.InvalidCastException> 异常。

但是，如果其中一个类型是你已定义的结构或类，并且在 `CType` 该结构或类上定义了，则如果转换满足你的要求，则转换可能会成功 `CType` 。 请参阅[如何：定义转换运算符](../procedures/how-to-define-a-conversion-operator.md)。

执行显式转换也称为将表达式*强制转换*为给定的数据类型或对象类。

## <a name="see-also"></a>另请参阅

- [Visual Basic 中的类型转换](type-conversions.md)
- [字符串和其他类型之间的转换](conversions-between-strings-and-other-types.md)
- [如何：在 Visual Basic 中将一个对象转换为其他类型](how-to-convert-an-object-to-another-type.md)
- [结构](structures.md)
- [数据类型](../../../language-reference/data-types/index.md)
- [Type Conversion Functions](../../../language-reference/functions/type-conversion-functions.md)
- [数据类型疑难解答](troubleshooting-data-types.md)
