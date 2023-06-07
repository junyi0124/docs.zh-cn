---
description: true 和 false 运算符 - C# 参考
title: true 和 false 运算符 - C# 参考
ms.date: 12/10/2018
helpviewer_keywords:
- false operator [C#]
- true operator [C#]
ms.assetid: 81a888fd-011e-4589-b242-6c261fea505e
ms.openlocfilehash: f20f71e31c77c035c48702f01208c5b29c90109c
ms.sourcegitcommit: d579fb5e4b46745fd0f1f8874c94c6469ce58604
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2020
ms.locfileid: "89132373"
---
# <a name="true-and-false-operators-c-reference"></a>true 和 false 运算符（C# 参考）

`true` 运算符返回 [bool](../builtin-types/bool.md) 值 `true`，以指明其操作数一定为 true。 `false` 运算符返回 `bool` 值 `true`，以指明其操作数一定为 false。 无法确保 `true` 和 `false` 运算符互补。 也就是说，`true` 和 `false` 运算符可能同时针对同一个操作数返回 `bool` 值 `false`。 如果某类型定义这两个运算符之一，它还必须定义另一个运算符。

> [!TIP]
> 如需支持三值逻辑（例如，在使用支持三值布尔类型的数据库时），请使用 `bool?` 类型。 C# 提供 `&` 和 `|` 运算符，它们通过 `bool?` 操作数支持三值逻辑。 有关详细信息，请参阅[布尔逻辑运算符](boolean-logical-operators.md)一文的[可以为 null 的布尔逻辑运算符](boolean-logical-operators.md#nullable-boolean-logical-operators)部分。

## <a name="boolean-expressions"></a>布尔表达式

包含已定义 `true` 运算符的类型可以是 [if](../keywords/if-else.md)、[do](../keywords/do.md)、[while](../keywords/while.md) 和 [for](../keywords/for.md) 语句以及[条件运算符 `?:`](conditional-operator.md) 中控制条件表达式的结果的类型。 有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)中的 [Boolean 表达式](~/_csharplang/spec/expressions.md#boolean-expressions)部分。

## <a name="user-defined-conditional-logical-operators"></a>用户定义的条件逻辑运算符

如果包含已定义 `true` 和 `false` 运算符的类型以某种方式[重载](operator-overloading.md)[逻辑 OR 运算符](boolean-logical-operators.md#logical-or-operator-) `|` 或[逻辑 AND 运算符](boolean-logical-operators.md#logical-and-operator-) `&`，可以对相应类型的操作数分别执行[条件逻辑 OR 运算符](boolean-logical-operators.md#conditional-logical-or-operator-) `||` 或[条件逻辑 AND 运算符](boolean-logical-operators.md#conditional-logical-and-operator-) `&&` 运算。 有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的[用户定义条件逻辑运算符](~/_csharplang/spec/expressions.md#user-defined-conditional-logical-operators)部分。

## <a name="example"></a>示例

下面的示例演示了定义 `true` 和 `false` 运算符的类型。 此外，该类型还重载了逻辑 AND 运算符 `&`，因此，也可以对相应类型的操作数计算运算符 `&&`。

[!code-csharp[true and false operators example](snippets/shared/TrueFalseOperators.cs)]

请注意 `&&` 运算符的短路行为。 当 `GetFuelLaunchStatus` 方法返回 `LaunchStatus.Red` 时，不会进行计算的 `&&` 运算符的右侧操作数。 这是因为 `LaunchStatus.Red` 一定为 false。 然后，逻辑 AND 运算符的结果不依赖右侧操作数的值。 示例的输出如下所示：

```console
Getting fuel launch status...
Wait!
```

## <a name="see-also"></a>另请参阅

- [C# 参考](../index.md)
- [C# 运算符和表达式](index.md)
