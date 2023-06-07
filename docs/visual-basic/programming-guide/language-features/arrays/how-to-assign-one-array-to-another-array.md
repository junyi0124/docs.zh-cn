---
title: 如何：将一个数组赋给另一个数组
ms.date: 07/20/2015
helpviewer_keywords:
- covariance, arrays
- arrays [Visual Basic], assigning
- arrays [Visual Basic], covariance
ms.assetid: 1ae89ea5-f292-4282-bcfc-e9b06b37fbd5
ms.openlocfilehash: c38def1ba9f3720bc760d6f6e4264510c884c930
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84413074"
---
# <a name="how-to-assign-one-array-to-another-array-visual-basic"></a>如何：将一个数组赋给另一个数组 (Visual Basic)

由于数组是对象，因此可以在赋值语句（如其他对象类型）中使用它们。 数组变量包含指向数组元素构成的数据的指针、秩和长度信息，并且赋值仅复制此指针。

### <a name="to-assign-one-array-to-another-array"></a>将一个数组赋给另一个数组

1. 确保两个数组具有相同的秩（维数）和兼容的元素数据类型。

2. 使用标准赋值语句将源数组分配给目标数组。 不要在数组名称后面加上括号。

    ```vb
    Dim formArray() As System.Windows.Forms.Form
    Dim controlArray() As System.Windows.Forms.Control
    controlArray = formArray
    ```

将一个数组分配到另一个数组时，下列规则适用：

- **秩相等。** 目标数组的秩（维数）必须与源数组的秩相同。

  如果两个数组的秩相等，则维度不需要相等。 给定维度中的元素数在赋值期间可能会更改。

- **元素类型。** 这两个数组都必须具有*引用类型*元素，或者两个数组都必须具有*值类型*元素。 有关详细信息，请参阅[值类型和引用类型](../data-types/value-types-and-reference-types.md)。

  - 如果两个数组都具有值类型元素，则元素数据类型必须完全相同。 唯一的例外是，你可以将元素数组分配 `Enum` 给该的基类型的数组 `Enum` 。

  - 如果两个数组都具有引用类型元素，则源元素类型必须派生自目标元素类型。 在这种情况下，两个数组与它们的元素具有相同的继承关系。 这称为*数组协方差*。

如果违反上述规则（例如，如果数据类型不兼容或秩不相等），编译器将报告错误。 您可以在代码中添加错误处理，以确保在尝试赋值之前数组是兼容的。 如果要避免引发异常，还可以使用[TryCast 运算符](../../../language-reference/operators/trycast-operator.md)关键字。

## <a name="see-also"></a>另请参阅

- [数组](index.md)
- [数组疑难解答](troubleshooting-arrays.md)
- [Enum 语句](../../../language-reference/statements/enum-statement.md)
- [数组转换](../data-types/array-conversions.md)
