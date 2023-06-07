---
title: 重载
ms.date: 07/20/2015
f1_keywords:
- vb.Overloads
- Overloads
helpviewer_keywords:
- Overloads keyword [Visual Basic]
- hiding by signature
- Shadows keyword [Visual Basic]
- signature, hiding by
ms.assetid: 0c6820b8-25b2-4664-bc59-5ca93c99c042
ms.openlocfilehash: bd0931cab520f8580c0d7473a44e350752e287bb
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84392101"
---
# <a name="overloads-visual-basic"></a>Overloads (Visual Basic)

指定某一属性或过程重新声明一个或多个具有相同名称的现有属性或过程。

## <a name="remarks"></a>备注

*重载*是为同一范围内的给定属性或过程名称提供多个定义的做法。 重新声明具有不同签名的属性或过程有时称为 "*按签名隐藏*"。

## <a name="rules"></a>规则

- **声明上下文。** `Overloads`只能在属性或过程声明语句中使用。

- **组合修饰符。** 不能 `Overloads` 在同一过程声明中同时指定和[阴影](shadows.md)。

- **必需的差异。** 此声明中的*签名*必须与它重载的每个属性或过程的签名不同。 签名包含属性或过程名以及以下内容：

  - 参数的数量

  - 参数顺序

  - 参数的数据类型

  - 类型参数的数量（针对泛型过程）

  - 返回类型（仅针对转换运算符过程）

  所有重载都必须有相同的名称，但每个重载必须在上面的一个或多个方面与所有其他重载不同。 这使编译器在代码调用属性或过程时可以区分要使用哪个版本。

- **不允许的差异。** 对于重载属性或过程而言，更改下面的一项或多项无效，因为它们不是签名的一部分：

  - 它是否返回值（针对过程）

  - 返回值的数据类型（除转换运算符之外）

  - 参数或类型参数的名称

  - 对类型参数的约束（针对泛型过程）

  - 参数修饰符关键字（如 `ByRef` 或 `Optional`）

  - 属性或过程修饰符关键字（如 `Public` 或 `Shared`）

- **可选修饰符。** 在 `Overloads` 同一个类中定义多个重载属性或过程时，无需使用修饰符。 然而，如果在某个声明中使用 `Overloads`，则必须在所有的声明中使用它。

- **隐藏和重载。** `Overloads`还可用于在基类中隐藏现有成员或重载成员集。 以这种方式使用 `Overloads` 时，应使用与基类成员相同的名称和参数列表来声明属性或方法，并且不提供 `Shadows` 关键字。

如果使用 `Overrides`，编译器将隐式添加 `Overloads`，以便库 API 更轻松地使用 C#。

`Overloads` 修饰符可用于下面的上下文中：

- [Function 语句](../statements/function-statement.md)

- [Operator Statement](../statements/operator-statement.md)

- [Property Statement](../statements/property-statement.md)

- [Sub 语句](../statements/sub-statement.md)

## <a name="see-also"></a>另请参阅

- [Shadows](shadows.md)
- [过程重载](../../programming-guide/language-features/procedures/procedure-overloading.md)
- [Generic Types in Visual Basic](../../programming-guide/language-features/data-types/generic-types.md)
- [运算符过程](../../programming-guide/language-features/procedures/operator-procedures.md)
- [如何：定义转换运算符](../../programming-guide/language-features/procedures/how-to-define-a-conversion-operator.md)
