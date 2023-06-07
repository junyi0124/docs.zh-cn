---
title: 处理 .NET 中的 I/O 错误
description: 了解如何处理 .NET 中的 I/O 错误。 将错误代码映射到异常、处理 I/O 操作中的异常，以及处理 IOException。
ms.date: 08/27/2018
dev_langs:
- csharp
- vb
helpviewer_keywords:
- I/O, exception handling
- I/O, errors
ms.workload:
- dotnet
- dotnetcore
ms.openlocfilehash: fe6796be91d31e3f7bf69605f39a39f3db6b3e83
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94830865"
---
# <a name="handling-io-errors-in-net"></a>处理 .NET 中的 I/O 错误

除了可能在任何方法调用中引发异常（如系统压力过大导致 <xref:System.OutOfMemoryException> 或由于编程器错误导致 <xref:System.NullReferenceException>），.NET 文件系统方法还可能引发以下异常：

- <xref:System.IO.IOException?displayProperty=nameWithType>，所有 <xref:System.IO> 异常类型的基类。 当出现操作系统的返回代码不直接映射到任何其他异常类型的错误时将引发此异常。
- <xref:System.IO.FileNotFoundException?displayProperty=nameWithType>。
- <xref:System.IO.DirectoryNotFoundException?displayProperty=nameWithType>。
- <xref:System.IO.DriveNotFoundException??displayProperty=nameWithType>。
- <xref:System.IO.PathTooLongException?displayProperty=nameWithType>。
- <xref:System.OperationCanceledException?displayProperty=nameWithType>。
- <xref:System.UnauthorizedAccessException?displayProperty=nameWithType>。
- 当 .NET Framework 和 .NET Core 2.0 以及先前版本上的路径字符无效时，将引发 <xref:System.ArgumentException?displayProperty=nameWithType>。
- 当 .NET Framework 中存在无效冒号时，将引发 <xref:System.NotSupportedException?displayProperty=nameWithType>。
- 当在受限制的信任中运行的应用程序缺少仅限于 .NET Framework 的所需权限时，将引发 <xref:System.Security.SecurityException?displayProperty=nameWithType>。 （完全信任是 .NET Framework 的默认设置。）

## <a name="mapping-error-codes-to-exceptions"></a>将错误代码映射到异常

由于文件系统为操作系统资源，.NET Core 和 .NET Framework 中的 I/O 方法将包装对基础操作系统的调用。 当由操作系统执行的代码出现 I/O 错误时，操作系统将对 .NET I/O 方法返回错误信息。 然后，该方法会将错误信息（通常采用错误代码形式）转换为 .NET 异常类型。 大多数情况下，可以通过直接将错误代码转换为其相应异常类型来完成此操作；它不基于方法调用的上下文执行任何特殊的错误映射。

例如，在 Windows 操作系统，返回 `ERROR_FILE_NOT_FOUND`（或 0x02）错误代码的方法调用会映射到 <xref:System.IO.FileNotFoundException>，`ERROR_PATH_NOT_FOUND` 错误代码（或 0x03）则映射到 <xref:System.IO.DirectoryNotFoundException>。

但是，操作系统返回特定错误代码的精确条件通常未记录或记录不当。 因此，会出现意外异常。 例如，因为使用的是目录而不是文件，可以预料到向 <xref:System.IO.DirectoryInfo.%23ctor%2A> 构造函数提供无效目录路径将引发 <xref:System.IO.DirectoryNotFoundException>。 但是，它也可能引发 <xref:System.IO.FileNotFoundException>。

## <a name="exception-handling-in-io-operations"></a>I/O 操作中的异常处理

由于操作系统的这一依赖性，相同异常条件（例如在我们的示例中没有发现错误目录）可能会导致 I/O 方法引发任何一种 I/O 异常。 这意味着，在调用 I/O API 时，代码应准备好处理大多数或者所有这些异常，如下表所示：

| 异常类型 | .NET Core/.NET 5+ | .NET Framework |
|---|---|---|
| <xref:System.IO.IOException> | 是 | 是 |
| <xref:System.IO.FileNotFoundException> | 是 | 是 |
| <xref:System.IO.DirectoryNotFoundException> | 是 | 是 |
| <xref:System.IO.DriveNotFoundException?> | 是 | 是 |
| <xref:System.IO.PathTooLongException> | 是 | 是 |
| <xref:System.OperationCanceledException> | 是 | 是 |
| <xref:System.UnauthorizedAccessException> | 是 | 是 |
| <xref:System.ArgumentException> | .NET core 2.0 及早期版本| 是 |
| <xref:System.NotSupportedException> | No | 是 |
| <xref:System.Security.SecurityException> | 否 | 仅受限的信任 |

## <a name="handling-ioexception"></a>处理 IOException

作为 <xref:System.IO> 命名空间中异常的基类，当任何错误代码未映射到预定义的异常类型时，也将引发 <xref:System.IO.IOException>。 这意味着异常可以由任何 I/O 操作引发。

> [!IMPORTANT]
> 因为 <xref:System.IO.IOException> 是 <xref:System.IO> 命名空间中其他异常类型的基类，应在处理其他 I/O 相关异常后处理 `catch` 块。

此外，从 .NET Core 2.1 开始，已删除对路径正确性（例如，为了确保路径中不存在无效字符）的验证检查，且运行时会引发从操作系统错误代码（而非从它自己的验证代码）映射的异常。 在这种情况下，最有可能引发的异常是 <xref:System.IO.IOException>，虽然也可能引发任何其他异常类型。

请注意，在异常处理代码中，应始终最后处理 <xref:System.IO.IOException>。 否则，因为它是所有其他 IO 异常的基类，将不会评估派生类的 catch 块。

在 <xref:System.IO.IOException> 情况下，可以从 [IOException.HResult](xref:System.Exception.HResult) 属性获取更多错误信息。 若要将 HResult 值转换为 Win32 错误代码，可以删除 32 位值的前 16 位。 下表列出了可能包装在 <xref:System.IO.IOException> 中的错误代码。

| HResult | 返回的常量 | 描述 |
| --- | --- | --- |
| ERROR_SHARING_VIOLATION | 32 | 缺少文件名称，或文件或目录正在使用中。 |
| ERROR_FILE_EXISTS | 80 | 该文件已存在。 |
| ERROR_INVALID_PARAMETER | 87 | 提供给该方法的参数无效。 |
| ERROR_ALREADY_EXISTS | 183 | 文件或目录已存在。 |

可以使用 catch 语句中的 `When` 子句来处理这些问题，如以下示例所示。

[!code-csharp[io-exception-handling](~/samples/snippets/standard/io/io-exceptions/cs/io-exceptions.cs)]
[!code-vb[io-exception-handling](~/samples/snippets/standard/io/io-exceptions/vb/io-exceptions.vb)]

## <a name="see-also"></a>请参阅

- [在 .NET 中处理和引发异常](../exceptions/index.md)
- [异常处理（任务并行库）](../parallel-programming/exception-handling-task-parallel-library.md)
- [针对异常的最佳做法](../exceptions/best-practices-for-exceptions.md)
- [如何在 catch 块中使用特定异常](../exceptions/how-to-use-specific-exceptions-in-a-catch-block.md)
