---
title: 如何在十六进制字符串与数值类型之间转换 - C# 编程指南
description: 了解如何在十六进制字符串与数值类型之间转换。 查看代码示例和其他可用资源。
ms.date: 07/20/2015
helpviewer_keywords:
- hexadecimal strings [C#], converting to numeric type
- conversions [C#], hexidecimal strings
- strings [C#], converting hexadecimal strings
- hexadecimal strings [C#]
ms.topic: how-to
ms.custom: contperf-fy21q2
ms.assetid: 7115c49f-7d1d-40c3-8bd9-aae0cc1d46b6
ms.openlocfilehash: 18021156af879f324993beca04531c8a822725db
ms.sourcegitcommit: d0990c1c1ab2f81908360f47eafa8db9aa165137
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97513232"
---
# <a name="how-to-convert-between-hexadecimal-strings-and-numeric-types-c-programming-guide"></a>如何在十六进制字符串与数值类型之间转换（C# 编程指南）

以下示例演示如何执行下列任务：  
  
- 获取[字符串](../../language-reference/builtin-types/reference-types.md)中每个字符的十六进制值。  
  
- 获取与十六进制字符串中的每个值对应的 [char](../../language-reference/builtin-types/char.md)。  
  
- 将十六进制 `string` 转换为 [int](../../language-reference/builtin-types/integral-numeric-types.md)。  
  
- 将十六进制 `string` 转换为 [float](../../language-reference/builtin-types/floating-point-numeric-types.md)。  
  
- 将[字节](../../language-reference/builtin-types/integral-numeric-types.md)数组转换为十六进制 `string`。  
  
## <a name="example"></a>示例  

 此示例输出 `string` 中每个字符的十六进制值。 首先，将 `string` 分析为字符数组。 然后，对每个字符调用 <xref:System.Convert.ToInt32%28System.Char%29>获取相应的数值。 最后，在 `string` 中将数字的格式设置为十六进制表示形式。  
  
 [!code-csharp[csProgGuideTypes#30](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#30)]  
  
## <a name="example"></a>示例  

 此示例分析十六进制值的 `string` 并输出对应于每个十六进制值的字符。 首先，调用 [Split(Char\[\])](xref:System.String.Split(System.Char[])) 方法以获取每个十六进制值作为数组中的单个 `string`。 然后，调用 <xref:System.Convert.ToInt32%28System.String%2CSystem.Int32%29>将十六进制值转换为表示为 [int](../../language-reference/builtin-types/integral-numeric-types.md) 的十进制值。示例中演示了 2 种不同方法，用于获取对应于该字符代码的字符。 第 1 种方法是使用 <xref:System.Char.ConvertFromUtf32%28System.Int32%29>，它将对应于整型参数的字符作为 `string` 返回。 第 2 种方法是将 `int` 显式转换为 [char](../../language-reference/builtin-types/char.md)。  
  
 [!code-csharp[csProgGuideTypes#31](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#31)]  
  
## <a name="example"></a>示例  

 此示例演示了将十六进制 `string` 转换为整数的另一种方法，即调用 <xref:System.Int32.Parse%28System.String%2CSystem.Globalization.NumberStyles%29> 方法。  
  
 [!code-csharp[csProgGuideTypes#32](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#32)]  
  
## <a name="example"></a>示例  

 下面的示例演示了如何使用 <xref:System.BitConverter?displayProperty=nameWithType> 类和 <xref:System.UInt32.Parse%2A?displayProperty=nameWithType> 方法将十六进制 `string` 转换为 [float](../../language-reference/builtin-types/floating-point-numeric-types.md)。  
  
 [!code-csharp[csProgGuideTypes#39](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#39)]  
  
## <a name="example"></a>示例  

 下面的示例演示了如何使用 <xref:System.BitConverter?displayProperty=nameWithType> 类将[字节](../../language-reference/builtin-types/integral-numeric-types.md)数组转换为十六进制字符串。  
  
 [!code-csharp[csProgGuideTypes#38](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#38)]  
  
## <a name="see-also"></a>请参阅

- [标准数字格式字符串](../../../standard/base-types/standard-numeric-format-strings.md)
- [类型](./index.md)
- [如何确定字符串是否表示数值](../strings/how-to-determine-whether-a-string-represents-a-numeric-value.md)
