---
title: 关键字参考
description: '查找有关所有 F # 语言关键字的信息的链接。'
f1_keywords:
- new_FS
- use_FS
- end_FS
- lsl_FS
- exception_FS
- asr_FS
- if_FS
- internal_FS
- default_FS
- in_FS
- lsr_FS
- open_FS
- static_FS
- assert_FS
- match_FS
- land_FS
- with_FS
- inherit_FS
- mutable_FS
- downto_FS
- false_FS
- sig_FS
- and_FS
- true_FS
- namespace_FS
- public_FS
- lxor_FS
- val_FS
- void_FS
- downcast_FS
- function_FS
- while_FS
- for_FS
- class_FS
- done_FS
- to_FS
- module_FS
- let_FS
- delegate_FS
- abstract_FS
- then_FS
- when_FS
- lazy_FS
- try_FS
- inline_FS
- do_FS
- upcast_FS
- begin_FS
- base_FS
- fun_FS
- struct_FS
- as_FS
- extern_FS
- null_FS
- lor_FS
- return_FS
- mod_FS
- private_FS
- of_FS
- or_FS
- member_FS
- type_FS
- rec_FS
- elif_FS
- override_FS
- interface_FS
- yield_FS
- else_FS
- finally_FS
- global_FS
- select_FS
- use!_FS
- const_FS
dev_langs:
- FSharp
ms.date: 08/15/2020
ms.openlocfilehash: 15505c123dd0d6497fbc80c8fc9f0910018911ea
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88558096"
---
# <a name="keyword-reference"></a>关键字参考

本主题包含有关所有 F # 语言关键字的信息的链接。

## <a name="f-keyword-table"></a>F # 关键字表

下表按字母顺序显示了所有 F # 关键字，同时还显示了简短说明以及包含详细信息的相关主题的链接。

