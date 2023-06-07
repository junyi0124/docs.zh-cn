---
description: continue 语句 - C# 参考
title: continue 语句 - C# 参考
ms.date: 07/20/2015
f1_keywords:
- continue_CSharpKeyword
- continue
helpviewer_keywords:
- continue keyword [C#]
ms.assetid: 8a5ac96f-f98a-4519-b32d-345847ed7be0
ms.openlocfilehash: 6c70934c3b861e1a1433e5c0b95bb32e9d717c53
ms.sourcegitcommit: eb7e87496f42361b1da98562dd75b516c9d58bbc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "91877647"
---
# <a name="continue-c-reference"></a>continue（C# 参考）

`continue` 语句将控制传递到其中出现的封闭 [while](./while.md)、[do](./do.md)、[for](./for.md) 或 [foreach](./foreach-in.md) 语句的下一次迭代。

## <a name="example"></a>示例

在本示例中，计数器最初是从 1 到 10 进行计数。 通过结合使用 `continue` 语句和表达式 `(i < 9)`，在 `i` 小于 9 的迭代中跳过 `continue` 和 `for` 主体末尾之间的语句。 在 `for` 循环的最后两次迭代（其中 i == 9，i == 10）中，不执行 `continue` 语句，且 `i` 的值将输出到控制台。

[!code-csharp[csrefKeywordsJump#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsJump/CS/csrefKeywordsJump.cs#3)]

## <a name="c-language-specification"></a>C# 语言规范

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 关键字](./index.md)
- [break 语句](/cpp/cpp/break-statement-cpp)
