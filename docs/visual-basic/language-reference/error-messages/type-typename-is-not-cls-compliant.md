---
title: 类型 <typename> 不符合 CLS
ms.date: 07/20/2015
f1_keywords:
- bc40041
- vbc40041
helpviewer_keywords:
- BC40041
ms.assetid: 634132c2-5646-44aa-98c6-f773e2e63882
ms.openlocfilehash: c50e721e9170a402724a11f3aab573c7e8abf4c1
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92161161"
---
# <a name="bc40041-type-typename-is-not-cls-compliant"></a>BC40041：类型 \<typename> 不符合 CLS

使用不符合 CLS 的数据类型声明了变量、属性或函数返回。

 若要使应用程序符合 [语言独立性并 Language-Independent 组件](../../../standard/language-independence-and-language-independent-components.md) (CLS) ，则该应用程序必须只使用符合 cls 的类型。

 以下 Visual Basic 数据类型不符合 CLS：

- [SByte 数据类型](../data-types/sbyte-data-type.md)

- [UInteger 数据类型](../data-types/uinteger-data-type.md)

- [ULong 数据类型](../data-types/ulong-data-type.md)

- [UShort 数据类型](../data-types/ushort-data-type.md)

 **错误 ID：** BC40041

## <a name="to-correct-this-error"></a>更正此错误

- 如果你的应用程序需要符合 CLS，请将此元素的数据类型更改为最接近的符合 CLS 的类型。 例如，如果不需要 2147483647 以上的数值范围，可以使用 `UInteger` 取代 `Integer` 。 如果确实需要更大范围，可以用 `UInteger` 代替 `Long`。

- 如果你的应用程序不需要符合 CLS，则无需更改任何内容。 不过，您应该知道其不相容性。
