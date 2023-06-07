---
title: Const 语句
ms.date: 05/12/2018
f1_keywords:
- vb.Const
helpviewer_keywords:
- Const statement [Visual Basic]
ms.assetid: 495b318d-b7c5-4198-94f8-0790a541b07a
ms.openlocfilehash: 3b05d4067ef99e03df07d2c316c982051180d961
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84382102"
---
# <a name="const-statement-visual-basic"></a>Const 语句 (Visual Basic)

声明和定义一个或多个常量。

## <a name="syntax"></a>语法

```vb
[ <attributelist> ] [ accessmodifier ] [ Shadows ]
Const constantlist
```

## <a name="parts"></a>组成部分

`attributelist`  
可选。 应用于此语句中声明的所有常量的特性列表。 请参阅尖括号中的[属性列表](attribute-list.md)（" `<` " 和 " `>` "）。

`accessmodifier`  
可选。 用于指定哪些代码可以访问这些常量。 可以是[公共](../modifiers/public.md)、[受保护](../modifiers/protected.md)、[朋友](../modifiers/friend.md)、[受保护的朋友](../modifiers/protected-friend.md)、[私有](../modifiers/private.md)或[私有保护](../modifiers/private-protected.md)的。

`Shadows`  
可选。 用于在基类中重新声明和隐藏编程元素。 请参阅[阴影](../modifiers/shadows.md)。

`constantlist`  
必需。 在此语句中声明的常量的列表。

`constant` `[ ,` `constant` `... ]`

每个 `constant` 都具有以下语法和部件：

`constantname` `[ As` `datatype` `] =` `initializer`

|组成部分|说明|
|----------|-----------------|
|`constantname`|必需。 常数的名称。 请参阅 [Declared Element Names](../../programming-guide/language-features/declared-elements/declared-element-names.md)。|
|`datatype`|如果 `Option Strict` 为，则为必需 `On` 。 常量的数据类型。|
|`initializer`|必需。 在编译时计算并分配给常量的表达式。|

## <a name="remarks"></a>备注

如果你的应用程序中有一个永不更改的值，则可以定义一个已命名的常量，并将其用于替代文本值。 名称比值更易于记忆。 可以仅定义一次常数，并在代码中的多个位置使用它。 如果在更高版本中，需要重新定义值， `Const` 只需进行更改即可。

只能 `Const` 在模块或过程级别使用。 这意味着变量的*声明上下文*必须是类、结构、模块、过程或块，而不能是源文件、命名空间或接口。 有关详细信息，请参阅[声明上下文和默认访问级别](declaration-contexts-and-default-access-levels.md)。

本地常量（在过程中）默认为公共访问，不能对其使用任何访问修饰符。 类和模块成员常量（任何过程外部）默认为私有访问，结构成员常量默认为公共访问。 您可以使用访问修饰符调整其访问级别。

## <a name="rules"></a>规则

- **声明上下文。** 在任何过程之外，在模块级别声明的常量是*成员常量*;它是声明它的类、结构或模块的成员。

  在过程级别声明的常量是*局部常量*;它在声明它的过程或块的本地。

- **属性.** 仅可将属性应用于成员常量，而不能应用于局部常数。 特性向程序集的元数据提供信息，这对于临时存储（如本地常量）没有意义。

- **组成.** 默认情况下，所有常量均为 `Shared` 、 `Static` 和 `ReadOnly` 。 在声明常数时不能使用任何这些关键字。

  在过程级别，不能使用 `Shadows` 或任何访问修饰符来声明局部常量。

- **多个常量。** 可以在同一声明语句中声明多个常量，并 `constantname` 为每个常量指定部件。 多个常量用逗号分隔。

## <a name="data-type-rules"></a>数据类型规则

- **数据类型。** `Const`语句可以声明变量的数据类型。 您可以指定任何数据类型或枚举的名称。

- **默认类型。** 如果不指定 `datatype` ，则常量将采用的数据类型 `initializer` 。 如果同时指定 `datatype` 和 `initializer` ，的数据类型 `initializer` 必须可转换为 `datatype` 。 如果两者都不 `datatype` `initializer` 存在，则数据类型默认为 `Object` 。

- **不同类型。** 您可以为不同的常量指定不同的数据类型， `As` 为您声明的每个变量使用单独的子句。 但是，不能通过使用 common 子句声明多个常量为同一类型 `As` 。

- **起始.** 必须初始化中每个常量的值 `constantlist` 。 使用可 `initializer` 提供要分配给常数的表达式。 表达式可以是文本的任意组合、已经定义的其他常数以及已定义的枚举成员。 可以使用算术运算符和逻辑运算符来合并此类元素。

  不能在中使用变量或函数 `initializer` 。 但是，可以使用转换关键字 `CByte` ，如和 `CShort` 。 `AscW`如果使用常量或参数调用该方法，则还可以使用 `String` `Char` ，因为在编译时可对其进行计算。

## <a name="behavior"></a>行为

- **内.** 本地常量只能从其过程或块中访问。 成员常量可从其类、结构或模块中的任何位置进行访问。

- **限定.** 类、结构或模块外的代码必须使用该类、结构或模块的名称来限定成员常量的名称。 过程或块外的代码不能引用该过程或块中的任何本地常数。

## <a name="example"></a>示例

下面的示例使用 `Const` 语句声明用于替代文本值的常量。

[!code-vb[VbVbalrStatements#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#13)]

## <a name="example"></a>示例

如果使用数据类型定义常量 `Object` ，则 Visual Basic 编译器会为其提供类型 `initializer` ，而不是 `Object` 。 在下面的示例中，常量 `naturalLogBase` 具有运行时类型 `Decimal` 。

[!code-vb[VbVbalrStatements#87](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#87)]

前面的示例对 <xref:System.Type.ToString%2A> <xref:System.Type> [GetType 运算符](../operators/gettype-operator.md)返回的对象使用方法，因为 <xref:System.Type> 不能使用将转换为 `String` `CStr` 。

## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.Strings.Asc%2A>
- <xref:Microsoft.VisualBasic.Strings.AscW%2A>
- [Enum 语句](enum-statement.md)
- [#Const 指令](../directives/const-directive.md)
- [Dim 语句](dim-statement.md)
- [ReDim 语句](redim-statement.md)
- [隐式转换和显式转换](../../programming-guide/language-features/data-types/implicit-and-explicit-conversions.md)
- [常量和枚举](../../programming-guide/language-features/constants-enums/index.md)
- [常量和枚举](../constants-and-enumerations.md)
- [Type Conversion Functions](../functions/type-conversion-functions.md)
