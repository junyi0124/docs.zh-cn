---
title: 编译器指令
description: '了解 F # 语言预处理器指令、条件编译指令、行指令和编译器指令。'
ms.date: 12/10/2018
f1_keywords:
- '#endif_FS'
ms.openlocfilehash: ff106339478c3413dc6458b12f12e1d3f9cd1fe5
ms.sourcegitcommit: 721c3e4bdbb1ea0bb420818ec944c538fe5c513a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96438175"
---
# <a name="compiler-directives"></a>编译器指令

本主题介绍处理器指令和编译器指令。

## <a name="preprocessor-directives"></a>预处理器指令

预处理器指令以 # 符号作为前缀，出现在行本身上。 它由预处理器进行解释，后者在编译器本身之前运行。

下表列出了 F# 中可用的预处理器指令。

|指令|描述|
|---------|-----------|
|`#if`*符号*|支持条件编译。 如果定义了符号，则包括之后的部分中的代码 `#if` 。 *symbol* 还可以对符号求反 `!` 。|
|`#else`|支持条件编译。 如果未定义与前面的 `#if` 一起使用的符号，则将一段代码标记为包含在内。|
|`#endif`|支持条件编译。 标记条件代码段的末尾。|
|`#`内嵌 *int*、<br/>`#`内嵌 *int* *字符串*、<br/>`#`内嵌 *int* *原义字符串*|指示原始源代码行和文件名以用于调试。 此功能是为生成 F# 源代码的工具而提供的。|
|`#nowarn`*warningcode*|禁用编译器警告。 若要禁用警告，请在编译器输出中查找其编号，然后将它包含在引号内。 省略“FS”前缀。 若要禁用同一行上的多个警告编号，请将每个编号用引号引起来，并使用空格分隔每个字符串。 例如：

`#nowarn "9" "40"`

禁用警告的效果适用于整个文件，包括指令前面的部分文件。 |

## <a name="conditional-compilation-directives"></a>条件编译指令

通过其中一个指令停用的代码在 Visual Studio Code 编辑器中显示为灰色。

> [!NOTE]
> 条件编译指令的行为与它在其他语言中的行为不同。 例如，不能使用涉及符号的布尔表达式，并且 `true` 和 `false` 没有特殊含义。 在 `if` 指令中使用的符号必须通过命令行或在项目设置中定义；没有 `define` 预处理器指令。

以下代码演示了 `#if`、`#else` 和 `#endif` 指令的用法。 在此示例中，代码包含两个版本的 `function1` 定义。 当 `VERSION1` 使用 [-define 编译器选项](./compiler-options.md)定义时，将 `#if` 激活指令和指令之间的代码 `#else` 。 否则，会激活 `#else` 与 `#endif` 之间的代码。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet7301.fs)]

F# 中没有 `#define` 预处理器指令。 必须使用编译器选项或项目设置来定义 `#if` 指令使用的符号。

条件编译指令可以进行嵌套。 缩进对于预处理器指令并不重要。

你还可以使用对符号求反 `!` 。 在此示例中，仅当 _不_ 调试时，字符串的值才是：

```fsharp
#if !DEBUG
let str = "Not debugging!"
#else
let str = "Debugging!"
#endif
```

## <a name="line-directives"></a>行指令

在生成时，编译器会通过引用每个错误发生时所在的行号来报告 F# 代码中的错误。 这些行号从 1 开始（表示文件中的第一行）。 但是，如果要从另一种工具生成 F# 源代码，则生成的代码中的行号通常没什么意义，因为生成的 F# 代码中的错误很可能是由其他来源导致的。 `#line` 指令提供了一种方法，使生成 F# 源代码的工具的作者可以将有关原始行号和源文件的信息传递给生成的 F# 代码。

使用 `#line` 指令时，必须将文件名括在引号内。 除非原义标记 (`@`) 出现在字符串前面，否则必须使用两个反斜杠字符（而不是一个）对反斜杠字符进行转义，才能在路径中使用它们。 以下是有效的行标记。 在这些示例中，假设原始文件 `Script1` 会在通过某种工具运行时产生自动生成的 F# 代码文件，并且这些指令的位置处的代码是从文件 `Script1` 中第 25 行处的一些标记生成的。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet7303.fs)]

这些标记指示在此位置生成的 F# 代码派生自位于 `Script1` 中第 `25` 行上或附近的一些构造。

## <a name="compiler-directives"></a>编译器指令

编译器指令类似于预处理器指令，因为它们以 # 符号作为前缀，但它们留给编译器进行解释和处理，而不是由预处理器进行解释。

下表列出了在 F# 中可用的编译器指令。

|指令|描述|
|---------|-----------|
|`#light` ["on" &#124; "off"]|启用或禁用轻量语法，以便与其他版本的 ML 兼容。 默认情况下，轻量语法处于启用状态。 详细语法始终处于启用状态。 因此，可以同时使用轻量语法和详细语法。 指令 `#light` 本身等效于 `#light "on"`。 如果指定 `#light "off"`，则必须对所有语言构造使用详细语法。 F# 文档中展示的语法基于使用轻量语法这一假设。 有关详细信息，请参阅 [详细语法](verbose-syntax.md)。|

对于解释器 ( # A0) 指令，请参阅 [与 F # 交互编程](../tools/fsharp-interactive/index.md)。

## <a name="see-also"></a>另请参阅

- [F# 语言参考](index.md)
- [编译器选项](compiler-options.md)
