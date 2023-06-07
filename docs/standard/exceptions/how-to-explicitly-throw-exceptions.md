---
title: 如何：显式引发异常
description: 了解如何使用 C# throw 语句或 Visual Basic Throw 语句在 .NET 中显式引发异常。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- exceptions, try/catch blocks
- FileNotFoundException class
- try/catch blocks
- exceptions, throwing
- implicitly throwing exceptions
ms.assetid: 72bdd157-caa9-4478-9ee3-cb4500b84528
ms.openlocfilehash: 37ba5f952d621ff2e209a3bac375b62894c944a7
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94828044"
---
# <a name="how-to-explicitly-throw-exceptions"></a>如何显式引发异常

可使用 C# [`throw`](../../csharp/language-reference/keywords/throw.md) 或 Visual Basic [`Throw`](../../visual-basic/language-reference/statements/throw-statement.md) 语句显式引发异常。 可使用 `throw` 语句再次引发捕获的异常。 向重新引发的异常添加信息以在调试时提供详细信息，这是很好的编码做法。

下方代码示例使用 `try`/`catch` 块来捕获可能的 <xref:System.IO.FileNotFoundException>。 以下 `try` 块为可捕获 <xref:System.IO.FileNotFoundException> 并在未找到数据文件时将消息写入控制台的 `catch` 块。 下一语句为 `throw` 语句，可引发新的 <xref:System.IO.FileNotFoundException> 并向异常添加文本信息。

[!code-csharp[Exception.Throwing#1](~/samples/snippets/csharp/VS_Snippets_CLR/Exception.Throwing/CS/throw.cs#1)]
[!code-vb[Exception.Throwing#1](~/samples/snippets/visualbasic/VS_Snippets_CLR/Exception.Throwing/VB/throw.vb#1)]  

## <a name="see-also"></a>请参阅

- [异常](index.md)
