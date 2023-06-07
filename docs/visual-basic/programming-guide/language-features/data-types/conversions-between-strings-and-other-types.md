---
title: 字符串和其他类型之间的转换
ms.date: 07/20/2015
helpviewer_keywords:
- data type conversion [Visual Basic], string
- conversions [Visual Basic], type
- string conversion [Visual Basic]
- conversions [Visual Basic], data type
- type conversion [Visual Basic], string
- regional options
ms.assetid: c3a99596-f09a-44a5-81dd-1b89a094f1df
ms.openlocfilehash: 823931f7d6beb8218e8b99d4a8d45716b7214304
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91077145"
---
# <a name="conversions-between-strings-and-other-types-visual-basic"></a>字符串和其他类型之间的转换 (Visual Basic)

您可以将数字、 `Boolean` 或日期/时间值转换为 `String` 。 您还可以反向转换（从字符串值转换为数字， `Boolean` 或），前提是 `Date` 字符串的内容可以解释为目标数据类型的有效值。 如果无法运行，则会发生运行时错误。  
  
 所有这些分配的转换均为任意方向，均为收缩转换。 应使用类型转换关键字 (、、、、、、、、、、、、、 `CBool` `CByte` `CDate` `CDbl` `CDec` `CInt` `CLng` `CSByte` `CShort` `CSng` `CStr` `CUInt` `CULng` `CUShort` 和 `CType`) 。 <xref:Microsoft.VisualBasic.Strings.Format%2A>和 <xref:Microsoft.VisualBasic.Conversion.Val%2A> 函数使你可以更进一步控制字符串和数字之间的转换。  
  
 如果已定义类或结构，则可以定义 `String` 与类或结构的类型之间的类型转换运算符。 有关更多信息，请参见 [How to: Define a Conversion Operator](../procedures/how-to-define-a-conversion-operator.md)。  
  
## <a name="conversion-of-numbers-to-strings"></a>将数字转换为字符串  

 您可以使用 `Format` 函数将数字转换为带格式的字符串，该字符串不仅可以包含适当的数字，还可以包含格式符号 `$` ，如) 、千位分隔符或 *数字分组 (符号* （例如 `,`) ）和小数点分隔符 (（如) ） (`.` 。 `Format`根据 Windows**控制面板**中指定的**区域选项**设置自动使用适当的符号。  
  
 请注意，串联 (`&`) 运算符可以隐式将数字转换为字符串，如下面的示例所示。  
  
```vb  
' The following statement converts count to a String value.  
Str = "The total count is " & count  
```  
  
## <a name="conversion-of-strings-to-numbers"></a>将字符串转换为数字  

 您可以使用 `Val` 函数将字符串中的数字显式转换为数字。 `Val` 读取字符串，直到遇到数字、空格、制表符、换行符或句点以外的字符。 序列 "&O" 和 "&H" 改变数值系统的基数，并终止扫描。 在停止读取之前，会 `Val` 将所有合适的字符转换为数值。 例如，下面的语句返回值 `141.825` 。  
  
 `Val("   14   1.825 miles")`  
  
 当 Visual Basic 将字符串转换为数值时，它将使用 Windows**控制面板**中指定的 "**区域选项**" 设置来解释千位分隔符、小数点分隔符和货币符号。 这意味着，转换可能会在一个设置中失败，而不是另一个设置。 例如， `"$14.20"` 在英语 (美国) 区域设置，而不是任何法语区域设置。  
  
## <a name="see-also"></a>请参阅

- [Visual Basic 中的类型转换](type-conversions.md)
- [Widening and Narrowing Conversions](widening-and-narrowing-conversions.md)
- [隐式转换和显式转换](implicit-and-explicit-conversions.md)
- [如何：在 Visual Basic 中将一个对象转换为其他类型](how-to-convert-an-object-to-another-type.md)
- [数组转换](array-conversions.md)
- [数据类型](../../../language-reference/data-types/index.md)
- [Type Conversion Functions](../../../language-reference/functions/type-conversion-functions.md)
- [开发全球化和本地化应用](/visualstudio/ide/globalizing-and-localizing-applications)
