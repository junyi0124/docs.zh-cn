---
title: Long 数据类型
ms.date: 01/31/2018
f1_keywords:
- vb.Long
helpviewer_keywords:
- identifier type characters [Visual Basic], &
- numbers [Visual Basic], whole
- whole numbers
- integral data types [Visual Basic]
- '& identifier type character'
- integer numbers
- literal type characters [Visual Basic], L
- numbers [Visual Basic], integer
- integers [Visual Basic], data types
- L literal type character [Visual Basic]
- integers [Visual Basic], types
- Long keyword [Visual Basic]
- data types [Visual Basic], integral
- data types [Visual Basic], assigning
- Long data type
ms.assetid: b4770c34-1804-4f8c-b512-c10b0893e516
ms.openlocfilehash: 7c076cd2198c85560f7c63c69e051697966c9524
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84415591"
---
# <a name="long-data-type-visual-basic"></a>Long 数据类型（Visual Basic）

保留已签名的64位（8字节）整数，取值范围为-9223372036854775808 到9223372036854775807（9.2. E + 18）。

## <a name="remarks"></a>备注

使用 `Long` 数据类型可包含太大而无法容纳在数据类型中的整数 `Integer` 。

`Long` 的默认值为 0。

## <a name="literal-assignments"></a>文本赋值

您可以 `Long` 通过将变量指定为十进制文本、十六进制文本、八进制文本或（从 Visual Basic 2017）作为二进制文本来声明和初始化变量。 如果整数文本在 `Long` 范围之外（即，如果它小于 <xref:System.Int64.MinValue?displayProperty=nameWithType> 或大于 <xref:System.Int64.MaxValue?displayProperty=nameWithType>），会发生编译错误。

在以下示例中，表示为十进制、十六进制和二进制文本的等于 4,294,967,296 的整数被分配给 `Long` 值。

[!code-vb[long](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#Long)]

> [!NOTE]
> 使用前缀 `&h` 或 `&H` 表示十六进制文本，使用前缀 `&b` 或表示 `&B` 二进制文本，使用前缀 `&o` 或 `&O` 表示八进制文本。 十进制文本没有前缀。

从 Visual Basic 2017 开始，还可以使用下划线字符 `_` 作为数字分隔符来增强可读性，如下面的示例所示。

[!code-vb[long](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#LongS)]

从 Visual Basic 15.5 开始，还可以使用下划线字符（ `_` ）作为前缀和十六进制、二进制或八进制数字之间的前导分隔符。 例如：

```vb
Dim number As Long = &H_0FAC_0326_1489_D68C
```

[!INCLUDE [supporting-underscores](../../../../includes/vb-separator-langversion.md)]

数字文本还可包含 `L` 用于表示数据类型的[类型字符](../../programming-guide/language-features/data-types/type-characters.md) `Long` ，如下面的示例所示。

```vb
Dim number = &H_0FAC_0326_1489_D68CL
```

## <a name="programming-tips"></a>编程提示

- **互操作注意事项。** 如果与不是为 .NET Framework 编写的组件（如自动化或 COM 对象）交互，请记住， `Long` 在其他环境中具有不同的数据宽度（32位）。 如果要将32位参数传递给此类组件，请 `Integer` `Long` 在新的 Visual Basic 代码中将其声明为而不是。

- **扩大.** `Long`数据类型扩大到 `Decimal` 、 `Single` 或 `Double` 。 这意味着，你可以将 `Long` 转换为这些类型中的任意类型，而不会遇到 <xref:System.OverflowException?displayProperty=nameWithType> 错误。

- **键入字符。** 将文本类型字符 `L` 追加到文本会将其强制转换为 `Long` 数据类型。 将标识符类型字符 `&` 追加到任何标识符会将其强制转换为 `Long`。

- **Framework 类型。** .NET Framework 中的对应类型是 <xref:System.Int64?displayProperty=nameWithType> 结构。

## <a name="see-also"></a>另请参阅

- <xref:System.Int64>
- [数据类型](index.md)
- [Integer 数据类型](integer-data-type.md)
- [Short 数据类型](short-data-type.md)
- [Type Conversion Functions](../functions/type-conversion-functions.md)
- [转换摘要](../keywords/conversion-summary.md)
- [有效使用数据类型](../../programming-guide/language-features/data-types/efficient-use-of-data-types.md)
