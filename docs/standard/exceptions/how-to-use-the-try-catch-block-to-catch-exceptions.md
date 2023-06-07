---
title: 如何：使用 Try-Catch 块捕获异常
description: 使用 try 块即可包含可能引发或导致异常的语句。 将用于处理异常的语句放置在一个或多个捕获块中。
ms.date: 02/06/2019
dev_langs:
- csharp
- vb
helpviewer_keywords:
- exceptions, try/catch blocks
- try blocks
- try/catch blocks
- catch blocks
ms.assetid: a3ce6dfd-1f64-471b-8ad8-8cfaf406275d
ms.openlocfilehash: cfe5b2b304cdb9efe7f0d91059fe1c279b4fa2dd
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94828005"
---
# <a name="how-to-use-the-trycatch-block-to-catch-exceptions"></a>如何使用 try/catch 块捕获异常

将可能引发异常的任何代码语句放置在 `try` 块中，将用于处理异常的语句放置在 `try` 块下的一个或多个 `catch` 块中。 每个 `catch` 块包括异常类型，并且可以包含处理该异常类型所需的其他语句。

在以下示例中，<xref:System.IO.StreamReader> 将打开一个名为 data.txt 的文件，并从文件中检索行。 因为代码可能会引发任何三个异常，因此将其放置于 `try` 块中。 三个 `catch` 块捕获异常并通过将结果向控制台显示来处理它们。

[!code-csharp[CatchException#3](~/samples/snippets/csharp/VS_Snippets_CLR/CatchException/CS/catchexception2.cs#3)]
[!code-vb[CatchException#3](~/samples/snippets/visualbasic/VS_Snippets_CLR/CatchException/VB/catchexception2.vb#3)]

公共语言运行时 (CLR) 会捕获未由 `catch` 块处理的异常。 如果异常由 CLR 捕获，则可能出现以下结果之一，具体取决于 CLR 配置：

- 出现“调试”对话框。
- 该程序停止执行，出现含有异常信息的对话框。
- 错误输出到[标准错误输出流](xref:System.Console.Error)。

> [!NOTE]
> 大多数代码可能会引发异常，并且一些异常（如 <xref:System.OutOfMemoryException>）可能随时由 CLR 本身引发。 虽然应用程序无需处理这些异常，但在编写供他人使用的库时，应注意到这种可能性。 有关何时在 `try` 块中设置代码的建议，请参阅[异常的最佳做法](best-practices-for-exceptions.md)。

## <a name="see-also"></a>请参阅

- [异常](index.md)
- [处理 .NET 中的 I/O 错误](../io/handling-io-errors.md)
