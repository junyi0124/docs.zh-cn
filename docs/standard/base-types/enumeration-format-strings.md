---
title: 枚举格式字符串
description: 使用 .NET 中的 Enum.ToString 方法创建枚举格式字符串。 格式化枚举成员的数字、十六进制或字符串值。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- format specifiers, enumeration format strings
- enumeration format strings
- formatting [.NET], enumeration
ms.assetid: dd1ff672-1052-42cf-8666-4924fb6cd1a1
ms.openlocfilehash: 02a12c36e47a82c15c01e578333e1c4465bab142
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94829617"
---
# <a name="enumeration-format-strings"></a>枚举格式字符串

可以使用 <xref:System.Enum.ToString%2A?displayProperty=nameWithType> 方法，新建表示枚举成员的数字值、十六进制值或字符串值的字符串对象。 此方法采用枚举格式设置字符串之一来指定要返回的值。

以下各部分列出了枚举格式设置字符串和它们返回的值。 这些格式说明符不区分大小写。

## <a name="g-or-g"></a>G 或 g

如有可能，将枚举项显示为字符串值，否则显示当前实例的整数值。 如果枚举使用 **Flags** 属性集进行定义，则每个有效项的字符串值会连接在一起（以逗号分隔）。 如果未设置 **Flags** 属性，则将无效值显示为数字项。 下面的示例演示 G 格式说明符。

[!code-csharp[Formatting.Enum#1](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.Enum/cs/enum1.cs#1)]
[!code-vb[Formatting.Enum#1](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Enum/vb/enum1.vb#1)]

## <a name="f-or-f"></a>F 或 f

如有可能，将枚举项显示为字符串值。 如果值可以完全显示为枚举中项的总和（即使未提供 **Flags** 属性），则每个有效项的字符串值会连接在一起（以逗号分隔）。 如果值不能由枚举项完全确定，则值会格式化为整数值。 下面的示例演示 F 格式说明符。

[!code-csharp[Formatting.Enum#2](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.Enum/cs/enum1.cs#2)]
[!code-vb[Formatting.Enum#2](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Enum/vb/enum1.vb#2)]

## <a name="d-or-d"></a>D 或 d

以尽可能短的表示形式将枚举项显示为整数值。 下面的示例演示 D 格式说明符。

[!code-csharp[Formatting.Enum#3](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.Enum/cs/enum1.cs#3)]
[!code-vb[Formatting.Enum#3](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Enum/vb/enum1.vb#3)]

## <a name="x-or-x"></a>X 或 x

将枚举项显示为十六进制值。 根据需要以前导零表示此值，以确保在枚举类型的[基础数值类型](xref:System.Enum.GetUnderlyingType%2A)中，结果字符串的每个字节都有两个字符。 下面的示例演示 X 格式说明符。 在示例中，这两者的基础类型 <xref:System.ConsoleColor> 和 <xref:System.IO.FileAttributes> 为 <xref:System.Int32>，或 32 位（或 4 字节）整数，它将生成 8 个字符的结果字符串。

[!code-csharp[Formatting.Enum#4](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.Enum/cs/enum1.cs#4)]
[!code-vb[Formatting.Enum#4](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Enum/vb/enum1.vb#4)]

## <a name="example"></a>示例

下面的示例定义一个名为 `Colors` 的枚举，它由三个项组成：`Red`、`Blue` 和 `Green`。

[!code-csharp[Formatting.Enum#5](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.Enum/cs/enum1.cs#5)]
[!code-vb[Formatting.Enum#5](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Enum/vb/enum1.vb#5)]

定义该枚举之后，可以按以下方式声明实例。

[!code-csharp[Formatting.Enum#6](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.Enum/cs/enum1.cs#6)]
[!code-vb[Formatting.Enum#6](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Enum/vb/enum1.vb#6)]

`Color.ToString(System.String)` 方法随后可以用于以不同方式显示枚举值（具体取决于传递给它的格式说明符）。

[!code-csharp[Formatting.Enum#7](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.Enum/cs/enum1.cs#7)]
[!code-vb[Formatting.Enum#7](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Enum/vb/enum1.vb#7)]

## <a name="see-also"></a>请参阅

- [格式设置类型](formatting-types.md)
