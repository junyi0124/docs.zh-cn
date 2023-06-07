---
title: Property 过程
ms.date: 07/20/2015
helpviewer_keywords:
- Set statement [Visual Basic], Property procedures
- Visual Basic code, procedures
- return values [Visual Basic], Property procedures
- syntax [Visual Basic], Property procedures
- procedures [Visual Basic], property
- Visual Basic code, properties
- procedures [Visual Basic], calling
- properties [Visual Basic], custom
- property procedures
- Get statement [Visual Basic], property procedures
ms.assetid: 46a98379-e1a2-45dd-a48c-b51213f5ab07
ms.openlocfilehash: cb5b0e12512e476b7c96bbfb19f8e4f470f6b498
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84363728"
---
# <a name="property-procedures-visual-basic"></a>Property 过程 (Visual Basic)

属性过程是一系列 Visual Basic 语句，这些语句操作模块、类或结构上的自定义属性。 属性过程也称为*属性访问器*。

Visual Basic 为以下属性过程提供：

- `Get`过程返回属性的值。 当您访问表达式中的属性时，将调用它。
- `Set`过程将属性设置为一个值，包括对象引用。 向属性赋值时，将调用此方法。

通常使用和语句来成对定义属性过程 `Get` `Set` ，但是，如果属性是只读的（[Get 语句](../../../language-reference/statements/get-statement.md)）或只写的（[Set 语句](../../../language-reference/statements/set-statement.md)），则可以单独定义任何一个过程。

`Get` `Set` 使用自动实现的属性时，可以忽略和过程。 有关详细信息，请参阅[自动实现的属性](./auto-implemented-properties.md)。

可以在类、结构和模块中定义属性。 `Public`默认情况下，属性为，这意味着您可以从应用程序中可访问该属性的容器的任何位置调用它们。

有关属性和变量的比较，请参阅[Visual Basic 中属性和变量之间的差异](differences-between-properties-and-variables.md)。

## <a name="declaration-syntax"></a>声明语法

属性本身由[属性语句](../../../language-reference/statements/property-statement.md)和语句中包含的代码块定义 `End Property` 。 在此块中，每个属性过程显示为包含在声明语句（ `Get` 或 `Set` ）和匹配声明中的内部块 `End` 。

声明属性及其过程的语法如下所示：

```vb
[Default] [Modifiers] Property PropertyName[(ParameterList)] [As DataType]
    [AccessLevel] Get
        ' Statements of the Get procedure.
        ' The following statement returns an expression as the property's value.
        Return Expression
    End Get
    [AccessLevel] Set[(ByVal NewValue As DataType)]
        ' Statements of the Set procedure.
        ' The following statement assigns newvalue as the property's value.
        LValue = NewValue
    End Set
End Property
' - or -
[Default] [Modifiers] Property PropertyName [(ParameterList)] [As DataType]
```

`Modifiers`可以指定有关重载、重写、共享和隐藏的访问级别和信息，以及属性是只读还是只写。 `AccessLevel` `Get` 或过程的可以是 `Set` 比为属性本身指定的访问级别更严格的任何级别。 有关详细信息，请参阅[Property 语句](../../../language-reference/statements/property-statement.md)。

### <a name="data-type"></a>数据类型

属性的数据类型和主体访问级别在语句中定义，而不是在 `Property` 属性过程中定义。 属性只能有一种数据类型。 例如，不能定义属性来存储 `Decimal` 值，而是检索 `Double` 值。

### <a name="access-level"></a>访问级别

但是，您可以为属性定义主体访问级别，并在它的某个属性过程中进一步限制访问级别。 例如，可以定义 `Public` 属性，然后定义 `Private Set` 过程。 此 `Get` 过程仍然有效 `Public` 。 只能更改某个属性过程的访问级别，并且只能使其比主体访问级别更严格。 有关详细信息，请参阅[如何：声明具有混合访问级别的属性](how-to-declare-a-property-with-mixed-access-levels.md)。

## <a name="parameter-declaration"></a>参数声明

声明每个参数的方式与处理[Sub 过程](sub-procedures.md)的方法相同，只不过传递机制必须是 `ByVal` 。

参数列表中每个参数的语法如下所示：

```vb
[Optional] ByVal [ParamArray] parametername As datatype
```

如果该参数是可选的，则还必须提供默认值作为其声明的一部分。 指定默认值的语法如下所示：

```vb
Optional ByVal parametername As datatype = defaultvalue
```

## <a name="property-value"></a>属性值

在 `Get` 过程中，返回值作为属性的值提供给调用表达式。

在 `Set` 过程中，新属性值将传递给语句的参数 `Set` 。 如果显式声明了某个参数，则必须使用与属性相同的数据类型声明该参数。 如果不声明参数，编译器将使用隐式参数 `Value` 来表示要分配给属性的新值。

## <a name="calling-syntax"></a>调用语法

通过引用属性，可以隐式调用属性过程。 使用属性的名称的方式与使用变量名称相同，只是必须为所有非可选参数提供值，并且必须将参数列表括在括号中。 如果未提供任何参数，则可以选择省略括号。

隐式调用过程的语法如下所示 `Set` ：

```vb
propertyname[(argumentlist)] = expression
```

隐式调用过程的语法如下所示 `Get` ：

```vb
lvalue = propertyname[(argumentlist)]
Do While (propertyname[(argumentlist)] > expression)
```

### <a name="illustration-of-declaration-and-call"></a>声明和调用的插图

以下属性将全名存储为两个构成名称：名字和姓氏。 在调用代码读取时 `fullName` ，此 `Get` 过程将组合这两个构成名称并返回全名。 当调用代码分配新的全名时，该 `Set` 过程会尝试将其分成两个构成名称。 如果找不到空间，则会将其存储为名字。

[!code-vb[VbVbcnProcedures#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#8)]

下面的示例演示对的属性过程的典型调用 `fullName` ：

[!code-vb[VbVbcnProcedures#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#9)]

## <a name="see-also"></a>另请参阅

- [过程](index.md)
- [Function 过程](function-procedures.md)
- [运算符过程](operator-procedures.md)
- [过程形参和实参](procedure-parameters-and-arguments.md)
- [Visual Basic 中属性和变量的差异](differences-between-properties-and-variables.md)
- [如何：创建属性](how-to-create-a-property.md)
- [如何：调用 Property 过程](how-to-call-a-property-procedure.md)
- [如何：在 Visual Basic 中声明和调用默认属性](how-to-declare-and-call-a-default-property.md)
- [如何：在属性中放置值](how-to-put-a-value-in-a-property.md)
- [如何：从属性获取值](how-to-get-a-value-from-a-property.md)
