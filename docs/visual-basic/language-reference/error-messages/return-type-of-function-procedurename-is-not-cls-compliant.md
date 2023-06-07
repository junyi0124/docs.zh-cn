---
title: 函数“<procedurename>”的返回类型不符合 CLS
ms.date: 07/20/2015
f1_keywords:
- bc40027
- vbc40027
helpviewer_keywords:
- BC40027
ms.assetid: 33c088c7-48e7-400c-920e-6d8967e1f3fc
ms.openlocfilehash: 9a877046a1b30e2e3773a41b8b44573e11ff1c96
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92159698"
---
# <a name="bc40027-return-type-of-function-procedurename-is-not-cls-compliant"></a>BC40027：函数 "" 的返回类型 \<procedurename> 不符合 CLS

`Function`过程被标记为 `<CLSCompliant(True)>` ，但会返回一个标记为、未 `<CLSCompliant(False)>` 标记或不合格的类型，因为它是不符合要求的类型。

 一个过程要符合[语言独立性和与语言无关的组件](../../../standard/language-independence-and-language-independent-components.md) (CLS)，必须只使用符合 CLS 的类型。 这适用于参数的类型、返回类型及其所有本地变量的类型。

 以下 Visual Basic 数据类型不符合 CLS：

- [SByte 数据类型](../data-types/sbyte-data-type.md)

- [UInteger 数据类型](../data-types/uinteger-data-type.md)

- [ULong 数据类型](../data-types/ulong-data-type.md)

- [UShort 数据类型](../data-types/ushort-data-type.md)

 当将 <xref:System.CLSCompliantAttribute> 应用到编程元素中时，需要将该特性的 `isCompliant` 参数设置为 `True` 或 `False` 来指示符合或不符合性。 此参数没有默认值，必须为其提供一个值。

 如果不将 <xref:System.CLSCompliantAttribute> 应用到元素，则它将被视为不符合规范。

 默认情况下，此消息是一个警告。 有关隐藏警告或将警告视为错误的信息，请参见 [Configuring Warnings in Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic)。

 **错误 ID：** BC40027

## <a name="to-correct-this-error"></a>更正此错误

- 如果 `Function` 过程必须返回此特定类型，请删除 <xref:System.CLSCompliantAttribute> 。 该过程不符合 CLS。

- 如果该 `Function` 过程必须符合 cls，则将返回类型更改为最接近的符合 CLS 的类型。 例如，如果不需要 2147483647 以上的数值范围，可以使用 `UInteger` 取代 `Integer` 。 如果确实需要更大范围，可以用 `UInteger` 代替 `Long`。

- 如果要与自动化或 COM 对象进行交互，请记住，某些类型的数据宽度与 .NET Framework 中的不同。 例如，`int` 在其他环境中通常为 16 位。 如果要将16位整数返回给此类组件，请 `Short` `Integer` 在托管的 Visual Basic 代码中将其声明为而不是。
