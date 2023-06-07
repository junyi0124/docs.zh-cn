---
title: Structure 语句
ms.date: 05/12/2018
f1_keywords:
- vb.Structure
- Structure
helpviewer_keywords:
- user-defined types [Visual Basic], Structure statement
- compound data types [Visual Basic]
- Structure keyword [Visual Basic]
- Structure statement [Visual Basic]
- UDT (user-defined types)
- types [Visual Basic], user-defined
ms.assetid: 9bd1deea-2a89-4cdc-812c-6dcbb947c391
ms.openlocfilehash: 120f836b9d49c00e9c53af0d1fc832e22c8cbbb8
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346460"
---
# <a name="structure-statement"></a>Structure 语句

声明结构名称，并引入结构所包含的变量、属性、事件和过程的定义。

## <a name="syntax"></a>语法

```vb
[ <attributelist> ] [ accessmodifier ] [ Shadows ] [ Partial ] _
Structure name [ ( Of typelist ) ]
    [ Implements interfacenames ]
    [ datamemberdeclarations ]
    [ methodmemberdeclarations ]
End Structure
```

## <a name="parts"></a>部件

|术语|Definition|
|---|---|
|`attributelist`|可选。 请参阅[特性列表](attribute-list.md)。|
|`accessmodifier`|可选。 可以是以下各项之一：<br /><br /> -   [公有](../modifiers/public.md)<br />-   [保护](../modifiers/protected.md)<br />-   [朋友](../modifiers/friend.md)<br />-   [私有](../modifiers/private.md)<br />- [受保护的朋友](../modifiers/protected-friend.md)<br/>- [私有保护](../modifiers/private-protected.md) <br /><br /> 请参阅 [Access levels in Visual Basic](../../programming-guide/language-features/declared-elements/access-levels.md)。|
|`Shadows`|可选。 请参阅[阴影](../modifiers/shadows.md)。|
|`Partial`|可选。 指示结构的分部定义。 请参阅[部分](../modifiers/partial.md)。|
|`name`|必需。 此结构的名称。 请参阅 [Declared Element Names](../../programming-guide/language-features/declared-elements/declared-element-names.md)。|
|`Of`|可选。 指定这是一个泛型结构。|
|`typelist`|如果使用 of 关键字，则[为](of-clause.md)必需。 此结构的类型参数列表。 请参阅[类型列表](type-list.md)。|
|`Implements`|可选。 指示此结构实现一个或多个接口的成员。 请参阅[Implements 语句](implements-statement.md)。|
|`interfacenames`|如果使用 `Implements` 语句，则是必需的。 此结构实现的接口的名称。|
|`datamemberdeclarations`|必需。 零个或多个 `Const`、`Dim`、`Enum`或 `Event` 语句声明结构的*数据成员*。|
|`methodmemberdeclarations`|可选。 `Function`、`Operator`、`Property`或 `Sub` 过程的零个或多个声明，它们充当结构的*方法成员*。|
|`End Structure`|必需。 终止 `Structure` 定义。|

## <a name="remarks"></a>备注

`Structure` 语句定义可自定义的复合值类型。 *结构*是早期版本的 Visual Basic 的用户定义类型（UDT）的泛化。 有关详细信息，请参阅[结构](../../programming-guide/language-features/data-types/structures.md)。

结构支持许多与类相同的功能。 例如，结构可以具有属性和过程，它们可以实现接口，并且它们可以具有参数化的构造函数。 但是，在继承、声明和使用等区域中，结构与类之间存在重大差异。 同样，类是引用类型，而结构是值类型。 有关详细信息，请参阅[结构和类](../../programming-guide/language-features/data-types/structures-and-classes.md)。

只能在命名空间或模块级别使用 `Structure`。 这意味着结构的*声明上下文*必须是源文件、命名空间、类、结构、模块或接口，不能是过程或块。 有关详细信息，请参阅[声明上下文和默认访问级别](declaration-contexts-and-default-access-levels.md)。

结构默认为[Friend](../modifiers/friend.md)访问。 您可以使用访问修饰符调整其访问级别。 有关详细信息，请参阅[Visual Basic 中的访问级别](../../programming-guide/language-features/declared-elements/access-levels.md)。

## <a name="rules"></a>规则

