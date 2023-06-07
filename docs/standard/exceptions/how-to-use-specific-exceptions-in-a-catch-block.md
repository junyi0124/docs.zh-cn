---
title: 如何：在 Catch 块中使用特定异常
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- exceptions, try/catch blocks
- try/catch blocks
- catch blocks
ms.assetid: 12af9ff3-8587-4f31-90cf-6c2244e0fdae
ms.openlocfilehash: bf4813e950b034cb807abebfe016c1e34487d594
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94828018"
---
# <a name="how-to-use-specific-exceptions-in-a-catch-block"></a>如何在 catch 块中使用特定异常

一般来说，编程最佳做法是，捕获特定类型的异常，而不是使用基本 `catch` 语句。

异常发生时，会在堆栈中向上传递，每个 catch 块都有机会处理异常。 Catch 语句的顺序非常重要。 在一般异常 catch 块或编译器发出错误前，将 catch 块指向特定异常。 通过匹配异常类型与 catch 块中指定的异常名称，确定合适的 catch 块。 如果没有特定的 catch 块，则将由一般 catch 块捕获异常（如果异常存在）。

下方代码示例使用 `try`/`catch` 块来捕获 <xref:System.InvalidCastException>。 该示例创建了一个名为 `Employee` 的类，其具有单一属性，雇员级别为 (`Emlevel`)。 `PromoteEmployee` 方法接收对象并递增雇员级别。 <xref:System.DateTime> 实例传递给 `PromoteEmployee` 方法时，<xref:System.InvalidCastException> 发生。

[!code-cpp[CatchException#2](../../../samples/snippets/cpp/VS_Snippets_CLR/CatchException/CPP/catchexception1.cpp#2)]
[!code-csharp[CatchException#2](../../../samples/snippets/csharp/VS_Snippets_CLR/CatchException/CS/catchexception1.cs#2)]
[!code-vb[CatchException#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CatchException/VB/catchexception1.vb#2)]

## <a name="see-also"></a>另请参阅

- [异常](index.md)
