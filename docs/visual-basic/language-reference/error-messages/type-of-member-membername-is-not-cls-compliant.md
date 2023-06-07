---
title: 成员“<membername>”的类型不符合 CLS
ms.date: 07/20/2015
f1_keywords:
- bc40025
- vbc40025
helpviewer_keywords:
- BC40025
ms.assetid: adbd34bb-43d2-4266-90e7-cd1afaf49b4e
ms.openlocfilehash: c8c14a2d6df8804967807af881384bc7d9ccd99f
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92163475"
---
# <a name="bc40025-type-of-member-membername-is-not-cls-compliant"></a>BC40025：成员 "" 的类型 \<membername> 不符合 CLS

为此成员指定的数据类型不是 [语言独立性的一部分，并且 Language-Independent 组件](../../../standard/language-independence-and-language-independent-components.md) (CLS) 。 这不是你的组件中的错误，因为 .NET Framework 和 Visual Basic 支持此数据类型。 但是，以严格符合 CLS 的代码编写的另一个组件可能不支持此数据类型。 此类组件可能无法与组件成功交互。

 以下 Visual Basic 数据类型不符合 CLS：

- [SByte 数据类型](../data-types/sbyte-data-type.md)

- [UInteger 数据类型](../data-types/uinteger-data-type.md)

- [ULong 数据类型](../data-types/ulong-data-type.md)

- [UShort 数据类型](../data-types/ushort-data-type.md)

 默认情况下，此消息是一个警告。 有关隐藏警告或将警告视为错误的详细信息，请参见 [Configuring Warnings in Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic)。

 **错误 ID：** BC40025

## <a name="to-correct-this-error"></a>更正此错误

- 如果你的组件只与其他 .NET Framework 组件交互，或者不与任何其他组件进行交互，则无需更改任何内容。

- 如果与不是为 .NET Framework 编写的组件进行交互，则可以通过反射或文档来确定它是否支持此数据类型。 如果是这样，则无需更改任何内容。

- 如果与不支持此数据类型的组件进行交互，则必须将其替换为最接近的符合 CLS 的类型。 例如，如果不需要 2147483647 以上的数值范围，可以使用 `UInteger` 取代 `Integer` 。 如果确实需要更大范围，可以用 `UInteger` 代替 `Long`。

- 如果要与自动化或 COM 对象进行交互，请记住，某些类型的数据宽度与 .NET Framework 中的不同。 例如，`uint` 在其他环境中通常为 16 位。 如果要将16位参数传递给此类组件，请 `UShort` `UInteger` 在托管的 Visual Basic 代码中将其声明为而不是。

## <a name="see-also"></a>请参阅

- [反射](../../../framework/reflection-and-codedom/reflection.md)
