---
title: 数组转换
ms.date: 07/20/2015
helpviewer_keywords:
- arrays [Visual Basic], converting type
- type conversion [Visual Basic], arrays
- conversions [Visual Basic], type
- arrays [Visual Basic], data types
- conversions [Visual Basic], data type
- object arrays [Visual Basic], converting type
- data type conversion [Visual Basic], array conversions
- conversions [Visual Basic], array types
- object arrays
ms.assetid: fceff7d2-a1b7-44c7-b9aa-8bd831d8a444
ms.openlocfilehash: 375c75c954f3be535272d674d9b786cad46b1a01
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91077184"
---
# <a name="array-conversions-visual-basic"></a>数组转换 (Visual Basic)

如果满足以下条件，则可以将数组类型转换为不同的数组类型：  
  
- **秩相等。** 两个数组的秩必须相同，也就是说，它们必须具有相同的维数。 但是，各个维度的长度不必相同。  
  
- **元素数据类型。** 这两个数组的元素的数据类型必须是引用类型。 `Integer` `Long` `Object` 由于至少涉及一种值类型，因此不能将数组转换为数组，甚至不能转换为数组。 有关详细信息，请参阅 [值类型和引用类型](value-types-and-reference-types.md)。  
  
- **Convertibility.** 可以在两个数组的元素类型之间进行扩大或收缩转换。 未能满足此要求的一个示例是在 `String` 数组和从派生的类的数组之间尝试转换 <xref:System.Attribute?displayProperty=nameWithType> 。 这两种类型共有，它们之间不存在任何类型的转换。  
  
 将一个数组类型转换为另一个数组类型是扩大或收缩，这取决于各个元素的转换是扩大还是收缩。 有关详细信息，请参阅 [Widening and Narrowing Conversions](widening-and-narrowing-conversions.md)。  
  
## <a name="conversion-to-an-object-array"></a>转换为对象数组  

 在 `Object` 不初始化数组的情况下声明数组时，其元素类型将 `Object` 保持未初始化状态。 将其设置为特定类的数组时，它将采用该类的类型。 但是，它的基础类型仍为 `Object` ，你随后可以将其设置为不相关的类的另一个数组。 由于所有类均派生自 `Object` ，因此可以将数组的元素类型从任何类更改为任何其他类。  
  
 在下面的示例中，类型与之间不存在转换 `student` `String` ，但均派生自 `Object` ，因此所有分配都有效。  
  
```vb  
' Assume student has already been defined as a class.  
Dim testArray() As Object  
' testArray is still an Object array at this point.  
Dim names() As String = New String(3) {"Name0", "Name1", "Name2", "Name3"}  
testArray = New student(3) {}  
' testArray is now of type student().  
testArray = names  
' testArray is now a String array.  
```  
  
### <a name="underlying-type-of-an-array"></a>数组的基础类型  

 如果最初使用特定类声明数组，则其基础元素类型为该类。 如果随后将其设置为另一个类的数组，则这两个类之间必须存在转换。  
  
 在下面的示例中， `students` 是一个 `student` 数组。 由于与之间不存在 `String` 转换 `student` ，因此最后一个语句会失败。  
  
```vb  
Dim students() As student  
Dim names() As String = New String(3) {"Name0", "Name1", "Name2", "Name3"}  
students = New Student(3) {}  
' The following statement fails at compile time.  
students = names  
```  
  
## <a name="see-also"></a>请参阅

- [数据类型](index.md)
- [Visual Basic 中的类型转换](type-conversions.md)
- [隐式转换和显式转换](implicit-and-explicit-conversions.md)
- [字符串和其他类型之间的转换](conversions-between-strings-and-other-types.md)
- [如何：在 Visual Basic 中将一个对象转换为其他类型](how-to-convert-an-object-to-another-type.md)
- [数据类型](../../../language-reference/data-types/index.md)
- [Type Conversion Functions](../../../language-reference/functions/type-conversion-functions.md)
- [数组](../arrays/index.md)
