---
title: 语言参考
description: 从这个对语言标记、概念、类型、表达式和编译器支持的构造主题的引用中查找 F# 语言功能信息。
ms.date: 08/13/2020
ms.openlocfilehash: 02711489c214c1fcdb2da80f30bff63d67769c17
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88558148"
---
# <a name="f-language-reference"></a>F# 语言参考

本部分是 F# 语言的一个参考，F# 语言是面向 .NET 的多范例编程语言。 F# 语言支持函数式、命令式、面向对象的编程模式。

## <a name="f-core-library-api-reference"></a>F# 核心库 API 参考

[F# 核心库 (FSharp.Core) API 参考](https://fsharp.github.io/fsharp-core-docs/)是所有 F# 核心库命名空间、模块、类型和函数的参考。

## <a name="f-tokens"></a>F# 标记

下表显示参考文章，这些文章提供在 F# 中用作标记的关键字、符号和文本表。

|Title|描述|
|-----|-----------|
|[关键字参考](keyword-reference.md)|包含指向所有 F# 语言关键字的相关信息的链接。|
|[符号和运算符参考](./symbol-and-operator-reference/index.md)|包括 F# 语言中使用的符号和运算符表。|
|[文字](literals.md)|介绍 F# 中文本值的语法以及如何指定 F# 文本的类型信息。|

## <a name="f-language-concepts"></a>F# 语言概念

下表显示介绍语言概念的可用参考主题。

|Title|描述|
|-----|-----------|
|[函数](./functions/index.md)|函数是任何编程语言中程序执行的基本单元。 和其他语言一样，F# 函数有一个名称，可以有形参并采用实参，且具有一个主体。 F# 还支持函数编程构造，例如将函数视为值、在表达式中使用未命名的函数、组合函数以形成新的函数、扩充函数以及通过部分应用函数参数来隐式定义函数。|
|[F# 类型](fsharp-types.md)|介绍 F# 中使用的类型以及如何命名和描述 F# 类型。|
|[类型推断](type-inference.md)|介绍 F# 编译器如何推断值、变量、参数和返回值的类型。|
|[自动泛化](./generics/automatic-generalization.md)|介绍 F# 中的泛型构造。|
|[继承](inheritance.md)|介绍在面向对象的编程中，用来建模“is-a”关系或子类型化的接口。|
|[成员](./members/index.md)|介绍 F# 对象类型的成员。|
|[形参和实参](Parameters-and-Arguments.md)|介绍对定义形参并将实参传递给函数、方法和属性的语言支持。 它包含如何通过引用进行传递的信息。|
|[运算符重载](operator-overloading.md)|介绍如何在类或记录类型中，以及在全局级别下重载算术运算符。|
|[强制转换和转换](casting-and-conversions.md)|介绍对 F# 中类型转换的支持。|
|[访问控制](access-control.md)|介绍 F# 中的访问控制。 访问控制意指声明哪些客户端能够使用特定程序元素，如类型、方法、函数等。|
|[模式匹配](pattern-matching.md)|提供模式的说明，其中模式是用于转换输入数据且在整个 F# 语言中使用的规则。 可将数据与模式进行比较、将数据分解为各个构成部分，或通过各种方式从数据中提取信息。|
|[活动模式](active-patterns.md)|介绍活动模式。 活动模式允许定义细分输入数据的已命名分区。 可使用活动模式以自定义方式为每个分区分解数据。|
|[断言](assertions.md)|介绍 `assert` 表达式，它是一种可用于测试表达式的调试功能。 在调试模式中遇到故障时，断言将生成一个系统错误对话框。|
|[异常处理](./exception-handling/index.md)|包含 F# 语言中的异常处理支持相关信息。|
|[attributes](attributes.md)|介绍启用要应用于编程构造的元数据的特性。|
|[资源管理：`use` 关键字](resource-management-the-use-keyword.md)|描述关键字 `use` 和 `using`，它们可以控制资源的初始化和发布|
|[namespaces](namespaces.md)|介绍 F# 中的命名空间支持。 命名空间通过使你能够将名称附加一组程序元素，实现将代码整理到相关的功能区域。|
|[模块](modules.md)|描述模块。 F# 模块是 F# 程序中的一组 F# 代码，例如值、类型和函数值。 对模块中的代码进行分组有助于将相关代码放在一起，并有助于避免程序中的名称冲突。|
|[导入声明：`open` 关键字](import-declarations-the-open-keyword.md)|描述 `open` 的工作方式。 导入声明指定模块或命名空间，无需使用完全限定的名称即可引用其中的元素。|
|[签名](signature-files.md)|介绍签名和签名文件。 签名文件包含有关一组 F# 程序元素（如类型、命名空间和模块）的公共签名的信息。 它可用于指定这些程序元素的可访问性。|
|[XML 文档](xml-documentation.md)|介绍对生成文档文件的 XML 文档注释（也称为三斜杠注释）的支持。 与其他 .NET 语言一样，可采用 F# 从代码注释生成文档。|
|[详细语法](verbose-syntax.md)|介绍未启用轻量语法时 F# 构造的语法。 详细语法由代码文件顶部的 `#light "off"` 指令指示。|
|[纯文本格式](plaintext-formatting.md)|了解如何在 F# 应用程序和脚本中使用 sprintf 和其他纯文本格式。|

## <a name="f-types"></a>F# 类型

下表提供的参考主题介绍 F# 语言支持的类型。

|Title|描述|
|-----|-----------|
|[值](./values/index.md)|介绍值，它是具有特定类型的不可变数量；值可以是整数或浮点数、字符或文本、列表、序列、数组、元组、可区分联合、记录、类类型或函数值。|
|[基本类型](basic-types.md)|介绍在 F# 语言中使用的主要基本类型。 它还提供每种类型的相应 .NET 类型和最小和最大值。|
|[unit 类型](unit-type.md)|介绍 `unit` 类型，它指示缺少某特定值；`unit` 类型只有一个值，不存在任何其他值或者不需要其他值时，该值作为一个占位符。|
|[字符串](strings.md)|介绍 F# 中的字符串。 `string` 类型将不可变的文本表示为 Unicode 字符的序列。 `string` 是 .NET Framework 中 `System.String` 的别名。|
|[元组](tuples.md)|介绍元组，它是未命名但有序的值的组合，值的类型可能不同。|
|[F# 集合类型](fsharp-collection-types.md)|F# 函数集合类型的概述，包括数组、列表、序列 (seq)、映射和集的类型。|
|[列表](lists.md)|介绍列表。 F# 中的列表是所有相同类型元素的有序、不可变系列。|
|[选项](options.md)|介绍选项类型。 某值可能存在或可能不存在时，将使用 F# 中的选项。 选项具有基础类型，可能包含该类型的值或者可能不包含值。|
|[序列](sequences.md)|介绍序列。 序列是同类型元素的逻辑系列。 各序列元素仅在必要时进行计算，因此表示形式可能比文本元素计数指示的小。|
|[数组](arrays.md)|介绍数组。 数组是连续数据元素的大小固定、从零开始的可变序列，其中数据元素都为同一类型。|
|[记录](records.md)|介绍记录。 记录表示已命名值的简单聚合，可选择包含成员。|
|[可区分联合](discriminated-unions.md)|介绍可区分联合，它提供对下面这种值的支持：该值可能是多种已命名用例中的一个，且每个用例可能具有不同的值和类型。|
|[枚举](enumerations.md)|介绍枚举，枚举是具有一组已定义的已命名值的类型。 可以使用它们来代替文本，使代码更具可读性且更易维护。|
|[引用单元格](reference-cells.md)|介绍引用单元格，它们是一些存储位置，可以利用它们来创建具有引用语义的可变变量。|
|[类型缩写](type-abbreviations.md)|介绍类型缩写，它们是类型的替代名称。|
|[类](classes.md)|介绍类，类是表示可具有属性、方法和事件的对象的类型。|
|[结构](structures.md)|介绍结构，结构是一种紧凑对象类型，对于数据量较少且行为简单的类型来说，结构可能比类更有效。|
|[接口](interfaces.md)|介绍接口，接口指定其他类实现的相关成员集。|
|[抽象类](abstract-classes.md)|介绍抽象类，抽象类是一些留有部分或全部成员未实现的类，以便可以由派生类来提供实现。|
|[类型扩展](type-extensions.md)|介绍类型扩展，允许通过类型扩展将新成员添加到之前已定义的对象类型。|
|[可变类型](flexible-types.md)|介绍可变类型。 可变类型批注指示参数、变量或值具有与所指定类型兼容的类型，其兼容性由面向对象的类或接口层次结构中的位置确定。|
|[委托](delegates.md)|介绍委托，它将函数调用表示为对象。|
|[度量单位](units-of-measure.md)|介绍度量单位。 F# 中的浮点值可以具有关联的度量单位，这些度量单位通常用于指示长度、体积、质量等等。|
|[类型提供程序](../tutorials/type-providers/index.md)|介绍类型提供程序，并提供使用内置类型提供程序访问数据库和 Web 服务的演练链接。|

## <a name="f-expressions"></a>F# 表达式

下表列出了介绍 F# 表达式的主题。

|Title|描述|
|-----|-----------|
|[条件表达式：`if...then...else`](conditional-expressions-if-then-else.md)|介绍 `if...then...else` 表达式，该表达式不仅可运行代码的不同分支，还可计算为不同的值，具体取决于给定的布尔表达式。|
|[Match 表达式](match-expressions.md)|介绍 `match` 表达式，该表达式提供基于表达式与一组模式的比较结果的分支控制。|
|[循环：`for...to` 表达式](loops-for-to-expression.md)|介绍 `for...to` 表达式，该表达式用于在一系列循环变量值中进行循环访问。|
|[循环：`for...in` 表达式](loops-for-in-expression.md)|介绍 `for...in` 表达式，该表达式是一种循环构造，用于循环访问可枚举集合（例如，范围表达式、序列、列表、数组或其他支持枚举的构造）中的模式匹配项。|
|[循环：`while...do` 表达式](loops-while-do-expression.md)|介绍 `while...do` 表达式，该表达式用于在指定的测试条件为 true 时执行迭代操作（循环）。|
|[对象表达式](object-expressions.md)|介绍对象表达式，这些表达式可用于创建动态创建的匿名对象类型的新实例，该对象类型基于现有基类型、接口或接口集。|
|[延迟表达式](lazy-expressions.md)|描述延迟表达式，这些表达式是指不会立即开始而要等到实际需要结果时才进行的计算。|
|[计算表达式](computation-expressions.md)|介绍 F# 中的计算表达式，它提供一种用于编写计算的方便语法，可以通过使用控制流构造和绑定来对这些计算进行排列和组合。 计算表达式可用于为 Monad提供一种方便语法，Monad 是一种可用于管理函数程序中数据、控制以及副作用的函数编程功能。 异步工作流是计算表达式的一种，它提供对异步计算和并行计算的支持。 有关详细信息，请参阅[异步工作流](asynchronous-workflows.md)。|
|[异步工作流](asynchronous-workflows.md)|介绍异步工作流，它是一种语言功能，可通过它用近似于编写同步代码的方式来编写异步代码。|
|[代码引用](code-quotations.md)|介绍代码引用，它是一种可让你以编程方式生成和处理 F# 代码表达式的语言功能。|
|[查询表达式](query-expressions.md)|介绍查询表达式，它是一种语言功能，为 F# 实现 LINQ 并使你可以对数据源或可枚举集合编写查询。|

## <a name="compiler-supported-constructs"></a>编译器支持的构造

下表列出了一些主题，这些主题介绍编译器支持的特殊构造。

|主题|描述|
|-----|-----------|
|[编译器选项](compiler-options.md)|介绍 F# 编译器的命令行选项。|
|[编译器指令](compiler-directives.md)|介绍处理器指令和编译器指令。|
|[源代码行标识符、文件标识符和路径标识符](source-line-file-path-identifiers.md)|介绍标识符 `__LINE__`、`__SOURCE_DIRECTORY__` 和 `__SOURCE_FILE__`，这些标识符是内置值，可在代码中使用这些值来访问源行号、目录和文件名。|
