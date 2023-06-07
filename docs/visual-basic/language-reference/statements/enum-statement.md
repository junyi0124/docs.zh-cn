---
title: Enum 语句
ms.date: 07/20/2015
f1_keywords:
- vb.Enum
helpviewer_keywords:
- enumerated constants [Visual Basic]
- Enum statement [Visual Basic]
- Private keyword [Visual Basic], Enum statements
- Public keyword [Visual Basic], in Enum statement
- variables [Visual Basic], enumeration
- constants [Visual Basic], enumerated
ms.assetid: a45e51f1-65ff-48e1-bf32-79130f137377
ms.openlocfilehash: 976cc68d67c69ec86918962ab2dd3406d15aed9a
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404727"
---
# <a name="enum-statement-visual-basic"></a>Enum 语句 (Visual Basic)

声明枚举并定义其成员的值。

## <a name="syntax"></a>语法

```vb
[ <attributelist> ] [ accessmodifier ]  [ Shadows ]
Enum enumerationname [ As datatype ]
   memberlist
End Enum
```

## <a name="parts"></a>组成部分

- `attributelist`

  可选。 应用于此枚举的特性的列表。 必须将[特性列表](attribute-list.md)括在尖括号（" `<` " 和 " `>` "）中。

  <xref:System.FlagsAttribute>特性指示枚举实例的值可以包含多个枚举成员，并且每个成员表示枚举值中的一个位域。

- `accessmodifier`

  可选。 指定哪些代码可以访问此枚举。 可以是以下其中一个值：

  - [公共](../modifiers/public.md)

  - [避免](../modifiers/protected.md)

  - [友好](../modifiers/friend.md)

  - 专用 

  - [Protected Friend](../modifiers/protected-friend.md)

  - [私有受保护](../modifiers/private-protected.md)

- `Shadows`

  可选。 指定此枚举重新声明并隐藏基类中具有相同名称的编程元素或重载元素集。 只能为枚举本身指定阴影，而不能在其任何成员上指定[阴影](../modifiers/shadows.md)。

- `enumerationname`

  必需。 枚举的名称。 有关有效名称的信息，请参阅已[声明的元素名称](../../programming-guide/language-features/declared-elements/declared-element-names.md)。

- `datatype`

  可选。 枚举及其所有成员的数据类型。

- `memberlist`

  必需。 在此语句中声明的成员常量的列表。 多个成员出现在单独的源代码行上。

  每个 `member` 都具有以下语法和部分：`[<attribute list>] member name [ = initializer ]`

  |组成部分|说明|
  |---|---|
  |`membername`|必需。 此成员的名称。|
  |`initializer`|可选。 在编译时计算并分配给此成员的表达式。|

- `End` `Enum`

  终止 `Enum` 块。

## <a name="remarks"></a>备注

如果你有一组在逻辑上彼此相关的不变值，则可以在枚举中将它们一起定义。 这为枚举及其成员提供有意义的名称，这些名称比它们的值更容易记忆。 然后，你可以在代码中的多个位置使用枚举成员。

使用枚举的优点包括：

- 减少由转置或错误错误导致的错误。

- 以后可以轻松地更改值。

- 使代码更易于阅读，这意味着会引入错误的可能性更小。

- 确保向前兼容性。 如果使用枚举，则如果将来有人更改了与成员名称相对应的值，你的代码将不太可能失败。

枚举具有名称、基础数据类型和成员集。 每个成员都表示一个常量。

在类、结构、模块或接口级别（在任何过程之外）声明的枚举是*成员枚举*。 它是声明它的类、结构、模块或接口的成员。

成员枚举可以从其类、结构、模块或接口中的任何位置进行访问。 类、结构或模块外的代码必须使用该类、结构或模块的名称来限定成员枚举的名称。 您可以通过向源文件添加[Imports](imports-statement-net-namespace-and-type.md)语句来避免使用完全限定的名称。

在任何类、结构、模块或接口的命名空间级别声明的枚举是它所显示的命名空间的成员。

枚举的*声明上下文*必须是源文件、命名空间、类、结构、模块或接口，不能是过程。 有关详细信息，请参阅[声明上下文和默认访问级别](declaration-contexts-and-default-access-levels.md)。

您可以将属性作为一个整体应用于枚举，而不是单独应用于其成员。 属性将信息提供给程序集的元数据。

## <a name="data-type"></a>数据类型

`Enum`语句可以声明枚举的数据类型。 每个成员都采用枚举的数据类型。 可以指定 `Byte` 、、 `Integer` `Long` 、 `SByte` 、、、 `Short` `UInteger` `ULong` 或 `UShort` 。

如果不 `datatype` 为枚举指定，则每个成员都采用其的数据类型 `initializer` 。 如果同时指定 `datatype` 和 `initializer` ，的数据类型 `initializer` 必须可转换为 `datatype` 。 如果两者都不 `datatype` `initializer` 存在，则数据类型默认为 `Integer` 。

