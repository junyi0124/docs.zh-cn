---
title: 布尔数据类型
ms.date: 07/20/2015
f1_keywords:
- vb.FALSE
- vb.TRUE
- vb.Boolean
helpviewer_keywords:
- Boolean data type
- Boolean values [Visual Basic], False keyword
- False keyword [Visual Basic]
- True keyword [Visual Basic]
- Boolean values [Visual Basic], True keyword
ms.assetid: 4858e630-4813-4216-a55e-f4d0feb884e4
ms.openlocfilehash: 851c524a83c5f24b77a151634a96798249c5905e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84374416"
---
# <a name="boolean-data-type-visual-basic"></a>Boolean 数据类型 (Visual Basic)

保存的值只能是 `True` 或 `False` 。 关键字 `True` 和 `False` 对应于变量的两个状态 `Boolean` 。  
  
## <a name="remarks"></a>备注  

 使用[布尔数据类型（Visual Basic）](boolean-data-type.md)来包含双状态值，如 true/false、yes/no 或 on/off。  
  
 `Boolean` 的默认值为 `False`。  
  
 `Boolean`值不会存储为数字，并且存储的值不应与数字等效。 永远不应编写依赖于和的等效数值的代码 `True` `False` 。 应尽可能将变量的使用限制 `Boolean` 为它们的设计逻辑值。  
  
## <a name="type-conversions"></a>类型转换  

 当 Visual Basic 将数值数据类型值转换为时 `Boolean` ，0变为， `False` 所有其他值将变为 `True` 。 当 Visual Basic 将 `Boolean` 值转换为数值类型时，将 `False` 变为0并 `True` 变为-1。  
  
 当在 `Boolean` 值和数值数据类型之间进行转换时，请记住，.NET Framework 转换方法并不总是产生与 Visual Basic 转换关键字相同的结果。 这是因为 Visual Basic 转换会保持与以前版本兼容的行为。 有关详细信息，请参阅[故障排除数据类型](../../programming-guide/language-features/data-types/troubleshooting-data-types.md)中的 "布尔类型不会精确转换为数值类型"。  
  
## <a name="programming-tips"></a>编程提示  
  
- **负数。** `Boolean`不是数值类型，不能表示负值。 在任何情况下，不应使用 `Boolean` 来保存数值。  
  
- **键入字符。** `Boolean`没有文本类型字符或标识符类型字符。  
  
- **Framework 类型。** .NET Framework 中的对应类型是 <xref:System.Boolean?displayProperty=nameWithType> 结构。  
  
## <a name="example"></a>示例  

 在下面的示例中， `runningVB` 是一个 `Boolean` 变量，用于存储简单的 yes/no 设置。  
  
```vb  
Dim runningVB As Boolean  
' Check to see if program is running on Visual Basic engine.  
If scriptEngine = "VB" Then  
    runningVB = True  
End If  
```  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Boolean?displayProperty=nameWithType>
- [数据类型](index.md)
- [Type Conversion Functions](../functions/type-conversion-functions.md)
- [转换摘要](../keywords/conversion-summary.md)
- [有效使用数据类型](../../programming-guide/language-features/data-types/efficient-use-of-data-types.md)
- [数据类型疑难解答](../../programming-guide/language-features/data-types/troubleshooting-data-types.md)
- [CType Function](../functions/ctype-function.md)
