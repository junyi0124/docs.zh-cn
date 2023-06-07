---
description: try-finally - C# 参考
title: try-finally - C# 参考
ms.date: 07/20/2015
f1_keywords:
- finally
- finally_CSharpKeyword
helpviewer_keywords:
- finally keyword [C#]
- try-finally statement [C#]
ms.assetid: c27623fb-7261-4464-862c-7a369d3c8f0a
ms.openlocfilehash: 621c5bf1607136361b72f978681607ec2aec150f
ms.sourcegitcommit: d579fb5e4b46745fd0f1f8874c94c6469ce58604
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2020
ms.locfileid: "89141967"
---
# <a name="try-finally-c-reference"></a>try-finally（C# 参考）

通过使用 `finally` 块，可以清除 [try](try-catch.md) 块中分配的任何资源，即使在 `try` 块中发生异常，也可以运行代码。 通常情况下，`finally` 块的语句会在控件离开 `try` 语句时运行。 正常执行中，执行 `break`、`continue`、`goto` 或 `return` 语句，或者从 `try` 语句外传播异常都可能会导致发生控件转换。

已处理的异常中会保证运行相关联的 `finally` 块。 但是，如果异常未经处理，则 `finally` 块的执行将取决于异常解除操作的触发方式。 反过来，这又取决于你计算机的设置方式。

通常情况下，当未经处理的异常终止应用程序时，`finally` 块是否运行已不重要。 但是，如果 `finally` 块中的语句必须在这种情况下运行，则可以将 `catch` 块添加到 `try`-`finally` 语句，这是其中一种解决方法。 另一种解决方法是，可以捕获可能在调用堆栈上方的 `try`-`finally` 语句的 `try` 块中引发的异常。 也就是说，可以通过以下几种方法来捕获异常：调用包含 `try`-`finally` 语句的方法、调用该方法或调用堆栈中的任何方法。 如果未捕获异常，则 `finally` 块的执行取决于操作系统是否选择触发异常解除操作。

## <a name="example"></a>示例

在以下示例中，无效的转换语句会导致 `System.InvalidCastException` 异常。 异常未经处理。

[!code-csharp[csrefKeywordsExceptions#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsExceptions/CS/csrefKeywordsExceptions.cs#4)]

在以下示例中，`TryCast` 方法导致的异常会在比调用堆栈更远的方法中被捕获。

[!code-csharp[csrefKeywordsExceptions#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsExceptions/CS/csrefKeywordsExceptions.cs#6)]

有关 `finally` 的详细信息，请参阅 [try-catch-finally](try-catch-finally.md)。

C# 还包含 [using 语句](using-statement.md)，它以简便语法为 <xref:System.IDisposable> 对象提供类似的功能。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)中的 [try 语句](~/_csharplang/spec/statements.md#the-try-statement)部分。

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 关键字](index.md)
- [try、throw 和 catch 语句 (C++)](/cpp/cpp/try-throw-and-catch-statements-cpp)
- [throw](throw.md)
- [try-catch](try-catch.md)
- [如何：显式抛出异常](../../../standard/exceptions/how-to-explicitly-throw-exceptions.md)
