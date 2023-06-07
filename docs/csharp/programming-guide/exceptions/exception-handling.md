---
title: 异常处理 - C# 编程指南
description: 了解异常处理。 请参阅 try-catch、try-finally 和 try-catch-finally 语句的示例。
ms.date: 12/09/2020
helpviewer_keywords:
- exception handling [C#], about exception handling
- exceptions [C#], handling
ms.assetid: b4e4ecf2-b907-4e58-891f-2563762258e9
ms.openlocfilehash: fabf1413ba498232e67f45460195fe96e25fab0a
ms.sourcegitcommit: 9b877e160c326577e8aa5ead22a937110d80fa44
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2020
ms.locfileid: "97110190"
---
# <a name="exception-handling-c-programming-guide"></a>异常处理（C# 编程指南）

C# 程序员使用 [try](../../language-reference/keywords/try-catch.md) 块来对可能受异常影响的代码进行分区。 关联的 [catch](../../language-reference/keywords/try-catch.md) 块用于处理生成的任何异常。 [finally](../../language-reference/keywords/try-finally.md) 块包含无论 `try` 块中是否引发异常都会运行的代码，如发布 `try` 块中分配的资源。 `try` 块需要一个或多个关联的 `catch` 块或一个 `finally` 块，或两者皆之。

下面的示例演示 `try-catch` 语句、`try-finally` 语句和 `try-catch-finally` 语句。

:::code language="csharp" source="snippets/exceptions/Program.cs" id="TryCatchStructure":::

:::code language="csharp" source="snippets/exceptions/Program.cs" id="TryFinallyStructure":::

:::code language="csharp" source="snippets/exceptions/Program.cs" id="TryCatchFinallyStructure":::

一个不具有 `catch` 或 `finally` 块的 `try` 块会导致编译器错误。

## <a name="catch-blocks"></a>catch 块

`catch` 块可以指定要捕获的异常的类型。 该类型规范称为异常筛选器。 异常类型应派生自 <xref:System.Exception>。 一般情况下，不要将 <xref:System.Exception> 指定为异常筛选器，除非了解如何处理可能在 `try` 块中引发的所有异常，或者已在 `catch` 块的末尾处包括了 [throw](../../language-reference/keywords/throw.md) 语句。

可将具有不同异常类的多个 `catch` 块链接在一起。 代码中 `catch` 块的计算顺序为从上到下，但针对引发的每个异常，仅执行一个 `catch` 块。 将执行指定所引发的异常的确切类型或基类的第一个 `catch` 块。 如果没有 `catch` 块指定匹配的异常类，则将选择不具有类型的 `catch` 块（如果语句中存在）。 务必首先定位具有最具体的（即，最底层派生的）异常类的 `catch` 块。

当以下条件为 true 时，捕获异常：

- 能够很好地理解可能会引发异常的原因，并且可以实现特定的恢复，例如捕获 <xref:System.IO.FileNotFoundException> 对象时提示用户输入新文件名。
- 可以创建和引发一个新的、更具体的异常。
  :::code language="csharp" source="snippets/exceptions/Program.cs" id="ThrowMoreSpecificException":::
- 想要先对异常进行部分处理，然后再将其传递以进行额外处理。 在下面的示例中，`catch` 块用于在重新引发异常之前将条目添加到错误日志。
  :::code language="csharp" source="snippets/exceptions/Program.cs" id="RethrowError":::

还可以指定异常筛选器，以向 catch 子句添加布尔表达式。 这表明仅当条件为 true 时，特定 catch 子句才匹配。 在以下示例中，两个 catch 子句均使用相同的异常类，但是会检查其他条件以创建不同的错误消息：

:::code language="csharp" source="snippets/exceptions/ExceptionFilter.cs" ID="DemonstrateExceptionFilter":::

始终返回 `false` 的异常筛选器可用于检查所有异常，但不可用于处理异常。 典型用途是记录异常：

:::code language="csharp" source="snippets/exceptions/ExceptionFilter.cs" ID="ShowExceptionFilter":::

`LogException` 方法始终返回 `false`，使用此异常筛选器的 `catch` 子句均不匹配。 catch 子句可以是通用的，使用 <xref:System.Exception?displayProperty=nameWithType>后面的子句可以处理更具体的异常类。

## <a name="finally-blocks"></a>Finally 块

`finally` 块让你可以清理在 `try` 块中所执行的操作。 如果存在 `finally` 块，将在执行 `try` 块和任何匹配的 `catch` 块之后，最后执行它。 无论是否会引发异常或找到匹配异常类型的 `catch` 块，`finally` 块都将始终运行。

`finally` 块可用于发布资源（如文件流、数据库连接和图形句柄）而无需等待运行时中的垃圾回收器来完成对象。 有关详细信息，请参阅 [using 语句](../../language-reference/keywords/using-statement.md)。

在下面的示例中，`finally` 块用于关闭在 `try` 块中打开的文件。 请注意，在关闭文件之前，将检查文件句柄的状态。 如果 `try` 块不能打开文件，则文件句柄仍将具有值 `null` 且 `finally` 块不会尝试将其关闭。 或者，如果在 `try` 块中成功打开文件，则 `finally` 块将关闭打开的文件。

:::code language="csharp" source="snippets/exceptions/Program.cs" id="CleanupIfNotNull":::

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](/dotnet/csharp/language-reference/language-specification/introduction)中的[异常](~/_csharplang/spec/exceptions.md)和 [try 语句](~/_csharplang/spec/statements.md#the-try-statement)。 该语言规范是 C# 语法和用法的权威资料。
  
## <a name="see-also"></a>请参阅

- [C# 参考](../../language-reference/index.md)
- [try-catch](../../language-reference/keywords/try-catch.md)
- [try-finally](../../language-reference/keywords/try-finally.md)
- [try-catch-finally](../../language-reference/keywords/try-catch-finally.md)
- [using 语句](../../language-reference/keywords/using-statement.md)
