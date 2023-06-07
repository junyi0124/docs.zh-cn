---
title: Byte 数据类型
ms.date: 01/31/2018
f1_keywords:
- vb.Byte
helpviewer_keywords:
- Byte data type
- data types [Visual Basic], assigning
ms.assetid: eed44dff-eaee-4937-a89f-444e418e74f6
ms.openlocfilehash: 97acd1bc2ff29bac6588216b9ee4a4f187078815
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84374312"
---
# <a name="byte-data-type-visual-basic"></a>Byte 数据类型（Visual Basic）

保留值介于0到255之间的无符号8位（1字节）整数。

## <a name="remarks"></a>备注

使用 `Byte` 数据类型来包含二进制数据。  
  
`Byte` 的默认值为 0。

## <a name="literal-assignments"></a>文本赋值

您可以 `Byte` 通过将变量指定为十进制文本、十六进制文本、八进制文本或（从 Visual Basic 2017）作为二进制文本来声明和初始化变量。 如果整数文本超出的范围 `Byte` （即，如果小于 <xref:System.Byte.MinValue?displayProperty=nameWithType> 或大于 <xref:System.Byte.MaxValue?displayProperty=nameWithType> ），则会发生编译错误。

在下面的示例中，表示为十进制、十六进制和二进制文本的整数等于201将从[整数](integer-data-type.md)隐式转换为 `byte` 值。

[!code-vb[Byte](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#Byte)]

> [!NOTE]
> 使用前缀 `&h` 或 `&H` 表示十六进制文本，使用前缀 `&b` 或表示 `&B` 二进制文本，使用前缀 `&o` 或 `&O` 表示八进制文本。 十进制文本没有前缀。

从 Visual Basic 2017 开始，还可以使用下划线字符 `_` 作为数字分隔符来增强可读性，如下面的示例所示。

[!code-vb[Byte](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#ByteS)]  

从 Visual Basic 15.5 开始，还可以使用下划线字符（ `_` ）作为前缀和十六进制、二进制或八进制数字之间的前导分隔符。 例如：

```vb
Dim number As Byte = &H_6A
```

[!INCLUDE [supporting-underscores](../../../../includes/vb-separator-langversion.md)]

## <a name="programming-tips"></a>编程提示

- **负数。** 由于 `Byte` 是一个无符号类型，因此它不能表示负数。 如果 `-` 对计算结果为类型的表达式使用一元减号（）运算符 `Byte` ，Visual Basic 会将表达式转换为 `Short` 第一个表达式。
  
- **格式转换。** 当 Visual Basic 读取或写入文件，或者在调用 Dll、方法和属性时，它可以在数据格式之间自动转换。 在 `Byte` 此类格式转换过程中将保留存储在变量和数组中的二进制数据。 不应将 `String` 变量用于二进制数据，因为在 ANSI 和 Unicode 格式之间进行转换时，可能会损坏其内容。

- **扩大.** `Byte`数据类型扩大到、、、 `Short` `UShort` 、、、、 `Integer` `UInteger` `Long` `ULong` `Decimal` `Single` 或 `Double` 。 这意味着你可以转换 `Byte` 为这些类型中的任何一种，而不会遇到 <xref:System.OverflowException?displayProperty=nameWithType> 错误。
  
- **键入字符。** `Byte`没有文本类型字符或标识符类型字符。

- **Framework 类型。** .NET Framework 中的对应类型是 <xref:System.Byte?displayProperty=nameWithType> 结构。

## <a name="example"></a>示例

 在下面的示例中， `b` 是一个 `Byte` 变量。 语句说明变量的范围以及对其应用移位运算符的应用程序。

 [!code-vb[VbVbalrDataTypes#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDataTypes/VB/Class1.vb#16)]  

## <a name="see-also"></a>另请参阅

- <xref:System.Byte?displayProperty=nameWithType>
- [数据类型](index.md)
- [Type Conversion Functions](../functions/type-conversion-functions.md)
- [转换摘要](../keywords/conversion-summary.md)
- [有效使用数据类型](../../programming-guide/language-features/data-types/efficient-use-of-data-types.md)
