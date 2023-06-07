---
description: 了解 C# 中的内置布尔类型
title: bool 类型 - C# 参考
ms.date: 11/26/2019
f1_keywords:
- bool
- bool_CSharpKeyword
- "true"
- "false"
- true_CSharpKeyword
- false_CSharpKeyword
helpviewer_keywords:
- bool data type [C#]
- Boolean [C#]
ms.assetid: 551cfe35-2632-4343-af49-33ad12da08e2
ms.openlocfilehash: e74fd76fcb19faa5860e48140da0fbd3db4afa47
ms.sourcegitcommit: 870bc4b4087510f6fba3c7b1c0d391f02bcc1f3e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2020
ms.locfileid: "92471884"
---
# <a name="bool-c-reference"></a>bool（C# 参考）

`bool` 类型关键字是 .NET <xref:System.Boolean?displayProperty=nameWithType> 结构类型的别名，它表示一个布尔值，可为 `true` 或 `false`。

若要使用 `bool` 类型的值执行逻辑运算，请使用[布尔逻辑](../operators/boolean-logical-operators.md)运算符。 `bool` 类型是 [比较](../operators/comparison-operators.md)和[相等](../operators/equality-operators.md)运算符的结果类型。 `bool` 表达式可以是 [if](../keywords/if-else.md)、[do](../keywords/do.md)、[while](../keywords/while.md) 和 [for](../keywords/for.md) 语句中以及[条件运算符 `?:`](../operators/conditional-operator.md) 中的控制条件表达式。

`bool` 类型的默认值为 `false`。

## <a name="literals"></a>文本

可使用 `true` 和 `false` 文本来初始化 `bool` 变量或传递 `bool` 值：

[!code-csharp-interactive[bool literals](snippets/shared/BoolType.cs#Literals)]

## <a name="three-valued-boolean-logic"></a>三值布尔逻辑

如需支持三值逻辑（例如，在使用支持三值布尔类型的数据库时），请使用可为空 `bool?` 类型。 对于 `bool?` 操作数，预定义的 `&` 和 `|` 运算符支持三值逻辑。 有关详细信息，请参阅[布尔逻辑运算符](../operators/boolean-logical-operators.md)一文的[可以为 null 的布尔逻辑运算符](../operators/boolean-logical-operators.md#nullable-boolean-logical-operators)部分。

有关可为空的值类型的详细信息，请参阅[可为空的值类型](nullable-value-types.md)。

## <a name="conversions"></a>转换

C# 仅提供了两个涉及 `bool` 类型的转换。 它们是对相应的可以为空的 `bool?` 类型的隐式转换以及对 `bool?` 类型的显式转换。 但是，.NET 提供了其他方法可用来转换到 `bool` 类型从或此类型进行转换。 有关详细信息，请参阅 <xref:System.Boolean?displayProperty=nameWithType> API 参考页的[转换为布尔值和从布尔值转换](/dotnet/api/system.boolean#converting-to-and-from-boolean-values)部分。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)中的 [bool 类型](~/_csharplang/spec/types.md#the-bool-type)部分。

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [值类型](value-types.md)
- [true 和 false 运算符](../operators/true-false-operators.md)
