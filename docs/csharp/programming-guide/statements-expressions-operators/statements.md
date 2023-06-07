---
title: 语句 - C# 编程指南
description: 了解 C# 编程中的语句。 查看语句类型的列表，并查看代码示例和其他资源。
ms.date: 07/20/2015
helpviewer_keywords:
- statements [C#], about statements
- C# language, statements
ms.assetid: 901bcde7-87de-4e15-833c-f9cfd40c8ce3
ms.openlocfilehash: 3f8ac88525c44f9572f4f647145ad251537aba57
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/04/2020
ms.locfileid: "87556743"
---
# <a name="statements-c-programming-guide"></a>语句（C# 编程指南）

程序执行的操作采用语句表达。 常见操作包括声明变量、赋值、调用方法、循环访问集合，以及根据给定条件分支到一个或另一个代码块。 语句在程序中的执行顺序称为“控制流”或“执行流”。 根据程序对运行时所收到的输入的响应，在程序每次运行时控制流可能有所不同。

语句可以是以分号结尾的单行代码，也可以是语句块中的一系列单行语句。 语句块括在括号 {} 中，并且可以包含嵌套块。 以下代码演示了两个单行语句示例和一个多行语句块：

[!code-csharp[csProgGuideStatements#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#1)]

## <a name="types-of-statements"></a>语句的类型

下表列出了 C# 中的各种语句类型及其关联的关键字，并提供指向包含详细信息的主题的链接：

|类别|C# 关键字/说明|
|--------------|---------------------------|
|[声明语句](#declaration-statements)|声明语句引入新的变量或常量。 变量声明可以选择为变量赋值。 在常量声明中必须赋值。|
|[表达式语句](#expression-statements)|用于计算值的表达式语句必须在变量中存储该值。|
|选择语句|选择语句用于根据一个或多个指定条件分支到不同的代码段。 有关详细信息，请参阅下列主题： <ul><li>[if](../../language-reference/keywords/if-else.md)</li><li>[else](../../language-reference/keywords/if-else.md)</li><li>[switch](../../language-reference/keywords/switch.md)</li><li>[case](../../language-reference/keywords/switch.md)</li></ul>|
|迭代语句|迭代语句用于遍历集合（如数组），或重复执行同一组语句直到满足指定的条件。 有关详细信息，请参阅下列主题： <ul><li>[do](../../language-reference/keywords/do.md)</li><li>[for](../../language-reference/keywords/for.md)</li><li>[foreach](../../language-reference/keywords/foreach-in.md)</li><li>[in](../../language-reference/keywords/foreach-in.md)</li><li>[while](../../language-reference/keywords/while.md)</li></ul>|
|跳转语句|跳转语句将控制转移给另一代码段。 有关详细信息，请参阅下列主题： <ul><li>[break](../../language-reference/keywords/break.md)</li><li>[continue](../../language-reference/keywords/continue.md)</li><li>[default](../../language-reference/keywords/switch.md)</li><li>[goto](../../language-reference/keywords/goto.md)</li><li>[return](../../language-reference/keywords/return.md)</li><li>[yield](../../language-reference/keywords/yield.md)</li></ul>|
|异常处理语句|异常处理语句用于从运行时发生的异常情况正常恢复。 有关详细信息，请参阅下列主题： <ul><li>[throw](../../language-reference/keywords/throw.md)</li><li>[try-catch](../../language-reference/keywords/try-catch.md)</li><li>[try-finally](../../language-reference/keywords/try-finally.md)</li><li>[try-catch-finally](../../language-reference/keywords/try-catch-finally.md)</li></ul>|
|[Checked 和 unchecked](../../language-reference/keywords/checked-and-unchecked.md)|Checked 和 unchecked 语句用于指定将结果存储在变量中、但该变量过小而不能容纳结果值时，是否允许数值运算导致溢出。 有关详细信息，请参阅 [checked](../../language-reference/keywords/checked.md) 和 [unchecked](../../language-reference/keywords/unchecked.md)。|
|`await` 语句|如果用 [async](../../language-reference/keywords/async.md) 修饰符标记方法，则可以使用该方法中的 [await](../../language-reference/operators/await.md) 运算符。 在控制到达异步方法的 `await` 表达式时，控制将返回到调用方，该方法中的进程将挂起，直到等待的任务完成为止。 任务完成后，可以在方法中恢复执行。<br /><br /> 有关简单示例，请参阅[方法](../classes-and-structs/methods.md)的“异步方法”一节。 有关详细信息，请参阅 [async 和 await 的异步编程](../concepts/async/index.md)。|
|`yield return` 语句|迭代器对集合执行自定义迭代，如列表或数组。 迭代器使用 [yield return](../../language-reference/keywords/yield.md) 语句返回元素，每次返回一个。 到达 `yield return` 语句时，会记住当前在代码中的位置。 下次调用迭代器时，将从该位置重新开始执行。<br /><br /> 有关更多信息，请参见 [迭代器](../concepts/iterators.md)。|
|`fixed` 语句|fixed 语句禁止垃圾回收器重定位可移动的变量。 有关详细信息，请参阅 [fixed](../../language-reference/keywords/fixed-statement.md)。|
|`lock` 语句|lock 语句用于限制一次仅允许一个线程访问代码块。 有关详细信息，请参阅 [lock](../../language-reference/keywords/lock-statement.md)。|
|带标签的语句|可以为语句指定一个标签，然后使用 [goto](../../language-reference/keywords/goto.md) 关键字跳转到该带标签的语句。 （参见下一行中的示例。）|
|[空语句](#the-empty-statement)|空语句只含一个分号。 不执行任何操作，可以在需要语句但不需要执行任何操作的地方使用。|

## <a name="declaration-statements"></a>声明语句

以下代码显示了具有和不具有初始赋值的变量声明的示例，以及具有必要初始化的常量声明。

[!code-csharp[csProgGuideStatements#23](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#23)]

## <a name="expression-statements"></a>表达式语句

以下代码显示了表达式语句的示例，包括赋值、使用赋值创建对象和方法调用。

[!code-csharp[csProgGuideStatements#24](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#24)]

## <a name="the-empty-statement"></a>空语句

以下示例演示了空语句的两种用法：

[!code-csharp[csProgGuideStatements#25](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#25)]

## <a name="embedded-statements"></a>嵌入式语句

某些语句（包括 [do](../../language-reference/keywords/do.md)、[while](../../language-reference/keywords/while.md)、[for](../../language-reference/keywords/for.md) 和 [foreach](../../language-reference/keywords/foreach-in.md)）后面始终跟有一条嵌入式语句。 此嵌入式语句可以是单个语句，也可以是语句块中括在括号 {} 内的多个语句。 甚至可以在括号 {} 内包含单行嵌入式语句，如以下示例所示：

[!code-csharp[csProgGuideStatements#26](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#26)]

未括在括号 {} 内的嵌入式语句不能作为声明语句或带标签的语句。 下面的示例对此进行了演示：

[!code-csharp[csProgGuideStatements#27](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#27)]

将该嵌入式语句放在语句块中以修复错误：

[!code-csharp[csProgGuideStatements#28](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#28)]

## <a name="nested-statement-blocks"></a>嵌套语句块

语句块可以嵌套，如以下代码所示：

[!code-csharp[csProgGuideStatements#29](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#29)]

## <a name="unreachable-statements"></a>无法访问的语句

如果编译器认为在任何情况下控制流都无法到达特定语句，将生成警告 CS0162，如下例所示：

[!code-csharp[csProgGuideStatements#22](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#22)]

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)中的[语句](~/_csharplang/spec/statements.md)部分。

## <a name="see-also"></a>另请参阅

- [C# 编程指南](../index.md)
- [语句关键字](../../language-reference/keywords/statement-keywords.md)
- [C# 运算符和表达式](../../language-reference/operators/index.md)
