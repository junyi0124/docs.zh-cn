---
title: Object Data Type
ms.date: 07/20/2015
f1_keywords:
- vb.Object
- vb.Variant
helpviewer_keywords:
- object variables [Visual Basic], Object type
- data types [Visual Basic], assigning
- Object data type
- Object data type [Visual Basic], reference
ms.assetid: 61ea4a7c-3b3d-48d4-adc4-eacfa91779b2
ms.openlocfilehash: 14770e74a0169dba3a45a04845dd32323e6201e3
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84415578"
---
# <a name="object-data-type"></a>Object Data Type

保存引用对象的地址。 可以将任何引用类型（字符串、数组、类或接口）分配给 `Object` 变量。 `Object`变量还可以引用任意值类型的数据（数值、、 `Boolean` `Char` 、 `Date` 、结构或枚举）。

## <a name="remarks"></a>备注

`Object`数据类型可以指向任何数据类型的数据，包括应用程序识别的任何对象实例。 在 `Object` 编译时不知道变量可能指向的数据类型时，请使用。

的默认值 `Object` 为 `Nothing` （空引用）。

## <a name="data-types"></a>数据类型

可以将任何数据类型的变量、常量或表达式分配给 `Object` 变量。 若要确定 `Object` 变量当前引用的数据类型，可以使用 <xref:System.Type.GetTypeCode%2A> 类的方法 <xref:System.Type?displayProperty=nameWithType> 。 下面的示例对此进行了演示。

```vb
Dim myObject As Object
' Suppose myObject has now had something assigned to it.
Dim datTyp As Integer
datTyp = Type.GetTypeCode(myObject.GetType())
```

`Object`数据类型为引用类型。 但是， `Object` 当变量引用值类型的数据时，Visual Basic 将变量视为值类型。

## <a name="storage"></a>存储

无论它引用哪种数据类型， `Object` 变量都不包含数据值本身，而是指向值的指针。 它在计算机内存中始终使用四个字节，但这不包括表示变量值的数据的存储。 由于使用指针查找数据的代码， `Object` 保存值类型的变量比显式类型化变量略慢。

## <a name="programming-tips"></a>编程提示

- **互操作注意事项。** 如果与不是为 .NET Framework 编写的组件（如自动化或 COM 对象）交互，请记住，其他环境中的指针类型与 Visual Basic 类型不兼容 `Object` 。

- **性能。** 用类型声明的变量的 `Object` 灵活性足以包含对任何对象的引用。 但是，在此类变量上调用方法或属性时，始终会产生*后期绑定*（在运行时）。 若要强制进行*早期绑定*（在编译时）和更好的性能，请使用特定类名称声明变量，或将其转换为特定数据类型。

  声明对象变量时，请尝试使用特定类类型， <xref:System.OperatingSystem> 而不是通用 `Object` 类型。 还应使用最具体的类，如 <xref:System.Windows.Forms.TextBox> 而不是 <xref:System.Windows.Forms.Control> ，以便您可以访问其属性和方法。 通常可以使用**对象浏览器**中的 "**类**" 列表来查找可用的类名。

- **扩大.** 所有数据类型和所有引用类型都扩大到 `Object` 数据类型。 这意味着，可以将任何类型转换为， `Object` 而不会遇到 <xref:System.OverflowException?displayProperty=nameWithType> 错误。

  但是，如果在值类型和之间 `Object` 进行转换，Visual Basic 将执行称为*装箱*和*取消装箱*的操作，这会使执行速度变慢。

- **键入字符。** `Object`没有文本类型字符或标识符类型字符。

- **Framework 类型。** .NET Framework 中的相应类型是 <xref:System.Object?displayProperty=nameWithType> 类。

## <a name="example"></a>示例

下面的示例演示 `Object` 指向对象实例的变量。

```vb
Dim objDb As Object
Dim myCollection As New Collection()
' Suppose myCollection has now been populated.
objDb = myCollection.Item(1)
```

## <a name="see-also"></a>另请参阅

- <xref:System.Object>
- [数据类型](index.md)
- [Type Conversion Functions](../functions/type-conversion-functions.md)
- [转换摘要](../keywords/conversion-summary.md)
- [有效使用数据类型](../../programming-guide/language-features/data-types/efficient-use-of-data-types.md)
- [如何：确定两个对象是否相关](../../programming-guide/language-features/variables/how-to-determine-whether-two-objects-are-related.md)
- [如何：确定两个对象是否相同](../../programming-guide/language-features/variables/how-to-determine-whether-two-objects-are-identical.md)
