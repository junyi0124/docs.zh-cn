---
title: ULong 数据类型
ms.date: 01/31/2018
f1_keywords:
- vb.ulong
helpviewer_keywords:
- numbers [Visual Basic], whole
- whole numbers
- integral data types [Visual Basic]
- integer numbers
- numbers [Visual Basic], integer
- integers [Visual Basic], data types
- integers [Visual Basic], types
- data types [Visual Basic], integral
- literal type characters [Visual Basic], UL
- ULong data type
- UL literal type characters [Visual Basic]
ms.assetid: 017e0702-774e-44ae-bedc-786b424ca84e
ms.openlocfilehash: ee9297ae917345d44d8e630bd09beea2245b56da
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84415513"
---
# <a name="ulong-data-type-visual-basic"></a>ULong 数据类型（Visual Basic）

保留未签名的64位（8字节）整数，范围为0到18446744073709551615（超过1.84 倍 10 ^ 19）。

## <a name="remarks"></a>备注

使用 `ULong` 数据类型可包含太大的二进制数据 `UInteger` 或可能的最大无符号整数值。

`ULong` 的默认值为 0。

## <a name="literal-assignments"></a>文本赋值

您可以 `ULong` 通过将变量指定为十进制文本、十六进制文本、八进制文本或（从 Visual Basic 2017）作为二进制文本来声明和初始化变量。 如果整数文本在 `ULong` 范围之外（即，如果它小于 <xref:System.UInt64.MinValue?displayProperty=nameWithType> 或大于 <xref:System.UInt64.MaxValue?displayProperty=nameWithType>），会发生编译错误。

在以下示例中，表示为十进制、十六进制和二进制文本且等于 7,934,076,125 的整数被分配给 `ULong` 值。

[!code-vb[ULong](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#ULong)]

> [!NOTE]
> 使用前缀 `&h` 或 `&H` 表示十六进制文本，使用前缀 `&b` 或表示 `&B` 二进制文本，使用前缀 `&o` 或 `&O` 表示八进制文本。 十进制文本没有前缀。

从 Visual Basic 2017 开始，还可以使用下划线字符 `_` 作为数字分隔符来增强可读性，如下面的示例所示。

[!code-vb[ULong](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#LongS)]

从 Visual Basic 15.5 开始，还可以使用下划线字符（ `_` ）作为前缀和十六进制、二进制或八进制数字之间的前导分隔符。 例如：

```vb
Dim number As ULong = &H_F9AC_0326_1489_D68C
```

[!INCLUDE [supporting-underscores](../../../../includes/vb-separator-langversion.md)]

数字文本还可以包含 `UL` 或 `ul` [类型字符](../../programming-guide/language-features/data-types/type-characters.md)来表示 `ULong` 数据类型，如下面的示例所示。

```vb
Dim number = &H_00_00_0A_96_2F_AC_14_D7ul
```

## <a name="programming-tips"></a>编程提示

- **负数。** 由于 `ULong` 是一个无符号类型，因此它不能表示负数。 如果 `-` 对计算结果为类型的表达式使用一元减号（）运算符 `ULong` ，Visual Basic 会将表达式转换为 `Decimal` 第一个表达式。

- **CLS 遵从性。** `ULong`数据类型不是[公共语言规范](https://www.ecma-international.org/publications/standards/Ecma-335.htm)（cls）的一部分，因此符合 CLS 的代码无法使用使用它的组件。

- **互操作注意事项。** 如果你与不是为 .NET Framework 编写的组件（如自动化或 COM 对象）进行交互，请记住，如之类的类型 `ulong` 可以在其他环境中具有不同的数据宽度（32位）。 如果要将32位参数传递给此类组件，请 `UInteger` `ULong` 在托管的 Visual Basic 代码中将其声明为而不是。

  此外，在 Windows 95、Windows 98、Windows ME 或 Windows 2000 上，自动化不支持64位整数。 不能将 Visual Basic `ULong` 参数传递到这些平台上的自动化组件。

- **扩大.** `ULong`数据类型扩大到 `Decimal` 、 `Single` 和 `Double` 。 这意味着你可以转换 `ULong` 为这些类型中的任何一种，而不会遇到 <xref:System.OverflowException?displayProperty=nameWithType> 错误。

- **键入字符。** 将文本类型字符追加 `UL` 到文本会将其强制转换为 `ULong` 数据类型。 `ULong`没有标识符类型字符。

- **Framework 类型。** .NET Framework 中的对应类型是 <xref:System.UInt64?displayProperty=nameWithType> 结构。

## <a name="see-also"></a>另请参阅

- <xref:System.UInt64>
- [数据类型](index.md)
- [Type Conversion Functions](../functions/type-conversion-functions.md)
- [转换摘要](../keywords/conversion-summary.md)
- [如何：调用需要使用无符号类型的 Windows 函数](../../programming-guide/com-interop/how-to-call-a-windows-function-that-takes-unsigned-types.md)
- [有效使用数据类型](../../programming-guide/language-features/data-types/efficient-use-of-data-types.md)
