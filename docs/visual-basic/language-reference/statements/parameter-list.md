---
title: 参数列表
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic code, procedures
- parameters [Visual Basic], Visual Basic
- parameters [Visual Basic], lists
- parameter lists [Visual Basic]
- Visual Basic code, parameter lists
- arguments [Visual Basic], Visual Basic
- procedures [Visual Basic], parameter lists
ms.assetid: 5d737319-0c34-4df9-a23d-188fc840becd
ms.openlocfilehash: 706fc2414806db5608cce410bf4156839ec2d83e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404312"
---
# <a name="parameter-list-visual-basic"></a>参数列表 (Visual Basic)

指定在调用过程时过程需要的参数。 多个参数之间用逗号分隔。 下面是一个参数的语法。

## <a name="syntax"></a>语法

```vb
[ <attributelist> ] [ Optional ] [{ ByVal | ByRef }] [ ParamArray ]
parametername[( )] [ As parametertype ] [ = defaultvalue ]
```

## <a name="parts"></a>组成部分

`attributelist`  
可选。 应用于此参数的特性列表。 必须将[特性列表](attribute-list.md)括在尖括号（" `<` " 和 " `>` "）中。

`Optional`  
可选。 指定在调用过程时不需要此参数。

`ByVal`  
可选。 指定过程不能替换或重新分配调用代码中的相应参数的基础变量元素。

`ByRef`  
可选。 指定过程可以采用与调用代码本身相同的方式来修改调用代码中的基础变量元素。

`ParamArray`  
可选。 指定参数列表中的最后一个参数是指定数据类型的元素的可选数组。 这样，调用代码就可以将任意数量的参数传递给该过程。

`parametername`  
必需。 表示参数的局部变量的名称。

`parametertype`  
如果 `Option Strict` 为，则为必需 `On` 。 表示参数的局部变量的数据类型。

`defaultvalue`  
参数需要 `Optional` 。 计算结果为参数的数据类型的任何常量或常量表达式。 如果类型为 `Object` ，或类、接口、数组或结构，则默认值只能是 `Nothing` 。

## <a name="remarks"></a>备注

参数括在括号内并用逗号分隔。 参数可以使用任何数据类型进行声明。 如果不指定 `parametertype` ，则默认为 `Object` 。

当调用代码调用过程时，它会将*参数*传递给每个必需的参数。 有关详细信息，请参阅[参数和参数之间的差异](../../programming-guide/language-features/procedures/differences-between-parameters-and-arguments.md)。

调用代码传递给每个参数的参数是指向调用代码中的基础元素的指针。 如果此元素不是*变量*（常量、文本、枚举或表达式），则任何代码都不能对其进行更改。 如果它是*变量*元素（已声明的变量、字段、属性、数组元素或结构元素），则调用代码可以更改它。 有关详细信息，请参阅可[修改和不可修改参数之间的差异](../../programming-guide/language-features/procedures/differences-between-modifiable-and-nonmodifiable-arguments.md)。

如果传递了 variable 元素 `ByRef` ，则该过程也可以更改它。 有关详细信息，请参阅[按值传递参数和通过引用传递参数之间的差异](../../programming-guide/language-features/procedures/differences-between-passing-an-argument-by-value-and-by-reference.md)。

## <a name="rules"></a>规则

- **括号.** 如果指定参数列表，则必须将其括在括号中。 如果没有参数，仍可使用括空列表的括号。 这可以通过阐明元素是一个过程来提高代码的可读性。

- **可选参数。** 如果对参数使用 `Optional` 修饰符，则列表中的所有后续参数也必须是可选的，并且可以使用修饰符进行声明 `Optional` 。

     每个可选参数声明都必须提供 `defaultvalue` 子句。

     有关详细信息，请参阅[可选参数](../../programming-guide/language-features/procedures/optional-parameters.md)。

- **参数数组。** 必须 `ByVal` 为 `ParamArray` 参数指定。

     `Optional` `ParamArray` 在同一参数列表中不能同时使用和。

     有关详细信息，请参阅[参数数组](../../programming-guide/language-features/procedures/parameter-arrays.md)。

- **传递机制。** 每个参数的默认机制为 `ByVal` ，这意味着过程无法更改基础变量元素。 但是，如果该元素是引用类型，则该过程可以修改基础对象的内容或成员，即使它无法替换或重新分配对象本身。

- **参数名称。** 如果参数的数据类型为数组，请在后面 `parametername` 紧跟括号。 有关参数名称的详细信息，请参阅已[声明的元素名称](../../programming-guide/language-features/declared-elements/declared-element-names.md)。

## <a name="example"></a>示例

下面的示例演示了一个 `Function` 用于定义两个参数的过程。

[!code-vb[VbVbalrStatements#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#2)]

## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.InteropServices.DllImportAttribute>
- [Function 语句](function-statement.md)
- [Sub 语句](sub-statement.md)
- [Declare Statement](declare-statement.md)
- [Structure 语句](structure-statement.md)
- [Option Strict 语句](option-strict-statement.md)
- [属性概述](../../programming-guide/concepts/attributes/index.md)
- [如何：在代码中拆分和合并语句](../../programming-guide/program-structure/how-to-break-and-combine-statements-in-code.md)
