---
title: UShort 数据类型
ms.date: 01/31/2018
f1_keywords:
- vb.ushort
helpviewer_keywords:
- numbers [Visual Basic], whole
- literal type characters [Visual Basic], US
- whole numbers
- integral data types [Visual Basic]
- integer numbers
- numbers [Visual Basic], integer
- integers [Visual Basic], data types
- integers [Visual Basic], types
- data types [Visual Basic], integral
- UShort data type
- US literal type characters [Visual Basic]
ms.assetid: 138db892-665d-4ba8-9cae-d8d91c4a8f39
ms.openlocfilehash: ee31156e00059699125fd72a7f091afbb21beab0
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84415474"
---
# <a name="ushort-data-type-visual-basic"></a>UShort 数据类型（Visual Basic）

保存16位（2字节）无符号整数，范围介于0到65535之间。  
  
## <a name="remarks"></a>备注

 使用 `UShort` 数据类型来包含对而言太大的二进制数据 `Byte` 。  
  
 `UShort` 的默认值为 0。  

## <a name="literal-assignments"></a>文本赋值

您可以 `UShort` 通过将变量指定为十进制文本、十六进制文本、八进制文本或（从 Visual Basic 2017）作为二进制文本来声明和初始化变量。 如果整数文本在 `UShort` 范围之外（即，如果它小于 <xref:System.UInt16.MinValue?displayProperty=nameWithType> 或大于 <xref:System.UInt16.MaxValue?displayProperty=nameWithType>），会发生编译错误。

在下面的示例中，表示为十进制、十六进制和二进制文本的整数等于65034，分配给 `UShort` 值。
  
[!code-vb[UShort](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#UShort)]

> [!NOTE]
> 使用前缀 `&h` 或 `&H` 表示十六进制文本，使用前缀 `&b` 或表示 `&B` 二进制文本，使用前缀 `&o` 或 `&O` 表示八进制文本。 十进制文本没有前缀。

从 Visual Basic 2017 开始，还可以使用下划线字符 `_` 作为数字分隔符来增强可读性，如下面的示例所示。

[!code-vb[UShort](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#UShortS)]

从 Visual Basic 15.5 开始，还可以使用下划线字符（ `_` ）作为前缀和十六进制、二进制或八进制数字之间的前导分隔符。 例如：

```vb
Dim number As UShort = &H_FF8C
```

[!INCLUDE [supporting-underscores](../../../../includes/vb-separator-langversion.md)]

数字文本还可以包含 `US` 或 `us` [类型字符](../../programming-guide/language-features/data-types/type-characters.md)来表示 `UShort` 数据类型，如下面的示例所示。

```vb
Dim number = &H_5826us
```

## <a name="programming-tips"></a>编程提示
  
- **负数。** 由于 `UShort` 是一个无符号类型，因此它不能表示负数。 如果 `-` 对计算结果为类型的表达式使用一元减号（）运算符 `UShort` ，Visual Basic 会将表达式转换为 `Integer` 第一个表达式。  
  
- **CLS 遵从性。** `UShort`数据类型不是[公共语言规范](https://www.ecma-international.org/publications/standards/Ecma-335.htm)（cls）的一部分，因此符合 CLS 的代码无法使用使用它的组件。
  
- **扩大.** `UShort`数据类型扩大到、、、 `Integer` `UInteger` 、、 `Long` `ULong` `Decimal` `Single` 和 `Double` 。 这意味着你可以转换 `UShort` 为这些类型中的任何一种，而不会遇到 <xref:System.OverflowException?displayProperty=nameWithType> 错误。  
  
- **键入字符。** 将文本类型字符追加 `US` 到文本会将其强制转换为 `UShort` 数据类型。 `UShort`没有标识符类型字符。  
  
- **Framework 类型。** .NET Framework 中的对应类型是 <xref:System.UInt16?displayProperty=nameWithType> 结构。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.UInt16>
- [数据类型](index.md)
- [Type Conversion Functions](../functions/type-conversion-functions.md)
- [转换摘要](../keywords/conversion-summary.md)
- [如何：调用需要使用无符号类型的 Windows 函数](../../programming-guide/com-interop/how-to-call-a-windows-function-that-takes-unsigned-types.md)
- [有效使用数据类型](../../programming-guide/language-features/data-types/efficient-use-of-data-types.md)