|关键字|链接|说明|
|-------|----|-----------|
|`abstract`|[成员](./members/index.md)<br /><br />[抽象类](abstract-classes.md)|指示一个方法，该方法在声明它的类型中没有实现，或者它是虚拟的且具有默认实现。|
|`and`|[`let` 绑定](./functions/let-bindings.md)<br /><br />[记录](records.md)<br /><br />[成员](./members/index.md)<br /><br />[约束](./generics/constraints.md)|在相互递归的绑定和记录中使用，在属性声明中使用，对泛型参数使用多个约束。|
|`as`|[类](classes.md)<br /><br />[模式匹配](Pattern-Matching.md)|用于为当前类对象指定对象名称。 还用于在模式匹配中为整个模式指定名称。|
|`assert`|[断言](assertions.md)|用于在调试过程中验证代码。|
|`base`|[类](classes.md)<br /><br />[继承](inheritance.md)|用作基类对象的名称。|
|`begin`|[详细语法](verbose-syntax.md)|在详细语法中，指示代码块的开头。|
|`class`|[类](classes.md)|在详细语法中，指示类定义的开头。|
|`default`|[成员](./members/index.md)|指示抽象方法的实现;与抽象方法声明一起使用，以创建虚拟方法。|
|`delegate`|[委托](delegates.md)|用于声明委托。|
|`do`|[do 绑定](./functions/do-bindings.md)<br /><br />[循环：`for...to` 表达式](loops-for-to-expression.md)<br /><br />[循环：`for...in` 表达式](loops-for-in-expression.md)<br /><br />[循环：`while...do` 表达式](loops-while-do-expression.md)|用于循环构造或执行命令性代码。|
|`done`|[详细语法](verbose-syntax.md)|在详细语法中，指示循环表达式中代码块的结束。|
|`downcast`|[强制转换和转换](casting-and-conversions.md)|用于转换为在继承链中较低的类型。|
|`downto`|[循环：`for...to` 表达式](loops-for-to-expression.md)|在 `for` 表达式中，在反向计数时使用。|
|`elif`|[条件表达式：`if...then...else`](conditional-expressions-if-then-else.md)|用于条件分支。 的缩写形式 `else if` 。|
|`else`|[条件表达式：`if...then...else`](conditional-expressions-if-then-else.md)|用于条件分支。|
|`end`|[结构](structures.md)<br /><br />[可区分联合](discriminated-unions.md)<br /><br />[记录](records.md)<br /><br />[类型扩展](type-extensions.md)<br /><br />[详细语法](verbose-syntax.md)|在类型定义和类型扩展中，指示成员定义节的结尾。<br /><br />在详细语法中，用于指定以关键字开头的代码块的结尾 `begin` 。|
|`exception`|[异常处理](./exception-handling/index.md)<br /><br />[异常类型](./exception-handling/exception-types.md)|用于声明异常类型。|
|`extern`|[外部函数](./functions/external-functions.md)|指示已声明的程序元素是在另一个二进制文件或程序集中定义的。|
|`false`|[基元类型](basic-types.md)|用作布尔文本。|
|`finally`|[异常：`try...finally` 表达式](./exception-handling/the-try-finally-expression.md)|与结合使用 `try` 可引入代码块，无论是否发生异常都将执行。|
|`fixed`|“固定”[](fixed.md)|用于在堆栈上 "固定" 指针，以防止它被垃圾回收。|
|`for`|[循环：`for...to` 表达式](loops-for-to-expression.md)<br /><br />[循环：for...in 表达式](loops-for-in-expression.md)|用于循环构造。|
|`fun`|[Lambda 表达式： `fun` 关键字](./functions/lambda-expressions-the-fun-keyword.md)|用于 lambda 表达式（也称为匿名函数）。|
|`function`|[Match 表达式](match-expressions.md)<br /><br />[Lambda 表达式：趣味关键字](./functions/lambda-expressions-the-fun-keyword.md)|用作关键字的更短替代 `fun` ，以及 `match` lambda 表达式中在单个参数上具有模式匹配的表达式。|
|`global`|[命名空间](namespaces.md)|用于引用顶层 .NET 命名空间。|
|`if`|[条件表达式：`if...then...else`](conditional-expressions-if-then-else.md)|用于条件性分支构造。|
|`in`|[循环：for...in 表达式](loops-for-in-expression.md)<br /><br />[详细语法](verbose-syntax.md)|用于序列表达式，并在详细语法中用于分隔来自绑定的表达式。|
|`inherit`|[继承](inheritance.md)|用于指定基类或基接口。|
|`inline`|[函数](./functions/index.md)<br /><br />[内联函数](./functions/inline-functions.md)|用于指示应直接集成到调用方的代码中的函数。|
|`interface`|[接口](interfaces.md)|用于声明和实现接口。|
|`internal`|[访问控制](access-control.md)|用于指定成员在程序集内可见，但在程序集外不可见。|
|`lazy`|[延迟表达式](lazy-expressions.md)|用于指定仅当需要结果时要执行的表达式。|
|`let`|[`let` 绑定](./functions/let-bindings.md)|用于将名称关联或绑定到值或函数。|
|`let!`|[异步工作流](asynchronous-workflows.md)<br /><br />[计算表达式](computation-expressions.md)|在异步工作流中用于将名称绑定到异步计算的结果，或者在其他计算表达式中用于将名称绑定到计算类型的结果。|
|`match`|[Match 表达式](match-expressions.md)|用于通过将值与模式进行比较来进行分支。|
|`match!`|[计算表达式](computation-expressions.md#match)|用于以内联方式对计算表达式和其结果的模式匹配进行内联调用。|
|`member`|[成员](./members/index.md)|用于声明对象类型中的属性或方法。|
|`module`|[模块](modules.md)|用于将名称与一组相关类型、值和函数关联，以从逻辑上将其与其他代码分开。|
|`mutable`|[let 绑定](./functions/let-bindings.md)|用于声明变量，即一个可以更改的值。|
|`namespace`|[命名空间](namespaces.md)|用于将名称与一组相关类型和模块关联起来，以逻辑方式将其与其他代码分开。|
|`new`|[构造函数](./members/constructors.md)<br /><br />[约束](./generics/constraints.md)|用于声明、定义或调用创建或可创建对象的构造函数。<br /><br />还在泛型参数约束中使用，以指示类型必须具有特定构造函数。|
|`not`|[符号和运算符参考](./symbol-and-operator-reference/index.md)<br /><br />[约束](./generics/constraints.md)|实际上不是关键字。 但是， `not struct` 组合用作泛型参数约束。|
|`null`|[空值](./values/null-values.md)<br /><br />[约束](./generics/constraints.md)|指示缺少对象。<br /><br />还在泛型参数约束中使用。|
|`of`|[可区分联合](discriminated-unions.md)<br /><br />[委托](delegates.md)<br /><br />[异常类型](./exception-handling/exception-types.md)|在可区分联合中用于指示值类别的类型，以及委托和异常声明中的类型。|
|`open`|[导入声明：`open` 关键字](import-declarations-the-open-keyword.md)|用于使命名空间或模块的内容在无限制的情况下可用。|
|`or`|[符号和运算符参考](./symbol-and-operator-reference/index.md)<br /><br />[约束](./generics/constraints.md)|作为布尔运算符与布尔条件一起使用 `or` 。 等效于 `||`。<br /><br />还在成员约束中使用。|
|`override`|[成员](./members/index.md)|用于实现与基版本不同的抽象或虚方法的版本。|
|`private`|[访问控制](access-control.md)|限制对同一类型或模块中的代码的成员的访问。|
|`public`|[访问控制](access-control.md)|允许从类型外部访问成员。|
|`rec`|[函数](./functions/index.md)|用于指示函数是递归的。|
|`return`|[异步工作流](Asynchronous-Workflows.md)<br /><br />[计算表达式](computation-expressions.md)|用于指示要作为计算表达式的结果提供的值。|
|`return!`|[计算表达式](computation-expressions.md)<br /><br />[异步工作流](asynchronous-workflows.md)|用于指示计算表达式，在计算该表达式时，将提供包含计算表达式的结果。|
|`select`|[查询表达式](query-expressions.md)|在查询表达式中用于指定要提取的字段或列。 请注意，这是一个上下文关键字，这意味着它实际上不是保留字，只在适当的上下文中充当关键字。|
|`static`|[成员](./members/index.md)|用于指示可以在没有类型实例的情况下调用的方法或属性，或在类型的所有实例之间共享的值成员。|
|`struct`|[结构](structures.md)<br /><br /> [元组](tuples.md)<br/><br/>[约束](./generics/constraints.md)|用于声明结构类型。<br /><br/>用于指定结构元组。<br/><br />还在泛型参数约束中使用。<br /><br />用于模块定义中的 OCaml 兼容性。|
|`then`|[条件表达式：`if...then...else`](conditional-expressions-if-then-else.md)<br /><br />[构造函数](./members/constructors.md)|用于条件表达式。<br /><br />还用于在对象构造后执行副作用。|
|`to`|[循环：`for...to` 表达式](loops-for-to-expression.md)|用于 `for` 循环以指示范围。|
|`true`|[基元类型](basic-types.md)|用作布尔文本。|
|`try`|[异常：try...with 表达式](./exception-handling/the-try-with-expression.md)<br /><br />[异常：try...finally 表达式](./exception-handling/the-try-finally-expression.md)|用于引入可能生成异常的代码块。 与或一起 `with` 使用 `finally` 。|
|`type`|[F# 类型](fsharp-types.md)<br /><br />[类](classes.md)<br /><br />[记录](records.md)<br /><br />[结构](structures.md)<br /><br />[枚举](enumerations.md)<br /><br />[可区分联合](discriminated-unions.md)<br /><br />[类型缩写](type-abbreviations.md)<br /><br />[度量单位](units-of-measure.md)|用于声明类、记录、结构、可区分联合、枚举类型、度量单位或类型缩写。|
|`upcast`|[强制转换和转换](casting-and-conversions.md)|用于转换为继承链中更高的类型。|
|`use`|[资源管理：`use` 关键字](resource-management-the-use-keyword.md)|`let`对于需要 `Dispose` 调用以释放资源的值，使用而不是。|
|`use!`|[计算表达式](computation-expressions.md)<br /><br />[异步工作流](asynchronous-workflows.md)|`let!`在异步工作流中使用，而不是在需要 `Dispose` 调用以释放资源的值的其他计算表达式中使用。|
|`val`|[显式字段： `val` 关键字](./members/explicit-fields-the-val-keyword.md)<br /><br />[签名](signature-files.md)<br /><br />[成员](./members/index.md)|在有限的情况下，在签名中用于指示值或声明成员的类型。|
|`void`|[基元类型](basic-types.md)|指示 .NET `void` 类型。 与其他 .NET 语言互操作时使用。|
|`when`|[约束](./generics/constraints.md)|用于 (在模式匹配 *时保护*) 并为泛型类型参数引入约束子句时的布尔条件。|
|`while`|[循环：`while...do` 表达式](loops-while-do-expression.md)|引入循环构造。|
|`with`|[Match 表达式](match-expressions.md)<br /><br />[对象表达式](object-expressions.md)<br /><br />[复制和更新记录表达式](copy-and-update-record-expressions.md)<br /><br />[类型扩展](type-extensions.md)<br /><br />[异常：`try...with` 表达式](./exception-handling/the-try-with-expression.md)|`match`在模式匹配表达式中与关键字一起使用。 还在对象表达式、记录复制表达式和类型扩展中用于引入成员定义，并用于引入异常处理程序。|
|`yield`|[列表](lists.md)、 [数组](arrays.md)、 [序列](sequences.md)|在列表、数组或序列表达式中用于生成序列的值。 通常可以省略，因为在大多数情况下它是隐式的。|
|`yield!`|[计算表达式](computation-expressions.md)<br /><br />[异步工作流](asynchronous-workflows.md)|在计算表达式中用于将给定计算表达式的结果追加到包含计算表达式的结果集合。|
|`const`|[类型提供程序](../tutorials/type-providers/index.md)| 类型提供程序允许使用 `const` 作为关键字来指定常量文本作为类型参数。|

以下令牌是在 F # 中保留的，因为它们是 OCaml 语言的关键字：

- `asr`
- `land`
- `lor`
- `lsl`
- `lsr`
- `lxor`
- `mod`
- `sig`

如果使用 `--mlcompatibility` 编译器选项，则可以使用上述关键字作为标识符。

以下令牌作为关键字保留，以供将来扩展 F # 语言：

- `atomic`
- `break`
- `checked`
- `component`
- `const`
- `constraint`
- `constructor`
- `continue`
- `eager`
- `event`
- `external`
- `functor`
- `include`
- `method`
- `mixin`
- `object`
- `parallel`
- `process`
- `protected`
- `pure`
- `sealed`
- `tailcall`
- `trait`
- `virtual`
- `volatile`

## <a name="see-also"></a>另请参阅

- [F# 语言参考](index.md)
- [符号和运算符参考](./symbol-and-operator-reference/index.md)
- [编译器选项](compiler-options.md)
