---
title: 参数“<parametername>”的类型不符合 CLS
ms.date: 07/20/2015
f1_keywords:
- vbc40028
- bc40028
helpviewer_keywords:
- BC40028
ms.assetid: dfa1f6f9-bb88-44ad-b85f-149144363d41
ms.openlocfilehash: 7043138f770264b73eeac79cd262104b88cd8516
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92161226"
---
# <a name="bc40028-type-of-parameter-parametername-is-not-cls-compliant"></a>BC40028：参数 "" 的类型 \<parametername> 不符合 CLS

过程标记为 `<CLSCompliant(True)>` ，但声明的参数具有标记为的类型 `<CLSCompliant(False)>` ，未标记或不合格，因为它是不符合要求的类型。

 一个过程要符合[语言独立性和与语言无关的组件](../../../standard/language-independence-and-language-independent-components.md) (CLS)，必须只使用符合 CLS 的类型。 这适用于参数的类型、返回类型及其所有本地变量的类型。

 以下 Visual Basic 数据类型不符合 CLS：

- [SByte 数据类型](../data-types/sbyte-data-type.md)

- [UInteger 数据类型](../data-types/uinteger-data-type.md)

- [ULong 数据类型](../data-types/ulong-data-type.md)

- [UShort 数据类型](../data-types/ushort-data-type.md)

 当将 <xref:System.CLSCompliantAttribute> 应用到编程元素中时，需要将该特性的 `isCompliant` 参数设置为 `True` 或 `False` 来指示符合或不符合性。 此参数没有默认值，必须为其提供一个值。

 如果不将 <xref:System.CLSCompliantAttribute> 应用到元素，则它将被视为不符合规范。

 默认情况下，此消息是一个警告。 有关隐藏警告或将警告视为错误的信息，请参见 [Configuring Warnings in Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic)。

 **错误 ID：** BC40028

## <a name="to-correct-this-error"></a>更正此错误

- 如果过程必须采用此特定类型的参数，请删除 <xref:System.CLSCompliantAttribute> 。 该过程不符合 CLS。

- 如果该过程必须符合 CLS，则将此参数的类型更改为最接近的符合 CLS 的类型。 例如，如果不需要 2147483647 以上的数值范围，可以使用 `UInteger` 取代 `Integer` 。 如果确实需要更大范围，可以用 `UInteger` 代替 `Long`。

- 如果要与自动化或 COM 对象进行交互，请记住，某些类型的数据宽度与 .NET Framework 中的不同。 例如，`int` 在其他环境中通常为 16 位。 如果接受此类组件的16位整数，请 `Short` `Integer` 在托管的 Visual Basic 代码中将其声明为而不是。
