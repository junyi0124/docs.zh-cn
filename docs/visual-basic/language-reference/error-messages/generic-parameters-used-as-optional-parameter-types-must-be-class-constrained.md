---
title: 用作可选参数类型的泛型参数必须受类约束
ms.date: 07/20/2015
f1_keywords:
- vbc32124
- bc32124
helpviewer_keywords:
- BC32124
ms.assetid: 55aa8b2a-9ce3-4620-a710-2f9b0feb6143
ms.openlocfilehash: 5e0d4eaf7557eb9a544a8845299f3d69dbb78486
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92163215"
---
# <a name="bc32124-generic-parameters-used-as-optional-parameter-types-must-be-class-constrained"></a>BC32124：用作可选参数类型的泛型参数必须受类约束

使用可选参数声明过程，该参数使用的类型参数不被约束为引用类型。

 必须始终为每个可选参数提供默认值。 如果参数为引用类型，则可选值必须为 `Nothing` ，它是任何引用类型的有效值。 但是，如果参数是值类型，则该类型必须是由 Visual Basic 预定义的基本数据类型。 这是因为组合值类型（如用户定义的结构）没有有效的默认值。

 将类型参数用于可选参数时，必须确保它是引用类型，以避免不具有有效默认值的值类型。 这意味着必须使用 `Class` 关键字或特定类的名称来约束类型参数。

 **错误 ID：** BC32124

## <a name="to-correct-this-error"></a>更正此错误

- 约束类型参数以仅接受引用类型，或不将其用于可选参数。

## <a name="see-also"></a>另请参阅

- [Generic Types in Visual Basic](../../programming-guide/language-features/data-types/generic-types.md)
- [Type List](../statements/type-list.md)
- [Class 语句](../statements/class-statement.md)
- [可选参数](../../programming-guide/language-features/procedures/optional-parameters.md)
- [结构](../../programming-guide/language-features/data-types/structures.md)
- [没](../nothing.md)
