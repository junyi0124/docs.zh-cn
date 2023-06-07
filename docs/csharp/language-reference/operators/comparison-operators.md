---
title: 比较运算符 - C# 参考
description: 了解可用于检查数值顺序的 C# 比较运算符。
ms.date: 05/11/2020
author: pkulikov
f1_keywords:
- <_CSharpKeyword
- '>_CSharpKeyword'
- <=_CSharpKeyword
- '>=_CSharpKeyword'
helpviewer_keywords:
- comparison operators [C#]
- relational operators [C#]
- less than operator [C#]
- < operator [C#]
- greater than operator [C#]
- '> operator [C#]'
- less than or equal to operator [C#]
- <= operator [C#]
- greater than or equal to operator [C#]
- '>= operator [C#]'
ms.openlocfilehash: 9fa739d8b5461d4043f3ae51f5d14949a95c68e5
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87916877"
---
# <a name="comparison-operators-c-reference"></a>比较运算符（C# 参考）

[`<`（小于）](#less-than-operator-)、[`>`（大于）](#greater-than-operator-)、[`<=`（小于或等于）](#less-than-or-equal-operator-)和 [`>=`（大于或等于）](#greater-than-or-equal-operator-)比较（也称为关系）运算符比较其操作数。 所有[整型](../builtin-types/integral-numeric-types.md)和[浮点](../builtin-types/floating-point-numeric-types.md)数值类型都支持这些运算符。

> [!NOTE]
> 对于 `==`、`<`、`>`、`<=` 和 `>=` 运算符，如果所有操作数都不是数字（<xref:System.Double.NaN?displayProperty=nameWithType> 或 <xref:System.Single.NaN?displayProperty=nameWithType>），则运算结果为 `false`。 这意味着 `NaN` 值不大于、小于或等于任何其他 `double`（或 `float`）值，包括 `NaN`。 有关更多信息和示例，请参阅 <xref:System.Double.NaN?displayProperty=nameWithType> 或 <xref:System.Single.NaN?displayProperty=nameWithType> 参考文章。

[char](../builtin-types/char.md) 类型也支持比较运算符。 在使用 `char` 操作数时，将比较对应的字符代码。

枚举类型也支持比较运算符。 对于相同[枚举](../builtin-types/enum.md)类型的操作数，基础整数类型的相应值会进行比较。

[`==` 和 `!=` 运算符](equality-operators.md)检查其操作数是否相等。

## <a name="less-than-operator-"></a>小于运算符 \<

如果左侧操作数小于右侧操作数，`<` 运算符返回 `true`，否则返回 `false`：

[!code-csharp-interactive[less than example](snippets/shared/ComparisonOperators.cs#Less)]

## <a name="greater-than-operator-"></a>大于运算符 >

如果左侧操作数大于右侧操作数，`>` 运算符返回 `true`，否则返回 `false`：

[!code-csharp-interactive[greater than example](snippets/shared/ComparisonOperators.cs#Greater)]

## <a name="less-than-or-equal-operator-"></a>小于或等于运算符 \<=

如果左侧操作数小于或等于右侧操作数，`<=` 运算符返回 `true`，否则返回 `false`：

[!code-csharp-interactive[less than or equal example](snippets/shared/ComparisonOperators.cs#LessOrEqual)]

## <a name="greater-than-or-equal-operator-"></a>大于或等于运算符 >=

如果左侧操作数大于或等于右侧操作数，`>=` 运算符返回 `true`，否则返回 `false`：

[!code-csharp-interactive[greater than or equal example](snippets/shared/ComparisonOperators.cs#GreaterOrEqual)]

## <a name="operator-overloadability"></a>运算符可重载性

用户定义类型可以[重载](operator-overloading.md)`<`、`>`、`<=` 和 `>=` 运算符。

如果某类型重载 `<` 或 `>` 运算符之一，它必须同时重载 `<` 和 `>`。 如果某类型重载 `<=` 或 `>=` 运算符之一，它必须同时重载 `<=` 和 `>=`。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)中的[关系和类型测试运算符](~/_csharplang/spec/expressions.md#relational-and-type-testing-operators)部分。

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 运算符和表达式](index.md)
- <xref:System.IComparable%601?displayProperty=nameWithType>
- [相等运算符](equality-operators.md)
