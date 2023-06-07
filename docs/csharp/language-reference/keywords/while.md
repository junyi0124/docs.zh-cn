---
description: while - C# 参考
title: while - C# 参考
ms.date: 05/28/2018
f1_keywords:
- while_CSharpKeyword
- while
helpviewer_keywords:
- while keyword [C#]
ms.assetid: 72a0765c-6852-4aca-b327-4a11cb7f5c59
ms.openlocfilehash: 97299f9ac6f2cdd6bbddf831b3ee2ad711935fbe
ms.sourcegitcommit: d579fb5e4b46745fd0f1f8874c94c6469ce58604
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2020
ms.locfileid: "89141863"
---
# <a name="while-c-reference"></a>while（C# 参考）

在指定的布尔表达式的计算结果为 `true` 时，`while` 语句会执行一条语句或一个语句块。 由于在每次执行循环之前都会计算此表达式，所以 `while` 循环会执行零次或多次。 这不同于 [do](do.md) 循环，该循环执行一次或多次。

在 `while` 语句块中的任何点，都可使用 [break](break.md) 语句中断循环。

可通过使用 [continue](continue.md) 语句直接步入 `while` 表达式的计算部分。 如果表达式计算结果为 `true`，则继续执行循环中的第一个语句。 否则，将在循环后的第一个语句处继续执行。

还可以使用 [goto](goto.md)、[return](return.md) 或 [throw](throw.md) 语句退出 `while` 循环。

## <a name="example"></a>示例

下面的示例演示 `while` 语句的用法。 选择“运行”以运行示例代码  。 然后可以修改代码并再次运行它。

[!code-csharp-interactive[while loop example](snippets/IterationKeywordsExamples.cs#3)]

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](/dotnet/csharp/language-reference/language-specification/introduction)中的 [while 语句](~/_csharplang/spec/statements.md#the-while-statement)部分。

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 关键字](index.md)
- [do 语句](do.md)
