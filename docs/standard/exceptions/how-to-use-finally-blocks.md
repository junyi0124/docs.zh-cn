---
title: 如何：使用 Finally 块
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- exceptions, try/catch blocks
- exceptions, finally blocks
- try/catch blocks
- finally blocks
- ArgumentOutOfRangeException class
ms.assetid: 4b9c0137-04af-4468-91d1-b9014df8ddd2
ms.openlocfilehash: 8bc36ce9a755762bb5159a27f9ef5699b2992f0e
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94828031"
---
# <a name="how-to-use-finally-blocks"></a>如何使用 finally 块

发生异常时，将停止执行且会向相应异常处理程序赋予控制权。 这意味着会绕过预计要执行的代码行。 即使引发了异常，也需要完成某些资源清理，例如关闭文件。 若要执行此操作，可以使用 `finally` 块。 无论是否引发异常，始终会执行 `finally` 块。

下方代码示例使用 `try`/`catch` 块来捕获 <xref:System.ArgumentOutOfRangeException>。 `Main` 方法会创建两个数组，并尝试将一个数组复制到另一个。 该操作将生成 <xref:System.ArgumentOutOfRangeException> 并且会向控制台写入错误。 `finally` 块执行时不考虑复制操作的结果。

[!code-cpp[CodeTryCatchFinallyExample#3](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeTryCatchFinallyExample/CPP/source2.cpp#3)]
[!code-csharp[CodeTryCatchFinallyExample#3](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeTryCatchFinallyExample/CS/source2.cs#3)]
[!code-vb[CodeTryCatchFinallyExample#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeTryCatchFinallyExample/VB/source2.vb#3)]  

## <a name="see-also"></a>另请参阅

- [异常](index.md)
