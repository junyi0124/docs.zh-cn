---
title: 使用实现 IDisposable 的对象
description: 了解如何在 .NET 中使用用于实现 IDisposable 接口的对象。 使用非托管资源的类型可实现 IDisposable 来允许资源回收。
ms.date: 05/13/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Dispose method
- try/finally block
- garbage collection, encapsulating resources
ms.assetid: 81b2cdb5-c91a-4a31-9c83-eadc52da5cf0
ms.openlocfilehash: 50769bdf684f6eb3a51dc2bd00c6a774a02976b7
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94827446"
---
# <a name="using-objects-that-implement-idisposable"></a>使用实现 IDisposable 的对象

公共语言运行时的垃圾回收器回收托管对象使用的内存，而使用非托管资源的类型则实现 <xref:System.IDisposable> 接口，以允许回收这些非托管资源所需的内存。 在使用完实现 <xref:System.IDisposable> 的对象后，应调用该对象的 <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> 实现。 可以通过两种方法执行此操作：

- 使用 C# `using` 语句（Visual Basic 中为 `Using`）。
- 通过实现 `try/finally` 块，并调用 `finally` 中的 <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType>。

## <a name="the-using-statement"></a>using 语句

C# 中的 [`using` 语句](../../csharp/language-reference/keywords/using-statement.md)和 Visual Basic 中的 [`Using` 语句](../../visual-basic/language-reference/statements/using-statement.md)可以简化为清理对象而必须编写的代码。 `using` 语句获取一个或多个资源，执行指定的语句，并自动处置对象。 但是，`using` 语句仅对在用于构造对象的方法的范围内使用的对象有用。

以下示例使用 `using` 语句创建并发布 <xref:System.IO.StreamReader?displayProperty=nameWithType> 对象。

[!code-csharp[Conceptual.Disposable#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.disposable/cs/using1.cs#1)]
[!code-vb[Conceptual.Disposable#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.disposable/vb/using1.vb#1)]

虽然 <xref:System.IO.StreamReader> 类实现了 <xref:System.IDisposable> 接口（这说明它使用的是非托管资源），但此示例不显式调用 <xref:System.IO.StreamReader.Dispose%2A?displayProperty=nameWithType> 方法。 当 C# 或 Visual Basic 编译器遇到 `using` 语句时，它会发出与以下显式包含 `try/finally` 块的代码等效的中间语言 (IL)。

[!code-csharp[Conceptual.Disposable#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.disposable/cs/using3.cs#3)]
[!code-vb[Conceptual.Disposable#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.disposable/vb/using3.vb#3)]

使用 C# `using` 语句，还可以在一个语句（在内部相当于嵌套语句 `using`）中获取多个资源。 下面的示例实例化两个 <xref:System.IO.StreamReader> 对象以读取两个不同文件的内容。

[!code-csharp[Conceptual.Disposable#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.disposable/cs/using4.cs#4)]

## <a name="tryfinally-block"></a>Try/finally 块

可以选择直接实现 `try/finally` 块，而不是将 `try/finally` 块包装在 `using` 语句中。 它可以是私有编码样式，或者你可能出于下列原因之一需要这样做：

- 包含 `catch` 块以处理 `try` 块中引发的异常。 否则，不会处理 `using` 语句中引发的任何异常。

- 实例化实现 <xref:System.IDisposable>（其范围对于声明它的块是非本地的）的对象。

下面的示例与上一示例类似，不同之处在于此示例使用 `try/catch/finally` 块实例化、使用和清理 <xref:System.IO.StreamReader> 对象，同时处理 <xref:System.IO.StreamReader> 构造函数及其 <xref:System.IO.StreamReader.ReadToEnd%2A> 方法抛出的任何异常。 `finally` 块中的代码检查实现 <xref:System.IDisposable> 的对象在其调用 <xref:System.IDisposable.Dispose%2A> 方法之前不为 `null`。 此操作失败会导致运行时发生 <xref:System.NullReferenceException> 异常。

[!code-csharp[Conceptual.Disposable#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.disposable/cs/using5.cs#6)]
[!code-vb[Conceptual.Disposable#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.disposable/vb/using5.vb#6)]

如果选择实现或必须实现 `try/finally` 块，可以遵循此基本模式，因为编程语言不支持 `using` 语句，但允许直接调用 <xref:System.IDisposable.Dispose%2A> 方法。

## <a name="idisposable-instance-members"></a>IDisposable 实例成员

如果类将 <xref:System.IDisposable> 实现保存为实例成员（字段或属性），类还应实现 <xref:System.IDisposable>。 有关详细信息，请参阅[实现级联释放](implementing-dispose.md#cascade-dispose-calls)。

## <a name="see-also"></a>请参阅

- [清理未托管资源](unmanaged.md)
- [using 语句（C# 参考）](../../csharp/language-reference/keywords/using-statement.md)
- [Using 语句 (Visual Basic)](../../visual-basic/language-reference/statements/using-statement.md)