- **层.** 可以在另一个结构中定义一个结构。 外部结构称为*包含结构*，而内部结构称为*嵌套结构*。 但是，不能通过包含结构访问嵌套结构的成员。 相反，您必须声明嵌套结构的数据类型的变量。

- **成员声明。** 必须声明结构的每个成员。 由于不能从结构继承结构成员，因此不能对其进行[保护](../modifiers/protected.md)或 `Protected Friend`。 不过，结构本身可以是 `Protected` 或 `Protected Friend`。
  
     您可以在结构中声明零个以上的非共享变量或非共享、非自定义的事件。 不能仅包含常量、属性和过程，即使其中一些是非共享的。

- **起始.** 不能将结构的任何非共享数据成员的值初始化为其声明的一部分。 您必须通过结构上的参数化构造函数来初始化此类数据成员，或者在创建了该结构的实例之后为成员指定一个值。

- **继承。** 结构不能从所有结构继承的 <xref:System.ValueType>以外的任何类型继承。 特别是，一个结构不能从另一个结构继承。

     不能在结构定义中使用[Inherits 语句](inherits-statement.md)，甚至可以指定 <xref:System.ValueType>。

- **部署.** 如果结构使用[Implements 语句](implements-statement.md)，则必须实现 `interfacenames`中指定的每个接口所定义的每个成员。

- **默认属性。** 结构最多可使用[默认](../modifiers/default.md)修饰符指定一个属性作为其*默认属性*。 有关详细信息，请参阅[默认值](../modifiers/default.md)。

## <a name="behavior"></a>行为

- **访问级别。** 在结构中，你可以使用其自己的访问级别声明每个成员。 所有结构成员默认为[公共](../modifiers/public.md)访问。 请注意，如果结构本身具有限制性更高的访问级别，则这将自动限制对其成员的访问，即使你使用访问修饰符调整其访问级别也是如此。

- **内.** 结构在其包含的命名空间、类、结构或模块的范围内。

     每个结构成员的作用域都是整个结构。

- **生存期.** 结构本身不具有生存期。 相反，该结构的每个实例都具有独立于所有其他实例的生存期。

     实例的生存期在由[新的 Operator](../operators/new-operator.md)子句创建时开始。 它在保存它的变量的生存期结束时结束。

     不能扩展结构实例的生存期。 模块提供了对静态结构功能的近似值。 有关详细信息，请参阅[Module 语句](module-statement.md)。

     结构成员的生存期取决于其声明的方式和位置。 有关详细信息，请参阅[类语句](class-statement.md)中的 "生存期"。

- **限定.** 结构外的代码必须使用该结构的名称来限定成员的名称。

     如果嵌套结构内的代码对编程元素进行非限定引用，则 Visual Basic 首先在嵌套结构中搜索元素，然后在其包含的结构中搜索元素，并将其放入外部的包含元素。 有关详细信息，请参阅 [References to Declared Elements](../../programming-guide/language-features/declared-elements/references-to-declared-elements.md)。

- **内存消耗。** 与所有复合数据类型一样，不能通过将其成员的标称存储分配相加来安全地计算结构的总内存消耗量。 而且，不能安全地假设内存中的存储顺序与声明顺序相同。 如果需要控制结构的存储布局，则可以将 <xref:System.Runtime.InteropServices.StructLayoutAttribute> 特性应用于 `Structure` 语句。

## <a name="example"></a>示例

下面的示例使用 `Structure` 语句为员工定义一组相关数据。 它显示了如何使用 `Public`、`Friend`和 `Private` 成员来反映数据项的敏感性。 它还显示过程、属性和事件成员。

[!code-vb[VbVbalrStatements#57](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#57)]

有关如何使用 `Structure`的详细信息，请参阅[结构变量](../../programming-guide/language-features/data-types/structure-variables.md)。

## <a name="see-also"></a>另请参阅

- [Class 语句](class-statement.md)
- [Interface 语句](interface-statement.md)
- [Module 语句](module-statement.md)
- [Dim 语句](dim-statement.md)
- [Const 语句](const-statement.md)
- [Enum 语句](enum-statement.md)
- [Event 语句](event-statement.md)
- [Operator Statement](operator-statement.md)
- [Property 语句](property-statement.md)
- [结构和类](../../programming-guide/language-features/data-types/structures-and-classes.md)
