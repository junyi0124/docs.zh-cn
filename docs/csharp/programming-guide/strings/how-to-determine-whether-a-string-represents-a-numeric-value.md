---
title: 如何确定字符串是否表示数值 - C# 编程指南
description: 了解如何确定字符串是否为指定数值类型的有效表示形式。 查看代码示例和其他资源。
ms.date: 07/20/2015
helpviewer_keywords:
- numeric strings [C#]
- validating numeric input [C#]
- strings [C#], numeric
ms.assetid: a4e84e10-ea0a-489f-a868-503dded9d85f
ms.openlocfilehash: cbe0703ca39422ac0a9e7a93bf2cfc4c3f8528f8
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91151423"
---
# <a name="how-to-determine-whether-a-string-represents-a-numeric-value-c-programming-guide"></a>如何确定字符串是否表示数值（C# 编程指南）

若要确定字符串是否是指定数值类型的有效表示形式，请使用由所有基元数值类型以及如 <xref:System.DateTime> 和 <xref:System.Net.IPAddress> 等类型实现的静态 `TryParse` 方法。 以下示例演示如何确定“108”是否为有效的 [int](../../language-reference/builtin-types/integral-numeric-types.md)。  
  
```csharp  
int i = 0;
string s = "108";  
bool result = int.TryParse(s, out i); //i now = 108  
```  
  
 如果该字符串包含非数字字符，或者数值对于指定的特定类型而言太大或太小，则 `TryParse` 将返回 false 并将 out 参数设置为零。 否则，它将返回 true 并将 out 参数设置为字符串的数值。  
  
> [!NOTE]
> 字符串可能仅包含数字字符，但对于你使用的 `TryParse` 方法的类型仍然无效。 例如，“256”不是 `byte` 的有效值，但对 `int` 有效。 “98.6”不是 `int` 的有效值，但它是有效的 `decimal`。  
  
## <a name="example"></a>示例  

 以下示例演示如何对 `long`、`byte` 和 `decimal` 值的字符串表示形式使用 `TryParse`。  
  
 [!code-csharp[csProgGuideStrings#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStrings/CS/Strings.cs#14)]  
  
## <a name="robust-programming"></a>可靠编程  

 基元数值类型还实现 `Parse` 静态方法，如果字符串不是有效数字，该方法将引发异常。 `TryParse` 通常更高效，因为如果数值无效，它仅返回 false。  
  
## <a name="net-security"></a>.NET 安全性  

 请务必使用 `TryParse` 或 `Parse` 方法验证控件（如文本框和组合框）中的用户输入。  
  
## <a name="see-also"></a>请参阅

- [如何将字节数组转换为 int](../types/how-to-convert-a-byte-array-to-an-int.md)
- [如何将字符串转换为数字](../types/how-to-convert-a-string-to-a-number.md)
- [如何在十六进制字符串与数值类型之间转换](../types/how-to-convert-between-hexadecimal-strings-and-numeric-types.md)
- [分析数值字符串](../../../standard/base-types/parsing-numeric.md)
- [格式设置类型](../../../standard/base-types/formatting-types.md)
