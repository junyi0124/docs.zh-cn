---
title: 语句
ms.date: 07/20/2015
helpviewer_keywords:
- variables [Visual Basic], declaring
- colons (:) [Visual Basic]
- constants [Visual Basic], defining
- underlines
- constants [Visual Basic], statements
- blue underline [Visual Basic]
- procedures [Visual Basic], statements
- variables [Visual Basic], assigning
- line breaks [Visual Basic], in code
- executable statements [Visual Basic]
- variables [Visual Basic], defining
- statements [Visual Basic], about statements
ms.assetid: fcfdee1a-82b7-4846-98f7-9ca3f5160089
ms.openlocfilehash: 09fe53f4bc2b6d025b762c6595c5337263456bae
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401974"
---
# <a name="statements-in-visual-basic"></a>语句 (Visual Basic)

Visual Basic 中的语句是一个完整的说明。 它可以包含关键字、运算符、变量、常量和表达式。 每个语句属于以下类别之一：

- **声明语句**，用于命名变量、常量或过程，还可以指定数据类型。

- 启动操作的**可执行语句**。 这些语句可以调用方法或函数，并且它们可以循环或分支到代码块。 可执行语句包括**赋值语句，赋值语句**将值或表达式分配给变量或常数。

本主题介绍每个类别。 此外，本主题还介绍如何在单个行上组合多个语句以及如何在多行上继续执行语句。

## <a name="declaration-statements"></a>声明语句

使用声明语句来命名和定义过程、变量、属性、数组和常量。 在声明编程元素时，还可以定义其数据类型、访问级别和作用域。 有关详细信息，请参阅[声明的元素特征](./declared-elements/declared-element-characteristics.md)。

下面的示例包含三个声明。

