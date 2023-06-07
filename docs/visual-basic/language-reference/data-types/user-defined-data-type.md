---
title: 用户定义的数据类型
ms.date: 07/20/2015
f1_keywords:
- UserDefined
- UDT
- vb.UDT
- User-Defined
- vb.UserDefined
- vb.User-Defined
helpviewer_keywords:
- user-defined data types [Visual Basic], Visual Basic
- user-defined types
- structures [Visual Basic], as user-defined data types
- user-defined types [Visual Basic], Visual Basic
- user-defined types [Visual Basic], structure declaration
- user-defined types [Visual Basic], structures in Visual Basic
- user-defined data types [Visual Basic], structure declaration
- data types [Visual Basic], assigning
- Structure statement [Visual Basic]
- data types [Visual Basic], user-defined
- user-defined data types [Visual Basic], structures in Visual Basic
- user-defined data types
- types [Visual Basic], user-defined
ms.assetid: be913dca-a364-4a51-96a1-549a1b390b0a
ms.openlocfilehash: fbd9536a54d7fb471d6cb2e130b14a84e40a4940
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84415487"
---
# <a name="user-defined-data-type"></a>用户定义的数据类型

以您定义的格式保存数据。 `Structure`语句定义格式。

以前版本的 Visual Basic 支持用户定义的类型（UDT）。 当前版本将 UDT 扩展到*结构*。 结构是一种或多种数据类型*成员*的串联。 Visual Basic 将结构视为单个单元，但你也可以单独访问其成员。

## <a name="remarks"></a>备注

当需要将各种数据类型组合成单个单元时，或在没有任何基本数据类型满足您的需求时，可定义和使用结构数据类型。

结构数据类型的默认值由其每个成员的默认值组合而成。

## <a name="declaration-format"></a>声明格式

结构声明以[结构语句](../statements/structure-statement.md)开头，以 `End Structure` 语句结尾。 `Structure`语句提供结构的名称，该结构也是结构正在定义的数据类型的标识符。 代码的其他部分可以使用此标识符来声明变量、参数和函数返回值，使其成为此结构的数据类型。

和语句之间的 `Structure` 声明 `End Structure` 定义结构的成员。

## <a name="member-access-levels"></a>成员访问级别

必须使用[Dim 语句](../statements/dim-statement.md)或指定访问级别的语句（如[Public](../modifiers/public.md)、 [Friend](../modifiers/friend.md)或[Private](../modifiers/private.md)）声明每个成员。 如果使用 `Dim` 语句，则访问级别默认为 "公共"。

## <a name="programming-tips"></a>编程提示

- **内存消耗。** 与所有复合数据类型一样，不能通过将其成员的标称存储分配相加来安全地计算结构的总内存消耗量。 而且，不能安全地假设内存中的存储顺序与声明顺序相同。 如果需要控制结构的存储布局，则可以将属性应用于 <xref:System.Runtime.InteropServices.StructLayoutAttribute> 该 `Structure` 语句。

- **互操作注意事项。** 如果与不是为 .NET Framework 编写的组件（如自动化或 COM 对象）交互，请记住，其他环境中的用户定义类型与 Visual Basic 结构类型不兼容。

- **扩大.** 不会与任何结构数据类型自动转换。 您可以使用[Operator 语句](../statements/operator-statement.md)在结构上定义转换运算符，并且可以将每个转换运算符声明为 `Widening` 或 `Narrowing` 。

- **键入字符。** 结构数据类型没有文本类型字符或标识符类型字符。

- **Framework 类型。** .NET Framework 中没有相应的类型。 所有结构都继承自 .NET Framework 类 <xref:System.ValueType?displayProperty=nameWithType> ，但没有单个结构对应于 <xref:System.ValueType?displayProperty=nameWithType> 。

## <a name="example"></a>示例

以下范例显示了结构声明的轮廓。

```vb
[Public | Protected | Friend | Protected Friend | Private] Structure structname
    {Dim | Public | Friend | Private} member1 As datatype1
    ' ...
    {Dim | Public | Friend | Private} memberN As datatypeN
End Structure
```

## <a name="see-also"></a>另请参阅

- <xref:System.ValueType>
- <xref:System.Runtime.InteropServices.StructLayoutAttribute>
- [数据类型](index.md)
- [Type Conversion Functions](../functions/type-conversion-functions.md)
- [转换摘要](../keywords/conversion-summary.md)
- [Structure 语句](../statements/structure-statement.md)
- [Widening](../modifiers/widening.md)
- [Narrowing](../modifiers/narrowing.md)
- [结构](../../programming-guide/language-features/data-types/structures.md)
- [有效使用数据类型](../../programming-guide/language-features/data-types/efficient-use-of-data-types.md)
