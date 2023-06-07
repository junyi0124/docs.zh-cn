---
title: UInteger 数据类型
ms.date: 01/31/2018
f1_keywords:
- vb.uinteger
helpviewer_keywords:
- numbers [Visual Basic], whole
- UInteger data type
- literal type characters [Visual Basic], UI
- whole numbers
- integral data types [Visual Basic]
- integer numbers
- numbers [Visual Basic], integer
- integers [Visual Basic], data types
- integers [Visual Basic], types
- UI literal type characters [Visual Basic]
- data types [Visual Basic], integral
ms.assetid: db7ddd34-4f23-46f5-84dd-8b0f83bb8729
ms.openlocfilehash: 11070f6c7f3259b8c34528eb54d99b031b68f9f9
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84415539"
---
# <a name="uinteger-data-type"></a>UInteger 数据类型

保存32位（4字节）无符号整数，取值范围为0到4294967295。

## <a name="remarks"></a>备注

`UInteger`数据类型以最有效的数据宽度提供最大的无符号值。

`UInteger` 的默认值为 0。

## <a name="literal-assignments"></a>文本赋值

您可以 `UInteger` 通过将变量指定为十进制文本、十六进制文本、八进制文本或（从 Visual Basic 2017）作为二进制文本来声明和初始化变量。 如果整数文本在 `UInteger` 范围之外（即，如果它小于 <xref:System.UInt32.MinValue?displayProperty=nameWithType> 或大于 <xref:System.UInt32.MaxValue?displayProperty=nameWithType>），会发生编译错误。

在以下示例中，表示为十进制、十六进制和二进制文本且等于 3,000,000,000 的整数被分配给 `UInteger` 值。

[!code-vb[UInteger](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#UInt)]

> [!NOTE]
> 使用前缀 `&h` 或 `&H` 表示十六进制文本，使用前缀 `&b` 或表示 `&B` 二进制文本，使用前缀 `&o` 或 `&O` 表示八进制文本。 十进制文本没有前缀。

从 Visual Basic 2017 开始，还可以使用下划线字符 `_` 作为数字分隔符来增强可读性，如下面的示例所示。

[!code-vb[UInteger](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#UIntS)]

从 Visual Basic 15.5 开始，还可以使用下划线字符（ `_` ）作为前缀和十六进制、二进制或八进制数字之间的前导分隔符。 例如：

```vb
Dim number As UInteger = &H_0F8C_0326
```

[!INCLUDE [supporting-underscores](../../../../includes/vb-separator-langversion.md)]

数字文本还可以包含 `UI` 或 `ui` [类型字符](../../programming-guide/language-features/data-types/type-characters.md)来表示 `UInteger` 数据类型，如下面的示例所示。

```vb
Dim number = &H_0FAC_14D7ui
```

## <a name="programming-tips"></a>编程提示

`UInteger`和 `Integer` 数据类型为32位处理器提供最佳性能，因为较小的整数类型（ `UShort` 、 `Short` 、 `Byte` 和 `SByte` ），即使它们使用较少的位，也需要更多的时间来加载、存储和提取。

- **负数。** 由于 `UInteger` 是一个无符号类型，因此它不能表示负数。 如果 `-` 对计算结果为类型的表达式使用一元减号（）运算符 `UInteger` ，Visual Basic 会将表达式转换为 `Long` 第一个表达式。

- **CLS 遵从性。** `UInteger`数据类型不是[公共语言规范](https://www.ecma-international.org/publications/standards/Ecma-335.htm)（cls）的一部分，因此符合 CLS 的代码无法使用使用它的组件。

- **互操作注意事项。** 如果你与不是为 .NET Framework 编写的组件（如自动化或 COM 对象）进行交互，请记住，如之类的类型 `uint` 可以在其他环境中具有不同的数据宽度（16位）。 如果要将16位参数传递给此类组件，请 `UShort` `UInteger` 在托管的 Visual Basic 代码中将其声明为而不是。

- **扩大.** `UInteger`数据类型扩大到、、、 `Long` `ULong` `Decimal` `Single` 和 `Double` 。 这意味着你可以转换 `UInteger` 为这些类型中的任何一种，而不会遇到 <xref:System.OverflowException?displayProperty=nameWithType> 错误。

- **键入字符。** 将文本类型字符追加 `UI` 到文本会将其强制转换为 `UInteger` 数据类型。 `UInteger`没有标识符类型字符。

- **Framework 类型。** .NET Framework 中的对应类型是 <xref:System.UInt32?displayProperty=nameWithType> 结构。

## <a name="see-also"></a>另请参阅

- <xref:System.UInt32>
- [数据类型](index.md)
- [Type Conversion Functions](../functions/type-conversion-functions.md)
- [转换摘要](../keywords/conversion-summary.md)
- [如何：调用需要使用无符号类型的 Windows 函数](../../programming-guide/com-interop/how-to-call-a-windows-function-that-takes-unsigned-types.md)
- [有效使用数据类型](../../programming-guide/language-features/data-types/efficient-use-of-data-types.md)
