---
title: 使用异常 - C# 编程指南
description: 了解如何使用异常。 异常由遇到错误的代码引发，由更正错误的代码捕捉。
ms.date: 12/09/2020
helpviewer_keywords:
- exception handling [C#], about exception handling
- exceptions [C#], about exceptions
ms.assetid: 71472c62-320a-470a-97d2-67995180389d
ms.openlocfilehash: 30a7c3eecb599493dfa74d664c4899836c1b9276
ms.sourcegitcommit: 9b877e160c326577e8aa5ead22a937110d80fa44
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2020
ms.locfileid: "97110216"
---
# <a name="use-exceptions-c-programming-guide"></a>使用异常（C# 编程指南）

在 C# 中，程序中的运行时错误通过使用一种称为“异常”的机制在程序中传播。 异常由遇到错误的代码引发，由能够更正错误的代码捕捉。 异常可由 .NET 运行时或由程序中的代码引发。 一旦引发了一个异常，此异常会在调用堆栈中传播，直到找到针对它的 `catch` 语句。 未捕获的异常由系统提供的通用异常处理程序处理，该处理程序会显示一个对话框。

异常由从 <xref:System.Exception> 派生的类表示。 此类标识异常的类型，并包含详细描述异常的属性。 引发异常涉及创建异常派生类的实例，配置异常的属性（可选），然后使用 `throw` 关键字引发该对象。 例如：

:::code language="csharp" source="snippets/exceptions/Program.cs" id="ThrowException":::

引发异常后，运行时将检查当前语句，以确定它是否在 `try` 块内。 如果在，则将检查与 `try` 块关联的所有 `catch` 块，以确定它们是否可以捕获该异常。 `Catch` 块通常会指定异常类型；如果该 `catch` 块的类型与异常或异常的基类的类型相同，则该 `catch` 块可处理该方法。 例如：

:::code language="csharp" source="snippets/exceptions/Program.cs" id="CatchException":::

如果引发异常的语句不在 `try` 块内或者包含该语句的 `try` 块没有匹配的 `catch` 块，则运行时将检查调用方法中是否有 `try` 语句和 `catch` 块。 运行时将继续调用堆栈，搜索兼容的 `catch` 块。 在找到并执行 `catch` 块之后，控制权将传递给 `catch` 块之后的下一个语句。

一个 `try` 语句可包含多个 `catch` 块。 将执行第一个能够处理该异常的 `catch` 语句；将忽略任何后续的 `catch` 语句，即使它们是兼容的也是如此。 Catch 块应始终按从最具有针对性（或派生程度最高）到最不具有针对性的顺序排列。 例如：

:::code language="csharp" source="snippets/exceptions/CatchOrder.cs":::

执行 `catch` 块之前，运行时会检查 `finally` 块。 `Finally` 块使程序员可以清除中止的 `try` 块可能遗留下的任何模糊状态，或者释放任何外部资源（例如图形句柄、数据库连接或文件流），而无需等待垃圾回收器在运行时完成这些对象。 例如：

:::code language="csharp" source="snippets/exceptions/Program.cs" id="TestFinally":::

如果 `WriteByte()` 引发了异常并且未调用 `file.Close()`，则第二个 `try` 块中尝试重新打开文件的代码将会失败，并且文件将保持锁定状态。 由于即使引发异常也会执行 `finally` 块，前一示例中的 `finally` 块可使文件正确关闭，从而有助于避免错误。

如果引发异常之后没有在调用堆栈上找到兼容的 `catch` 块，则会出现以下三种情况之一：

- 如果异常存在于终结器内，将中止终结器，并调用基类终结器（如果有）。
- 如果调用堆栈包含静态构造函数或静态字段初始值设定项，将引发 <xref:System.TypeInitializationException>，同时将原始异常分配给新异常的 <xref:System.Exception.InnerException%2A> 属性。
- 如果到达线程的开头，则终止线程。