## <a name="initializing-members"></a>初始化成员

`Enum`语句可初始化中所选成员的内容 `memberlist` 。 用于 `initializer` 提供要分配给成员的表达式。

如果不 `initializer` 为成员指定，Visual Basic 会将其初始化为零（如果它是中的第一个 `member` `memberlist` ），或者将其值指定为大于前面的值 `member` 。

每个中提供的表达式 `initializer` 可以是文本的任意组合、已经定义的其他常数以及已定义的枚举成员（包括此枚举的上一个成员）。 可以使用算术运算符和逻辑运算符来合并此类元素。

不能在中使用变量或函数 `initializer` 。 但是，可以使用转换关键字 `CByte` ，如和 `CShort` 。 `AscW`如果使用常量或参数调用该方法，则还可以使用 `String` `Char` ，因为在编译时可对其进行计算。

枚举的值不能为浮点值。 如果为成员分配浮点值并且 `Option Strict` 将设置为 on，则会发生编译器错误。 如果 `Option Strict` 为 off，则将值自动转换为 `Enum` 类型。

如果成员的值超出了基础数据类型允许的范围，或将任何成员初始化为基础数据类型允许的最大值，则编译器将报告错误。

## <a name="modifiers"></a>修饰符

类、结构、模块和接口成员枚举默认为公共访问。 您可以使用访问修饰符调整其访问级别。 命名空间成员枚举默认为 friend 访问。 可以将其访问级别调整为 "公共"，而不是 "私有" 或 "受保护"。 有关详细信息，请参阅[Visual Basic 中的访问级别](../../programming-guide/language-features/declared-elements/access-levels.md)。

所有枚举成员均具有公共访问权限，因此不能对它们使用任何访问修饰符。 但是，如果枚举本身具有限制性更高的访问级别，则将优先使用指定的枚举访问级别。

默认情况下，所有枚举都是类型，其字段为常量。 因此 `Shared` ， `Static` `ReadOnly` 在声明枚举或其成员时，不能使用、和关键字。

## <a name="assigning-multiple-values"></a>分配多个值

枚举通常表示互斥的值。 通过 <xref:System.FlagsAttribute> 在声明中包含特性 `Enum` ，可以将多个值分配给枚举的实例。 <xref:System.FlagsAttribute>特性指定将枚举视为位域（即一组标志）。 它们称为*按位*枚举。

使用属性声明枚举时 <xref:System.FlagsAttribute> ，我们建议你使用2的幂，即1、2、4、8、16等值作为值。 我们还建议将 "无" 作为值为0的成员的名称。 有关其他指南，请参阅 <xref:System.FlagsAttribute> 和 <xref:System.Enum> 。

## <a name="example"></a>示例

下面的示例演示如何使用 `Enum` 语句。 请注意，成员称为 `EggSizeEnum.Medium` ，而不是 `Medium` 。

[!code-vb[VbEnumsTask#41](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class1.vb#41)]

## <a name="example"></a>示例

下面的示例中的方法在类的外部 `Egg` 。 因此， `EggSizeEnum` 完全限定为 `Egg.EggSizeEnum` 。

[!code-vb[VbEnumsTask#42](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class1.vb#42)]

## <a name="example"></a>示例

下面的示例使用 `Enum` 语句来定义一组相关的命名常量值。 在这种情况下，值是可以选择为数据库设计数据输入窗体的颜色。

[!code-vb[VbEnumsTask#30](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#30)]

## <a name="example"></a>示例

下面的示例显示了包含正负数的值。

[!code-vb[VbEnumsTask#31](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#31)]

## <a name="example"></a>示例

在下面的示例中， `As` 子句用于指定 `datatype` 枚举的。

[!code-vb[VbEnumsTask#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#6)]

## <a name="example"></a>示例

下面的示例演示如何使用按位枚举。 可以将多个值分配给位枚举的实例。 `Enum`声明包含 <xref:System.FlagsAttribute> 特性，该特性指示可将枚举视为一组标志。

[!code-vb[VbEnumsTask#61](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class1.vb#61)]

## <a name="example"></a>示例

下面的示例循环访问枚举。 它使用 <xref:System.Enum.GetNames%2A> 方法从枚举中检索成员名称的数组，并 <xref:System.Enum.GetValues%2A> 检索成员值的数组。

[!code-vb[VbEnumsTask#51](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class1.vb#51)]

## <a name="see-also"></a>另请参阅

- <xref:System.Enum>
- <xref:Microsoft.VisualBasic.Strings.AscW%2A>
- [Const 语句](const-statement.md)
- [Dim 语句](dim-statement.md)
- [隐式转换和显式转换](../../programming-guide/language-features/data-types/implicit-and-explicit-conversions.md)
- [Type Conversion Functions](../functions/type-conversion-functions.md)
- [常量和枚举](../constants-and-enumerations.md)
