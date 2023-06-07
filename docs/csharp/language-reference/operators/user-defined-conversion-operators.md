---
title: 用户定义转换运算符 - C# 引用
description: 了解如何在 C# 中定义自定义隐式和显式类型转换。
ms.date: 07/09/2019
f1_keywords:
- explicit_CSharpKeyword
- implicit_CSharpKeyword
- explicit
- implicit
helpviewer_keywords:
- explicit keyword [C#]
- implicit keyword [C#]
- conversion operator [C#]
- user-defined conversion [C#]
ms.openlocfilehash: ab977408ed891bd7c996ce3644b22ea984425664
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90536374"
---
# <a name="user-defined-conversion-operators-c-reference"></a>用户定义转换运算符（C# 引用）

用户定义类型可以定义从或到另一个类型的自定义隐式或显式转换。

隐式转换无需调用特殊语法，并且可以在各种情况（例如，在赋值和方法调用中）下发生。 预定义的 C# 隐式转换始终成功，且永远不会引发异常。 用户定义隐式转换也应如此。 如果自定义转换可能会引发异常或丢失信息，请将其定义为显式转换。

[is](type-testing-and-cast.md#is-operator) 和 [as](type-testing-and-cast.md#as-operator) 运算符不考虑使用用户定义转换。 [强制转换表达式](type-testing-and-cast.md#cast-expression)用于调用用户定义显式转换。

`operator` 和 `implicit` 或 `explicit` 关键字分别用于定义隐式转换或显式转换。 定义转换的类型必须是该转换的源类型或目标类型。 可用两种类型中的任何一种类型来定义两种用户定义类型之间的转换。

下面的示例展示如何定义隐式转换和显式转换：

[!code-csharp[implicit an explicit conversions](snippets/shared/UserDefinedConversions.cs)]

`operator` 关键字也可用于重载预定义的 C# 运算符。 有关详细信息，请参阅[运算符重载](operator-overloading.md)。

## <a name="c-language-specification"></a>C# 语言规范

有关更多信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的以下部分：

- [转换运算符](~/_csharplang/spec/classes.md#conversion-operators)
- [用户定义的转换](~/_csharplang/spec/conversions.md#user-defined-conversions)
- [隐式转换](~/_csharplang/spec/conversions.md#implicit-conversions)
- [显式转换](~/_csharplang/spec/conversions.md#explicit-conversions)

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 运算符和表达式](index.md)
- [运算符重载](operator-overloading.md)
- [类型测试和强制转换运算符](type-testing-and-cast.md)
- [强制转换和类型转换](../../programming-guide/types/casting-and-type-conversions.md)
- [设计准则 - 转换运算符](../../../standard/design-guidelines/operator-overloads.md#conversion-operators)
- [Chained user-defined explicit conversions in C#](/archive/blogs/ericlippert/chained-user-defined-explicit-conversions-in-c)（C# 中链接在一起的用户定义的显式转换）