[!code-vb[VbVbalrStatements#80](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#80)]

第一个声明为 `Sub` 语句。 它与它的匹配 `End Sub` 语句一起声明名为的过程 `applyFormat` 。 它还指定 `applyFormat` 为 `Public` ，这意味着可引用它的任何代码都可以调用它。

第二个声明是 `Const` 语句，该语句声明常量 `limit` ，并指定 `Integer` 数据类型和值33。

第三个声明是 `Dim` 语句，该语句声明变量 `thisWidget` 。 数据类型是一个特定对象，即从类创建的对象 `Widget` 。 可以将变量声明为任意基本数据类型，或者声明为在所使用的应用程序中公开的任何对象类型。

### <a name="initial-values"></a>初始值

当包含声明语句的代码运行时，Visual Basic 保留已声明的元素所需的内存。 如果元素包含值，Visual Basic 会将其初始化为其数据类型的默认值。 有关详细信息，请参阅[Dim 语句](../../language-reference/statements/dim-statement.md)中的 "行为"。

可以将初始值分配给变量作为其声明的一部分，如下面的示例所示。

[!code-vb[VbVbalrStatements#81](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#81)]

如果变量是一个对象变量，则可以在使用[New 运算符](../../language-reference/operators/new-operator.md)关键字声明它时显式创建该类的实例，如下面的示例所示。

[!code-vb[VbVbalrStatements#82](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#82)]

请注意，在声明语句中指定的初始值不会分配给变量，直到执行到达其声明语句。 在此之前，变量包含其数据类型的默认值。

## <a name="executable-statements"></a>可执行语句

可执行语句执行操作。 它可以调用过程、分支到代码中的其他位置、循环几个语句或计算表达式。 赋值语句是可执行语句的特殊情况。

下面的示例使用 `If...Then...Else` 控件结构基于变量的值运行不同的代码块。 在每个代码块中， `For...Next` 循环将运行指定的次数。

[!code-vb[VbVbalrStatements#83](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#83)]

`If`前面的示例中的语句检查参数的值 `clockwise` 。 如果值为 `True` ，则它将调用的 `spinClockwise` 方法 `aWidget` 。 如果值为 `False` ，则它将调用的 `spinCounterClockwise` 方法 `aWidget` 。 `If...Then...Else`控件结构以结尾 `End If` 。

`For...Next`每个块中的循环将调用相应的方法，这几次等于参数的值 `revolutions` 。

## <a name="assignment-statements"></a>赋值语句

赋值语句执行赋值运算，其中包含赋值运算符（）右侧的值 `=` 并将其存储在左侧的元素中，如下面的示例中所示。

[!code-vb[VbVbalrStatements#73](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#73)]

在前面的示例中，赋值语句将文本值42存储在变量中 `v` 。

### <a name="eligible-programming-elements"></a>符合条件的编程元素

赋值运算符左侧的编程元素必须能够接受和存储值。 这意味着它必须是不是[ReadOnly](../../language-reference/modifiers/readonly.md)的变量或属性，或者必须是数组元素。 在赋值语句的上下文中，此类元素有时*称为左值，表示*"左值"。

赋值运算符右侧的值是由表达式生成的，表达式可以包含文本、常量、变量、属性、数组元素、其他表达式或函数调用的任意组合。 下面的示例对此进行了演示。

[!code-vb[VbVbalrStatements#74](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#74)]

前面的示例将变量中保存的值添加 `y` 到变量中保存的值 `z` ，然后添加调用函数返回的值 `findResult` 。 然后，此表达式的总值存储在变量中 `x` 。

### <a name="data-types-in-assignment-statements"></a>赋值语句中的数据类型

除了数值之外，赋值运算符还可以赋值 `String` ，如下面的示例所示。

[!code-vb[VbVbalrStatements#75](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#75)]

你还可以 `Boolean` 使用 `Boolean` 文字或表达式来分配值 `Boolean` ，如下面的示例所示。

[!code-vb[VbVbalrStatements#76](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#76)]

同样，可以将适当的值分配给 `Char` 、 `Date` 或数据类型的编程元素 `Object` 。 还可以将对象实例分配给声明为从中创建该实例的类的元素。

### <a name="compound-assignment-statements"></a>复合赋值语句

*复合赋值语句*首先对表达式执行操作，然后将其分配给编程元素。 下面的示例说明其中一个运算符， `+=` 它将运算符左侧的变量值加在右侧的表达式的值。

[!code-vb[VbVbalrStatements#77](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#77)]

前面的示例将1添加到的值 `n` ，然后将该新值存储在中 `n` 。 它是以下语句的速记等效项：

[!code-vb[VbVbalrStatements#78](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#78)]

可以使用此类型的运算符执行各种复合赋值运算。 有关这些运算符的列表以及它们的详细信息，请参阅[赋值运算符](../../language-reference/operators/assignment-operators.md)。

串联赋值运算符（ `&=` ）可用于向现有字符串的末尾添加字符串，如下面的示例所示。

[!code-vb[VbVbalrStatements#79](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#79)]

### <a name="type-conversions-in-assignment-statements"></a>赋值语句中的类型转换

分配给变量、属性或数组元素的值必须是适合于该目标元素的数据类型。 通常，应尝试生成与目标元素的数据类型相同的值。 但是，在赋值期间，某些类型可以转换为其他类型。

有关数据类型之间的转换的信息，请参阅[Visual Basic 中的类型转换](./data-types/type-conversions.md)。 简言之，Visual Basic 会自动将给定类型的值转换为它扩展到的任何其他类型。 *扩大转换*是指在运行时始终成功，不会丢失任何数据。 例如，Visual Basic `Integer` 根据需要将值转换为 `Double` ，因为 `Integer` 扩大到了 `Double` 。 有关详细信息，请参阅 [Widening and Narrowing Conversions](./data-types/widening-and-narrowing-conversions.md)。

*收缩转换*（不扩大的转换）在运行时或数据丢失时具有失败的风险。 您可以通过使用类型转换函数显式执行收缩转换，也可以通过设置来指示编译器隐式执行所有转换 `Option Strict Off` 。 有关详细信息，请参阅[隐式和显式转换](./data-types/implicit-and-explicit-conversions.md)。

## <a name="putting-multiple-statements-on-one-line"></a>在一行上放置多个语句

可以将多个语句用冒号（）字符分隔在一行上 `:` 。 下面的示例对此进行了演示。

[!code-vb[VbVbalrStatements#70](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#70)]

尽管偶尔很方便，但这种形式的语法使您的代码更难以阅读和维护。 因此，建议您将一条语句保存到一行。

## <a name="continuing-a-statement-over-multiple-lines"></a>在多行上继续执行语句

语句通常适用于一行，但当它太长时，可以使用行继续符（包含一个空格，后跟一个下划线字符（ `_` ），后跟回车符）来继续执行下一行。 在下面的示例中， `MsgBox` 可执行语句继续经过两行。

[!code-vb[VbVbalrStatements#71](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#71)]

### <a name="implicit-line-continuation"></a>隐式行继续

在许多情况下，可以在下一连续行继续执行语句，而无需使用下划线字符（ `_` ）。 以下语法元素隐式地在下一行代码中继续执行语句。

- 在逗号（ `,` ）之后。 例如：

   [!code-vb[VbVbalrLineContinuation#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#1)]

- 在左括号（ `(` ）或右括号（ `)` ）之前。 例如：

   [!code-vb[VbVbalrLineContinuation#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#2)]

- 在左大括号（ `{` ）之后或右大括号（）之前 `}` 。 例如：

    [!code-vb[VbVbalrLineContinuation#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#3)]

    有关详细信息，请参阅[对象初始值设定项：命名类型和匿名类型](./objects-and-classes/object-initializers-named-and-anonymous-types.md)或[集合初始值设定项](./collection-initializers/index.md)。

- 在 `<%=` XML 文本中打开的嵌入式表达式（）之后或在嵌入的表达式（）结束之前 `%>` 。 例如：

   [!code-vb[VbVbalrLineContinuation#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#4)]

   有关详细信息，请参阅[XML 中的嵌入式表达式](./xml/embedded-expressions-in-xml.md)。

- 串联运算符（）之后 `&` 。 例如：

   [!code-vb[VbVbcnConventions#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnConventions/vb/Class1.vb#9)]

   有关详细信息，请参阅[按功能列出的运算符](../../language-reference/operators/operators-listed-by-functionality.md)。

- 赋值运算符（，，，，，，，，，， `=` `&=` `:=` ）之后 `+=` `-=` `*=` `/=` `\=` `^=` `<<=` `>>=` 。 例如：

   [!code-vb[VbVbalrLineContinuation#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#5)]

   有关详细信息，请参阅[按功能列出的运算符](../../language-reference/operators/operators-listed-by-functionality.md)。

- 二元运算符（，，，，，，，，，，，，，，，，，，， `+` `-` `/` `*` `Mod` `<>` `<` `>` `<=` `>=` ）在 `^` `>>` `<<` `And` `AndAlso` `Or` `OrElse` `Like` `Xor` 表达式中。 例如：

   [!code-vb[VbVbalrLineContinuation#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#7)]

   有关详细信息，请参阅[按功能列出的运算符](../../language-reference/operators/operators-listed-by-functionality.md)。

- 在 `Is` 和 `IsNot` 运算符之后。 例如：

   [!code-vb[VbVbalrLineContinuation#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#8)]

   有关详细信息，请参阅[按功能列出的运算符](../../language-reference/operators/operators-listed-by-functionality.md)。

- 在成员限定符字符（ `.` ）之后、成员名称之前。 例如：

   [!code-vb[VbVbalrLineContinuation#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#5)]

   但是，在 `_` 使用 `With` 语句或在类型的初始化列表中提供值时，必须在成员限定符后面包含行继续符（）。 `=`使用 `With` 语句或对象初始化列表时，请考虑在赋值运算符后中断行（例如）。 例如：

   [!code-vb[VbVbalrLineContinuation#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#14)]

   有关详细信息，请参阅[With .。。End With 语句](../../language-reference/statements/with-end-with-statement.md)或[对象初始值设定项：命名类型和匿名类型](./objects-and-classes/object-initializers-named-and-anonymous-types.md)。

- 在 XML 轴属性限定符（ `.` 或 `.@` 或）之后 `...` 。 但是，在使用关键字时，如果指定成员限定符，则必须包含行继续符（ `_` ） `With` 。 例如：

   [!code-vb[VbVbalrLineContinuation#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#9)]

   有关详细信息，请参阅[XML 轴属性](../../language-reference/xml-axis/index.md)。

- 在指定特性时，小于号（<）或大于号（ `>` ）之前。 当指定特性时，还应在大于号（ `>` ）之后。 但是，在 `_` 指定程序集级别或模块级特性时，必须包含行继续符（）。 例如：

   [!code-vb[VbVbalrLineContinuation#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#10)]

   有关详细信息，请参阅[属性概述](../concepts/attributes/index.md)。

- 查询运算符前后（、、、、、、、、、、、、、、、、、 `Aggregate` `Distinct` `From` `Group By` `Group Join` `Join` `Let` `Order By` `Select` `Skip` `Skip While` `Take` `Take While` `Where` `In` `Into` `On` `Ascending` 和 `Descending` ）。 不能在由多个关键字（ `Order By` 、 `Group Join` 、 `Take While` 和）构成的查询运算符的关键字之间进行分隔 `Skip While` 。 例如：

   [!code-vb[VbVbalrLineContinuation#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#11)]

   有关详细信息，请参阅[查询](../../language-reference/queries/index.md)。

- 在 `In` 语句中的关键字之后 `For Each` 。 例如：

   [!code-vb[VbVbalrLineContinuation#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#12)]

   有关详细信息，请参阅[For Each .。。下一语句](../../language-reference/statements/for-each-next-statement.md)。

- 在 `From` 集合初始值设定项中的关键字之后。 例如：

   [!code-vb[VbVbalrLineContinuation#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrlinecontinuation/vb/module1.vb#13)]

   有关详细信息，请参阅[集合初始值设定项](collection-initializers/index.md)。

## <a name="adding-comments"></a>添加注释

源代码并非始终能一目了然，即使是编写它的程序员也是如此。 为帮助记录其代码，大多数程序员可以自由地利用嵌入的注释。 代码中的注释可以解释过程或特定的指令，以便以后阅读或使用它。 Visual Basic 将在编译过程中忽略注释，并且它们不会影响编译的代码。

注释行以撇号（ `'` ）或 `REM` 后跟空格开头。 它们可以添加到代码中的任何位置，但在字符串中除外。 若要将注释追加到语句，请在语句后插入撇号或 `REM` 后跟注释。 注释还可以在单独的行上。 下面的示例演示了这些可能。

[!code-vb[VbVbalrStatements#72](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#72)]

## <a name="checking-compilation-errors"></a>检查编译错误

如果键入代码行后，将显示带有蓝色波浪下划线的行（也可能会显示错误消息），语句中存在语法错误。 您必须找出语句出错的内容（通过查看任务列表，或将鼠标指针悬停在错误上并阅读错误消息），然后更正错误。 在代码中修复所有语法错误之前，程序将无法正确编译。

## <a name="related-sections"></a>相关章节

|术语|定义|
|---|---|
|[赋值运算符](../../language-reference/operators/assignment-operators.md)|提供指向介绍赋值运算符（如、和）的语言参考页的链接 `=` `*=` `&=` 。|
|[运算符和表达式](./operators-and-expressions/index.md)|演示如何将元素与运算符组合以生成新值。|
|[如何：在代码中拆分和合并语句](../program-structure/how-to-break-and-combine-statements-in-code.md)|说明如何将单个语句分解为多行，以及如何在同一行上放置多个语句。|
|[如何：标记语句](../program-structure/how-to-label-statements.md)|说明如何为代码行添加标签。|
